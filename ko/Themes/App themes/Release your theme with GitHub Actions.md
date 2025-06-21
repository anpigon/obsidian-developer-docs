테마를 수동으로 릴리스하는 것은 시간이 많이 걸리고 오류가 발생하기 쉽습니다. 이 가이드에서는 새 태그를 만들 때 [GitHub Actions](https://github.com/features/actions)를 사용하여 자동으로 릴리스를 생성하도록 테마를 구성합니다.

1. 테마의 루트 디렉토리에서 `.github/workflows` 아래에 다음 내용으로 `release.yml` 파일을 만듭니다:

   ```yml
   name: Obsidian 테마 릴리스

   on:
     push:
       tags:
         - "*"

   jobs:
     build:
       runs-on: ubuntu-latest

       steps:
         - uses: actions/checkout@v3

         - name: 릴리스 생성
           env:
             GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
           run: |
             tag="${GITHUB_REF#refs/tags/}"

             gh release create "$tag" \
               --title="$tag" \
               --generate-notes \
               --draft \
               manifest.json theme.css
   ```

2. 터미널에서 워크플로우를 커밋합니다.

   ```bash
   git add .github/workflows/release.yml
   git commit -m "릴리스 워크플로우 추가"
   git push origin main
   ```

3. `manifest.json` 파일의 버전과 일치하는 태그를 만듭니다.

   ```bash
   git tag -a 1.0.1 -m "1.0.1"
   git push origin 1.0.1
   ```

   - `-a`는 [주석 태그](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%ED%83%9C%EA%B7%B8)를 만듭니다.
   - `-m`은 릴리스 이름을 지정합니다. Obsidian 플러그인의 경우 버전과 동일해야 합니다.

4. GitHub에서 저장소로 이동하여 **Actions** 탭을 선택합니다. 워크플로우가 아직 실행 중이거나 이미 완료되었을 수 있습니다.

5. 워크플로우가 완료되면 저장소의 기본 페이지로 돌아가서 오른쪽 사이드바에서 **Releases**를 선택합니다. 워크플로우가 초안 GitHub 릴리스를 만들고 필요한 자산을 바이너리 첨부 파일로 업로드했습니다.

6. 릴리스 이름 오른쪽에 있는 **Edit**(연필 아이콘)를 선택합니다.

7. 릴리스 노트를 추가하여 사용자에게 이 릴리스에서 변경된 내용을 알린 다음 **Publish release**를 선택합니다.

이제 새 태그를 만들 때마다 자동으로 GitHub 릴리스를 생성하도록 테마를 성공적으로 설정했습니다.

- 이 테마의 첫 번째 릴리스인 경우 이제 [[Submit your theme|테마를 제출]]할 준비가 되었습니다.
- 이미 게시된 테마에 대한 업데이트인 경우 사용자는 이제 최신 버전으로 업데이트할 수 있습니다.
