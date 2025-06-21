---
aliases:
  - RTL
description: Obsidian은 아랍어, 디베히어, 히브리어, 페르시아어, 시리아어, 우르두어와 같은 오른쪽에서 왼쪽으로 쓰는(RTL) 언어를 지원합니다. 이러한 언어는 6억 명 이상이 사용합니다. Obsidian용 플러그인과 테마를 개발할 때 인터페이스 변경 사항이 언어 인터페이스 및 콘텐츠의 방향에 어떻게 적응할지 고려하는 것이 중요합니다.
---

> [!Warning] Obsidian 1.6의 새로운 기능
> Obsidian 1.6에는 미러링된 UI와 혼합 언어 지원 등 오른쪽에서 왼쪽으로 쓰는 언어에 대한 많은 개선 사항이 포함되어 있습니다. 이러한 변경 사항은 테마와 플러그인에 영향을 줄 수 있습니다.

Obsidian은 아랍어, 디베히어, 히브리어, 페르시아어, 시리아어, 우르두어와 같은 오른쪽에서 왼쪽으로 쓰는(RTL) 언어를 지원합니다. 이러한 언어는 6억 명 이상이 사용합니다. Obsidian용 플러그인과 테마를 개발할 때 인터페이스 변경 사항이 언어 인터페이스 및 콘텐츠의 방향에 어떻게 적응할지 고려하는 것이 중요합니다.

RTL 언어는 Obsidian 내에서 두 가지 중요한 맥락으로 존재할 수 있습니다: 앱 인터페이스와 노트의 콘텐츠.

- **앱 인터페이스**는 Obsidian 설정에서 선택한 언어에 의해 정의됩니다. 사용자가 RTL 언어를 선택하면 앱 인터페이스가 자동으로 반전되고 `body` 요소에 `.mod-rtl` 클래스가 추가됩니다. 특정 인터페이스 언어는 `html` 요소의 `lang` 속성에도 추가됩니다.
- **노트의 콘텐츠**는 왼쪽에서 오른쪽으로(LTR) 쓰는 언어, RTL 언어로 작성되거나 동일한 노트 내에서 LTR 및 RTL 언어를 혼합하여 작성할 수 있습니다. Obsidian은 에디터에서 언어의 방향을 자동으로 감지하고 각 줄에 `dir` 속성을 추가합니다.

사용자가 인터페이스 언어로 RTL 언어를 선택하거나 Obsidian 설정에서 RTL을 기본 에디터 방향으로 설정하면 에디터에 `dir="rtl"` 속성이 추가됩니다.

> [!INFO] 혼합 방향 지원
> 많은 RTL 사용자는 인터페이스에 LTR 언어를 사용하면서 일부 노트를 RTL 언어로 작성하거나 동일한 노트 내에서 LTR 및 RTL 언어를 혼합하여 사용하는 것을 선택합니다.

## RTL 인터페이스에 대한 사용자 기대치

주요 운영 체제는 RTL 언어 사용자를 위해 인터페이스를 반전시킵니다. 운영 체제에서 제공하는 사용자 인터페이스 구성 요소는 일반적으로 수평으로 미러링됩니다. 이렇게 작동하지 않는 앱은 RTL 사용자에게 어색하게 느껴질 수 있습니다.

다음 가이드는 LTR 및 RTL 모두에서 작동하는 인터페이스를 디자인하는 데 유용한 참조를 제공합니다:

