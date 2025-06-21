[Obsidian v1.7.1](https://obsidian.md/changelog/2024-08-27-desktop-v1.7.1/)에서 "Debug startup time" 뷰를 추가했습니다. 이 뷰는 앱이 시작되는 데 걸리는 시간을 나타냅니다.

플러그인은 앱 로드 시간에 중요한 역할을 합니다. Obsidian이 올바르게 작동하도록 보장하기 위해, Obsidian은 사용자가 앱과 상호 작용하기 전에 모든 플러그인을 로드합니다.

### 플러그인의 로드 시간을 어떻게 개선할 수 있나요?

- 플러그인 `onload`를 단순화하세요.
- 플러그인 뷰 생성자를 확인하세요.
- [일반적인 함정](#Pitfalls)을 피하세요.

먼저, 쉬운 것부터 시작합시다. 플러그인의 프로덕션 빌드를 사용하고 있는지 확인하세요. esbuild, rollup 또는 webpack과 같은 번들러를 사용하는 경우, "개발" 빌드 또는 "프로덕션" 빌드를 생성할 수 있습니다. 프로덕션 빌드는 일반적으로 더 작고, 더 빨리 로드되며, 테스트에만 사용되는 코드를 제거합니다. 릴리스를 생성할 때 `main.js` 파일이 프로덕션 빌드인지 확인하세요.

빌드 구성에서 플러그인 코드를 최소화하는 것을 고려해야 합니다. 이렇게 하면 전체 플러그인 파일 크기가 작아져 플러그인이 디스크에서 읽고 로드하는 속도가 빨라집니다.

다음으로, 플러그인의 `onload` 함수 내에서 비용이 많이 드는 작업을 수행하지 않는지 확인하세요. `onload` 함수는 플러그인 초기화에 필요한 코드만 포함해야 합니다. 여기에는 명령어, 뷰 유형 및 마크다운 후처리기 등록과 같은 앱 등록이 포함됩니다. 계산 비용이 많이 들거나 데이터를 가져오는 작업은 포함되어서는 안 됩니다.

플러그인이 사용자 정의 뷰를 생성하는 경우, 사용자 정의 뷰 생성자에 유의하세요. Obsidian이 열릴 때, 사용자의 작업 공간에 저장된 모든 뷰를 다시 엽니다. 뷰가 로드되면(그리고 [[Understanding deferred views|지연되지 않으면]]), 이는 앱 로드 시간에 직접적인 영향을 미칩니다.

### 시작 시 실행하고 싶은 코드가 있다면 어디에 두어야 하나요?

대부분의 경우, 코드를 `onLayoutReady` 콜백 안에 래핑하고 싶을 것입니다. 이러한 콜백은 지연되며 Obsidian 로딩이 완료된 후에만 호출됩니다.

## 함정

### `vault.on('create')` 수신

Obsidian의 vault 초기화 프로세스의 일부로, 모든 파일에 대해 `create`를 호출합니다. 플러그인이 새로 생성된 파일에 반응해야 하는 경우, 먼저 작업 공간이 준비될 때까지 기다려야 합니다. vault 이벤트 등록은 `onLayoutReady` 콜백 내에 있어야 합니다. 이렇게 하면 작업 공간이 완전히 초기화될 때까지 이벤트에 반응하지 않게 됩니다.

#### 옵션 A. 레이아웃이 준비되었는지 확인

```ts
class MyPlugin extends Plugin {
    onload(app: App) {
	    super(app);
        this.registerEvent(this.app.vault.on('create', this.onCreate, this));
    }

	onCreate() {
	    if (!this.app.workspace.layoutReady) {
	      // 작업 공간이 아직 로딩 중이므로 아무것도 하지 않음
	      return;
	    }
		// ...
	}
}
```

#### 옵션 B. 레이아웃이 준비되면 핸들러 등록

```ts
class MyPlugin extends Plugin {
    onload(app: App) {
	    super(app);
	    this.app.workspace.onLayoutReady(() => {
	        this.registerEvent(this.app.vault.on('create', this.onCreate, this));
	    });
    }

	onCreate() {
		// ...
	}
}
```

플러그인 최적화에 대한 추가 도움이 필요하면 [[Home#Join the developer community|개발자 커뮤니티의 도움]]을 요청하세요!
