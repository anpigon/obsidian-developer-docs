상태 표시줄에 새 블록을 만들려면 `onload()` 메소드에서 [[addStatusBarItem|addStatusBarItem()]]을 호출하세요. `addStatusBarItem()` 메소드는 자신만의 요소를 추가할 수 있는 [[HTML elements]]를 반환합니다.

> [!caution] Obsidian 모바일
> 사용자 정의 상태 표시줄 항목은 Obsidian 모바일 앱에서 지원되지 **않습니다**.

```ts
import { Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    const item = this.addStatusBarItem();
    item.createEl('span', { text: 'Hello from the status bar 👋' });
  }
}
```

> [!note]
> `createEl()` 메소드 사용 방법에 대한 자세한 내용은 [[HTML elements]]를 참조하세요.

`addStatusBarItem()`을 여러 번 호출하여 여러 상태 표시줄 항목을 추가할 수 있습니다. Obsidian은 항목들 사이에 간격을 추가하므로, 간격을 더 세밀하게 제어해야 하는 경우 동일한 상태 표시줄 항목에 여러 HTML 요소를 만들어야 합니다.

```ts
import { Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    const fruits = this.addStatusBarItem();
    fruits.createEl('span', { text: '🍎' });
    fruits.createEl('span', { text: '🍌' });

    const veggies = this.addStatusBarItem();
    veggies.createEl('span', { text: '🥦' });
    veggies.createEl('span', { text: '🥬' });
  }
}
```

위 예제는 다음 상태 표시줄을 결과로 보여줍니다:

![[../../Assets/status-bar.png]]
