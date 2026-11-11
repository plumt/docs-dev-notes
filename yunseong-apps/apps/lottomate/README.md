# Lottomate

## Basic Information

- **App Name**: Lottomate
- **Package ID**: `com.yunseong.lottomate` (dev 플레이버는 `.dev` 접미사)
- **Version Code**: 1
- **Version Name**: 1.0.0
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: Active Development
- **딥링크 스킴**: `yunseong-lottomate`

상세 설계는 [architecture.md](architecture.md), 모듈별 책임은 [modules.md](modules.md) 를 본다.

## Description

Lottomate 는 Jetpack Compose 로 만든 로또 관리 앱이다. 당첨번호 조회·동기화, 내 번호와 즐겨찾기 관리, 통계, 당첨 확인, 추첨 카운트다운, 번호 불러오기(카메라·사진에서 티켓 QR 과 구매내역 자동 판별), FCM 푸시 알림, 공지사항을 제공한다. 모든 화면은 종이 티켓 느낌의 디자인(Paper 컴포넌트)으로 통일돼 있다.

앱 모듈은 조립만 한다. 소스는 Application, MainActivity, `di/` 3파일, `navigation/` 3파일이 전부이고 화면과 도메인은 `core`·`feature` 하위 모듈에 있다.

## API Keys & Credentials

API 키와 URL 은 `clients/lottomate/config.json`(`common`·`dev`·`prod` 세 섹션)에서 읽어 빌드 때 XOR 암호화 NDK 바이너리에 넣고, 런타임에 `NativeConfig` JNI 클래스로 복호화한다. 환경에 따라 갈리는 키는 AdMob 앱 id 하나뿐이다. 값은 저장소에 두지 않으며 키 이름만 적는다.

| Key Name | Purpose |
|----------|---------|
| `admob_app_id` | AdMob 앱 ID (dev/prod 분기) |
| `admob_banner_id` | 배너 광고 단위 |
| `admob_native_id` | 네이티브 광고 단위 |
| `lotto_official_intro_url` / `lotto_official_main_url` | 공식 안내·메인 페이지 |
| `app_privacy_policy_url` / `app_terms_of_service_url` | 개인정보 처리방침·이용약관 |
| `fcm_topic_lotto_check` / `fcm_topic_purchase_reminder` | FCM 토픽 이름 |

서명용 키스토어 정보는 코드와 별도로 로컬 환경에서만 관리한다.

## Features

- **당첨번호**: 최신·회차별 당첨번호와 전 회차(묶음 문서 하나)를 모두 Firestore 에서 받아 로컬 DB 에 동기화. 외부 로또 사이트는 앱이 직접 부르지 않는다
- **내 번호**: 번호 저장·삭제·즐겨찾기, 이번주 구매/달력/즐겨찾기 3탭, 수동 입력 시트
- **통계**: 번호 빈도, 당첨금 기록, 월별 추이, 번호별 성과 (Vico 차트)
- **당첨 확인**: 저장 번호를 당첨번호와 대조해 등수 판정
- **카운트다운**: 추첨(토요일 20:35)까지 남은 시간과 추첨 중/결과 발표 상태
- **번호 불러오기**: 카메라나 사진에서 로또 QR 을 읽고, QR 이 아니면 구매내역 글자를 인식해 번호를 저장한다. 저장 전에 회차를 검증한다
- **FCM 알림**: 당첨 결과(토요일 20:55 KST), 구매 알림(토요일 18:00 KST). 알림 설정에서 토픽 구독을 토글
- **알림함**: 앱 내 알림 이력. 항목의 딥링크로 화면 이동
- **강제 업데이트**: Remote Config 기반 버전 확인 게이트
- **광고**: AdMob 배너·네이티브, 설정에서 "광고 제거" 구매·복원(판매 여부는 Remote Config 스위치로 제어)
- **공지사항**: 공지 목록과 N 배지, 정책 변경 팝업

## 화면 구성

스플래시(시작) -> 홈 · 내 번호 · 통계 · 더보기 4개 탭. 알림함, 개인정보 처리방침, 이용약관은 바텀바를 끄는 보조 목적지다.

