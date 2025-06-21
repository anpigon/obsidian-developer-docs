뷰(Views)는 Obsidian이 콘텐츠를 표시하는 방법을 결정합니다. 파일 탐색기, 그래프 뷰, 마크다운 뷰는 모두 뷰의 예시이지만, 플러그인에 맞는 방식으로 콘텐츠를 표시하는 사용자 정의 뷰를 만들 수도 있습니다.

사용자 정의 뷰를 만들려면 [[ItemView|ItemView]] 인터페이스를 확장하는 클래스를 만듭니다:

```ts
import { ItemView, WorkspaceLeaf } from 'obsidian';

export const VIEW_TYPE_EXAMPLE = 'example-view';

export class ExampleView extends ItemView {
  constructor(leaf: WorkspaceLeaf) {
    super(leaf);
  }

  getViewType() {
    return VIEW_TYPE_EXAMPLE;
  }

  getDisplayText() {
    return 'Example view';
  }

  async onOpen() {
    const container = this.contentEl;
    container.empty();
    container.createEl('h4', { text: 'Example view' });
  }

  async onClose() {
    // 정리할 것이 없습니다.
  }
}
```

> [!note]
> `createEl()` 메소드 사용 방법에 대한 자세한 내용은 [[HTML elements]]를 참조하세요.

각 뷰는 텍스트 문자열로 고유하게 식별되며, 여러 작업에서 사용하려는 뷰를 지정해야 합니다. 이 가이드의 뒷부분에서 보게 될 것처럼, 이를 상수 `VIEW_TYPE_EXAMPLE`로 추출하는 것이 좋습니다.

- `getViewType()`은 뷰의 고유 식별자를 반환합니다.
- `getDisplayText()`는 뷰의 사람이 읽기 쉬운 이름을 반환합니다.
- `onOpen()`은 새 리프(leaf) 내에서 뷰가 열릴 때 호출되며 뷰의 콘텐츠를 빌드하는 역할을 합니다.
- `onClose()`는 뷰가 닫혀야 할 때 호출되며 뷰에서 사용하는 모든 리소스를 정리하는 역할을 합니다.

사용자 정의 뷰는 플러그인이 활성화될 때 등록되고 플러그인이 비활성화될 때 정리되어야 합니다:

```ts
import { Plugin, WorkspaceLeaf } from 'obsidian';
import { ExampleView, VIEW_TYPE_EXAMPLE } from './view';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.registerView(
      VIEW_TYPE_EXAMPLE,
      (leaf) => new ExampleView(leaf)
    );

    this.addRibbonIcon('dice', 'Activate view', () => {
      this.activateView();
    });
  }

  async onunload() {
  }

  async activateView() {
    const { workspace } = this.app;

    let leaf: WorkspaceLeaf | null = null;
    const leaves = workspace.getLeavesOfType(VIEW_TYPE_EXAMPLE);

    if (leaves.length > 0) {
      // 우리 뷰가 있는 리프가 이미 존재하므로 그것을 사용합니다.
      leaf = leaves[0];
    } else {
      // 작업 공간에서 우리 뷰를 찾을 수 없으므로
      // 오른쪽 사이드바에 새 리프를 만듭니다.
      leaf = workspace.getRightLeaf(false);
      await leaf.setViewState({ type: VIEW_TYPE_EXAMPLE, active: true });
    }

    // 축소된 사이드바에 있는 경우 리프를 "드러냅니다".
    workspace.revealLeaf(leaf);
  }
}
```

[[registerView|registerView()]]의 두 번째 인수는 등록하려는 뷰의 인스턴스를 반환하는 팩토리 함수입니다.

> [!warning]
> 플러그인에서 뷰에 대한 참조를 절대 관리하지 마세요. Obsidian은 뷰 팩토리 함수를 여러 번 호출할 수 있습니다. 뷰에서 부작용을 피하고, 뷰 인스턴스에 접근해야 할 때마다 `getLeavesOfType()`을 사용하세요.
>
> ```ts
> this.app.workspace.getLeavesOfType(VIEW_TYPE_EXAMPLE).forEach((leaf) => {
>   if (leaf.view instanceof ExampleView) {
>     // 뷰 인스턴스에 접근합니다.
>   }
> });
