플러그인을 Obsidian 커뮤니티와 공유하고 싶다면, 가장 좋은 방법은 [공식 플러그인 목록](https://github.com/obsidianmd/obsidian-releases/blob/master/community-plugins.json)에 제출하는 것입니다. 우리가 플러그인을 검토하고 게시하면, 사용자는 Obsidian 내에서 직접 설치할 수 있습니다. 또한 Obsidian 웹사이트의 [플러그인 디렉토리](https://obsidian.md/plugins)에도 소개됩니다.

플러그인의 초기 버전만 제출하면 됩니다. 플러그인이 게시된 후에는 사용자가 Obsidian 내에서 직접 GitHub의 새 릴리스를 다운로드할 수 있습니다.

## 사전 준비

이 가이드를 완료하려면 다음이 필요합니다:

- [GitHub](https://github.com/signup) 계정.

## 시작하기 전에

플러그인을 제출하기 전에 리포지토리의 루트 폴더에 다음 파일이 있는지 확인하세요:

- 플러그인의 목적과 사용 방법을 설명하는 `README.md`.
- 다른 사람이 플러그인과 소스 코드를 어떻게 사용할 수 있는지 결정하는 `LICENSE`. 플러그인에 [라이선스를 추가](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-a-license-to-a-repository)하는 데 도움이 필요하면 [라이선스 선택](https://choosealicense.com/)을 참조하세요.
- 플러그인을 설명하는 `manifest.json`. 자세한 내용은 [[Manifest]]를 참조하세요.

## 1단계: GitHub에 플러그인 게시하기

> [!note] 템플릿 리포지토리
> 템플릿 리포지토리 중 하나에서 플러그인을 생성한 경우 이 단계를 건너뛸 수 있습니다.

플러그인을 검토하려면 GitHub의 소스 코드에 접근해야 합니다. GitHub에 익숙하지 않은 경우, [새 리포지토리 생성](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository) 방법에 대한 GitHub 문서를 참조하세요.

## 2단계: 릴리스 생성하기

이 단계에서는 제출할 준비가 된 플러그인 릴리스를 준비합니다.

1.  `manifest.json`에서 `version`을 [시맨틱 버전 관리(Semantic Versioning)](https://semver.org/) 사양을 따르는 새 버전으로 업데이트합니다(예: 초기 릴리스의 경우 `1.0.0`). 버전은 `x.y.z` 형식만 지원됩니다.
2.  [GitHub 릴리스를 생성](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository#creating-a-release)합니다. 릴리스의 "태그 버전"은 `manifest.json`의 버전과 일치해야 합니다.
3.  릴리스 이름을 입력하고 설명 필드에 설명합니다. Obsidian은 릴리스 이름을 아무것도 사용하지 않으므로 원하는 대로 이름을 지정해도 됩니다.
4.  다음 플러그인 자산을 릴리스에 바이너리 첨부 파일로 업로드합니다:

    - `main.js`
    - `manifest.json`
    - `styles.css` (선택 사항)

## 3단계: 검토를 위해 플러그인 제출하기

이 단계에서는 검토를 위해 Obsidian 팀에 플러그인을 제출합니다.

1.  [community-plugins.json](https://github.com/obsidianmd/obsidian-releases/edit/master/community-plugins.json)에서 JSON 배열의 끝에 새 항목을 추가합니다.

    ```json
    {
      "id": "doggo-dictation",
      "name": "Doggo Dictation",
      "author": "John Dolittle",
      "description": "Transcribes dog speech into notes.",
      "repo": "drdolittle/doggo-dictation"
    }
    ```

    - `id`, `name`, `author`, `description`은 플러그인이 사용자에게 어떻게 표시되는지를 결정하며, [[Manifest]]의 해당 속성과 일치해야 합니다.
    - `id`는 플러그인에 고유합니다. `community-plugins.json`을 검색하여 동일한 ID를 가진 기존 플러그인이 없는지 확인하세요. `id`는 `obsidian`을 포함할 수 없습니다.
    - `repo`는 GitHub 리포지토리의 경로입니다. 예를 들어, GitHub 리포지토리가 https://github.com/your-username/your-repo-name에 있는 경우 경로는 `your-username/your-repo-name`입니다.

    이전 항목의 닫는 중괄호 `}` 뒤에 쉼표를 추가하는 것을 잊지 마세요.

2.  오른쪽 상단에서 **Commit changes...**를 선택합니다.
3.  **Propose changes**를 선택합니다.
4.  **Create pull request**를 선택합니다.
5.  **Preview**를 선택한 다음 **Community Plugin**을 선택합니다.
6.  **Create pull request**를 클릭합니다.
7.  풀 리퀘스트 이름에 "Add plugin: [...]"을 입력합니다. 여기서 [...]는 플러그인 이름입니다.
8.  풀 리퀘스트 설명에 세부 정보를 입력합니다. 확인란의 경우 대괄호 사이에 `x`를 삽입하여 `[x]` 완료로 표시합니다.
9.  **Create pull request**를 클릭합니다 (마지막으로 🤞).

이제 Obsidian 플러그인 디렉토리에 플러그인을 제출했습니다. 편안히 앉아 친절한 봇의 초기 검증을 기다리세요. 결과가 준비되기까지 몇 분이 걸릴 수 있습니다.

- PR에 **Ready for review** 레이블이 표시되면 제출이 자동 검증을 통과한 것입니다.
- PR에 **Validation failed** 레이블이 표시되면, 봇이 **Ready for review** 레이블을 할당할 때까지 나열된 모든 문제를 해결해야 합니다.

제출이 검토 준비가 되면, 편안히 앉아 Obsidian 팀이 검토하기를 기다릴 수 있습니다.

> [!question] 플러그인 검토에 얼마나 걸리나요?
> 제출 검토에 걸리는 시간은 Obsidian 팀의 현재 작업량에 따라 다릅니다. 팀은 아직 작으므로 플러그인이 검토될 때까지 인내심을 가져주세요. 현재로서는 제출 검토 시기에 대한 추정치를 제공할 수 없습니다.

## 4단계: 리뷰 의견 해결하기

리뷰어가 플러그인을 검토하면, 풀 리퀘스트에 리뷰 결과와 함께 댓글을 추가할 것입니다. 리뷰어는 플러그인을 업데이트하도록 요구하거나 개선 방법에 대한 제안을 할 수 있습니다.

필요한 변경 사항을 해결하고 새 변경 사항으로 GitHub 릴리스를 업데이트하세요. 피드백을 해결했음을 알리기 위해 PR에 댓글을 남기세요. 새 PR을 열지 마세요.

필요한 모든 변경 사항이 해결되었는지 확인하는 즉시 플러그인을 게시할 것입니다.

> [!note]
> Obsidian 팀원만 플러그인을 게시할 수 있지만, 다른 커뮤니티 구성원도 그동안 제출물을 검토해 줄 수 있습니다.

## 다음 단계

플러그인을 검토하고 게시한 후에는 커뮤니티에 알릴 시간입니다:

- 포럼의 [Share & showcase](https://forum.obsidian.md/c/share-showcase/9)에서 발표하세요.
- [Discord](https://discord.gg/veuWUTm)의 `#updates` 채널에서 발표하세요. `#updates`에 게시하려면 [`developer` 역할](https://discord.com/channels/686053708261228577/702717892533157999/830492034807758859)이 필요합니다.
