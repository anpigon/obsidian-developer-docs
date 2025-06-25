이 페이지는 플러그인 작성자가 플러그인을 제출할 때 흔히 받는 리뷰 의견을 나열합니다.

이 페이지의 가이드라인은 권장 사항이지만, 심각성에 따라 위반 사항을 해결하도록 요구할 수 있습니다.

> [!important] 플러그인 개발자를 위한 정책
> [[Developer policies]]와 [[Submission requirements for plugins]]를 반드시 읽어보세요.

## 일반

### 전역 앱 인스턴스 사용 피하기

전역 앱 객체인 `app` (또는 `window.app`) 사용을 피하세요. 대신 플러그인 인스턴스에서 제공하는 참조인 `this.app`을 사용하세요.

전역 앱 객체는 디버깅 목적으로 만들어졌으며 향후 제거될 수 있습니다.

### 불필요한 콘솔 로깅 피하기

불필요한 로깅을 피해주세요.
기본 설정에서는 개발자 콘솔에 오류 메시지만 표시되어야 하며, 디버그 메시지는 표시되지 않아야 합니다.

### 폴더를 사용하여 코드 베이스 정리 고려하기

플러그인이 하나 이상의 `.ts` 파일을 사용하는 경우, 리뷰와 유지보수를 용이하게 하기 위해 폴더로 정리하는 것을 고려해보세요.

### 플레이스홀더 클래스 이름 바꾸기

샘플 플러그인에는 `MyPlugin`, `MyPluginSettings`, `SampleSettingTab`과 같은 일반적인 클래스에 대한 플레이스홀더 이름이 포함되어 있습니다. 플러그인 이름을 반영하도록 이 이름들을 변경하세요.

