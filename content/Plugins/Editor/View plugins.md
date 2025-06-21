뷰 플러그인(View plugin)은 에디터 [[Viewport|뷰포트]]에 접근할 수 있는 [[Editor extensions|에디터 확장 기능]]입니다.

> [!note]
> 이 페이지는 Obsidian 플러그인 개발자를 위해 공식 CodeMirror 6 문서를 요약한 것입니다. 상태 관리에 대한 더 자세한 정보는 [Affecting the View](https://codemirror.net/docs/guide/#affecting-the-view)를 참조하세요.

## 필수 조건
- [[Viewport|뷰포트]] 기본 이해

## 뷰 플러그인 생성
뷰 플러그인은 뷰포트가 재계산된 _후에_ 실행되는 에디터 확장 기능입니다. 이는 뷰포트에 접근할 수 있음을 의미하지만, 동시에 뷰 플러그인은 뷰포트에 영향을 미치는 변경 사항을 만들 수 없음을 의미합니다. 예를 들어 문서에 블록이나 줄 바꿈을 삽입하는 등의 작업은 불가능합니다.

> [!tip]
> 블록이나 줄 바꿈을 삽입하는 등 에디터의 수직 레이아웃에 영향을 주는 변경을 원한다면 [[State fields|상태 필드]]를 사용해야 합니다.

뷰 플러그인을 생성하려면 [PluginValue](https://codemirror.net/docs/ref/#view.PluginValue)를 구현하는 클래스를 만들고 [ViewPlugin.fromClass()](https://codemirror.net/docs/ref/#view.ViewPlugin^fromClass) 함수에 전달하세요.

```ts
import {
  ViewUpdate,
  PluginValue,
  EditorView,
  ViewPlugin,
} from '@codemirror/view';

class ExamplePlugin implements PluginValue {
  constructor(view: EditorView) {
    // ...
  }

  update(update: ViewUpdate) {
    // ...
  }

  destroy() {
    // ...
  }
}

export const examplePlugin = ViewPlugin.fromClass(ExamplePlugin);
```

뷰 플러그인의 세 가지 메서드는 라이프사이클을 제어합니다:

- `constructor()`: 플러그인을 초기화합니다.
- `update()`: 사용자가 텍스트를 입력하거나 선택하는 등 변경 사항이 있을 때 플러그인을 업데이트합니다.
- `destroy()`: 플러그인 정리 작업을 수행합니다.

예제의 뷰 플러그인은 동작하지만 많은 기능을 하지 않습니다. 플러그인이 업데이트되는 원인을 더 잘 이해하려면 `update()` 메서드에 `console.log(update);` 줄을 추가하여 모든 업데이트를 콘솔에 출력할 수 있습니다.

## 다음 단계
뷰 플러그인에서 [[Decorations|데코레이션]]을 제공하여 문서 표시 방식을 변경하세요.
