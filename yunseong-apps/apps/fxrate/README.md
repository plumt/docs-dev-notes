# FxRate (원레이트)

## Basic Information

- **App Name**: 원레이트 (dev 빌드는 "원레이트 DEV")
- **Version Code**: 1
- **Version Name**: 1.0.0
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: In Development (미출시)
- **Gradle 경로**: `:apps:fxrate`

## Description

통화별 환율을 보여주고 금액을 환산하며, 임계값 알림 규칙을 관리하고, 홈 화면 위젯으로 즐겨찾기 환율을 띄우는 앱이다. 화면과 위젯은 환율을 Room 캐시에서만 읽는다. 캐시는 서버가 만든 환율 문서(Firestore)를 환율 화면이 보이는 동안 구독해 채운다(`core:remote`). 구조는 [architecture.md](architecture.md), 기능별 상세는 [features.md](features.md) 를 본다.

## API Keys & Credentials

- 앱은 외부 환율 API 를 직접 부르지 않으므로 환율 API 키가 앱에 없다
- 앱이 쓰는 비밀 값은 광고 키와 문의 메일 정도다. 저장소 밖 설정 파일에 두고 빌드 때 NDK 네이티브 라이브러리로 넣는다

## Features

- **환율 탭**: 기준 통화 환산, 목록 전체 금액 환산, 즐겨찾기 순서 편집, 통화 상세 시트
- **알림 탭**: 임계값 알림 규칙 목록·추가·수정·삭제·켜고 끄기 (개수 상한 있음)
- **설정 탭**: 기준 통화 변경, 공지사항, 출처·약관·처리방침·라이선스·문의·앱 버전, 광고 제거
- **스플래시**: 강제 업데이트 게이트. 버전을 확인하지 못하면(오프라인 새 실행 포함) 들어가지 않는다
- **홈 화면 위젯**: 즐겨찾기 통화 환율을 런처에 표시 (Glance)

## Dependencies

### App-specific Modules
- `:apps:fxrate:core:common` - 환산·카탈로그·표기·소스 계약 (순수 JVM 로직)
- `:apps:fxrate:core:database` - Room 캐시, 즐겨찾기, 알림 규칙, 기준 통화 설정
- `:apps:fxrate:core:remote` - 서버 환율 문서 구독(화면이 보이는 동안만)
- `:apps:fxrate:core:widget` - Glance 위젯
- `:apps:fxrate:core:demo-seed` - 스크린샷 촬영용 데이터 (debug 전용)
- `:apps:fxrate:feature:rates` / `alerts` / `settings` / `splash`

### Shared Modules
- `app-shell`(앱 셸·딥링크), `app-update`(업데이트 게이트), `settings-ui`, `oss-licenses`, `notice`, `legal`, `webview`, `remove-ads`, `favorites`, `widget-base`, `backoff`(구독 재시도 간격), `demo-seed`, `designsystem`(Paper), `admob`, `billing`, `firebase:{firestore,crashlytics,remote-config,app-check}`, `fundamental:common`

### External Services
- Firebase(Firestore 환율 문서, Crashlytics, Remote Config, App Check), AdMob, Google Play 결제(광고 제거)
- 서버 코드는 별도 저장소에서 관리한다. 앱과 서버의 계약은 앱 저장소에 사본을 둔다

## Build Configuration

```bash
./gradlew :apps:fxrate:assembleDevDebug
./gradlew :apps:fxrate:assembleProdRelease
./gradlew :apps:fxrate:installDevDebug
```

### Flavors

`dev`, `prod`. 규약 플러그인의 allowlist 로 생성되며 서명은 플레이버 기준이다. 개발용 목 데이터와 촬영용 데이터는 debug 에만 있고 release 에는 코드가 없다.

## Testing

```bash
./gradlew :apps:fxrate:test
```

환산·서식·DAO 변동 기준·알림 편집 상태 같은 로직과 서버 문서 구독, 딥링크 라우터가 JVM 단위 테스트로, 환율·알림·설정 화면은 Robolectric UI 테스트로 검증된다.

## Notes

- 환율 문서 구독은 동작한다. 알림 규칙의 서버 반영과 푸시는 아직 계획 단계라 규칙은 로컬에만 쌓이고 "반영 대기" 로 표시된다
- 서버 중계가 환율 소스의 재배포 금지에 걸리는지는 2026-10 약관 확인으로 문제없다고 결론 냈다
