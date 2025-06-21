모바일 기기용 플러그인을 개발하는 방법을 알아보세요.

## 데스크톱에서 모바일 기기 에뮬레이션하기

개발자 도구에서 직접 모바일 기기에서 실행되는 Obsidian을 에뮬레이션할 수 있습니다.

1.  **개발자 도구**를 엽니다.
2.  **콘솔** 탭을 선택합니다.
3.  다음을 입력하고 `Enter`를 누릅니다.

   ```ts
   this.app.emulateMobile(true);
   ```

모바일 에뮬레이션을 비활성화하려면 다음을 입력하고 `Enter`를 누릅니다:

```ts
this.app.emulateMobile(false);
```


> [!tip]
> 모바일 에뮬레이션을 번갈아 토글하려면 `this.app.isMobile` 플래그를 사용할 수 있습니다:
>
> ```ts
> this.app.emulateMobile(!this.app.isMobile);
> ```

## 실제 모바일 기기에서 웹뷰 검사하기

### Android

Android의 개발자 설정에서 USB 디버깅을 활성화하면 Android 기기에서 실행 중인 Obsidian을 검사할 수 있습니다. 그런 다음 데스크톱/노트북의 크롬 기반 브라우저로 이동하여 `chrome://inspect/`로 이동합니다. 모든 작업을 올바르게 수행했다면, 휴대폰/태블릿을 USB를 통해 PC에 연결하고 해당 링크에서 브라우저를 열었을 때 기기가 팝업으로 나타나며, 거기서부터 일반적인 개발자 도구를 실행할 수 있습니다.

더 자세한 정보는 여기서 찾을 수 있습니다: https://developer.chrome.com/docs/devtools/remote-debugging
### iOS

iOS 16.4 이상을 실행하는 iOS 기기와 macOS 기반 컴퓨터에서 Obsidian을 검사할 수 있습니다. 설정 방법에 대한 지침은 여기서 찾을 수 있습니다: https://webkit.org/web-inspector/enabling-web-inspector/

## 플랫폼별 기능

플러그인이 실행 중인 플랫폼을 감지하려면 [[Platform]]을 사용할 수 있습니다:

```ts
import { Platform } from 'obsidian';

if (Platform.isIosApp) {
  // ...
}

if (Platform.isAndroidApp) {
  // ...
}
```

## 모바일 기기에서 플러그인 비활성화하기

플러그인에 Node.js 또는 Electron API가 필요한 경우, 사용자가 모바일 기기에 플러그인을 설치하지 못하도록 할 수 있습니다.

데스크톱 앱만 지원하려면 [[Manifest]]에서 `isDesktopOnly`를 `true`로 설정하세요.

## 문제 해결

이 섹션에서는 모바일 기기용으로 개발할 때 흔히 발생하는 문제들을 나열합니다.

### Node 및 Electron API

Node.js API와 Electron API는 모바일 기기에서 사용할 수 없습니다. 플러그인이나 그 종속성에서 이러한 라이브러리를 호출하면 플러그인이 충돌할 수 있습니다.

### 정규 표현식의 Lookbehind

정규 표현식의 Lookbehind는 iOS 16.4 이상에서만 지원되며, 일부 iPhone 및 iPad 사용자는 여전히 이전 버전을 사용할 수 있습니다. iOS 사용자를 위한 대체(fallback) 기능을 구현하려면 [[#Platform-specific features]]를 참조하거나, 특정 브라우저 버전을 감지하는 JavaScript 라이브러리를 사용하세요.

더 자세한 정보와 정확한 버전 통계는 [Can I Use](https://caniuse.com/js-regexp-lookbehind)를 참조하세요. "Safari on iOS"를 찾아보세요.
