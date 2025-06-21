Obsidian API의 여러 UI 구성 요소를 사용하면 함께 제공되는 아이콘을 구성할 수 있습니다. 내장된 아이콘 중 하나를 선택하거나 직접 추가할 수 있습니다.

## 사용 가능한 아이콘 찾아보기

[lucide.dev](https://lucide.dev/)로 이동하여 사용 가능한 모든 아이콘과 해당 이름을 확인하세요.

**참고:** 현재 v0.446.0까지의 아이콘만 지원됩니다.

## 아이콘 사용하기

사용자 정의 인터페이스에서 아이콘을 사용하려면 [[setIcon|setIcon()]] 유틸리티 함수를 사용하여 [[HTML elements|HTML 요소]]에 아이콘을 추가하세요. 다음 예제는 상태 표시줄에 아이콘을 추가합니다:

```ts
import { Plugin, setIcon } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    const item = this.addStatusBarItem();
    setIcon(item, 'info');
  }
}
```

아이콘 크기를 변경하려면 아이콘을 포함하는 요소에 사전 설정된 크기를 사용하여 `--icon-size` [[Reference/CSS variables/Foundations/Icons|CSS 변수]]를 설정하세요:

```css
div {
  --icon-size: var(--icon-size-m);
}
```

## 나만의 아이콘 추가하기

플러그인에 사용자 정의 아이콘을 추가하려면 [[addIcon|addIcon()]] 유틸리티를 사용하세요:

```ts
import { addIcon, Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    addIcon('circle', `<circle cx="50" cy="50" r="50" fill="currentColor" />`);

    this.addRibbonIcon('circle', 'Click me', () => {
      console.log('Hello, you!');
    });
  }
}
```

`addIcon`은 두 개의 인수를 받습니다:

1.  아이콘을 고유하게 식별할 이름.
2.  주변 `<svg>` 태그를 제외한 아이콘의 SVG 내용.

아이콘이 제대로 그려지려면 `0 0 100 100` 뷰 박스 안에 맞아야 합니다.

`addIcon`을 호출한 후에는 내장된 아이콘처럼 아이콘을 사용할 수 있습니다.

### 아이콘 디자인 가이드라인

Obsidian 인터페이스와의 호환성 및 통일성을 위해 아이콘은 [Lucide의 가이드라인](https://lucide.dev/guide/design/icon-design-guide)을 따라야 합니다:

- 아이콘은 24x24 픽셀 캔버스에서 디자인되어야 합니다.
- 아이콘은 캔버스 내에 최소 1픽셀의 여백이 있어야 합니다.
- 아이콘은 2픽셀의 획 두께를 가져야 합니다.
- 아이콘은 둥근 조인을 사용해야 합니다.
- 아이콘은 둥근 캡을 사용해야 합니다.
- 아이콘은 중앙 정렬된 획을 사용해야 합니다.
- 아이콘의 도형(예: 사각형)은 2픽셀의 테두리 반경을 가져야 합니다.
- 개별 요소는 서로 2픽셀의 간격이 있어야 합니다.

Lucide는 또한 Illustrator, Figma, Inkscape와 같은 벡터 편집기를 위한 [템플릿과 가이드](https://github.com/lucide-icons/lucide/blob/main/CONTRIBUTING.md)를 제공합니다.
