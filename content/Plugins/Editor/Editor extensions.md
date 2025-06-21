---
aliases: editor extension
---

에디터 확장 기능(Editor extensions)을 사용하면 Obsidian에서 노트를 편집하는 경험을 커스터마이즈할 수 있습니다. 이 페이지에서는 에디터 확장 기능이 무엇인지, 언제 사용해야 하는지 설명합니다.

Obsidian은 마크다운 에디터를 구동하기 위해 CodeMirror 6(CM6)을 사용합니다. Obsidian과 마찬가지로 CM6에는 _확장 기능(extensions)_이라는 자체 플러그인이 있습니다. 즉, Obsidian의 _에디터 확장 기능_은 _CodeMirror 6 확장 기능_과 동일합니다.

에디터 확장 기능을 구축하기 위한 API는 약간 독특하며 시작하기 전에 기본적인 아키텍처 이해가 필요합니다. 이 섹션은 시작하기에 충분한 컨텍스트와 예제를 제공하기 위한 것입니다. 에디터 확장 기능 구축에 대해 더 배우고 싶다면 [CodeMirror 6 문서](https://codemirror.net/docs/)를 참조하세요.

## 에디터 확장 기능이 필요한가요?

에디터 확장 기능을 구축하는 것은 도전적일 수 있으므로, 구축을 시작하기 전에 정말로 필요한지 고려하세요.

- 읽기 모드에서 마크다운을 HTML로 변환하는 방식을 변경하려면 [[Markdown post processing|마크다운 후처리기]]를 구축하는 것을 고려하세요.
- 라이브 프리뷰에서 문서의 모양과 느낌을 변경하려면 에디터 확장 기능을 구축해야 합니다.

## 에디터 확장 기능 등록

CodeMirror 6(CM6)은 웹 기술을 사용하여 코드를 편집하기 위한 강력한 엔진입니다. 핵심적으로 에디터 자체는 최소한의 기능 세트를 가지고 있습니다. 현대적인 에디터에서 기대할 수 있는 모든 기능은 선택할 수 있는 _확장 기능_으로 제공됩니다. Obsidian은 이러한 확장 기능 중 많은 것을 기본으로 제공하지만, 자신만의 확장 기능을 등록할 수도 있습니다.

에디터 확장 기능을 등록하려면 Obsidian 플러그인의 `onload` 메서드에서 [[registerEditorExtension|registerEditorExtension()]]을 사용하세요:

```ts
onload() {
  this.registerEditorExtension([examplePlugin, exampleField]);
}
```

CM6는 여러 유형의 확장 기능을 지원하지만, 가장 일반적인 두 가지는 [[View plugins]]와 [[State fields]]입니다.
<DocCardList items={useCurrentSidebarCategory().items}/>