- [Apple RTL 휴먼 인터페이스 가이드라인](https://developer.apple.com/design/human-interface-guidelines/right-to-left)
- [RTL 스타일링 101](https://rtlstyling.com/)
- [MDN 논리적 속성 및 값](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_logical_properties_and_values)

## 플러그인과 테마를 언어 방향에 구애받지 않도록 만들기

Obsidian은 웹 기술을 사용하여 구축되었으므로 기존 CSS 및 HTML 기능을 사용하여 인터페이스가 언어 방향에 적응하도록 합니다.

### 논리적 속성 사용, 방향성 속성 피하기

CSS를 사용하여 위치 및 간격을 추가할 때마다 `left` 및 `right`와 같은 방향성 대안 대신 `start` 및 `end`와 같은 논리적 속성 및 값을 사용하세요. 논리적 속성 및 값의 전체 목록은 [MDN 문서](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_logical_properties_and_values)를 참조하세요.

방향성 속성보다 논리적 속성을 선호하세요:

| 속성                 | 방향성          | 논리적                 |
| -------------------- | --------------- | ---------------------- |
| 여백(Margins)        | `margin-left`   | `margin-inline-start`  |
|                      | `margin-right`  | `margin-inline-end`    |
| 안쪽 여백(Padding)   | `padding-left`  | `padding-inline-start` |
|                      | `padding-right` | `padding-inline-end`   |
| 테두리(Borders)      | `border-left`   | `border-inline-start`  |
|                      | `border-right`  | `border-inline-end`    |
| 절대 위치(Absolute)  | `left`          | `inset-inline-start`   |
|                      | `right`         | `inset-inline-end`     |

방향성 값보다 논리적 값을 선호하세요:

| 값                   | 방향성              | 논리적                |
| -------------------- | ------------------- | --------------------- |
| 플로트(Float)        | `float: left`       | `float: inline-start` |
|                      | `float: right`      | `float: inline-end`   |
| 텍스트 정렬(Align)   | `text-align: left`  | `text-align: start`   |
|                      | `text-align: right` | `text-align: end`     |

### 필요한 경우 대체 값 사용하기

일부 사용자는 최신 버전의 Chromium이 포함되지 않은 이전 Obsidian 설치 프로그램을 사용할 수 있습니다.

- 최신 선택자를 사용하는 선택자는 전체 블록이 깨지는 것을 방지하기 위해 `@supports`로 보호해야 합니다.
- 100% 지원되지 않는 속성이 있는 경우 규칙을 2줄로 나눕니다. 첫 번째 줄은 대체를 제공해야 합니다. 두 번째 줄은 새 값을 적용하려고 시도해야 합니다. 이 줄이 실패하면 이전 스타일이 적용되고 정상적으로 대체됩니다.

```css
.supported,
.unsupported {
  /* 실행되지 않음 */
}

.supported {
  /* 실행됨 */
}

.unsupported {
  /* 실행되지 않음 */
}

@supports selector(:dir(*)) {
  /* :dir()이 지원되는 경우 실행됨 */
}
```

## RTL을 위한 Obsidian CSS 헬퍼 및 규칙

### 언어 방향 선택자

#### 전역 선택자

**Settings → General**에서 RTL 언어를 선택하면 `body` 요소에 `.mod-rtl` 클래스가 추가됩니다. 인터페이스 언어를 변경하려면 사용자가 Obsidian을 다시 시작해야 합니다.

`.mod-rtl`을 사용하여 플러그인 또는 테마의 인터페이스 요소 방향을 설정할 수 있습니다. 예:

```css
.mod-rtl .plugin-class {
  direction: rtl;
}
```

또한 특정 인터페이스 언어는 `html` 요소의 `lang` 속성에도 추가됩니다. 예: 아랍어의 경우 `lang="ar"`.

#### 에디터 선택자

사용자가 **Settings → General**에서 RTL 인터페이스 언어를 선택하거나 **Settings → Editor**에서 RTL을 기본 에디터 방향으로 설정하면 `.markdown-source-view` 요소에 `dir="rtl"` 속성이 추가됩니다.

파일을 편집할 때, 첫 번째 강력한 방향성 문자를 감지하여 `.cm-line` 요소에 `dir` 속성이 `rtl` 또는 `ltr`로 설정됩니다. 강력한 방향성 문자가 없는 경우, 에디터는 이전 강력한 방향성 줄의 방향을 기본값으로 사용합니다.

읽기 모드에서는 각 블록에 `dir="auto"` 속성을 사용하여 줄의 방향이 자동으로 설정됩니다.

### 아이콘은 자동으로 미러링됩니다

Obsidian은 [Lucide](https://lucide.dev/) 아이콘 라이브러리를 사용합니다. 거의 모든 아이콘이 대칭이거나 LTR 편향을 가지고 있기 때문에 Obsidian은 인터페이스가 RTL 모드일 때 아이콘의 방향을 자동으로 반전시킵니다. RTL 모드에서 특정 아이콘이 반전되는 것을 방지하려면 변환을 명시적으로 해제해야 합니다.

예를 들어 `.left-icon`이 RTL 언어에 대해 미러링되지 않도록 하려면:

```css
.mod-rtl svg.svg-icon.left-icon {
	transform: unset;
}
```

### 수평 계산에 방향 변수 사용하기

CSS 변수 `--direction`은 논리적 값을 사용할 수 없는 경우 언어 방향에 따라 요소를 수평으로 이동시키기 위해 `translateX()`와 같은 계산에 사용할 수 있습니다.

| 변수          | LTR 값 | RTL 값 |
| ------------- | ------ | ------ |
| `--direction` | `1`    | `-1`   |

### 요소에 가장 적합한 양방향 처리 선택하기

CSS [unicode-bidi](https://developer.mozilla.org/en-US/docs/Web/CSS/unicode-bidi) 속성은 양방향 콘텐츠가 처리되는 방식을 결정하는 데 사용할 수 있습니다.

`plaintext` 값을 사용하는 것은 특정 경우에 유용할 수 있습니다. Obsidian UI에서는 LTR 또는 RTL일 수 있는 단일 줄의 콘텐츠가 있을 때마다 `plaintext` 값이 사용됩니다. 예를 들어, 파일 이름, 개요 항목, 툴팁, 상태 표시줄 요소 등이 있습니다. 이렇게 하면 콘텐츠의 올바른 방향을 보장하고 필요한 경우 줄임표(...)로 긴 이름을 자를 수 있습니다.
