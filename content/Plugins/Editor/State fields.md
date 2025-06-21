상태 필드(State field)는 사용자 정의 에디터 상태를 관리할 수 있는 [[Editor extensions|에디터 확장 기능]]입니다. 이 페이지는 계산기 확장 기능을 구현하면서 상태 필드를 구축하는 과정을 안내합니다.

계산기는 현재 상태에 숫자를 더하거나 빼고, 다시 시작하려면 상태를 초기화할 수 있어야 합니다.

이 페이지를 마치면 상태 필드를 구축하는 기본 개념을 이해하게 될 것입니다.

> [!note]
> 이 페이지는 Obsidian 플러그인 개발자를 위해 공식 CodeMirror 6 문서를 요약한 것입니다. 상태 필드에 대한 더 자세한 정보는 [State Fields](https://codemirror.net/docs/guide/#state-fields)를 참조하세요.

## 필수 조건
- [[State management]] 기본 이해

## 상태 효과 정의
상태 효과(State effect)는 수행하려는 상태 변경을 설명합니다. 클래스의 메서드라고 생각할 수 있습니다.

계산기 예제에서는 각 계산기 연산에 대한 상태 효과를 정의합니다:

```ts
const addEffect = StateEffect.define<number>();
const subtractEffect = StateEffect.define<number>();
const resetEffect = StateEffect.define();
```

꺾쇠 괄호 `<>` 사이의 타입은 효과에 대한 입력 타입을 정의합니다. 예를 들어 더하거나 빼려는 숫자입니다. 리셋 효과는 입력이 필요 없으므로 생략할 수 있습니다.

## 상태 필드 정의
생각과 달리 상태 필드는 실제로 상태를 _저장_하지 않습니다. 상태를 _관리_합니다. 상태 필드는 현재 상태를 가져와 상태 효과를 적용하고 새 상태를 반환합니다.

상태 필드는 트랜잭션의 효과에 따라 수학적 연산을 적용하는 계산기 로직을 포함합니다. 트랜잭션에는 두 개의 추가와 같은 여러 효과가 포함될 수 있으므로 상태 필드는 이를 모두 하나씩 적용해야 합니다.

```ts
export const calculatorField = StateField.define<number>({
  create(state: EditorState): number {
    return 0;
  },
  update(oldState: number, transaction: Transaction): number {
    let newState = oldState;

    for (let effect of transaction.effects) {
      if (effect.is(addEffect)) {
        newState += effect.value;
      } else if (effect.is(subtractEffect)) {
        newState -= effect.value;
      } else if (effect.is(resetEffect)) {
        newState = 0;
      }
    }

    return newState;
  },
});
```

- `create`는 계산기가 시작하는 값을 반환합니다.
- `update`는 효과를 적용하는 로직을 포함합니다.
- `effect.is()`는 효과를 적용하기 전에 타입을 확인할 수 있게 합니다.

## 상태 효과 전달
상태 필드에 상태 효과를 적용하려면 트랜잭션의 일부로 에디터 뷰에 전달해야 합니다.

```ts
view.dispatch({
  effects: [addEffect.of(num)],
});
```

더 친숙한 API를 제공하는 도우미 함수 세트를 정의할 수도 있습니다:

```ts
export function add(view: EditorView, num: number) {
  view.dispatch({
    effects: [addEffect.of(num)],
  });
}

export function subtract(view: EditorView, num: number) {
  view.dispatch({
    effects: [subtractEffect.of(num)],
  });
}

export function reset(view: EditorView) {
  view.dispatch({
    effects: [resetEffect.of(null)],
  });
}
```

## 다음 단계
상태 필드에서 [[Decorations]]를 제공하여 문서 표시 방식을 변경하세요.
