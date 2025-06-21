명령어는 사용자가 [명령어 팔레트(Command Palette)](https://help.obsidian.md/Plugins/Command+palette)에서 또는 단축키를 사용하여 수행할 수 있는 작업입니다.

![[../../Assets/command.png]]

플러그인에 새 명령어를 등록하려면 `onload()` 메소드 내에서 [[addCommand|addCommand()]] 메소드를 호출합니다:

```ts
import { Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    this.addCommand({
      id: 'print-greeting-to-console',
      name: 'Print greeting to console',
      callback: () => {
        console.log('Hey, you!');
      },
    });
  }
}
```

## 조건부 명령어

명령어가 특정 조건에서만 실행될 수 있는 경우, 대신 [[checkCallback|checkCallback()]] 사용을 고려하세요.

`checkCallback`은 두 번 실행됩니다. 첫째, 명령어가 실행될 수 있는지 예비 확인을 수행합니다. 둘째, 작업을 수행합니다.

두 실행 사이에 시간이 경과할 수 있으므로, 두 호출 모두에서 확인을 수행해야 합니다.

콜백이 예비 확인을 수행해야 하는지 또는 작업을 수행해야 하는지 결정하기 위해 `checking` 인수가 콜백에 전달됩니다.

- `checking`이 `true`로 설정된 경우, 예비 확인을 수행합니다.
- `checking`이 `false`로 설정된 경우, 작업을 수행합니다.

다음 예제의 명령어는 필수 값에 따라 달라집니다. 두 실행 모두에서 콜백은 값이 있는지 확인하지만 `checking`이 `false`인 경우에만 작업을 수행합니다.

```ts
this.addCommand({
  id: 'example-command',
  name: 'Example command',
  // highlight-next-line
  checkCallback: (checking: boolean) => {
    const value = getRequiredValue();

    if (value) {
      if (!checking) {
        doCommand(value);
      }

      return true
    }

    return false;
  },
});
```

## 에디터 명령어

명령어가 에디터에 접근해야 하는 경우, 활성 에디터와 해당 뷰를 인수로 제공하는 [[editorCallback|editorCallback()]]을 사용할 수도 있습니다.

```ts
this.addCommand({
  id: 'example-command',
  name: 'Example command',
  editorCallback: (editor: Editor, view: MarkdownView) => {
    const sel = editor.getSelection()

    console.log(`You have selected: ${sel}`);
  },
}
```

> [!note]
> 에디터 명령어는 활성 에디터를 사용할 수 있을 때만 명령어 팔레트에 나타납니다.

에디터 콜백이 특정 조건에서만 실행될 수 있는 경우, 대신 [[editorCheckCallback|editorCheckCallback()]] 사용을 고려하세요. 자세한 내용은 [[#Conditional commands]]를 참조하세요.

```ts
this.addCommand({
  id: 'example-command',
  name: 'Example command',
  editorCheckCallback: (checking: boolean, editor: Editor, view: MarkdownView) => {
    const value = getRequiredValue();

    if (value) {
      if (!checking) {
        doCommand(value);
      }

      return true
    }

    return false;
  },
});
```

## 단축키(Hot keys)

사용자는 키보드 단축키 또는 _단축키(hot key)_ 를 사용하여 명령어를 실행할 수 있습니다. 사용자가 직접 구성할 수도 있지만, 기본 단축키를 제공할 수도 있습니다.

> [!warning]
> 다른 사람이 사용하도록 의도된 플러그인에 기본 단축키를 설정하지 마세요. 단축키는 다른 플러그인이나 사용자 자신이 정의한 단축키와 충돌할 가능성이 높습니다.

이 예제에서 사용자는 Ctrl(Mac에서는 Cmd)과 Shift를 함께 누른 다음 키보드에서 `a` 키를 눌러 명령어를 실행할 수 있습니다.

```ts
this.addCommand({
  id: 'example-command',
  name: 'Example command',
  hotkeys: [{ modifiers: ['Mod', 'Shift'], key: 'a' }],
  callback: () => {
    console.log('Hey, you!');
  },
});
```

> [!note]
> Mod 키는 Windows 및 Linux에서는 Ctrl이 되고 macOS에서는 Cmd가 되는 특수 수정자 키입니다.
