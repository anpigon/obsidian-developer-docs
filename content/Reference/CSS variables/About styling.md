Obsidian 앱은 [Cascading Style Sheets](https://ko.wikipedia.org/wiki/CSS)(CSS)를 사용하여 사용자 인터페이스의 디자인을 제어합니다. CSS는 웹사이트 및 웹 기반 앱에 사용되는 것과 동일한 마크업 언어이므로 온라인에서 CSS 사용 및 편집 방법을 배우는 데 도움이 되는 많은 리소스를 찾을 수 있습니다.

Obsidian에는 일관되게 아름다운 사용자 인터페이스를 가능하게 하는 수백 개의 [[CSS variables]]가 포함되어 있습니다.

## 플러그인용

자신만의 사용자 정의 요소에 내장된 CSS 변수를 사용하면 플러그인에서 아름답고 커뮤니티 테마와 호환되는 네이티브 느낌의 사용자 인터페이스를 만들 수 있습니다.

**styles.css**:

```css
.todo-list {
  background-color: var(--background-secondary);
}
```

## 테마 및 스니펫용

Obsidian CSS 변수의 기본값을 재정의하여 복잡한 CSS 선택자 없이도 아름다운 테마를 만들 수 있습니다.

**theme.css**:

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

CSS 변수를 사용하여 테마를 만드는 방법에 대해 자세히 알아보려면 [[Build a theme]]을 참조하세요.
