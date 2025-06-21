사용자가 플러그인의 일부를 직접 구성할 수 있도록 하려면 _설정(settings)_ 으로 노출할 수 있습니다.

이 가이드에서는 다음과 같은 설정 페이지를 만드는 방법을 배웁니다 👇

![[../../Assets/settings.png]]

플러그인에 설정을 추가하는 주된 이유는 사용자가 Obsidian을 종료한 후에도 유지되는 구성을 저장하기 위함입니다. 다음 예제는 디스크에서 설정을 저장하고 로드하는 방법을 보여줍니다:

```ts
import { Plugin } from 'obsidian';
import { ExampleSettingTab } from './settings';

interface ExamplePluginSettings {
  dateFormat: string;
}

const DEFAULT_SETTINGS: Partial<ExamplePluginSettings> = {
  dateFormat: 'YYYY-MM-DD',
};

export default class ExamplePlugin extends Plugin {
  settings: ExamplePluginSettings;

  async onload() {
    await this.loadSettings();

    this.addSettingTab(new ExampleSettingTab(this.app, this));
  }

  async loadSettings() {
    this.settings = Object.assign({}, DEFAULT_SETTINGS, await this.loadData());
  }

  async saveSettings() {
    await this.saveData(this.settings);
  }
}
```

> [!warning] 설정의 중첩된 속성
> `Object.assign()`은 중첩된 속성에 대한 참조를 복사합니다(얕은 복사). 설정 객체에 중첩된 속성이 포함된 경우, 각 중첩된 속성을 재귀적으로 복사해야 합니다(깊은 복사). 그렇지 않으면 중첩된 속성에 대한 변경 사항이 `Object.assign()`을 사용하여 복사된 모든 객체에 적용됩니다.

여기에는 많은 내용이 있으니 🤯, 각 부분을 자세히 살펴보겠습니다.

## 설정 정의 생성하기

먼저, 사용자가 구성할 수 있도록 하려는 설정에 대한 정의 `ExamplePluginSettings`를 만들어야 합니다. 플러그인이 활성화되어 있는 동안 `settings` 멤버 변수에서 설정에 접근할 수 있습니다.

```ts
interface ExamplePluginSettings {
  dateFormat: string;
}

export default class ExamplePlugin extends Plugin {
  settings: ExamplePluginSettings;

  // ...
}
```

## 설정 객체 저장 및 로드하기

[[loadData|loadData()]] 및 [[saveData|saveData()]]는 디스크에서 데이터를 저장하고 검색하는 쉬운 방법을 제공합니다. 이 예제는 또한 플러그인의 다른 부분에서 `loadData()` 및 `saveData()`를 더 쉽게 사용할 수 있도록 하는 두 가지 헬퍼 메소드를 소개합니다.

```ts
export default class ExamplePlugin extends Plugin {

  // ...

  async loadSettings() {
    this.settings = Object.assign({}, DEFAULT_SETTINGS, await this.loadData());
  }

  async saveSettings() {
    await this.saveData(this.settings);
  }
}
```

마지막으로, 플러그인이 로드될 때 설정을 로드해야 합니다:

```ts
async onload() {
  await this.loadSettings();

  // ...
}
```

## 기본값 제공하기

사용자가 플러그인을 처음 활성화하면 설정이 아직 구성되지 않았습니다. 앞의 예제는 누락된 설정에 대한 기본값을 제공합니다.

이것이 어떻게 작동하는지 이해하기 위해 다음 코드를 살펴보겠습니다:

```ts
Object.assign({}, DEFAULT_SETTINGS, await this.loadData())
```

`Object.assign()`은 한 객체에서 다른 객체로 모든 속성을 복사하는 JavaScript 함수입니다. `loadData()`에서 반환된 모든 속성은 `DEFAULT_SETTINGS`의 속성을 덮어씁니다.

```ts
const DEFAULT_SETTINGS: Partial<ExamplePluginSettings> = {
  dateFormat: 'YYYY-MM-DD',
};
```

> [!tip]
> `Partial<Type>`은 `Type`의 모든 속성을 선택적으로 설정한 타입을 반환하는 TypeScript 유틸리티입니다. 기본값을 제공하려는 속성만 정의하면서 타입 검사를 활성화할 수 있습니다.

## 설정 탭 등록하기

이제 플러그인은 플러그인 구성을 저장하고 로드할 수 있지만, 사용자는 아직 설정을 변경할 방법이 없습니다. 설정 탭을 추가하면 사용자가 플러그인 설정을 업데이트할 수 있는 사용하기 쉬운 인터페이스를 제공할 수 있습니다:

```ts
this.addSettingTab(new ExampleSettingTab(this.app, this));
```

여기서 `ExampleSettingTab`은 [[PluginSettingTab|PluginSettingTab]]을 확장하는 클래스입니다:

```ts
import ExamplePlugin from './main';
import { App, PluginSettingTab, Setting } from 'obsidian';

export class ExampleSettingTab extends PluginSettingTab {
  plugin: ExamplePlugin;

  constructor(app: App, plugin: ExamplePlugin) {
    super(app, plugin);
    this.plugin = plugin;
  }

  display(): void {
    let { containerEl } = this;

    containerEl.empty();

    new Setting(containerEl)
      .setName('Date format')
      .setDesc('Default date format')
      .addText((text) =>
        text
          .setPlaceholder('MMMM dd, yyyy')
          .setValue(this.plugin.settings.dateFormat)
          .onChange(async (value) => {
            this.plugin.settings.dateFormat = value;
            await this.plugin.saveSettings();
          })
      );
  }
}
```

`display()`는 설정 탭의 내용을 빌드하는 곳입니다. 자세한 내용은 [[HTML elements]]를 참조하세요.

`new Setting(containerEl)`은 컨테이너 요소에 설정을 추가합니다. 이 예제는 `addText()`를 사용하여 텍스트 필드를 사용하지만, 다른 여러 설정 유형을 사용할 수 있습니다.

텍스트 필드의 값이 변경될 때마다 설정 객체를 업데이트한 다음 디스크에 저장합니다:

```ts
.onChange(async (value) => {
  this.plugin.settings.dateFormat = value;
  await this.plugin.saveSettings();
})
```

잘했습니다! 💪 사용자들은 플러그인과 상호 작용하는 방식을 사용자 정의할 수 있는 방법을 제공해 준 것에 대해 감사할 것입니다. 다음 가이드로 넘어가기 전에, 배운 내용을 바탕으로 다른 설정을 추가하여 실험해보세요.
