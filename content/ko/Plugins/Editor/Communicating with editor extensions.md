에디터 확장 기능을 만들었다면, 에디터 외부(예: [[Commands|명령]]이나 [[Ribbon actions|리본 액션]]을 통해)와 통신하고 싶을 수 있습니다.

[[MarkdownView|MarkdownView]]에서 CodeMirror 6 에디터에 접근할 수 있습니다. 하지만 Obsidian API가 실제로 에디터를 노출하지 않기 때문에 TypeScript에게 그것이 존재한다고 믿도록 `@ts-expect-error`를 사용해야 합니다.

```ts
import { EditorView } from '@codemirror/view';

// @ts-expect-error, 타입이 정의되지 않음
const editorView = view.editor.cm as EditorView;
```

## 뷰 플러그인

`EditorView.plugin()` 메서드를 통해 [[View plugins|뷰 플러그인]] 인스턴스에 접근할 수 있습니다.

```ts
this.addCommand({
	id: 'example-editor-command',
	name: '에디터 명령 예제',
	editorCallback: (editor, view) => {
		// @ts-expect-error, 타입이 정의되지 않음
		const editorView = view.editor.cm as EditorView;

		const plugin = editorView.plugin(examplePlugin);

		if (plugin) {
			plugin.addPointerToSelection(editorView);
		}
	},
});
```

## 상태 필드

에디터 뷰에서 직접 변경 사항을 전달하고 [[State fields#Dispatching state effects|상태 효과를 전달]]할 수 있습니다.

```ts
this.addCommand({
	id: 'example-editor-command',
	name: '에디터 명령 예제',
	editorCallback: (editor, view) => {
		// @ts-expect-error, 타입이 정의되지 않음
		const editorView = view.editor.cm as EditorView;

		editorView.dispatch({
			effects: [
				// ...
			],
		});
	},
});
