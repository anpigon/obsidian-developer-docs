컨텍스트 메뉴를 열고 싶다면 [[Menu|Menu]]를 사용하세요:

```ts
import { Menu, Notice, Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.addRibbonIcon('dice', 'Open menu', (event) => {
      const menu = new Menu();

      menu.addItem((item) =>
        item
          .setTitle('Copy')
          .setIcon('documents')
          .onClick(() => {
            new Notice('Copied');
          })
      );

      menu.addItem((item) =>
        item
          .setTitle('Paste')
          .setIcon('paste')
          .onClick(() => {
            new Notice('Pasted');
          })
      );

      menu.showAtMouseEvent(event);
    });
  }
}
```

[[showAtMouseEvent|showAtMouseEvent()]]는 마우스로 클릭한 위치에 메뉴를 엽니다.

> [!tip]
> 메뉴가 나타나는 위치를 더 세밀하게 제어해야 하는 경우, `menu.showAtPosition({ x: 20, y: 20 })`을 사용하여 Obsidian 창의 왼쪽 상단 모서리를 기준으로 한 위치에 메뉴를 열 수 있습니다.

사용할 수 있는 아이콘에 대한 자세한 내용은 [[Plugins/User interface/Icons|Icons]]를 참조하세요.

`file-menu` 및 `editor-menu` 작업 공간 이벤트를 구독하여 파일 메뉴 또는 에디터 메뉴에 항목을 추가할 수도 있습니다:

![[../../Assets/context-menu-positions.png]]

```ts
import { Notice, Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.registerEvent(
      this.app.workspace.on('file-menu', (menu, file) => {
        menu.addItem((item) => {
          item
            .setTitle('Print file path 👈')
            .setIcon('document')
            .onClick(async () => {
              new Notice(file.path);
            });
        });
      })
    );

    this.registerEvent(
      this.app.workspace.on("editor-menu", (menu, editor, view) => {
        menu.addItem((item) => {
          item
            .setTitle('Print file path 👈')
            .setIcon('document')
            .onClick(async () => {
              new Notice(view.file.path);
            });
        });
      })
    );
  }
}
```

이벤트 처리에 대한 자세한 내용은 [[Events]]를 참조하세요.
