데코레이션(Decorations)을 사용하면 [[Editor extensions|에디터 확장 기능]]에서 콘텐츠를 어떻게 그리거나 스타일링할지 제어할 수 있습니다. 에디터에서 요소를 추가, 교체하거나 스타일을 변경하여 모양과 느낌을 바꾸려면 대부분 데코레이션을 사용해야 합니다.

이 페이지를 읽고 나면 다음을 할 수 있게 됩니다:
- 에디터 외관을 변경하기 위해 데코레이션을 사용하는 방법 이해
- 상태 필드와 뷰 플러그인을 사용하여 데코레이션을 제공하는 차이점 이해

> [!note]
> 이 페이지는 Obsidian 플러그인 개발자를 위해 공식 CodeMirror 6 문서를 요약한 것입니다. 상태 필드에 대한 더 자세한 정보는 [Decorating the Document](https://codemirror.net/docs/guide/#decorating-the-document)를 참조하세요.

## 필수 조건
- [[State fields]] 기본 이해
- [[View plugins]] 기본 이해

## 개요
데코레이션 없이는 문서가 일반 텍스트로 렌더링됩니다. 전혀 흥미롭지 않죠. 데코레이션을 사용하면 텍스트 강조나 커스텀 HTML 요소 추가와 같이 문서 표시 방식을 변경할 수 있습니다.

다음 유형의 데코레이션을 사용할 수 있습니다:
- [마크 데코레이션](https://codemirror.net/docs/ref/#view.Decoration%5Emark): 기존 요소 스타일링
- [위젯 데코레이션](https://codemirror.net/docs/ref/#view.Decoration%5Ewidget): 문서에 요소 삽입
- [교체 데코레이션](https://codemirror.net/docs/ref/#view.Decoration%5Ereplace): 문서 일부를 다른 요소로 숨기거나 교체
- [라인 데코레이션](https://codemirror.net/docs/ref/#view.Decoration%5Eline): 문서 자체가 아닌 라인에 스타일 추가

데코레이션을 사용하려면 에디터 확장 기능 내부에서 데코레이션을 생성하고 확장 기능이 이를 에디터에 _제공_하도록 해야 합니다. 데코레이션을 에디터에 제공하는 방법은 두 가지입니다: [[State fields|상태 필드]]를 사용해 _직접_ 제공하거나 [[View plugins|뷰 플러그인]]을 사용해 _간접적으로_ 제공하는 방법입니다.

## 뷰 플러그인과 상태 필드 중 어떤 것을 사용해야 하나요?
뷰 플러그인과 상태 필드 모두 데코레이션을 제공할 수 있지만 몇 가지 차이점이 있습니다.

- [[Viewport]] 내부에 있는 내용을 기반으로 데코레이션을 결정할 수 있다면 뷰 플러그인을 사용하세요.
- 뷰포트 외부의 데코레이션을 관리해야 한다면 상태 필드를 사용하세요.
- 줄 바꿈 추가와 같이 뷰포트 내용을 변경할 수 있는 변경을 원한다면 상태 필드를 사용하세요.

두 접근 방식 중 어느 것으로도 확장 기능을 구현할 수 있다면 일반적으로 뷰 플러그인이 더 나은 성능을 제공합니다. 예를 들어 문서의 맞춤법을 검사하는 에디터 확장 기능을 구현한다고 가정해 보겠습니다.

한 가지 방법은 전체 문서를 외부 맞춤법 검사기에 전달한 다음 맞춤법 오류 목록을 반환받는 것입니다. 이 경우 각 오류를 데코레이션에 매핑하고 현재 뷰포트에 무엇이 있는지와 관계없이 데코레이션을 관리하기 위해 상태 필드를 사용해야 합니다.

다른 방법은 뷰포트에 보이는 내용만 맞춤법 검사를 하는 것입니다. 확장 기능은 사용자가 문서를 스크롤할 때 지속적으로 맞춤법 검사를 실행해야 하지만 수백만 줄의 텍스트가 있는 문서도 맞춤법 검사를 할 수 있습니다.

![상태 필드 vs 뷰 플러그인](decorations.svg)

## 데코레이션 제공
불릿 리스트 항목을 이모지로 바꾸는 에디터 확장 기능을 만들고 싶다고 가정해 보겠습니다. 뷰 플러그인이나 상태 필드를 사용하여 이를 구현할 수 있으며 각각 약간의 차이가 있습니다. 이 섹션에서는 두 유형의 확장 기능으로 구현하는 방법을 살펴보겠습니다.

두 구현 모두 동일한 핵심 로직을 공유합니다:
1. [syntaxTree](https://codemirror.net/docs/ref/#language.syntaxTree)를 사용하여 리스트 항목 찾기
2. 각 리스트 항목에 대해 선행 하이픈 `-`을 _위젯_으로 교체

### 위젯
위젯은 에디터에 추가할 수 있는 커스텀 HTML 요소입니다. 문서의 특정 위치에 위젯을 삽입하거나 콘텐츠 일부를 위젯으로 교체할 수 있습니다.

다음 예제는 HTML 요소 `<span>👉</span>`을 반환하는 위젯을 정의합니다. 나중에 이 위젯을 사용하게 됩니다.

```ts
import { EditorView, WidgetType } from '@codemirror/view';

export class EmojiWidget extends WidgetType {
  toDOM(view: EditorView): HTMLElement {
    const div = document.createElement('span');

    div.innerText = '👉';

    return div;
  }
}
```

문서의 콘텐츠 범위를 이모지 위젯으로 교체하려면 [교체 데코레이션](https://codemirror.net/docs/ref/#view.Decoration%5Ereplace)을 사용하세요.

```ts
const decoration = Decoration.replace({
  widget: new EmojiWidget()
});
```

### 상태 필드
상태 필드에서 데코레이션을 제공하려면:
1. `DecorationSet` 타입으로 [[State fields#Defining a state field|상태 필드 정의]]
2. 상태 필드에 `provide` 속성 추가

   ```ts
   provide(field: StateField<DecorationSet>): Extension {
     return EditorView.decorations.from(field);
   },
   ```

```ts
import { syntaxTree } from '@codemirror/language';
import {
  Extension,
  RangeSetBuilder,
  StateField,
  Transaction,
} from '@codemirror/state';
import {
  Decoration,
  DecorationSet,
  EditorView,
  WidgetType,
} from '@codemirror/view';
import { EmojiWidget } from 'emoji';

export const emojiListField = StateField.define<DecorationSet>({
  create(state): DecorationSet {
    return Decoration.none;
  },
  update(oldState: DecorationSet, transaction: Transaction): DecorationSet {
    const builder = new RangeSetBuilder<Decoration>();

    syntaxTree(transaction.state).iterate({
      enter(node) {
        if (node.type.name.startsWith('list')) {
          // '-' 또는 '*'의 위치
          const listCharFrom = node.from - 2;

          builder.add(
            listCharFrom,
            listCharFrom + 1,
            Decoration.replace({
              widget: new EmojiWidget(),
            })
          );
        }
      },
    });

    return builder.finish();
  },
  provide(field: StateField<DecorationSet>): Extension {
    return EditorView.decorations.from(field);
  },
});
```

### 뷰 플러그인
뷰 플러그인을 사용하여 데코레이션을 관리하려면:
1. [[View plugins#Creating a view plugin|뷰 플러그인 생성]]
2. 플러그인에 `DecorationSet` 멤버 속성 추가
3. `constructor()`에서 데코레이션 초기화
4. `update()`에서 데코레이션 재구성

모든 업데이트가 데코레이션 재구성의 이유는 아닙니다. 다음 예제는 기본 문서나 뷰포트가 변경될 때만 데코레이션을 재구성합니다.

```ts
import { syntaxTree } from '@codemirror/language';
import { RangeSetBuilder } from '@codemirror/state';
import {
  Decoration,
  DecorationSet,
  EditorView,
  PluginSpec,
  PluginValue,
  ViewPlugin,
  ViewUpdate,
  WidgetType,
} from '@codemirror/view';
import { EmojiWidget } from 'emoji';

class EmojiListPlugin implements PluginValue {
  decorations: DecorationSet;

  constructor(view: EditorView) {
    this.decorations = this.buildDecorations(view);
  }

  update(update: ViewUpdate) {
    if (update.docChanged || update.viewportChanged) {
      this.decorations = this.buildDecorations(update.view);
    }
  }

  destroy() {}

  buildDecorations(view: EditorView): DecorationSet {
    const builder = new RangeSetBuilder<Decoration>();

    for (let { from, to } of view.visibleRanges) {
      syntaxTree(view.state).iterate({
        from,
        to,
        enter(node) {
          if (node.type.name.startsWith('list')) {
            // '-' 또는 '*'의 위치
            const listCharFrom = node.from - 2;

            builder.add(
              listCharFrom,
              listCharFrom + 1,
              Decoration.replace({
                widget: new EmojiWidget(),
              })
            );
          }
        },
      });
    }

    return builder.finish();
  }
}

const pluginSpec: PluginSpec<EmojiListPlugin> = {
  decorations: (value: EmojiListPlugin) => value.decorations,
};

export const emojiListPlugin = ViewPlugin.fromClass(
  EmojiListPlugin,
  pluginSpec
);
```

`buildDecorations()`는 에디터 뷰를 기반으로 완전한 데코레이션 세트를 구성하는 도우미 메서드입니다.

`ViewPlugin.fromClass()` 함수의 두 번째 인수에 주목하세요. `PluginSpec`의 `decorations` 속성은 뷰 플러그인이 데코레이션을 에디터에 제공하는 방법을 지정합니다.

뷰 플러그인은 사용자에게 보이는 내용을 알고 있으므로 `view.visibleRanges`를 사용하여 구문 트리에서 방문할 부분을 제한할 수 있습니다.
