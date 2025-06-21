플러그인(Plugin)을 사용하면 자신만의 기능으로 Obsidian을 확장하여 맞춤형 노트 작성 경험을 만들 수 있습니다.

이 튜토리얼에서는 샘플 플러그인을 소스 코드(Source Code)에서 컴파일(Compile)하고 Obsidian에 로드하는 방법을 배웁니다.

## 학습 내용

이 튜토리얼을 완료하면 다음을 할 수 있게 됩니다:

- Obsidian 플러그인 개발을 위한 환경 구성
- 소스 코드에서 플러그인 컴파일
- 플러그인을 수정한 후 다시 로드

## 사전 요구사항

이 튜토리얼을 완료하려면 다음이 필요합니다:

- 로컬 머신에 설치된 [Git](https://git-scm.com/)
- [Node.js](https://Node.js.org/en/about/)용 로컬 개발 환경
- [Visual Studio Code](https://code.visualstudio.com/)와 같은 코드 에디터(Code Editor)

## 시작하기 전에

플러그인을 개발할 때 한 번의 실수로 볼트(Vault)에 의도하지 않은 변경이 발생할 수 있습니다. 데이터 손실을 방지하려면 메인 볼트에서 플러그인을 개발해서는 안 됩니다. 항상 플러그인 개발 전용 별도 볼트를 사용하세요.

[빈 볼트 만들기](https://help.obsidian.md/Getting+started/Create+a+vault#Create+empty+vault).

## 1단계: 샘플 플러그인 다운로드

이 단계에서는 Obsidian이 찾을 수 있도록 볼트의 [`.obsidian` 디렉터리](https://help.obsidian.md/Advanced+topics/How+Obsidian+stores+data#Per+vault+data) 내 `plugins` 디렉터리에 샘플 플러그인을 다운로드합니다.

이 튜토리얼에서 사용할 샘플 플러그인은 [GitHub 저장소](https://github.com/obsidianmd/obsidian-sample-plugin)에서 사용할 수 있습니다.

1. 터미널(Terminal) 창을 열고 프로젝트 디렉터리를 `plugins` 디렉터리로 변경합니다.

   ```bash
   cd path/to/vault
   mkdir .obsidian/plugins
   cd .obsidian/plugins
   ```

2. Git을 사용하여 샘플 플러그인을 클론(Clone)합니다.

   ```bash
   git clone https://github.com/obsidianmd/obsidian-sample-plugin.git
   ```

> [!tip] GitHub 템플릿 저장소
> 샘플 플러그인의 저장소는 GitHub 템플릿 저장소(Template Repository)입니다. 즉, 샘플 플러그인에서 자신만의 저장소를 만들 수 있습니다. 방법을 알아보려면 [템플릿에서 저장소 만들기](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template#creating-a-repository-from-a-template)를 참조하세요.
>
> 샘플 플러그인을 클론할 때는 자신의 저장소 URL을 사용하는 것을 잊지 마세요.

## 2단계: 플러그인 빌드

이 단계에서는 Obsidian이 로드할 수 있도록 샘플 플러그인을 컴파일합니다.

1. 플러그인 디렉터리로 이동합니다.

   ```bash
   cd obsidian-sample-plugin
   ```

2. 의존성(Dependencies)을 설치합니다.

   ```bash
   npm install
   ```

3. 소스 코드를 컴파일합니다. 다음 명령은 터미널에서 계속 실행되며 소스 코드를 수정할 때 플러그인을 다시 빌드합니다.

   ```bash
   npm run dev
   ```

플러그인 디렉터리에 이제 플러그인의 컴파일된 버전이 포함된 `main.js` 파일이 있는 것을 확인할 수 있습니다.

## 3단계: 플러그인 활성화

Obsidian에서 플러그인을 로드하려면 먼저 활성화해야 합니다.

1. Obsidian에서 **설정**을 엽니다.
2. 사이드 메뉴에서 **커뮤니티 플러그인**을 선택합니다.
3. **커뮤니티 플러그인 켜기**를 선택합니다.
4. **설치된 플러그인** 아래에서 **Sample Plugin** 옆의 토글 버튼을 선택하여 활성화합니다.

이제 Obsidian에서 플러그인을 사용할 준비가 되었습니다. 다음으로 플러그인을 일부 변경해보겠습니다.

## 4단계: 플러그인 매니페스트 업데이트

이 단계에서는 플러그인 매니페스트(Manifest) `manifest.json`을 업데이트하여 플러그인 이름을 변경합니다. 매니페스트에는 플러그인의 이름과 설명 같은 정보가 포함되어 있습니다.

1. 코드 에디터에서 `manifest.json`을 엽니다.
2. `id`를 `"hello-world"`와 같은 고유 식별자로 변경합니다.
3. `name`을 `"Hello world"`와 같은 사용자 친화적인 이름으로 변경합니다.
4. 플러그인 폴더 이름을 플러그인의 `id`와 일치하도록 변경합니다.
5. 플러그인 매니페스트의 새 변경 사항을 로드하려면 Obsidian을 다시 시작합니다.

**설치된 플러그인**으로 돌아가서 플러그인 이름이 변경한 내용을 반영하여 업데이트된 것을 확인하세요.

`manifest.json`을 변경할 때마다 Obsidian을 다시 시작하는 것을 잊지 마세요.

## 5단계: 소스 코드 업데이트

사용자가 플러그인과 상호작용할 수 있도록 사용자가 선택했을 때 인사말을 표시하는 _리본 아이콘(Ribbon Icon)_을 추가합니다.

1. 코드 에디터에서 `main.ts`를 엽니다.
2. 플러그인 클래스(Class) 이름을 `MyPlugin`에서 `HelloWorldPlugin`으로 변경합니다.
3. `obsidian` 패키지(Package)에서 `Notice`를 가져옵니다(Import).

   ```ts
   import { Notice, Plugin } from 'obsidian';
   ```

4. `onload()` 메서드(Method)에서 다음 코드를 추가합니다:

   ```ts
   this.addRibbonIcon('dice', 'Greet', () => {
     new Notice('Hello, world!');
   });
   ```

5. **명령 팔레트(Command Palette)**에서 **저장하지 않고 앱 다시 로드**를 선택하여 플러그인을 다시 로드합니다.

이제 Obsidian 창 왼쪽의 리본에서 주사위 아이콘을 볼 수 있습니다. 이를 선택하면 오른쪽 상단 모서리에 메시지가 표시됩니다.

**소스 코드를 변경한 후에는 플러그인을 다시 로드해야 한다**는 점을 기억하세요. 커뮤니티 플러그인 패널에서 비활성화한 다음 다시 활성화하거나 이 단계의 5번에서 설명한 대로 명령 팔레트를 사용하면 됩니다.

> [!tip] 핫 리로딩(Hot Reloading)
> 개발하는 동안 플러그인을 자동으로 다시 로드하려면 [Hot-Reload](https://github.com/pjeby/hot-reload) 플러그인을 설치하세요.

## 결론

이 튜토리얼에서는 TypeScript API를 사용하여 첫 번째 Obsidian 플러그인을 빌드했습니다. 플러그인을 수정하고 다시 로드하여 Obsidian 내에서 변경 사항을 반영했습니다.
