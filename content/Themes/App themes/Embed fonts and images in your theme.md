테마에 글꼴 및 이미지와 같은 자산을 포함하는 방법을 알아보세요.

> [!warning] 원격 콘텐츠 로딩
> Obsidian이 오프라인에서 작동하고 사용자 개인 정보를 보호하기 위해 테마는 [[Developer policies|네트워크를 통해 원격 콘텐츠를 로드할 수 없습니다]]. 자세한 내용은 [[Theme guidelines#Keep resources local|자산을 로컬로 유지]]를 참조하세요.

## 데이터 URL 사용

테마에 글꼴, 아이콘, 이미지와 같은 자산을 포함하려면 [url()](https://developer.mozilla.org/ko/docs/Web/CSS/url) 함수에 [데이터 URL](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Data_URLs)을 전달하여 CSS 파일에 _포함_해야 합니다.

자산에 대한 데이터 URL을 만들려면 다음 형식을 사용하여 URL을 만드세요:

```css
url("data:<MIME_TYPE>;base64,<BASE64_DATA>")
```

- `<MIME_TYPE>`을 자산의 MIME 유형으로 바꿉니다. 모르는 경우 [일반적인 MIME 유형](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types/Common_types)을 참조하세요.
- `<BASE64_DATA>`를 자산의 [Base64](https://ko.wikipedia.org/wiki/Base64) 인코딩 표현으로 바꿉니다.

다음 예제는 GIF 파일을 배경 이미지로 포함합니다:

```css
h1 {
  background-image: url("data:image/gif;base64,R0lGODdhAQADAPABAP////8AACwAAAAAAQADAAACAgxQADs=")
}
```

## 자산 인코딩

자산을 base64로 인코딩하는 방법에 대한 지침은 [데이터를 base64 형식으로 인코딩](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Data_URLs#encoding_data_into_base64_format)을 참조하세요.

인코딩을 위한 많은 무료 온라인 도구 중 하나를 사용할 수도 있습니다.

글꼴의 경우:

- WOFF2 글꼴 파일용 [Woff2Base](https://hellogreg.github.io/woff2base/)
- [Aspose](https://products.aspose.app/font/base64)는 다양한 글꼴 형식을 지원합니다.

이미지의 경우:

- [WebSemantics](https://websemantics.uk/tools/image-to-data-uri-converter/)는 JPEG, JPG, GIF, PNG, SVG를 변환합니다.
- [Base64 Guru](https://base64.guru/converter/encode/image)는 다양한 이미지 형식을 지원합니다.
- [Yoksel URL-encoder for SVG](https://yoksel.github.io/url-encoder/)는 SVG 파일에 최적화되어 있습니다.

## 파일 크기 고려

자산을 포함하면 테마의 파일 크기가 증가하여 다음과 같은 상황에서 성능이 저하될 수 있습니다:

- 커뮤니티 테마 디렉토리에서 테마 다운로드 및 업데이트
- Obsidian 앱에서 테마 로드 및 사용
- 코드 에디터에서 테마 편집. [Sass](https://sass-lang.com/) 또는 [Less](https://lesscss.org/)와 같은 CSS 전처리기를 사용하여 테마를 여러 파일로 나누는 것을 고려하세요.
