[[Plugin|Plugin]] 클래스는 플러그인의 생명주기(lifecycle)를 정의하고 모든 플러그인에서 사용할 수 있는 작업들을 노출합니다:

```ts
import { Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    // 플러그인에 필요한 리소스를 설정합니다.
  }
  async onunload() {
    // 플러그인에서 설정한 모든 리소스를 해제합니다.
  }
}
```

## 플러그인 생명주기

[[onload|onload()]]는 사용자가 Obsidian에서 플러그인을 사용하기 시작할 때마다 실행됩니다. 이곳에서 플러그인의 대부분의 기능을 설정하게 됩니다.

[[onunload|onunload()]]는 플러그인이 비활성화될 때 실행됩니다. 플러그인이 비활성화된 후 Obsidian의 성능에 영향을 주지 않으려면 플러그인에서 사용하는 모든 리소스를 여기서 해제해야 합니다.

이 메소드들이 언제 호출되는지 더 잘 이해하기 위해, 플러그인이 로드되거나 언로드될 때마다 콘솔에 메시지를 출력할 수 있습니다. 콘솔은 개발자가 코드 상태를 모니터링할 수 있게 해주는 유용한 도구입니다.

콘솔을 보려면:

1.  Windows 및 Linux에서는 `Ctrl+Shift+I`를, macOS에서는 `Cmd-Option-I`를 눌러 개발자 도구를 토글합니다.
2.  개발자 도구 창에서 콘솔 탭을 클릭합니다.

```ts
import { Plugin } from 'obsidian';

export default class ExamplePlugin extends Plugin {
  async onload() {
    console.log('loading plugin')
  }
  async onunload() {
    console.log('unloading plugin')
  }
}
