Obsidian 인터페이스의 왼쪽에 있는 사이드바는 주로 _리본(ribbon)_ 으로 알려져 있습니다. 리본의 목적은 플러그인에 의해 정의된 작업을 호스팅하는 것입니다.

리본에 작업을 추가하려면 [[addRibbonIcon|addRibbonIcon()]] 메소드를 사용하세요:

```ts
import { Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.addRibbonIcon('dice', 'Print to console', () => {
      console.log('Hello, you!');
    });
  }
}
```

첫 번째 인수는 사용할 아이콘을 지정합니다. 사용 가능한 아이콘 및 자신만의 아이콘을 추가하는 방법에 대한 자세한 내용은 [[Plugins/User interface/Icons|Icons]]를 참조하세요.

> [!note]
> 사용자는 리본에서 플러그인의 아이콘을 제거하거나 리본을 완전히 숨기도록 선택할 수 있습니다. 따라서 [[Plugins/User interface/Commands|명령어]]를 생성하는 것과 같이 리본에 있는 기능에 접근할 수 있는 대체 방법을 포함하는 것이 좋습니다.
