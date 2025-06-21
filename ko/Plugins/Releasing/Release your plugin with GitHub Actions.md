플러그인을 수동으로 릴리스하는 것은 시간이 많이 걸리고 오류가 발생하기 쉽습니다. 이 가이드에서는 [GitHub Actions](https://github.com/features/actions)를 사용하여 새 태그를 생성할 때 자동으로 릴리스를 생성하도록 플러그인을 설정합니다.

1.  플러그인의 루트 디렉토리에서 `.github/workflows` 아래에 `release.yml`이라는 파일을 만들고 다음 내용을 추가합니다:

    ```yml
    name: Release Obsidian plugin

    on:
      push:
        tags:
          - "*"

    jobs:
      build:
        runs-on: ubuntu-latest
        permissions:
          contents: write
        steps:
          - uses: actions/checkout@v3

          - name: Use Node.js
            uses: actions/setup-node@v3
            with:
              node-version: "18.x"

          - name: Build plugin
            run: |
              npm install
              npm run build

          - name: Create release
            env:
              GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
            run: |
              tag="${GITHUB_REF#refs/tags/}"

              gh release create "$tag" \
                --title="$tag" \
                --draft \
                main.js manifest.json styles.css
    ```

2.  터미널에서 워크플로우를 커밋합니다.

    ```bash
    git add .github/workflows/release.yml
    git commit -m "Add release workflow"
    git push origin main
    ```

3.  GitHub에서 리포지토리로 이동하여 **Settings** 탭을 선택합니다. 왼쪽 사이드바에서 **Actions** 메뉴를 확장하고 **General** 메뉴로 이동한 다음 **Workflow permissions** 섹션으로 스크롤하여 **Read and write permissions** 옵션을 선택하고 저장합니다.

4.  `manifest.json` 파일의 버전과 일치하는 태그를 생성합니다.

    ```bash
    git tag -a 1.0.1 -m "1.0.1"
    git push origin 1.0.1
    ```

    - `-a`는 [주석 태그(annotated tag)](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_creating_tags)를 생성합니다.
    - `-m`은 릴리스 이름을 지정합니다. Obsidian 플러그인의 경우 버전과 동일해야 합니다.

5.  GitHub에서 리포지토리로 이동하여 **Actions** 탭을 선택합니다. 워크플로우가 아직 실행 중이거나 이미 완료되었을 수 있습니다.

6.  워크플로우가 완료되면 리포지토리의 기본 페이지로 돌아가 오른쪽 사이드바에서 **Releases**를 선택합니다. 워크플로우는 초안 GitHub 릴리스를 생성하고 필요한 자산을 바이너리 첨부 파일로 업로드했습니다.

7.  릴리스 이름 오른쪽에 있는 **Edit**(연필 아이콘)를 선택합니다.

8.  이 릴리스에서 변경된 내용을 사용자에게 알리기 위해 릴리스 노트를 추가한 다음 **Publish release**를 선택합니다.

이제 새 태그를 생성할 때마다 자동으로 GitHub 릴리스를 생성하도록 플러그인을 성공적으로 설정했습니다.

- 이 플러그인의 첫 번째 릴리스인 경우, 이제 [[Submit your plugin|플러그인을 제출]]할 준비가 되었습니다.
- 이미 게시된 플러그인의 업데이트인 경우, 사용자는 이제 최신 버전으로 업데이트할 수 있습니다.