## 모바일
![[Mobile development#Node and Electron APIs]]

![[Mobile development#Lookbehind in regular expressions]]
## UI 텍스트

이 섹션은 설정, 명령어, 버튼 등 사용자 인터페이스의 텍스트 서식 지정에 대한 가이드라인을 나열합니다.

아래 **Settings → Appearance**의 예시는 사용자 인터페이스 텍스트에 대한 가이드라인을 보여줍니다.

![[settings-headings.png]]

1. [[#Only use headings under settings if you have more than one section.|일반 설정은 상단에 있으며 제목이 없습니다]].
2. [[#Avoid "settings" in settings headings|섹션 제목에는 "settings"라는 단어가 포함되지 않습니다]].
3. [[#Use Sentence case in UI]].

Obsidian용 텍스트 작성 및 서식 지정에 대한 자세한 내용은 [스타일 가이드](https://help.obsidian.md/Contributing+to+Obsidian/Style+guide)를 참조하세요.

### 설정 아래에 섹션이 두 개 이상인 경우에만 제목 사용하기

설정 탭에 "General", "Settings" 또는 플러그인 이름과 같은 최상위 제목을 추가하지 마세요.

설정 아래에 섹션이 두 개 이상 있고 그중 하나에 일반 설정이 포함된 경우, 제목을 추가하지 않고 상단에 유지하세요.

예를 들어, **Settings → Appearance** 아래의 설정을 보세요.

### 설정 제목에 "settings" 피하기

설정 탭에서 설정을 정리하기 위해 제목을 추가할 수 있습니다. 이 제목에 "settings"라는 단어를 포함하지 마세요. 설정 탭 아래의 모든 것이 설정이므로 모든 제목에 반복하는 것은 중복입니다.

- "Advanced settings" 대신 "Advanced"를 선호합니다.
- "Settings for templates" 대신 "Templates"를 선호합니다.

### UI에서 문장 케이스 사용하기

UI 요소의 모든 텍스트는 [Title Case](https://en.wikipedia.org/wiki/Title_case) 대신 [Sentence case](https://en.wiktionary.org/wiki/sentence_case)를 사용해야 합니다. 문장 케이스에서는 문장의 첫 단어와 고유 명사만 대문자로 표기합니다.

- "Template Folder Location" 대신 "Template folder location"을 선호합니다.
- "Create New Note" 대신 "Create new note"를 선호합니다.

### `<h1>`, `<h2>` 대신 `setHeading` 사용하기

HTML의 제목 요소를 사용하면 다른 플러그인 간에 스타일이 일관되지 않을 수 있습니다.
대신 다음을 사용하는 것을 선호해야 합니다:
```ts
new Setting(containerEl).setName('your heading title').setHeading();
```
## 보안

### `innerHTML`, `outerHTML`, `insertAdjacentHTML` 피하기

사용자 정의 입력을 사용하여 `innerHTML`, `outerHTML`, `insertAdjacentHTML`로 DOM 요소를 만드는 것은 보안 위험을 초래할 수 있습니다.

다음 예제는 사용자 입력 `${name}`을 포함하는 문자열을 사용하여 DOM 요소를 만듭니다. `name`은 `<script>alert()</script>`와 같은 다른 DOM 요소를 포함할 수 있으며, 잠재적인 공격자가 사용자의 컴퓨터에서 임의의 코드를 실행하도록 허용할 수 있습니다.

```ts
function showName(name: string) {
  let containerElement = document.querySelector('.my-container');
  // 이렇게 하지 마세요
  containerElement.innerHTML = `<div class="my-class"><b>Your name is: </b>${name}</div>`;
}
```

대신, `createEl()`, `createDiv()`, `createSpan()`과 같은 DOM API 또는 Obsidian 헬퍼 함수를 사용하여 프로그래밍 방식으로 DOM 요소를 만드세요. 자세한 내용은 [[HTML elements]]를 참조하세요.

HTML 요소의 내용을 정리하려면 `el.empty();`를 사용하세요.

## 리소스 관리

### 플러그인이 언로드될 때 리소스 정리하기

이벤트 리스너와 같이 플러그인에 의해 생성된 모든 리소스는 플러그인이 언로드될 때 파괴되거나 해제되어야 합니다.

가능하면 [[registerEvent|registerEvent()]] 또는 [[addCommand|addCommand()]]와 같은 메소드를 사용하여 플러그인이 언로드될 때 리소스를 자동으로 정리하세요.

```ts
export default class MyPlugin extends Plugin {
  onload() {
    this.registerEvent(this.app.vault.on('create', this.onCreate));
  }

  onCreate: (file: TAbstractFile) => {
    // ...
  }
}
```

> [!note]
> 플러그인이 언로드될 때 제거가 보장되는 리소스는 정리할 필요가 없습니다. 예를 들어, DOM 요소에 `mouseenter` 리스너를 등록하면 해당 요소가 범위를 벗어날 때 이벤트 리스너가 가비지 컬렉션됩니다.

### `onunload`에서 리프(leaf) 분리하지 않기

사용자가 플러그인을 업데이트할 때, 열려 있는 모든 리프는 사용자가 어디로 옮겼는지에 관계없이 원래 위치에서 다시 초기화됩니다.

## 명령어

### 명령어에 기본 단축키 설정 피하기

기본 단축키를 설정하면 플러그인 간에 충돌이 발생할 수 있으며 사용자가 이미 구성한 단축키를 덮어쓸 수 있습니다.

또한 모든 운영 체제에서 사용 가능한 기본 단축키를 선택하기 어렵습니다.

### 명령어에 적절한 콜백 유형 사용하기

플러그인에 명령어를 추가할 때 적절한 콜백 유형을 사용하세요.

- 명령어가 무조건 실행되는 경우 `callback`을 사용합니다.
- 명령어가 특정 조건에서만 실행되는 경우 `checkCallback`을 사용합니다.

명령어에 열려 있고 활성화된 마크다운 에디터가 필요한 경우, `editorCallback` 또는 해당 `editorCheckCallback`을 사용하세요.

## 작업 공간

### `workspace.activeLeaf`에 직접 접근 피하기

활성 뷰에 접근하려면 대신 [[getActiveViewOfType|getActiveViewOfType()]]를 사용하세요:

```ts
const view = this.app.workspace.getActiveViewOfType(MarkdownView);

// getActiveViewOfType은 활성 뷰가 null이거나 MarkdownView가 아닌 경우 null을 반환합니다.
if (view) {
  // ...
}
```

활성 노트의 에디터에 접근하려면 대신 `activeEditor`를 사용하세요:

```ts
const editor = this.app.workspace.activeEditor?.editor;

if (editor) {
    // ...
}
```

### 사용자 정의 뷰에 대한 참조 관리 피하기

사용자 정의 뷰에 대한 참조를 관리하면 메모리 누수나 의도하지 않은 결과를 초래할 수 있습니다.

이렇게 **하지 마세요**:

```ts
this.registerView(MY_VIEW_TYPE, () => this.view = new MyCustomView());
```

대신 이렇게 하세요:

```ts
this.registerView(MY_VIEW_TYPE, () => new MyCustomView());
```

플러그인에서 뷰에 접근하려면 `Workspace.getActiveLeavesOfType()`를 사용하세요:

```ts
for (let leaf of app.workspace.getActiveLeavesOfType(MY_VIEW_TYPE)) {
  let view = leaf.view;
  if (view instanceof MyCustomView) {
    // ...
  }
}
```

## 저장소(Vault)

### 활성 파일에 대해 `Vault.modify` 대신 Editor API 선호하기

활성 노트를 편집하려면 [[Vault/modify|Vault.modify()]] 대신 [[Editor]] 인터페이스를 사용하세요.

Editor는 커서 위치, 선택 영역, 접힌 내용과 같은 활성 노트에 대한 정보를 유지합니다. [[Vault/modify|Vault.modify()]]를 사용하여 노트를 편집하면 모든 정보가 손실되어 사용자 경험이 저하됩니다.

Editor는 노트의 일부를 작게 변경할 때 더 효율적입니다.

### 백그라운드에서 파일을 수정하려면 `Vault.modify` 대신 `Vault.process` 선호하기

현재 열려 있지 않은 노트를 편집하려면 [[modify|Vault.modify]] 대신 [[Reference/TypeScript API/Vault/process|Vault.process]] 함수를 사용하세요.

`process` 함수는 파일을 원자적으로 수정하므로, 플러그인이 동일한 파일을 수정하는 다른 플러그인과 충돌하지 않습니다.

### 노트의 프론트매터를 수정하려면 `FileManager.processFrontMatter` 선호하기

노트의 프론트매터를 추출하고 YAML을 수동으로 파싱하고 수정하는 대신 [[processFrontMatter|FileManager.processFrontMatter]] 함수를 사용해야 합니다.

`processFrontMatter`는 원자적으로 실행되므로 파일을 수정해도 동일한 파일을 편집하는 다른 플러그인과 충돌하지 않습니다.
또한 생성된 YAML의 일관된 레이아웃을 보장합니다.

### Adapter API 대신 Vault API 선호하기

Obsidian은 파일 작업을 위한 두 가지 API를 노출합니다: Vault API (`app.vault`)와 Adapter API (`app.vault.adapter`).

Adapter API의 파일 작업이 많은 개발자에게 더 익숙하지만, Vault API는 어댑터에 비해 두 가지 주요 이점이 있습니다.

- **성능:** Vault API에는 파일이 이미 Obsidian에 알려진 경우 파일 읽기 속도를 높일 수 있는 캐싱 계층이 있습니다.
- **안전성:** Vault API는 동시에 쓰기 중인 파일을 읽는 것과 같은 경쟁 조건을 피하기 위해 파일 작업을 직렬로 수행합니다.

### 경로로 파일을 찾기 위해 모든 파일 반복 피하기

이것은 특히 큰 저장소에서 비효율적입니다. 대신 [[getFileByPath|Vault.getFileByPath]], [[getFolderByPath|Vault.getFolderByPath]] 또는 [[getAbstractFileByPath|Vault.getAbstractFileByPath]]를 사용하세요.

이렇게 **하지 마세요**:

```ts
this.app.vault.getFiles().find(file => file.path === filePath);
```

대신 이렇게 하세요:

```ts
const filePath = 'folder/file.md';
// 파일을 가져오려면
const file = this.app.vault.getFileByPath(filePath);
```

```ts
const folderPath = 'folder';
// 또는 폴더를 가져오려면
const folder = this.app.vault.getFolderByPath(folderPath);
```

제공된 경로가 폴더용인지 파일용인지 확실하지 않은 경우 다음을 사용하세요:
```ts
const abstractFile = this.app.vault.getAbstractFileByPath(filePath);

if (file instanceof TFile) {
	// 파일입니다
}
if (file instanceof TFolder) {
	// 폴더입니다
}
```

### 사용자 정의 경로를 정리하기 위해 `normalizePath()` 사용하기

저장소의 파일이나 폴더에 대한 사용자 정의 경로를 받거나 플러그인 코드에서 자체 경로를 구성할 때마다 [[normalizePath|normalizePath()]]를 사용하세요.

`normalizePath()`는 경로를 받아 파일 시스템 및 크로스 플랫폼 사용에 안전하도록 정리합니다. 이 함수는 다음을 수행합니다:

- `\` 또는 `/`를 하나 이상의 `\` 또는 `/`를 단일 `/`로 바꾸는 등 순방향 및 역방향 슬래시 사용을 정리합니다.
- 선행 및 후행 순방향 및 역방향 슬래시를 제거합니다.
- 줄 바꿈 없는 공백 `\u00A0`을 일반 공백으로 바꿉니다.
- 경로를 [String.prototype.normalize](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/normalize)를 통해 실행합니다.

```ts
import { normalizePath } from 'obsidian';
const pathToPlugin = normalizePath('//my-folder\file');
// pathToPlugin은 "//my-folder\"가 아닌 "my-folder/file"을 포함합니다
```

## 에디터

### 에디터 확장 변경 또는 재구성하기

[[registerEditorExtension|registerEditorExtension()]]을 사용하여 [[Editor extensions|에디터 확장]]을 등록한 후 변경하거나 재구성하려면 [[updateOptions|updateOptions()]]를 사용하여 모든 에디터를 업데이트하세요.

```ts
class MyPlugin extends Plugin {
  private editorExtension: Extension[] = [];

  onload() {
    //...

    this.registerEditorExtension(this.editorExtension);
  }

  updateEditorExtension() {
    // 동일한 참조를 유지하면서 배열 비우기
    // (여기서 새 배열을 만들지 마세요)
    this.editorExtension.length = 0;

    // 새 에디터 확장 만들기
    let myNewExtension = this.createEditorExtension();
    // 배열에 추가하기
    this.editorExtension.push(myNewExtension);

    // 모든 에디터에 변경 사항 적용하기
    this.app.workspace.updateOptions();
  }
}

```
## 스타일링

### 하드코딩된 스타일링 금지

이렇게 **하지 마세요**:

```ts
const el = containerEl.createDiv();
el.style.color = 'white';
el.style.backgroundColor = 'red';
```

사용자가 플러그인의 스타일링을 쉽게 수정할 수 있도록 하려면 CSS 클래스를 사용해야 합니다. 플러그인 코드에 스타일링을 하드코딩하면 테마와 스니펫으로 수정하는 것이 불가능해집니다.

대신 이렇게 **하세요**:

```ts
const el = containerEl.createDiv({cls: 'warning-container'});
```

 플러그인 CSS에 다음을 추가하세요:

```css
.warning-container {
	color: var(--text-normal);
	background-color: var(--background-modifier-error);
}
```

플러그인의 스타일링을 Obsidian 및 다른 플러그인과 일관되게 만들려면 Obsidian에서 제공하는 [[CSS variables]]를 사용해야 합니다.
사용 사례에 맞는 변수가 없는 경우 직접 만들 수 있습니다.

## TypeScript

### `var` 대신 `const`와 `let` 선호하기

자세한 내용은 [현대 JavaScript에서 var가 구식으로 간주되는 4가지 이유](https://javascript.plainenglish.io/4-reasons-why-var-is-considered-obsolete-in-modern-javascript-a30296b5f08f)를 참조하세요.

### Promise 대신 async/await 선호하기

최신 버전의 JavaScript와 TypeScript는 비동기 코드를 실행하기 위해 `async` 및 `await` 키워드를 지원하여 Promise를 사용하는 것보다 더 읽기 쉬운 코드를 작성할 수 있습니다.

이렇게 **하지 마세요**:

```ts
function test(): Promise<string | null> {
  return requestUrl('https://example.com')
    .then(res => res.text
    .catch(e => {
      console.log(e);
      return null;
    });
}
```

대신 이렇게 하세요:

```ts
async function AsyncTest(): Promise<string | null> {
  try {
    let res = await requestUrl('https://example.com');
    let text = await r.text;
    return text;
  }
  catch (e) {
    console.log(e);
    return null;
  }
}
