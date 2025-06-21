---
description: 플러그인 개발자가 자신의 플러그인을 자체 검토하기 위한 체크리스트입니다.
permalink: oo24/plugin
---
## 릴리스 & 네이밍

- [ ] `MyPlugin`과 `SampleSettingTab`같은 플레이스홀더(Placeholder) 이름을 제거하세요.
- [ ] 절대적으로 필요한 경우가 아니라면 이름에 "Obsidian"을 포함하지 마세요. 대부분의 경우 중복입니다.
- [ ] 명령 이름에 플러그인 이름을 포함하지 마세요. Obsidian이 자동으로 추가합니다.
- [ ] 명령에 플러그인 ID를 접두사로 사용하지 마세요. Obsidian이 자동으로 추가합니다.
- [ ] 저장소에 `main.js`를 포함하지 마세요. 릴리스에만 포함하세요.
- [ ] 아직 추가하지 않았다면 `fundingUrl`을 추가하여 플러그인 사용자가 지원을 보여줄 수 있도록 고려해 보세요. [자세히 알아보기](https://docs.obsidian.md/Reference/Manifest#fundingUrl).

## 호환성

- [ ] 명령에 기본 핫키(Hotkey)를 제공하지 마세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines#Avoid+setting+a+default+hotkey+for+commands).
- [ ] 코어 스타일링을 재정의하지 마세요. 필요한 경우 자신만의 클래스를 추가하고 해당 클래스에만 스타일링을 적용하세요.
- [ ] 더 이상 사용되지 않는 메서드(Method)를 찾기 위해 코드를 스캔하세요 (IDE에서 보통 취소선 텍스트로 표시됨).
- [ ] JavaScript나 HTML에서 스타일을 할당하지 마세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines#No+hardcoded+styling).
- [ ] 구성 디렉터리에 액세스해야 하는 경우 하드코딩된 `.obsidian` 폴더에 액세스하지 마세요. 위치가 사용자 정의될 수 있으므로 `Vault.configDir`을 사용하세요.

## 모바일 지원

매니페스트(Manifest)에서 `isDesktopOnly`가 false로 설정된 경우에만 이 섹션을 완료하세요.

- [ ] `fs`, `path`, `electron`과 같은 node.js 모듈을 사용하지 마세요.
- [ ] 16.4 미만의 iOS 버전을 지원하려면 정규식 룩비하인드(Lookbehind)를 사용하지 마세요 (플러그인에서 정규식을 사용하지 않는다면 무시). [자세히 알아보기](https://docs.obsidian.md/Plugins/Getting+started/Mobile+development#Lookbehind+in+regular+expressions).
- [ ] `FileSystemAdapter` 클래스를 사용하지 마세요.
- [ ] `process.platform`을 사용하지 말고 Obsidian의 `Platform`을 사용하세요. [API 링크](https://docs.obsidian.md/Reference/TypeScript+API/Platform).
- [ ] `fetch`나 `axios.get`을 사용하지 말고 Obsidian의 `requestUrl`을 사용하세요. [API 링크](https://docs.obsidian.md/Reference/TypeScript+API/requestUrl).

## 코딩 스타일

- [ ] `var`를 사용하지 마세요. 대신 `let`이나 `const`를 사용하세요. [자세히 알아보기](https://javascript.plainenglish.io/4-reasons-why-var-is-considered-obsolete-in-modern-javascript-a30296b5f08f).
- [ ] 전역 `app` 인스턴스(Instance)를 사용하지 마세요. 대신 플러그인 인스턴스에 제공되는 `this.app`을 사용하세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines#Avoid%20using%20global%20app%20instance).
- [ ] `main.ts`가 커지면 코드를 찾기 쉽도록 더 작은 파일이나 폴더로 나누세요.
- [ ] 가독성을 위해 `Promise`를 사용하는 대신 가능한 한 `async`와 `await`를 사용하세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines#Prefer+async%2Fawait+over+Promise).
- [ ] 전역 변수를 사용하지 마세요. 변수를 클래스나 함수의 범위 내에 유지하도록 노력하세요. [자세히 알아보기](http://wiki.c2.com/?GlobalVariablesAreBad).
- [ ] `TFile`, `TFolder`, `FileSystemAdapter`와 같은 다른 타입으로 캐스팅하기 전에 `instanceof`로 테스트하세요.
- [ ] `as any`를 사용하지 말고 적절한 타이핑을 사용하세요.

## API 사용

- [ ] `Vault.modify`를 사용하지 마세요. 활성 파일을 편집하려면 `Editor` 인터페이스를 사용하세요. 백그라운드에서 편집하려면 `Vault.process`를 사용하세요.
- [ ] 프론트매터(Frontmatter)를 수동으로 읽고 쓰지 마세요. 대신 `FileManager.processFrontMatter`를 사용하세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines#Prefer+%60FileManager.processFrontMatter%60+to+modify+frontmatter+of+a+note).
- [ ] 파일을 삭제하기 위해 `vault.delete`를 사용하지 마세요. 사용자 기본 설정에 따라 파일이 삭제되도록 `trashFile`을 사용하세요. [자세히 알아보기](https://docs.obsidian.md/Reference/TypeScript+API/FileManager/trashFile).
- [ ] 가능한 한 `Adapter` API를 사용하지 마세요. 대신 `Vault` API를 사용하세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines#Prefer+the+Vault+API+over+the+Adapter+API).
- [ ] 플러그인 데이터 읽기와 쓰기를 직접 관리하지 마세요. 대신 `Plugin.loadData()`와 `Plugin.saveData()`를 사용하세요.
- [ ] 사용자 정의 경로를 받는 경우 `normalizePath()`를 사용하세요. [자세히 알아보기](https://docs.obsidian.md/Reference/TypeScript+API/normalizePath).

## 성능

- [ ] 플러그인의 로드 시간을 최적화하세요. [상세 가이드](https://docs.obsidian.md/Plugins/Guides/Optimizing+plugin+load+time).
- [ ] 경로로 파일이나 폴더를 찾기 위해 모든 파일을 반복하지 마세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines#Avoid+iterating+all+files+to+find+a+file+by+its+path).
- [ ] 플러그인이 Obsidian 1.7.2+ (현재 얼리 액세스)와 호환되기를 원한다면 `DeferredViews`와 작동하도록 플러그인을 업데이트하세요. [상세 가이드](https://docs.obsidian.md/Plugins/Guides/Understanding+deferred+views).
- [ ] `moment`를 사용하는 경우 다른 복사본을 가져오지 않도록 `import { moment} from 'obsidian'`을 사용하고 있는지 확인하세요.
- [ ] 릴리스를 위해 `main.js`를 최소화하세요.
- [ ] 생성자나 `onload()` 함수 대신 `workspace.onLayoutReady()`에서 초기 UI 설정을 수행하세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Guides/Optimizing+plugin+load+time#If+you+have+code+that+you+want+to+run+at+startup%2C+where+should+it+go%3F).

## 사용자 인터페이스

- [ ] 섹션이 두 개 이상 없다면 설정 제목을 사용하지 마세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines#Only+use+headings+under+settings+if+you+have+more+than+one+section).
- [ ] 설정 제목에 "setting"이나 "option"이라는 단어를 포함하지 마세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines#Avoid+%22settings%22+in+settings+headings).
- [ ] Obsidian UI의 나머지 부분과 일관성을 유지하기 위해 UI 요소의 모든 텍스트에 문장 케이스(Sentence Case)를 사용하세요. [자세히 알아보기](https://en.wiktionary.org/wiki/sentence_case).
- [ ] 설정 헤더에 `<h1>`이나 `<h2>`를 사용하지 마세요. 대신 Obsidian API를 사용하세요. [자세히 알아보기](https://docs.obsidian.md/Plugins/Releasing/Plugin+guidelines#Use+%60setHeading%60+instead+of+a+%60%3Ch1%3E%60%2C+%60%3Ch2%3E%60).
- [ ] 절대적으로 필요한 경우가 아니라면 `console.log`를 사용하지 마세요. 프로덕션에 필요하지 않은 테스트 콘솔 로그를 제거하세요.
