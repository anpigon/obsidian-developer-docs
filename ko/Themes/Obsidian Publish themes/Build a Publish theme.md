[Obsidian Publish](https://help.obsidian.md/Obsidian+Publish/Introduction+to+Obsidian+Publish) 사이트용 테마를 만들 수 있습니다. Obsidian Publish용 테마는 Obsidian 앱과 동일한 [[CSS variables]] 및 [[CSS variables#Obsidian Publish|Publish 관련 CSS 변수]]를 사용합니다.

> [!tip] `body`, `:root`, `.theme-dark`, `.theme-light` 선택자에 대한 자세한 내용은 [[Build a theme]]를 참조하세요.

사이트용 테마를 만들려면:

1. 볼트의 루트 폴더에 `publish.css`라는 파일을 추가합니다.
	- Obsidian은 CSS 파일 편집을 지원하지 않으므로 외부 편집기를 사용하여 이 파일을 만들어야 합니다.
2. `publish.css`를 게시하여 라이브 Publish 사이트에서 테마를 활성화합니다.

**예시:**

```css
.published-container {
  --page-width: 800px;
  --page-side-padding: 48px;
  
  /* ... 라이트 또는 다크 모드가 활성화되어도 변경되지 않는 Publish용 CSS 변수. 때로는 .theme-light 또는 .theme-dark의 색상 변수에 연결됩니다. */
}

.theme-light {
  --background-primary: #ebf2ff;
  --h1-color: #000000;
 
  /* ... 라이트 모드가 활성화되었을 때의 CSS 색상 변수 */
}
.theme-dark {
  --background-primary: #1f2a3f;
  --h1-color: #ffffff;
  
  /* ... 다크 모드가 활성화되었을 때의 CSS 색상 변수 */
}
```

사이트 사용자 정의에 대한 자세한 내용은 [사이트 사용자 정의](https://help.obsidian.md/Obsidian+Publish/Customize+your+site)를 참조하세요.
