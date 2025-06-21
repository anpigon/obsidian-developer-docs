이 페이지는 게시되기 위해 모든 플러그인이 따라야 하는 플러그인별 요구 사항으로 [[Developer policies]]를 확장합니다.

## 재정적 지원 서비스 링크에만 `fundingUrl` 사용하기

Buy Me A Coffee나 GitHub Sponsors와 같은 서비스를 사용하여 플러그인에 대한 재정적 지원을 받는 경우 [[Manifest#fundingUrl|fundingUrl]]을 사용하세요.

기부를 받지 않는 경우, 매니페스트에서 `fundingUrl`을 제거하세요.

## 적절한 `minAppVersion` 설정하기

[[Reference/Manifest|매니페스트]]의 `minAppVersion`은 플러그인이 호환되는 Obsidian 앱의 최소 필수 버전으로 설정해야 합니다.
적절한 버전 번호를 모르는 경우, 최신 안정 빌드 번호를 사용하세요.

## 플러그인 설명은 짧고 간단하게 유지하기

좋은 플러그인 설명은 사용자가 플러그인을 빠르고 간결하게 이해하는 데 도움이 됩니다. 좋은 설명은 종종 다음과 같은 행동문으로 시작합니다:

- "선택한 텍스트를 ...로 번역합니다"
- "...에서 노트를 자동으로 생성합니다"
- "...에서 노트를 가져옵니다"
- "...에서 하이라이트와 주석을 동기화합니다"
- "...에서 링크를 엽니다"

"이것은 플러그인입니다"로 설명을 시작하지 마세요. 커뮤니티 플러그인 디렉토리의 맥락에서 사용자에게는 명백할 것입니다.

설명은 다음을 따라야 합니다:

- [Obsidian 스타일 가이드](https://help.obsidian.md/Contributing+to+Obsidian/Style+guide)를 따릅니다.
- 최대 250자여야 합니다.
- 마침표 `.`로 끝나야 합니다.
- 이모티콘이나 특수 문자를 사용하지 마세요.
- "Obsidian", "Markdown", "PDF"와 같은 약어, 고유 명사 및 상표에 대해 올바른 대소문자를 사용하세요. 용어의 대소문자 표기가 확실하지 않은 경우, 해당 웹사이트나 위키피디아 설명을 참조하세요.

## Node.js 및 Electron API는 데스크톱에서만 허용됩니다

Node.js 및 Electron API는 Obsidian의 데스크톱 버전에서만 사용할 수 있습니다. 예를 들어, `fs`, `crypto`, `os`와 같은 Node.js 패키지는 데스크톱에서만 사용할 수 있습니다.

플러그인이 이러한 API 중 하나라도 사용하는 경우, `manifest.json`에서 `isDesktopOnly`를 `true`로 **반드시** 설정해야 합니다.

> [!tip]
> 많은 Node.js 기능에는 웹 API 대안이 있습니다:
>
> - [`crypto`](https://nodejs.org/api/crypto.html) 대신 [`SubtleCrypto`](https://developer.mozilla.org/en-US/docs/Web/API/SubtleCrypto).
> - 클립보드 내용에 접근하기 위한 `navigator.clipboard.readText()` 및 `navigator.clipboard.writeText()`.

## 명령어 ID에 플러그인 ID를 포함하지 마세요

Obsidian은 자동으로 명령어 ID에 플러그인 ID를 접두사로 붙입니다.
직접 플러그인 ID를 포함할 필요가 없습니다.

## 모든 샘플 코드 제거하기

샘플 플러그인에는 플러그인에 필요한 가장 일반적인 작업을 수행하는 방법에 대한 예제가 포함되어 있습니다.
이는 시작을 돕기 위한 것일 뿐이며, 제출 전에 플러그인에서 샘플 코드를 제거해야 합니다.
