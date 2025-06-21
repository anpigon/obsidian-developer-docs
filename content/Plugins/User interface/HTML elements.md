Obsidian API의 여러 구성 요소, 예를 들어 [[Settings]]는 _컨테이너 요소(container elements)_ 를 노출합니다:

```ts
import { App, PluginSettingTab } from 'obsidian';

class ExampleSettingTab extends PluginSettingTab {
  plugin: ExamplePlugin;

  constructor(app: App, plugin: ExamplePlugin) {
    super(app, plugin);
    this.plugin = plugin;
  }

  display(): void {
    // highlight-next-line
    let { containerEl } = this;

    // ...
  }
}
```

컨테이너 요소는 Obsidian 내에서 사용자 정의 인터페이스를 만들 수 있게 해주는 `HTMLElement` 객체입니다.

## `createEl()`을 사용하여 HTML 요소 생성하기

컨테이너 요소를 포함한 모든 `HTMLElement`는 원본 요소 아래에 `HTMLElement`를 생성하는 `createEl()` 메소드를 노출합니다.

예를 들어, 컨테이너 요소 내부에 `<h1>` 제목 요소를 추가하는 방법은 다음과 같습니다:

```ts
containerEl.createEl('h1', { text: 'Heading 1' });
```

`createEl()`은 새 요소에 대한 참조를 반환합니다:

```ts
const book = containerEl.createEl('div');
book.createEl('div', { text: 'How to Take Smart Notes' });
book.createEl('small', { text: 'Sönke Ahrens' });
```

## 요소 스타일링하기

플러그인 루트 디렉토리에 `styles.css` 파일을 추가하여 플러그인에 사용자 정의 CSS 스타일을 추가할 수 있습니다. 이전 책 예제에 대한 스타일을 추가하려면:

```css title="styles.css"
.book {
  border: 1px solid var(--background-modifier-border);
  padding: 10px;
}

.book__title {
  font-weight: 600;
}

.book__author {
  color: var(--text-muted);
}
```

> [!tip]
> `--background-modifier-border`와 `--text-muted`는 Obsidian 자체에서 정의하고 사용하는 [CSS 변수](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)입니다. 스타일에 이 변수들을 사용하면 사용자가 다른 테마를 사용하더라도 플러그인이 멋지게 보일 것입니다! 🌈

HTML 요소가 스타일을 사용하도록 하려면 HTML 요소의 `cls` 속성을 설정하세요:

```ts
const book = containerEl.createEl('div', { cls: 'book' });
book.createEl('div', { text: 'How to Take Smart Notes', cls: 'book__title' });
book.createEl('small', { text: 'Sönke Ahrens', cls: 'book__author' });
```

이제 훨씬 좋아 보입니다! 🎉

![[../../Assets/styles.png]]

### 조건부 스타일

사용자 설정이나 다른 값에 따라 요소의 스타일을 변경하려면 `toggleClass` 메소드를 사용하세요:

```ts
element.toggleClass('danger', status === 'error');
