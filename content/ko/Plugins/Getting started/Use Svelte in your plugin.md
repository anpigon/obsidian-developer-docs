이 가이드에서는 React나 Vue와 같은 전통적인 프레임워크의 경량 대안인 [Svelte](https://svelte.dev/)를 사용하도록 플러그인을 설정하는 방법을 설명합니다.

Svelte는 코드를 전처리하고 최적화된 순수 JavaScript를 출력하는 컴파일러를 중심으로 구축되었습니다. 이는 상태 변경을 추적하기 위해 가상 DOM이 필요 없다는 것을 의미하며, 플러그인이 최소한의 추가 오버헤드로 실행될 수 있게 합니다.

Svelte에 대해 더 배우고 사용법을 알고 싶다면, [튜토리얼](https://svelte.dev/tutorial/svelte/welcome-to-svelte)과 [문서](https://svelte.dev/docs/svelte/overview)를 참조하세요.

이 가이드는 [[Build a plugin]]을 완료했다고 가정합니다.

> [!tip] Visual Studio Code
> Svelte에는 Svelte 컴포넌트에서 구문 강조 및 풍부한 IntelliSense를 활성화하는 [공식 Visual Studio Code 확장 프로그램](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode)이 있습니다.

## 플러그인 설정하기

Svelte로 플러그인을 빌드하려면 종속성을 설치하고 Svelte를 사용하여 작성된 코드를 컴파일하도록 플러그인을 설정해야 합니다.
TypeScript의 *타입 전용* 기능만 사용하려는 경우 `svelte-preprocess`는 필요하지 않습니다.

1.  플러그인 종속성에 Svelte를 추가합니다:

    ```bash
    npm install --save-dev svelte svelte-preprocess esbuild-svelte svelte-check
    ```

    > [!info]
    > Svelte는 최소 TypeScript 5.0이 필요합니다. TypeScript 5.0으로 업데이트하려면 터미널에서 다음을 실행하세요.
    >
    > ```bash
    > npm install typescript@~5.0.0
    > ```

2.  `tsconfig.json`을 확장하여 일반적인 Svelte 문제에 대한 추가 유형 검사를 활성화합니다. `svelte-preprocess`에는 `verbatimModuleSyntax`가 필요하고 `svelte-check`가 올바르게 작동하려면 `skipLibCheck`가 필요합니다.

    ```json
    {
      "compilerOptions": {
        "verbatimModuleSyntax": true,
        "skipLibCheck": true,
        // ...
      },
      "include": [
        "**/*.ts",
        "**/*.svelte"
      ]
    }
    ```

3.  `esbuild.config.mjs`에서 파일 상단에 다음 가져오기를 추가합니다:

    ```js
    import esbuildSvelte from 'esbuild-svelte';
    import { sveltePreprocess } from 'svelte-preprocess';
    ```

4.  플러그인 목록에 Svelte를 추가합니다.

    ```js
    const context = await esbuild.context({
      plugins: [
        esbuildSvelte({
          compilerOptions: { css: 'injected' },
          preprocess: sveltePreprocess(),
        }),
      ],
      // ...
    });
    ```
  
5.  `package.json`에 `svelte-check`를 실행하는 스크립트를 추가합니다.
   
    ```json
    {
      // ...
      "scripts": {
        // ...
        "svelte-check": "svelte-check --tsconfig tsconfig.json"
      }
    }
    ```

## Svelte 컴포넌트 생성하기

플러그인의 루트 디렉토리에 `Counter.svelte`라는 새 파일을 만듭니다:

```tsx
<script lang="ts">
  interface Props {
    startCount: number;
  }

  let {
    startCount
  }: Props = $props();

  let count = $state(startCount);

  export function increment() {
    count += 1;
  }
</script>

<div class="number">
  <span>My number is {count}!</span>
</div>

<style>
  .number {
    color: red;
  }
</style>
```

## Svelte 컴포넌트 마운트하기

Svelte 컴포넌트를 사용하려면 기존 [[HTML elements|HTML 요소]]에 마운트해야 합니다. 예를 들어, Obsidian의 사용자 정의 [[ItemView|ItemView]]에 마운트하는 경우:

```ts
import { ItemView, WorkspaceLeaf } from 'obsidian';

// Counter Svelte 컴포넌트와 `mount`, `unmount` 메소드를 가져옵니다.
import Counter from './Counter.svelte';
import { mount, unmount } from 'svelte';

export const VIEW_TYPE_EXAMPLE = 'example-view';

export class ExampleView extends ItemView {
  // 이 ItemView에 마운트된 Counter 인스턴스를 유지할 변수입니다.
  counter: ReturnType<typeof Counter> | undefined;

  constructor(leaf: WorkspaceLeaf) {
    super(leaf);
  }

  getViewType() {
    return VIEW_TYPE_EXAMPLE;
  }

  getDisplayText() {
    return 'Example view';
  }

  async onOpen() {
    // Svelte 컴포넌트를 ItemView의 content 요소에 첨부하고 필요한 props를 제공합니다.
    this.counter = mount(Counter, {
      target: this.contentEl,
      props: {
        startCount: 5,
      }
    });

    // 컴포넌트 인스턴스가 타입화되었으므로, 내보낸 `increment` 메소드는 TypeScript에 알려져 있습니다.
    this.counter.increment();
  }

  async onClose() {
    if (this.counter) {
      // ItemView에서 Counter를 제거합니다.
      unmount(this.counter);
    }
  }
}
```

이 새로운 뷰를 사용자 인터페이스에 통합하는 방법에 대한 자세한 내용은 [[Views]]를 참조하세요.
