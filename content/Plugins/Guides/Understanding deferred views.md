Obsidian v1.7.2부터 Obsidian이 로드될 때 모든 뷰는 **DeferredView**의 인스턴스로 생성됩니다. 뷰가 화면에 표시되면(즉, 포함된 탭 그룹 내에서 탭이 선택되면), `leaf`는 다시 렌더링되고 뷰는 올바른 `View` 인스턴스로 전환됩니다.

이 변경으로 인해 플러그인이 현재 만들고 있는 일부 가정이 깨질 수 있습니다.

### `leaf.view`에 접근하기

플러그인이 작업 공간을 반복하는 경우(`iterateAllLeaves` 또는 `getLeavesOfType` 사용), `leaf.view`에 대한 가정을 하기 전에 `instanceof` 확인을 수행하는 것이 이제 매우 중요합니다.

```ts
// 나쁜 예
workspace.iterateAllLeaves(leaf => {
    if (leaf.view.getViewType() === 'my-view') {
        let view = leaf.view as MyCustomView;
        ...
    }
});

// 좋은 예
workspace.iterateAllLeaves(leaf => {
    if (leaf.view instanceof MyCustomView) {
        ...
    }
});
```

```ts
// 나쁜 예
let leaf = workspace.getLeavesOfType('my-view').first();
if (leaf) {
	let view = leaf.view as MyCustomView;
}
...

// 좋은 예
let leaf = workspace.getLeavesOfType('my-view').first();
if (leaf && leaf.view instanceof MyCustomView) {
    ...
}
```

이렇게 하면 플러그인이 작업 공간에 대한 잘못된 가정으로 인해 중단되고 오류가 발생하는 것을 방지할 수 있습니다.

### 작업 공간 어디에서나 `CustomView`에 접근하기

> 따라야 할 일반적인 규칙: 플러그인이 뷰와 통신하려고 시도하는 경우 해당 뷰는 보여야 합니다.

플러그인이 작업 공간의 `CustomView` 인스턴스에 접근해야 하는 경우, 이전 코드 조각이 작동하지 않는 것을 알 수 있습니다.

대부분의 사용 사례에서 해결책은 간단합니다:

```ts
let leaf = workspace.getLeavesOfType('my-view').first();
if (leaf) {
	await workspace.revealLeaf(leaf); // 뷰가 보이도록 하고, 뷰가 완전히 로드되도록 `await`합니다.
	if (leaf.view instanceof MyCustomView) {
		let view = leaf.view; // 이제 CustomView를 가졌습니다.
	}
}
```

대부분의 경우, 이것이 사용자 정의 뷰에 접근하는 올바른 방법입니다.

### 드러내지 않고 `CustomView`에 접근하기 (고급)

뷰를 드러내지 않고 접근하고 싶은 경우가 있습니다. 예를 들어, 플러그인이 기존 뷰 유형에 수정을 적용하는 경우입니다.

이 경우 뷰가 로드되도록 수동으로 요청해야 합니다.

```ts
let leaves = workspace.getLeavesOfType('my-view');
for (let leaf of leaves) {
  if (requireApiVersion('1.7.2')) {
    await leaf.loadIfDeferred(); // 뷰가 완전히 로드되었는지 확인
  }
  // 여기서 수정 수행...
}
```

> [!Warning] 성능 경고
> `loadIfDeferred`를 수동으로 호출하면, 플러그인은 주어진 뷰에서 이 성능 최적화를 제거합니다. 이것을 *아껴서* 사용하세요.
