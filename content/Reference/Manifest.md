---
cssclasses: reference
---

이 페이지는 매니페스트 `manifest.json`의 스키마를 설명합니다.

## 속성

다음 속성은 플러그인과 테마 모두에 사용할 수 있습니다.

| 속성 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| `author` | `string` | **예** | 저자 이름. |
| `minAppVersion` | `string` | **예** | 최소 필수 Obsidian 버전. |
| `name` | `string` | **예** | 표시 이름. |
| `version` | `string` | **예** | [유의적 버전](https://semver.org/)을 사용하는 `x.y.z` 형식의 버전. |
| `authorUrl` | `string` | 아니요 | 저자 웹사이트 URL. |
| `fundingUrl` | `string` 또는 [`object`](#fundingurl) | 아니요 | 사용자가 프로젝트를 재정적으로 지원할 수 있는 URL 또는 여러 URL. |

## 플러그인 관련 속성

다음 속성은 플러그인에서만 사용할 수 있습니다.

| 속성 | 타입 | 필수 | 설명 |
| --- | --- | --- | --- |
| `description` | `string` | **예** | 플러그인 설명. |
| `id` | `string` | **예** | 플러그인 ID. |
| `isDesktopOnly` | `boolean` | **예** | 플러그인이 NodeJS 또는 Electron API를 사용하는지 여부. |

> [!note]
> 로컬 개발의 경우 `id`는 플러그인의 폴더 이름과 일치해야 합니다. 그렇지 않으면 `onExternalSettingsChange`와 같은 일부 메서드가 호출되지 않습니다.

## fundingUrl

`fundingUrl`은 단일 URL이 있는 문자열이거나 여러 URL이 있는 객체일 수 있습니다.

**단일 URL**:

```json
{
  "fundingUrl": "https://buymeacoffee.com"
}
```

**여러 URL**:

```json
{
  "fundingUrl": {
    "Buy Me a Coffee": "https://buymeacoffee.com",
    "GitHub Sponsor": "https://github.com/sponsors",
    "Patreon": "https://www.patreon.com/"
  }
}
