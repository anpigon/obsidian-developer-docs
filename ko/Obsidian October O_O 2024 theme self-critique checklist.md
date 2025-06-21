---
description: 테마 개발자가 자신의 테마를 자체 검토하기 위한 체크리스트입니다.
permalink: oo24/theme
---
## 호환성

- [ ] 가능한 한 CSS 변수(CSS Variables)를 사용하세요. [자세히 알아보기](https://docs.obsidian.md/Reference/CSS+variables/CSS+variables).
- [ ] `!important`를 사용하지 마세요.
- [ ] 라이브 프리뷰 에디터(Live Preview Editor)에서 사용되는 클래스의 수직 마진(Vertical Margin)을 변경하지 말고 대신 패딩(Padding)을 사용하세요.
- [ ] 최신 실험적 CSS 기능을 사용하는 경우 README에 필요한 최소 설치 프로그램 버전을 언급하세요.

## 성능

- [ ] 절대적으로 필요한 경우가 아니라면 `:has()`를 사용하지 마세요. 특히 캔버스(Canvas)에서 성능 문제를 일으킵니다.
- [ ] 폰트나 이미지와 같은 자산(Asset)에 링크하지 마세요. 로컬에 보관하세요. [자세히 알아보기](https://docs.obsidian.md/Themes/App+themes/Theme+guidelines#Keep+assets+local).

## 릴리스

- [ ] 절대적으로 필요한 경우가 아니라면 이름에 "Obsidian"을 포함하지 마세요. 대부분의 경우 중복입니다.
- [ ] 스크린샷 파일이 최신 상태인지 확인하세요. 이러한 스크린샷은 테마 디렉터리에서 썸네일(Thumbnail)로 표시됩니다.
- [ ] README가 최신 상태인지 확인하세요. 이는 모든 잠재적 사용자가 테마 디렉터리에서 테마를 확인할 때 보는 내용입니다.
- [ ] 디렉터리에서 빠르게 로드되도록 스크린샷을 작게 유지하세요. 512 x 288 픽셀 크기를 권장합니다.
- [ ] 다른 사람들이 테마와 소스 코드를 어떻게 사용할지 알 수 있도록 라이선스(License)가 있는지 확인하세요.
