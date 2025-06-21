이 튜토리얼에서는 Obsidian용 테마 개발을 시작하는 방법을 배웁니다. 테마를 사용하면 CSS를 사용하여 Obsidian의 모양과 느낌을 사용자 정의할 수 있습니다.

## 학습 내용

이 튜토리얼을 완료하면 다음을 할 수 있게 됩니다:

- Obsidian 테마 개발을 위한 환경 구성
- CSS 변수를 사용하여 Obsidian의 모양 변경
- 라이트 및 다크 색상 구성표를 모두 지원하는 테마 만들기

## 필수 조건

이 튜토리얼을 완료하려면 다음이 필요합니다:

- 로컬 머신에 설치된 [Git](https://git-scm.com/)
- [Visual Studio Code](https://code.visualstudio.com/)와 같은 코드 에디터

## 1단계: 샘플 테마 다운로드

이 단계에서는 Obsidian이 찾을 수 있도록 볼트의 [`.obsidian` 디렉토리](https://help.obsidian.md/Advanced+topics/How+Obsidian+stores+data#Per+vault+data) 내 `themes` 디렉토리에 샘플 테마를 다운로드합니다.

이 튜토리얼에서 사용할 샘플 테마는 [GitHub 저장소](https://github.com/obsidianmd/obsidian-sample-theme)에서 사용할 수 있습니다.

1. 터미널 창을 열고 프로젝트 디렉토리를 `themes` 디렉토리로 변경합니다.

   ```bash
   cd path/to/vault/.obsidian/themes
   ```

2. Git을 사용하여 샘플 테마를 복제합니다.

   ```bash
   git clone https://github.com/obsidianmd/obsidian-sample-theme.git "Sample Theme"
   ```

> [!tip] GitHub 템플릿 저장소
> 샘플 테마의 저장소는 GitHub 템플릿 저장소이므로 샘플 테마에서 자신만의 저장소를 만들 수 있습니다. 방법을 알아보려면 [템플릿에서 저장소 만들기](https://docs.github.com/ko/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template#creating-a-repository-from-a-template)를 참조하세요.
>
> 샘플 테마를 복제할 때는 자신의 저장소 URL을 사용하는 것을 잊지 마세요.

## 2단계: 테마 활성화

1. Obsidian에서 **설정**을 엽니다.
2. 사이드 메뉴에서 **모양**을 선택합니다.
3. **테마** 옆의 드롭다운 목록에서 **Sample Theme**을 선택합니다.

샘플 테마를 활성화했습니다. 다음으로 몇 가지 변경 사항을 적용해 보겠습니다.

## 3단계: 매니페스트 업데이트

이 단계에서는 매니페스트 `manifest.json`을 업데이트하여 테마 이름을 변경합니다. 매니페스트에는 테마의 이름과 설명 같은 정보가 포함되어 있습니다.

1. 코드 에디터에서 `manifest.json`을 엽니다.
2. `name`을 `"Disco Lights"`와 같은 사용자 친화적인 이름으로 변경합니다.
3. `themes` 아래의 테마 디렉토리 이름을 동일한 이름으로 변경합니다. 테마 디렉토리 이름은 `manifest.json`의 `name` 속성과 정확히 일치해야 합니다.

   ```bash
   mv "Sample Theme" "Disco Lights"
   ```

4. 매니페스트의 새 변경 사항을 로드하려면 Obsidian을 다시 시작합니다.

**설정 → 모양 → 테마**로 돌아가서 테마 이름이 변경된 것을 확인하세요.

`manifest.json`을 변경할 때마다 Obsidian을 다시 시작하는 것을 잊지 마세요.

## 4단계: 글꼴 변경

Obsidian은 [CSS 변수](https://developer.mozilla.org/ko/docs/Web/CSS/Using_CSS_custom_properties)를 사용하여 사용자 인터페이스를 스타일링합니다. 이 단계에서는 CSS 변수를 사용하여 에디터의 글꼴을 변경합니다.

1. 예를 들어 "테마 개발"이라는 새 노트를 만듭니다.
2. 노트에 다음 텍스트를 입력합니다:

   ```md
   테마를 사용하면 [Obsidian](https://obsidian.md)을 원하는 대로 꾸밀 수 있습니다.
   ```

3. `theme.css`에 다음을 추가합니다:

   ```css
   body {
     --font-text-theme: Georgia, serif;
   }
   ```

에디터는 정의한 글꼴을 사용하여 노트를 표시합니다.

## 5단계: 배경색 변경

테마는 라이트 및 다크 색상 구성표를 모두 지원할 수 있습니다. `.theme-dark` 또는 `.theme-light` 아래에 CSS 변수를 정의하세요.

1. `theme.css`에 다음을 추가합니다:

   ```css
   .theme-dark {
     --background-primary: #18004F;
     --background-secondary: #220070;
   }

   .theme-light {
     --background-primary: #ECE4FF;
     --background-secondary: #D9C9FF;
   }
   ```

2. Obsidian에서 **설정**을 엽니다.
3. **모양** 아래에서 **기본 색상 구성표**를 "라이트"와 "다크" 사이에서 전환합니다.

Obsidian이 선택한 색상 구성표에 따라 색상을 선택하는 것을 볼 수 있습니다. 더 극적인 변화를 위해 색상을 `red`, `green` 또는 `blue`로 변경해 보세요.

## 6단계: 입력 필드 호버 테두리 색상 변경

`:root` 선택자는 테마 내의 모든 자식 요소에서 변수에 액세스하려는 경우 일반적으로 사용됩니다. 이 선택자는 종종 플러그인 변수로 채워집니다.

사용법을 설명하는 예는 다음과 같습니다:

> [!example]
> 설정 및 노트 내용과 같이 Obsidian 내 다양한 위치에서 찾을 수 있는 입력 필드를 생각해 보겠습니다. 이 입력 필드에 특정한 변수를 정의하려면 `:root` 선택자를 사용할 수 있습니다.
>
> ```css
> :root {
>    --input-focus-border-color: Highlight;
>    --input-focus-outline: 1px solid Canvas;
>    --input-unfocused-border-color: transparent;
>    --input-disabled-border-color: transparent;
>    --input-hover-border-color: black;
>    /* 루트에 대한 기본 입력 변수 */
 > }
 > ```

이제 CSS에서 호버 테두리 색상을 수정해 보겠습니다:

```css
:root {
   --input-hover-border-color: red;
/* 검은색에서 빨간색으로 변경 */
}
```

이 업데이트를 통해 입력 필드 위로 마우스를 가져가면 테두리 색상이 밝은 빨간색으로 변경됩니다.

> [!tip]
> 라이트 및 다크 테마 모두에서 동일하게 유지되어야 하는 스타일을 정의할 때는 `body` 선택자를 사용하는 것이 좋습니다.
>
> 라이트 및 다크 테마 간에 전환할 때 스타일이 변경되기를 원하는 경우에만 `.theme-dark` 또는 `.theme-light` 선택자를 사용하세요.
>
> 또한 `:root`를 신중하고 고려하여 사용하는 것이 중요합니다. 변수를 `body`, `.theme-dark` 또는 `.theme-light` 선택자 내에 배치할 수 있다면 그렇게 하는 것이 좋습니다.

## 7단계: 사용 중인 CSS 변수 찾기

Obsidian은 사용자 인터페이스의 다양한 부분을 사용자 정의하기 위해 400개 이상의 다양한 CSS 변수를 노출합니다.
이러한 변수 중 다수는 [[CSS variables]]에서 찾을 수 있습니다.

또는 앱을 검사하여 특정 요소를 스타일링하는 데 사용되는 변수를 찾을 수 있습니다.
이 단계에서는 리본 배경을 변경하기 위한 CSS 변수를 찾습니다.

1. Obsidian에서 `Ctrl`+`Shift`+`I`(macOS에서는 `Cmd`+`Option`+`I`)를 눌러 **개발자 도구**를 엽니다.
2. **소스** 탭을 엽니다.
3. **페이지 → top → obsidian.md** 아래에서 **app.css**를 선택합니다.
4. `app.css`의 맨 위로 스크롤하여 사용 가능한 모든 CSS 변수를 찾습니다.
5. `Ctrl`+`F`(macOS에서는 `Cmd`+`F`)를 누르고 "  --ribbon-"을 입력하여 리본과 관련된 변수를 검색합니다. 두 개의 공백은 사용처가 아닌 정의를 반환합니다.

결과 중 하나는 `--ribbon-background`이며, 유망해 보입니다. 확실히 하려면 HTML을 검사하여 특정 요소에서 사용하는 CSS 변수를 찾을 수도 있습니다.

1. **개발자 도구**의 왼쪽 상단 모서리에서 사각형 위에 커서가 있는 아이콘을 선택합니다.
2. Obsidian 창 왼쪽의 리본 중앙을 선택합니다.

**개발자 도구** 오른쪽의 **스타일** 탭에서 선택한 요소에 적용된 CSS(예: `background-color: var(--ribbon-background)`)를 볼 수 있습니다.

이제 `--ribbon-background`가 리본 배경색을 제어한다는 것을 알았으므로 `theme.css`에 다음을 추가합니다:

```css
body {
  --ribbon-background: magenta;
}
```

## 결론

이 튜토리얼에서는 첫 번째 Obsidian 테마를 만들었습니다. 테마를 수정하고 다시 로드하여 Obsidian 내에서 변경 사항을 반영했습니다. 또한 사용자 인터페이스의 특정 부분을 스타일링하기 위한 CSS 변수를 찾는 방법도 배웠습니다.
