# yunseongApps

Android 앱 7개와 공용 모듈을 한 저장소에서 관리하는 모노레포의 공개 문서입니다.
소스 저장소는 비공개이며, 이 저장소는 구조·설계 결정·모듈 책임을 설명하는 문서만 담습니다.

## 한눈에 보기

- 앱 7개: Lottomate, Lunar, QRScanner, Diding, Moduta, Fxrate, AirTalk (+ 출시하지 않는 견본 앱 Inventory)
- Gradle 모듈 172개 (shared core 56, shared feature 28, 앱 전용 88)
- Clean Architecture + Modular Architecture, 의존 방향은 `apps → shared/feature → shared/core` 단방향
- 계층 규칙·화면 모듈 규격·앱 모듈 모양을 문서가 아니라 빌드(Convention Plugin의 Guard)가 강제
- 기술 스택: Kotlin 2.2.10, AGP 9.0.1, Compose BOM 2025.02.00, Hilt 2.56.2, Room 2.7.0-alpha12
- SDK: minSdk 29, targetSdk 36, compileSdk 36, Java 17

## 앱 현황

| 앱 | 한 줄 소개 | 패키지 | Flavor | 문서 |
|----|-----------|--------|--------|------|
| Lottomate (로픽) | 로또 번호 관리, 당첨 확인, 통계, 당첨 푸시 알림. [Google Play](https://play.google.com/store/apps/details?id=com.plumt.lottomate) 출시, 1.0.8 | `com.plumt.lottomate` | dev / prod | [apps/lottomate](apps/lottomate/README.md) |
| Lunar (루나) | 달 위상 대시보드와 달력, 달 이벤트 알림. [Google Play](https://play.google.com/store/apps/details?id=com.plumt.lunar) 출시, 1.3.10 | `com.plumt.lunar` | dev / prod | [apps/lunar](apps/lunar/README.md) |
| QRScanner (큐링) | QR 다중 스캔, 생성, 이력. Play 내부 테스트, 1.0.1 | `com.plumt.qrscanner` | dev / prod | [apps/qrscanner](apps/qrscanner/README.md) |
| Diding (던잇) | 알람 기반 TODO·일정 관리. 미출시 | - | dev / prod | [apps/diding](apps/diding/README.md) |
| Moduta (모두타) | 서울시 버스·지하철 실시간 정보, 막차 알림, 지도. 미출시 | - | dev / prod | [apps/moduta](apps/moduta/README.md) |
| Fxrate (원레이트) | 환율 조회와 임계값 알림, 홈 위젯. 미출시 | - | dev / prod | [apps/fxrate](apps/fxrate/README.md) |
| AirTalk (에어톡) | 종단간 암호화 채팅과 게시판. 미출시 | - | dev / prod | [apps/airtalk](apps/airtalk/README.md) |
| Inventory (견본) | 아직 채택 앱이 없는 공용 모듈을 한 앱에 엮어 컴파일·DI 그래프를 검증. 출시하지 않음 | - | dev / prod | [apps/inventory](apps/inventory/README.md) |

모든 앱이 dev/prod 환경 flavor를 갖습니다(debug·release 빌드 타입과 조합해 4개 변형이며, dev 용 release 변형은 만들지 않습니다). 앱 모듈에는 조립 코드(Application, MainActivity, 내비게이션, DI 바인딩)만 두고,
기능은 `apps/<앱>/core`, `apps/<앱>/feature` 하위 모듈에 둡니다.

## 모듈 구성

```
yunseongApps/
├── apps/                  앱 8개 (각 앱은 조립 모듈 + core/feature 하위 모듈)
├── shared/
│   ├── core/              공통 인프라 (fundamental, common, firebase)
│   └── feature/           여러 앱이 공유하는 기능 (lock, social-login, notification-*, notice 등)
├── build-logic/           Convention Plugin, Guard, NDK 설정 생성기 (별도 Gradle 빌드)
└── scripts/               스캐폴딩, 문서·표준 검사, 출시 원장, 테스트 집계, Appium E2E, 배포 스크립트
```

| 계층 | 모듈 수 | 역할 |
|------|--------|------|
| Shared Core - Fundamental | 4 | 필수 인프라 (common, local-notification, alarm, presentation-base) |
| Shared Core - Common | 42 | network, database, ui, designsystem, admob, billing, deeplink, navigation-base, app-shell, crypto-e2ee, recurrence, backoff 등 |
| Shared Core - Firebase | 10 | core, auth, app-check, crashlytics, database, firestore, functions, messaging, remote-config, storage |
| (달 위상 계산) | 0 | 공개 SDK 로 이전 중 — [sdk-kotlin-astro](https://github.com/plumt/sdk-kotlin-astro) 를 의존 |
| Shared Feature | 28 | 2개 이상 앱이 쓰는 기능 (api/impl/ui 분리 포함) |
| 앱 전용 | 앱별 4~18 (조립 모듈 포함) | Lottomate 18, Moduta 14, AirTalk 13, Lunar 10, Diding 10, Fxrate 10, QRScanner 9, Inventory 4 |

shared 모듈의 상세는 [shared/core](shared/core.md), [shared/feature](shared/feature.md)를 참고하세요.

## 핵심 설계 요약

- 의존 방향 규칙을 빌드가 막음: 역참조, 앱 간 참조, 계층 역전, feature 간 교차 참조는 configure 단계에서 실패
- 공용 모듈이 앱 전용 구현을 필요로 하면 포트 인터페이스를 두고 각 앱이 구현·바인딩
- Hilt 멀티바인딩(`@Multibinds` + `@IntoSet`)으로 소셜 로그인·화면 목적지·FCM 핸들러를 의존 추가만으로 등록
- 화면 모듈은 `convention.feature` 하나로 구성하고 `StateViewModel`/`MviViewModel` 골격 사용
- API 키 등 런타임 값은 BuildConfig 대신 NDK(XOR + JNI)로 숨김
- 전 앱이 Firebase App Check 를 붙이고, 서버 호출에는 App Check 토큰을 헤더로 실어 보냅니다(debug 는 디버그 공급자, 운영은 Play Integrity)
- 범용 기능(음력·천문, 권한, QR, 광고, 소셜 로그인, 시간 등)은 공개 SDK(`sdk-kotlin-*`, Maven Central `net.plumt.*`)로 옮기는 중이며, 해당 shared 모듈 문서는 [shared/core](shared/core.md)·[shared/feature](shared/feature.md) 에 이전 표기만 둡니다
- 빌드 전 검증으로 "빌드는 성공하지만 출시 후에야 드러나는" 오류(빈 키, 테스트 광고 ID, 딥링크 스킴 불일치, 서명 누락)를 차단

자세한 내용은 [ARCHITECTURE.md](ARCHITECTURE.md)에 있습니다.

## 문서 목록

| 문서 | 내용 |
|------|------|
| [ARCHITECTURE.md](ARCHITECTURE.md) | 계층 구조, 의존성 규칙, 주요 아키텍처 패턴, 보안 구조 |
| [docs/build-and-tooling.md](docs/build-and-tooling.md) | Convention Plugin, Guard, 빌드 커맨드, 테스트 인프라, 문서 계약과 동기화 방식 |
| [docs/backend-and-web.md](docs/backend-and-web.md) | 별도 저장소로 분리한 서버·웹과 앱의 계약 |
| [shared/core](shared/core.md) | Shared Core 모듈 문서 |
| [shared/feature](shared/feature.md) | Shared Feature 모듈 문서 |
| `apps/<앱>/README.md` | 앱별 구조와 설계 (위 앱 현황 표의 링크) |

## 스크린샷

스크린샷은 `screenshots/` 폴더에 앱별로 정리합니다.

## 백엔드와 웹

Firebase 서버 코드와 웹은 2026-10 에 별도 저장소로 분리했습니다. 앱과 서버의 계약을 어떻게 맞추는지는 [docs/backend-and-web.md](docs/backend-and-web.md)를 참고하세요.

## 이 저장소에 대하여

소스 저장소의 `AGENTS.md`와 `docs/`를 기준으로 구조 문서를 추려 공개용으로 정리한 것입니다.
키, 서명 정보, 내부 주소 같은 민감 값은 싣지 않습니다.
