읽기 모드에서 마크다운 문서가 렌더링되는 방식을 변경하려면 자신만의 _마크다운 후처리기(Markdown post processor)_를 추가할 수 있습니다. 이름에서 알 수 있듯이, 후처리기는 마크다운이 HTML로 처리된 _후에_ 실행됩니다. 이를 통해 렌더링된 문서에 [[HTML elements|HTML 요소]]를 추가, 제거 또는 교체할 수 있습니다.

다음 예제는 두 개의 콜론 `:` 사이에 텍스트가 있는 코드 블록을 찾아 적절한 이모지로 교체합니다:

```ts
import { Plugin } from 'obsidian';

const ALL_EMOJIS: Record<string, string> = {
  ':+1:': '👍',
  ':sunglasses:': '😎',
  ':smile:': '😄',
};

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.registerMarkdownPostProcessor((element, context) => {
      const codeblocks = element.findAll('code');

      for (let codeblock of codeblocks) {
        const text = codeblock.innerText.trim();
        if (text[0] === ':' && text[text.length - 1] === ':') {
          const emojiEl = codeblock.createSpan({
            text: ALL_EMOJIS[text] ?? text,
          });
          codeblock.replaceWith(emojiEl);
        }
      }
    });
  }
}
```

## 마크다운 코드 블록 후처리

[Mermaid](https://mermaid-js.github.io/) 다이어그램을 다음과 같은 텍스트 정의가 있는 `mermaid` 코드 블록으로 생성할 수 있다는 것을 알고 계셨나요?:

````md
```mermaid
flowchart LR
    Start --> Stop
```
````

미리보기 모드로 전환하면 코드 블록의 텍스트가 다음과 같은 다이어그램으로 변환됩니다:

```mermaid
flowchart LR
    Start --> Stop
```

Mermaid와 같은 사용자 정의 코드 블록을 추가하려면 [[registerMarkdownCodeBlockProcessor|registerMarkdownCodeBlockProcessor()]]를 사용할 수 있습니다. 다음 예제는 CSV 데이터가 포함된 코드 블록을 테이블로 렌더링합니다:

```ts
import { Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.registerMarkdownCodeBlockProcessor('csv', (source, el, ctx) => {
      const rows = source.split('\n').filter((row) => row.length > 0);

      const table = el.createEl('table');
      const body = table.createEl('tbody');

      for (let i = 0; i < rows.length; i++) {
        const cols = rows[i].split(',');

        const row = body.createEl('tr');

        for (let j = 0; j < cols.length; j++) {
          row.createEl('td', { text: cols[j] });
        }
      }
    });
  }
}
