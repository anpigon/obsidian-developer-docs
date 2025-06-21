Obsidian에서 노트들의 모음을 볼트(Vault)라고 합니다. 볼트는 하나의 폴더와 그 하위 폴더들로 구성됩니다.

플러그인은 다른 Node.js 애플리케이션처럼 파일 시스템에 접근할 수 있지만, [[Reference/TypeScript API/Vault|Vault]] 모듈은 볼트 내의 파일과 폴더를 더 쉽게 다룰 수 있도록 도와줍니다.

다음 예제는 볼트 내의 모든 마크다운 파일 경로를 재귀적으로 출력합니다:

```ts
const files = this.app.vault.getMarkdownFiles()

for (let i = 0; i < files.length; i++) {
  console.log(files[i].path);
}
```

> [!tip]
> 마크다운 문서뿐만 아니라 _모든_ 파일을 나열하려면 [[getFiles|getFiles()]]를 사용하세요.

## 파일 읽기

파일 내용을 읽는 두 가지 메서드가 있습니다: [[Reference/TypeScript API/Vault/read|read()]]와 [[cachedRead|cachedRead()]].

- 내용을 사용자에게 표시만 하려면 `cachedRead()`를 사용하여 디스크에서 여러 번 읽는 것을 피하세요.
- 내용을 읽고 변경한 후 디스크에 다시 쓰려면 `read()`를 사용하여 오래된 복사본으로 파일을 덮어쓰는 것을 피하세요.

> [!info]
> `cachedRead()`와 `read()`의 유일한 차이점은 플러그인이 파일을 읽기 직전에 Obsidian 외부에서 파일이 수정되었을 때입니다. 파일 시스템이 Obsidian에 외부에서 파일이 변경되었음을 알리자마자 `cachedRead()`는 `read()`와 _똑같이_ 동작합니다. 마찬가지로 Obsidian 내에서 파일을 저장하면 읽기 캐시도 플러시됩니다.

다음 예제는 볼트 내의 모든 마크다운 파일 내용을 읽고 평균 문서 크기를 반환합니다:

```ts
import { Notice, Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.addRibbonIcon('info', '파일 평균 길이 계산', async () => {
      const fileLength = await this.averageFileLength();
      new Notice(`파일 평균 길이는 ${fileLength}자 입니다.`);
    });
  }

  async averageFileLength(): Promise<number> {
    const { vault } = this.app;

    const fileContents: string[] = await Promise.all(
      vault.getMarkdownFiles().map((file) => vault.cachedRead(file))
    );

    let totalLength = 0;
    fileContents.forEach((content) => {
      totalLength += content.length;
    });

    return totalLength / fileContents.length;
  }
}
```

## 파일 수정

기존 파일에 텍스트 내용을 쓰려면 [[modify|Vault.modify()]]를 사용하세요.

```ts
function writeCurrentDate(vault: Vault, file: TFile): Promise<void> {
  return vault.modify(file, `오늘은 ${new Intl.DateTimeFormat().format(new Date())}입니다.`);
}
```

파일의 현재 내용을 기반으로 수정하려면 [[process|Vault.process()]]를 사용하세요. 두 번째 인자는 현재 파일 내용을 제공하고 수정된 내용을 반환하는 콜백입니다.

```ts
// emojify는 모든 :)를 🙂로 바꿉니다.
function emojify(vault: Vault, file: TFile): Promise<string> {
  return vault.process(file, (data) => {
    return data.replace(':)', '🙂');
  })
}
```

`Vault.process()`는 [[Reference/TypeScript API/Vault/read|Vault.read()]]와 [[modify|Vault.modify()]] 위에 구축된 추상화로, 현재 내용을 읽고 업데이트된 내용을 쓰는 사이에 파일이 변경되지 않음을 보장합니다. 의도하지 않은 데이터 손실을 피하기 위해 항상 `Vault.process()`를 `Vault.read()`/`Vault.modify()`보다 선호하세요.

### 비동기 수정

[[process|Vault.process()]]는 동기식 수정만 지원합니다. 파일을 비동기적으로 수정해야 하는 경우:

1. [[cachedRead|Vault.cachedRead()]]로 파일을 읽으세요.
2. 비동기 작업을 수행하세요.
3. [[Reference/TypeScript API/Vault/process|Vault.process()]]로 파일을 업데이트하세요.

`process()` 콜백의 `data`가 `cachedRead()`가 반환한 데이터와 같은지 확인하는 것을 잊지 마세요. 같지 않다면 다른 프로세스에 의해 파일이 변경된 것이므로 사용자에게 확인을 요청하거나 다시 시도할 수 있습니다.

## 파일 삭제

파일을 삭제하는 두 가지 메서드가 있습니다: [[delete|delete()]]와 [[trash|trash()]]. 사용자가 마음을 바꿀 수 있도록 허용할지 여부에 따라 사용할 메서드가 달라집니다.

- `delete()`는 흔적 없이 파일을 제거합니다.
- `trash()`는 파일을 휴지통으로 이동합니다.

`trash()`를 사용할 때는 시스템의 휴지통으로 이동할지, 아니면 사용자 볼트 루트의 `.trash` 폴더로 이동할지 선택할 수 있습니다.

## 파일인가 폴더인가?

일부 작업은 파일이나 폴더일 수 있는 [[TAbstractFile|TAbstractFile]] 객체를 반환하거나 받아들입니다. `TAbstractFile`을 사용하기 전에 항상 구체적인 타입을 확인하세요.

```ts
const folderOrFile = this.app.vault.getAbstractFileByPath('folderOrFile');

if (folderOrFile instanceof TFile) {
  console.log('파일입니다!');
} else if (folderOrFile instanceof TFolder) {
  console.log('폴더입니다!');
}
