Obsidian 커뮤니티와 테마를 공유하려면 [공식 테마 목록](https://github.com/obsidianmd/obsidian-releases/blob/master/community-css-themes.json)에 제출하는 것이 가장 좋은 방법입니다. 검토 후 테마가 게시되면 사용자는 Obsidian 내에서 직접 설치할 수 있으며 [플러그인 디렉토리](https://obsidian.md/plugins)에도 소개됩니다.

테마의 초기 버전만 제출하면 됩니다. 테마가 게시된 후 사용자는 GitHub에서 새 릴리스를 Obsidian 내에서 자동으로 다운로드할 수 있습니다.

## 필수 조건

이 가이드를 완료하려면 다음이 필요합니다:
- [GitHub](https://github.com/signup) 계정

## 시작하기 전에

테마를 제출하기 전에 저장소 루트 폴더에 다음 파일이 있는지 확인하세요:
- 테마를 설명하는 `README.md` 파일
- 테마와 소스 코드 사용 방식을 결정하는 `LICENSE` 파일. 라이선스 선택에 도움이 필요하면 [Choose a License](https://choosealicense.com/)를 참조하세요.
- 커뮤니티 테마 스토어에 표시될 테마 스크린샷 (권장 이미지 크기: 512 x 288 픽셀)
- 테마를 설명하는 `manifest.json` 파일. 자세한 내용은 [[Manifest]]를 참조하세요.

## 1단계: GitHub에 테마 게시

> [!note] 템플릿 저장소
> 템플릿 저장소에서 테마를 생성한 경우 이 단계를 건너뛸 수 있습니다.

테마를 검토하려면 GitHub의 소스 코드에 액세스할 수 있어야 합니다. GitHub에 익숙하지 않은 경우 [새 저장소 만들기](https://docs.github.com/ko/repositories/creating-and-managing-repositories/creating-a-new-repository) 방법을 참조하세요.

## 2단계: 테마 검토 요청

이 단계에서는 Obsidian 팀에 테마 검토를 요청합니다.

1. [community-css-themes.json](https://github.com/obsidianmd/obsidian-releases/edit/master/community-css-themes.json)에서 JSON 배열 끝에 새 항목을 추가하세요. 다음은 [Minimal](https://github.com/kepano/obsidian-minimal) 테마의 예시입니다.

   ```json
   {
     "name": "Minimal",
     "author": "kepano",
     "repo": "kepano/obsidian-minimal",
     "screenshot": "dark-simple.png",
     "modes": ["dark", "light"]
   },
   ```

   - `name`과 `author`는 사용자에게 표시되는 방식이며 [[Manifest]]의 해당 속성과 일치해야 합니다.
   - `repo`는 GitHub 저장소 경로입니다 (예: `your-username/your-repo-name`).
   - `screenshot`은 테마 스크린샷 경로입니다 (권장 비율 16:9, 크기 512 x 288 픽셀).
   - `modes`는 테마가 지원하는 색상 모드를 나열합니다.

   이전 항목의 닫는 중괄호 `}` 뒤에 쉼표를 추가하는 것을 잊지 마세요.

2. 오른쪽 상단에서 **Commit changes...** 선택
3. **Propose changes** 선택
4. **Create pull request** 선택
5. **Preview** → **Community Theme** 선택
6. **Create pull request** 클릭
7. 풀 리퀘스트 제목에 "Add [...] theme" 입력 (여기서 [...]는 테마 이름)
8. 설명란에 세부 정보 작성. 체크박스는 `[x]`로 표시
9. **Create pull request** 클릭 (마지막으로 🤞)

이제 테마를 Obsidian 테마 디렉토리에 제출했습니다. 검토가 완료될 때까지 기다리세요.

> [!question] 검토에 얼마나 걸리나요?
> 검토 시간은 Obsidian 팀의 작업량에 따라 다릅니다. 팀 규모가 작으므로 검토가 완료될 때까지 기다려 주세요. 현재 검토 예상 시간을 알려드릴 수 없습니다.

## 3단계: 검토 의견 처리

검토자가 테마를 검토한 후 풀 리퀘스트에 검토 결과를 댓글로 추가합니다. 테마 업데이트를 요구하거나 개선 사항을 제안할 수 있습니다.

필요한 변경 사항을 처리하고 GitHub 릴리스를 업데이트하세요. PR에 댓글을 남겨 피드백을 처리했음을 알리세요. 새 PR을 열지 마세요.

필요한 모든 변경 사항이 처리되면 테마를 게시할 것입니다.

> [!note]
> Obsidian 팀원만 테마를 게시할 수 있지만, 그동안 다른 커뮤니티 구성원이 검토를 도울 수 있습니다.

## 다음 단계

테마가 검토되고 게시되면 커뮤니티에 알릴 차례입니다:
- [포럼의 Share & showcase](https://forum.obsidian.md/c/share-showcase/9)에 공지
- [Discord](https://discord.gg/veuWUTm)의 `#updates` 채널에 공지 (`developer` 역할 필요)
