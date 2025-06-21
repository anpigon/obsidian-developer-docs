모달은 정보를 표시하고 사용자로부터 입력을 받습니다. 모달을 생성하려면 [[Reference/TypeScript API/Modal|Modal]]을 확장하는 클래스를 만듭니다:

```ts
import { App, Modal } from 'obsidian';

export class ExampleModal extends Modal {
  constructor(app: App) {
    super(app);
	this.setContent('Look at me, I\'m a modal! 👀')
  }
}
```

모달을 열려면 `ExampleModal`의 새 인스턴스를 만들고 [[Reference/TypeScript API/Modal/open|open()]]을 호출합니다:

```ts
import { Plugin } from 'obsidian';
import { ExampleModal } from './modal';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.addCommand({
      id: 'display-modal',
      name: 'Display modal',
      callback: () => {
        new ExampleModal(this.app).open();
      },
    });
  }
}
```

## 사용자 입력 받기

이전 예제의 모달은 텍스트만 표시했습니다. 사용자 입력을 처리하는 좀 더 복잡한 예제를 살펴보겠습니다.

![[../../Assets/modal-input.png]]

```ts
import { App, Modal, Setting } from 'obsidian';

export class ExampleModal extends Modal {
  constructor(app: App, onSubmit: (result: string) => void) {
    super(app);
	this.setTitle('What\'s your name?');

	let name = '';
    new Setting(this.contentEl)
      .setName('Name')
      .addText((text) =>
        text.onChange((value) => {
          name = value;
        }));

    new Setting(this.contentEl)
      .addButton((btn) =>
        btn
          .setButtonText('Submit')
          .setCta()
          .onClick(() => {
            this.close();
            onSubmit(name);
          }));
  }
}
```

사용자가 **Submit**을 클릭하면 `onSubmit` 콜백에서 결과가 반환됩니다:

```ts
new ExampleModal(this.app, (result) => {
  new Notice(`Hello, ${result}!`);
}).open();
```

## 제안 목록에서 선택하기

[[SuggestModal|SuggestModal]]은 사용자에게 제안 목록을 표시할 수 있는 특수 모달입니다.

![[../../Assets/suggest-modal.gif]]

```ts
import { App, Notice, SuggestModal } from 'obsidian';

interface Book {
  title: string;
  author: string;
}

const ALL_BOOKS = [
  {
    title: 'How to Take Smart Notes',
    author: 'Sönke Ahrens',
  },
  {
    title: 'Thinking, Fast and Slow',
    author: 'Daniel Kahneman',
  },
  {
    title: 'Deep Work',
    author: 'Cal Newport',
  },
];

export class ExampleModal extends SuggestModal<Book> {
  // 사용 가능한 모든 제안을 반환합니다.
  getSuggestions(query: string): Book[] {
    return ALL_BOOKS.filter((book) =>
      book.title.toLowerCase().includes(query.toLowerCase())
    );
  }

  // 각 제안 항목을 렌더링합니다.
  renderSuggestion(book: Book, el: HTMLElement) {
    el.createEl('div', { text: book.title });
    el.createEl('small', { text: book.author });
  }

  // 선택된 제안에 대한 작업을 수행합니다.
  onChooseSuggestion(book: Book, evt: MouseEvent | KeyboardEvent) {
    new Notice(`Selected ${book.title}`);
  }
}
```

Obsidian API는 `SuggestModal` 외에도 제안을 위한 훨씬 더 전문화된 유형의 모달인 [[FuzzySuggestModal|FuzzySuggestModal]]을 제공합니다. 각 항목이 렌더링되는 방식에 대한 동일한 제어권을 제공하지는 않지만, 기본적으로 [유사 문자열 검색(fuzzy string search)](https://en.wikipedia.org/wiki/Approximate_string_matching)을 사용할 수 있습니다.

![[../../Assets/fuzzy-suggestion-modal.png]]

```ts
export class ExampleModal extends FuzzySuggestModal<Book> {
  getItems(): Book[] {
    return ALL_BOOKS;
  }

  getItemText(book: Book): string {
    return book.title;
  }

  onChooseItem(book: Book, evt: MouseEvent | KeyboardEvent) {
    new Notice(`Selected ${book.title}`);
  }
}