## Dependencies

앱 모듈이 직접 의존하는 것을 분류한다. 자세한 책임은 [modules.md](modules.md) 에 있다.

### App-specific Modules
- core: `common`, `countdown`, `my-numbers`, `statistics`, `winning-numbers`, `winning-checker`, `receipt-import` (`database` 는 이들이 사용)
- feature: `splash`, `home`, `my-numbers-overview`, `my-numbers`, `statistics`, `more`, `messaging`, `notifications`

### Shared Modules
- feature: `qr:multi-scanner`, `chart`, `notification-inbox`(impl/ui), `notification-settings`, `app-update`, `notice`, `permission-notice`, `remove-ads`, `oss-licenses`, `settings-ui`, `webview`
- core: `common:app-shell`, `common:time-sync`, `common:admob`, `common:billing`, `fundamental:common`, `fundamental:permission`, `permissions:permission-notification`, `firebase:{firestore,crashlytics,remote-config,messaging,app-check}`

### External Services
- **Firebase**: Crashlytics, Cloud Messaging, Remote Config, Firestore, App Check (당첨번호는 Cloud Functions 가 DB 에 기록)
- **AdMob**: 배너·네이티브 광고
- **Google Play 결제**: 광고 제거 1회 상품

## Build Configuration

플레이버(`dev`, `prod`)·서명·빌드 타입은 Convention Plugin(`convention.android.application`)이 만들며 앱 모듈은 값만 더한다. dev 용 release 변형(devRelease)은 만들지 않는다.

| 변형 | applicationId | 서명 | 축소 |
|---|---|---|---|
| devDebug | `com.yunseong.lottomate.dev` | dev 키 | 끔 |
| prodDebug | `com.yunseong.lottomate` | 운영 키 | 끔 |
| prodRelease | `com.yunseong.lottomate` | 운영 키 | R8 |

```bash
./gradlew :apps:lottomate:assembleDevDebug     # dev 디버그
./gradlew :apps:lottomate:assembleProdRelease  # 운영 릴리스
./gradlew :apps:lottomate:installDevDebug      # 기기 설치
```

릴리스 빌드 전에는 `preBuildVerify` 가 필수 설정 키(약관 URL 2개 등), 운영 광고 키에 구글 테스트 id 가 섞였는지, 딥링크 스킴과 매니페스트의 일치를 검사한다.

## Testing

```bash
./gradlew :apps:lottomate:test                 # 유닛 테스트
./gradlew :apps:lottomate:connectedAndroidTest # 계측 테스트
```

E2E 는 testTag 기반 Appium 하네스(콜드 스타트 스모크 시나리오)를 쓴다. 실행 엔진은 저장소 공용 러너이고 앱은 설정·시나리오·실행 래퍼만 가진다. dev 플레이버 고정이다.

## Architecture 요약

- **UI**: Jetpack Compose + Material 3, 라이트 고정 브랜드 테마(밝은 종이 + 빨강 포인트)
- **DI**: Hilt
- **Navigation**: Navigation Compose, 목적지가 바텀바 표시를 직접 켜고 끈다
- **패턴**: Clean Architecture + MVVM/MVI(`onIntent`), 계층 분리는 core(data/domain)와 feature(UI)로 나눈다
- **Security**: NDK/JNI XOR 암호화 설정, SQLCipher 암호화 Room DB, 릴리스에서 `Log` 호출 제거
- **Build System**: Convention Plugins + Version Catalog

자세한 내용은 [architecture.md](architecture.md).

## Notes

- Firebase 설정 파일은 플레이버별 소스셋(`src/dev`, `src/prod`)에 두며 저장소에 공개하지 않는다
- API 키와 URL 은 `clients/lottomate/config.json` 으로만 관리하고 소스 리터럴로 박지 않는다
- 앱 이름은 플레이버 소스셋의 리소스에만 있다

## 스크린샷

스크린샷은 [screenshots](../../screenshots/lottomate/) 폴더를 참고한다.
