[[Reference/TypeScript API/Editor|Editor]] 클래스는 편집 모드에서 활성화된 마크다운 문서를 읽고 조작하기 위한 작업을 제공합니다.

명령에서 편집기에 접근하려면 [[Commands#Editor commands|editorCallback]]을 사용하세요.

다른 곳에서 편집기를 사용하려면 활성 뷰에서 접근할 수 있습니다:

```ts
const view = this.app.workspace.getActiveViewOfType(MarkdownView);

// 사용자가 마크다운 파일을 편집 중인지 확인
if (view) {
	const cursor = view.editor.getCursor();

	// ...
}
```

> [!note]
> Obsidian은 기본 텍스트 편집기로 [CodeMirror](https://codemirror.net/) (CM)를 사용하며, API의 일부로 CodeMirror 편집기를 노출합니다. `Editor`는 CM6과 CM5(레거시 편집기, 데스크톱에서만 사용 가능) 간의 기능을 연결하기 위한 추상화 역할을 합니다. CodeMirror 인스턴스에 직접 접근하는 대신 `Editor`를 사용하면 두 플랫폼에서 모두 플러그인이 작동하도록 보장할 수 있습니다.

## 커서 위치에 텍스트 삽입

[[replaceRange|replaceRange()]] 메서드는 두 커서 위치 사이의 텍스트를 교체합니다. 하나의 위치만 제공하면 해당 위치와 다음 위치 사이에 새 텍스트를 삽입합니다.

다음 명령은 커서 위치에 오늘 날짜를 삽입합니다:

```ts
import { Editor, moment, Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.addCommand({
      id: 'insert-todays-date',
      name: '오늘 날짜 삽입',
      editorCallback: (editor: Editor) => {
        editor.replaceRange(
          moment().format('YYYY-MM-DD'),
          editor.getCursor()
        );
      },
    });
  }
}
```

![[editor-todays-date.gif]]

## 현재 선택 영역 교체

선택된 텍스트를 수정하려면 [[replaceSelection|replaceSelection()]]을 사용하여 현재 선택 영역을 새 텍스트로 교체하세요.

다음 명령은 현재 선택 영역을 읽어 대문자로 변환합니다:

```ts
import { Editor, Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.addCommand({
      id: 'convert-to-uppercase',
      name: '대문자로 변환',
      editorCallback: (editor: Editor) => {
        const selection = editor.getSelection();
        editor.replaceSelection(selection.toUpperCase());
      },
    });
  }
}
```

![[editor-uppercase.gif]]
