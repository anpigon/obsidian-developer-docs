이 가이드에서는 [React](https://react.dev/)를 사용하도록 플러그인을 설정합니다. React를 사용하도록 변환하려는 [[Views|사용자 정의 뷰]]가 이미 있는 플러그인을 가지고 있다고 가정합니다.

플러그인을 빌드하기 위해 별도의 프레임워크를 사용할 필요는 없지만, React를 사용하려는 몇 가지 이유가 있습니다:

- React에 대한 기존 경험이 있고 익숙한 기술을 사용하고 싶을 때.
- 플러그인에서 재사용하고 싶은 기존 React 컴포넌트가 있을 때.
- 플러그인에 복잡한 상태 관리나 일반적인 [[HTML elements]]로 구현하기 번거로운 다른 기능이 필요할 때.

## 플러그인 설정하기

1.  플러그인 종속성에 React를 추가합니다:

    ```bash
    npm install react react-dom
    ```

2.  React에 대한 타입 정의를 추가합니다:

    ```bash
    npm install --save-dev @types/react @types/react-dom
    ```

3.  `tsconfig.json`의 `compilerOptions` 객체에서 JSX 지원을 활성화합니다:

    ```ts
    {
      "compilerOptions": {
        "jsx": "react-jsx"
      }
    }
    ```

## React 컴포넌트 생성하기

플러그인 루트 디렉토리에 `ReactView.tsx`라는 새 파일을 만들고 다음 내용을 추가합니다:

```tsx title="ReactView.tsx"
export const ReactView = () => {
  return <h4>Hello, React!</h4>;
};
```

## React 컴포넌트 마운트하기

React 컴포넌트를 사용하려면 [[HTML elements|HTML 요소]]에 마운트해야 합니다. 다음 예제는 `ReactView` 컴포넌트를 `this.contentEl` 요소에 마운트합니다:

```tsx
import { StrictMode } from 'react';
import { ItemView, WorkspaceLeaf } from 'obsidian';
import { Root, createRoot } from 'react-dom/client';
import { ReactView } from './ReactView';

const VIEW_TYPE_EXAMPLE = 'example-view';

class ExampleView extends ItemView {
	root: Root | null = null;

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
		this.root = createRoot(this.contentEl);
		this.root.render(
			<StrictMode>
				<ReactView />,
			</StrictMode>,
		);
	}

	async onClose() {
		this.root?.unmount();
	}
}
```

`createRoot` 및 `unmount()`에 대한 자세한 내용은 [ReactDOM](https://react.dev/reference/react-dom/client/createRoot#root-render) 문서를 참조하세요.

React 컴포넌트를 [[Plugins/User interface/Status bar|상태 표시줄 항목]]과 같은 모든 `HTMLElement`에 마운트할 수 있습니다. 작업이 끝나면 `this.root.unmount()`를 호출하여 제대로 정리해야 합니다.

## App 컨텍스트 생성하기

React 컴포넌트 중 하나에서 [[Reference/TypeScript API/App|App]] 객체에 접근하려면 이를 종속성으로 전달해야 합니다. 플러그인이 커지면서 `App` 객체를 몇 군데에서만 사용하더라도 전체 컴포넌트 트리를 통해 전달하기 시작합니다.

또 다른 대안은 앱에 대한 React 컨텍스트를 만들어 React 뷰 내부의 모든 컴포넌트에서 전역적으로 사용할 수 있도록 하는 것입니다.

1.  `createContext()`를 사용하여 새 앱 컨텍스트를 만듭니다.

    ```tsx title="context.ts"
    import { createContext } from 'react';
    import { App } from 'obsidian';

    export const AppContext = createContext<App | undefined>(undefined);
    ```

2.  `ReactView`를 컨텍스트 제공자로 감싸고 앱을 값으로 전달합니다.

    ```tsx title="view.tsx"
    this.root = createRoot(this.contentEl);
    this.root.render(
      <AppContext.Provider value={this.app}>
        <ReactView />
      </AppContext.Provider>
    );
    ```

3.  컴포넌트에서 컨텍스트를 더 쉽게 사용할 수 있도록 사용자 정의 훅을 만듭니다.

    ```tsx title="hooks.ts"
    import { useContext } from 'react';
    import { AppContext } from './context';

    export const useApp = (): App | undefined => {
      return useContext(AppContext);
    };
    ```

4.  `ReactView` 내의 모든 React 컴포넌트에서 훅을 사용하여 앱에 접근합니다.

    ```tsx title="ReactView.tsx"
    import { useApp } from './hooks';

    export const ReactView = () => {
      const { vault } = useApp();

      return <h4>{vault.getName()}</h4>;
    };
    ```

자세한 내용은 React 문서의 [Passing Data Deeply with Context](https://react.dev/learn/passing-data-deeply-with-context) 및 [Reusing Logic with Custom Hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)를 참조하세요.
