---
cssclasses: reference
---

Obsidian의 새 버전마다 플러그인을 위한 새로운 기능이 도입될 수 있습니다. 안타깝게도 플러그인에서 최근에 도입된 기능을 사용하면 아직 최신 버전의 Obsidian으로 업데이트하지 않은 사용자의 설치가 중단될 수 있습니다. 이를 방지하기 위해 `versions.json`을 사용하면 사용자 Obsidian 앱 버전에 따라 플러그인 버전을 제어할 수 있습니다.

`versions.json`에는 JSON 객체가 포함되어 있으며, 키는 플러그인 버전이고 값은 해당 `minAppVersion`입니다.

사용자가 [[Reference/Manifest|매니페스트]]의 `minAppVersion`보다 낮은 Obsidian 앱 버전으로 플러그인을 설치하려고 하면 Obsidian은 플러그인 저장소의 루트에서 `versions.json` 파일을 찾습니다.

다음 예에서 사용자는 Obsidian 1.1.0을 설치했지만 플러그인 `minAppVersion`은 1.2.0입니다.

**manifest.json**:

```json
{
  // ...

  "version": "1.0.0",
  "minAppVersion": "1.2.0"
}
```

사용자가 Obsidian 앱 버전 1.1.0을 실행하는 경우 Obsidian은 `versions.json`을 참조하여 대체 버전이 있는지 확인합니다.

**versions.json**:

```json
{
  "0.1.0": "1.0.0",
  "0.12.0": "1.1.0",
}
```

이 경우 1.1.0에 대한 최신 플러그인 버전은 0.12.0입니다.

> [!important]
> `versions.json`에 모든 플러그인 릴리스를 나열할 필요는 없습니다. 플러그인의 `minAppVersion`을 변경하는 경우에만 `versions.json`을 업데이트하면 됩니다.
