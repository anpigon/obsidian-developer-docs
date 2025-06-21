Obsidian의 많은 인터페이스에서는 애플리케이션 전체에서 발생하는 이벤트를 구독할 수 있습니다. 예를 들어 사용자가 파일을 변경할 때와 같은 이벤트입니다.

등록된 모든 이벤트 핸들러는 플러그인이 언로드될 때 분리되어야 합니다. 이를 보장하는 가장 안전한 방법은 [[registerEvent|registerEvent()]] 메서드를 사용하는 것입니다.

```ts
import { Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.registerEvent(this.app.vault.on('create', () => {
      console.log('새 파일이 생성되었습니다');
    }));
  }
}
```

## 타이밍 이벤트

일정한 간격으로 함수를 반복적으로 호출하려면 [[registerInterval|registerInterval()]] 메서드와 함께 [`window.setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/setInterval) 함수를 사용하세요.

다음 예제는 상태 표시줄에 현재 시간을 1초마다 업데이트하여 표시합니다:

```ts
import { moment, Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  statusBar: HTMLElement;

  async onload() {
    this.statusBar = this.addStatusBarItem();

    this.updateStatusBar();

    this.registerInterval(
      window.setInterval(() => this.updateStatusBar(), 1000)
    );
  }

  updateStatusBar() {
    this.statusBar.setText(moment().format('H:mm:ss'));
  }
}
```

> [!tip] 날짜와 시간
> [Moment](https://momentjs.com/)는 날짜와 시간을 다루는 인기 있는 JavaScript 라이브러리입니다. Obsidian은 내부적으로 Moment를 사용하므로 별도로 설치할 필요가 없습니다. 대신 Obsidian API에서 가져올 수 있습니다:
>
> ```ts
> import { moment } from 'obsidian';
