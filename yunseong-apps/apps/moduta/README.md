# Moduta (모두타)

## Basic Information

- **App Name**: 모두타 (dev 플레이버는 "모두타 DEV")
- **Version**: 1.0.0 (1)
- **Min SDK / Target SDK**: 29 / 36
- **Status**: 미출시 (개발 중)
- **Gradle 경로**: `:apps:moduta`
- **플레이버**: `dev` · `prod` (규약 플러그인이 생성)

## Description

서울시 버스·지하철 실시간 정보, 내 주변 정류장, 대기질을 하단 탭 다섯 개로 묶은 독립 앱이다. 서울 열린데이터광장 버스 API 를 도메인 계약으로 감싸 노선 검색과 실시간 위치를 제공하고, 카카오맵 위에 버스와 정류장을 그린다. 지하철은 번들 노선도 위에 실시간 열차 위치를 배치한다. 상세 구조는 [architecture.md](architecture.md) 를 본다.

## Features

- **버스 검색**: 노선 번호로 검색하고 즐겨찾기를 토글한다 (시작 탭)
- **지하철**: 호선 목록·즐겨찾는 역·역 검색, 세로 노선도, 역 도착정보 시트
- **내 주변**: 위치 권한과 GPS 를 거쳐 가까운 정류장과 즐겨찾는 정류장을 나열한다
- **대기질**: 에어코리아 측정소 대기질을 한 화면으로 보여 준다
- **더보기**: 막차 알림 관리, 광고 제거 구매·복원, 공지사항, 이용약관·처리방침, 광고 동의 변경, 문의, 오픈소스 라이선스
- **버스 지도**: 카카오맵 위에 노선 버스의 실시간 위치를 화면이 보이는 동안 주기적으로 갱신한다
- **정류장 상세**: 도착 안내와 경유 노선 카드, 정류장 막차 알림
- **막차 알림**: 역·정류장의 막차 출발 N분 전 로컬 알림 규칙을 저장하고 예약한다
- **홈 위젯**: 즐겨찾기 정류장·역 한 곳의 도착정보를 Glance 위젯으로 띄운다
- **스플래시**: 버전 확인 게이트. 확인에 실패하면 오프라인 새 실행을 막는다
- **광고**: AdMob 배너·네이티브·종료 팝업

## Dependencies

### 앱 전용 모듈
- `:apps:moduta:core:database` - SQLCipher 로 암호화한 Room DB (버스 캐시, 즐겨찾기, 막차 알림 규칙)
- `:apps:moduta:core:seoul-bus` - 서울 버스 API 도메인과 데이터 레이어
- `:apps:moduta:core:seoul-subway` - 지하철 실시간 위치·도착·막차 데이터, 번들 노선도
- `:apps:moduta:core:last-call` - 막차 알림 규칙 저장·발화 시각 계산·예약·발화 처리
- `:apps:moduta:core:widget` - 즐겨찾기 도착정보 홈 위젯과 갱신 워커
- `:apps:moduta:core:demo-seed` - 스토어 스크린샷 촬영용 데이터(debug 빌드 전용)
- `:apps:moduta:feature:bus-search` / `subway` / `nearby-station` / `air-quality` / `settings` - 5개 탭
- `:apps:moduta:feature:bus-map` - 지도 화면과 정류장 상세
- `:apps:moduta:feature:splash` - 버전 확인 게이트

### 공유 모듈
- 앱 셸·내비게이션·Paper 디자인 시스템: `app-shell`, `designsystem`, `widget-base`
- 데이터·기반: `network`, `database`(SQLCipher 팩토리), `favorites`, `location-provider`, `air-quality`, `fundamental:{common,alarm,local-notification}`
- 공용 순수 로직: `service-day`(자정이 아닌 시각에 바뀌는 운행일), `recurrence`(요일 반복 규칙)
- 권한: 공개 SDK `net.plumt.permission` + 연결 모듈 `shared:core:common:permission`
- 수익·운영: `admob`, `billing`, `remove-ads`, `notice`, `legal`, `webview`, `permission-notice`, `app-update`
- Firebase: `crashlytics`, `app-check`

### 외부 서비스
- Kakao Map SDK, 서울 열린데이터광장 버스·지하철 API, 에어코리아 대기질 API
- Firebase(Crashlytics, App Check), AdMob, Google Play 결제(광고 제거)

## 키 관리

- 비밀 설정값은 저장소에 두지 않는다. 빌드 때 암호화해 NDK `NativeConfig` 로 넣고 런타임에 JNI 로 복호화한다. `BuildConfig` 는 APK 에 평문으로 남아 키에 쓰지 않는다
- 버스·지하철·대기질 공공 API 키는 앱에 두지 않는다. 서버 프록시가 App Check 로 호출자를 확인한 뒤 키를 붙인다. 프록시는 별도 저장소에서 관리한다
- 카카오맵 키는 dev·prod 앱을 따로 등록했다. release 빌드는 필수 키가 비면 빌드 전 검증에서 실패하고, debug 는 경고만 낸다
- 서명 키는 dev 플레이버와 prod 플레이버가 다르며 환경변수나 로컬 설정으로 받는다

## Build Configuration

```bash
./gradlew :apps:moduta:assembleDevDebug
./gradlew :apps:moduta:assembleProdRelease
./gradlew :apps:moduta:installDevDebug
./gradlew :apps:moduta:test
```

플레이버·서명은 규약 플러그인이 만들므로 앱 모듈에서 다시 선언하지 않는다. release 는 R8 축소를 켠다.

## Testing

- 앱 모듈: 딥링크 host 매핑 일치, 경로 인자 인코딩, DI·셸 구성, 광고 셸 스크린샷
- 하위 모듈: 데이터 매핑·열차 배치·막차 시각·위젯 갱신 정책 등 단위 테스트, 공용 모듈로 바꾼 계산의 기존 구현 대비 패리티 테스트
- E2E: testTag 기반 콜드 스타트 스모크(Appium, dev 플레이버)

## Notes

- 카카오맵 SDK 는 Application 에서 초기화하고, SDK 가 매니페스트도 직접 읽어 같은 키를 매니페스트 placeholder 로도 넣는다. 두 경로가 같은 플레이버 값을 보게 유지한다
- 자동 백업은 꺼져 있다. 암호화 DB 는 복원해도 열리지 않는다
- 날씨 기능은 데이터 출처가 달라 범위에서 제외했다
