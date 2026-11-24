# Lunar (루나)

달의 위상을 보여 주는 앱이다. Google Play 에 [출시](https://play.google.com/store/apps/details?id=com.plumt.lunar)했다. 위상·음력 일자·조도·다가오는 삭망 같은 값을 서버 API 없이 Meeus 천문 알고리즘으로 기기에서 직접 계산한다. Jetpack Compose 와 멀티모듈 구조(앱 조립 모듈 + 화면별 feature 모듈)로 만들었다.

상세 문서: [architecture.md](architecture.md) (구조·설계 결정) · [features.md](features.md) (화면·기능별 동작)

## Basic Information

- **App Name**: 루나 (dev 플레이버는 DEV 표시)
- **Package ID**: `com.plumt.lunar` (dev 플레이버는 `.dev` 접미사, 코드 namespace 는 `com.yun.lunarphase`)
- **Version Code**: 15
- **Version Name**: 1.3.10
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: Released (Google Play 프로덕션, 2026-11-24)
- **Google Play**: [루나](https://play.google.com/store/apps/details?id=com.plumt.lunar)
- **Gradle 경로**: `:apps:lunar`
- **플레이버**: `dev` · `prod` (규약 플러그인의 allowlist 가 생성)
- **딥링크 스킴**: `yunseong-lunar`

## Description

앱은 하단 탭 다섯 개로 구성된다. 오늘의 달 대시보드(홈), 월별 달력, 그 해의 슈퍼문·미니문·블루문을 모은 달 탐구, KASI 천문현상 해설을 보여 주는 이달의 하늘, 알림 설정이다. 앱 모듈은 진입점·내비게이션·전역 DI 만 갖고 화면은 전부 feature 모듈이 소유한다.

달 위상 표시는 2026-09 말부터 공공데이터 API 대신 Meeus 계산 하나로 일원화했다. 외부 API 는 하루 한 번 월령만 제공해 삭망을 보간하면 날짜가 최대 1일, 시각이 최대 3시간 어긋났기 때문이다. Meeus 계산은 기기에서 2~3분 오차 수준으로 끝난다.

## Features

- **홈**: 오늘의 달 위상(Canvas 로 그린 달 이미지), 조도, 음력 일자, 다가오는 위상 타임라인, 보름달/신월까지 카운트다운, 월출·월몰과 달 보기 날씨
- **달력**: 월별 격자/목록 탭, 날짜별 위상·삭망 시각·블루문 표시
- **달 탐구**: 그 해의 블루문·슈퍼문·미니문을 로컬 계산으로 시간순 나열
- **이달의 하늘**: KASI 월별 해설과 일정, 일정을 누르면 위키백과 용어 설명 펼침
- **설정**: 달 알림 토글 2종(달의 위상 / 특별한 달), 앱 버전, 개인정보 처리방침, 문의, 오픈소스 라이선스
- **알림**: FCM data 메시지를 로컬 알림으로 변환(새달·보름달·슈퍼문·미니문·블루문), 알림함, 첫 실행 시 알림 구독 안내
- **스플래시와 강제 업데이트**: 페이드인 뒤 버전 확인, 조회 실패 시 연결 실패 안내와 다시 시도
- **광고**: AdMob 배너(바텀바와 함께 표시)
- **딥링크**: `yunseong-lunar://home | calendar | explore | settings | notifications` 형태(경로 없는 host 만 허용)

## Dependencies

### 앱 전용 모듈
- `:apps:lunar:core:common` - 달 위상 그림, 포맷, 모델, 알림 설정(DataStore), 테마
- `:apps:lunar:feature:home` / `calendar` / `explore` / `sky` / `settings` - 탭 화면
- `:apps:lunar:feature:splash` - 시작 화면(앱 NavGraph 가 직접 연결)
- `:apps:lunar:feature:notification` - 달 이벤트 FCM 핸들러(조립 전용)

### 공유 모듈
- `:shared:core:common:app-shell` - 앱 셸(Scaffold, 바텀바, 딥링크 호스트 Activity)
- 달 계산(위상 이벤트, 월령, 블루문, 슈퍼문)은 공개 SDK [sdk-kotlin-astro](https://github.com/plumt/sdk-kotlin-astro)(`net.plumt.astro:lunar`)를 의존한다. 이 저장소의 `shared:core:lunar:calculator` 모듈은 SDK 로 이전하며 없어졌다
- `:shared:core:common:admob` - 배너 광고(공개 SDK 로 이전 중)
- `:shared:core:common:astro-event`, `weather-kma`, `wikipedia`, `moon-viewing`, `location-provider` - 천문현상·날씨·용어 설명·달 보기 판단·현재 위치
- `:shared:core:firebase:messaging`, `crashlytics` - FCM 라우터, 크래시 리포터
- `:shared:feature:app-update`, `oss-licenses`, `settings-ui`, `notification-settings`, `notification-inbox:*`
- `:shared:core:fundamental:*` (common, permission, local-notification), `:shared:core:permissions:*`

### 주요 라이브러리
Hilt(DI), Jetpack Compose + Material 3, Kotlin Coroutines/Flow, Joda-Time, DataStore Preferences(알림 설정), Firebase Cloud Messaging / Crashlytics

### 외부 서비스
- Firebase(FCM, Crashlytics): 이 앱 전용 Firebase 프로젝트를 쓰며 다른 앱의 `google-services.json` 과 섞지 않는다
- AdMob, KASI(한국천문연구원) 천문현상, 기상청, 위키백과

## 설정값과 키 관리

설정값은 소스에 적지 않고 클라이언트별 설정 파일에서 읽는다. 값은 이 문서에 두지 않는다.

- 설정 JSON 의 값은 빌드 때 XOR 암호화되어 NDK 네이티브 라이브러리로 만들어지고, 런타임에 JNI 호출(`NativeConfig`)로 복호화한다
- 광고 단위 ID 는 `IS_PROD` 로 갈린다. prod 만 실 배너 ID 를 쓰고 dev 는 테스트 ID 를 쓴다
- AdMob App ID 는 매니페스트 플레이스홀더로 flavor 별 주입한다
- `preBuildVerify` 가 prodRelease 에서 구글 테스트 퍼블리셔 ID 가 남아 있으면 빌드를 막는다
- 릴리스 서명 정보는 환경변수 또는 로컬 설정 파일에서 읽고, 서명 키는 buildType 이 아니라 flavor 로 가른다

## Build Configuration

```bash
# Debug build (dev 플레이버)
./gradlew :apps:lunar:assembleDevDebug

# Release build (prod 플레이버)
./gradlew :apps:lunar:assembleProdRelease

# Install on device
./gradlew :apps:lunar:installDevDebug
```

앱 모듈에는 `src/test`·`src/androidTest` 가 없다. 화면 모듈별 단위 테스트와 Appium E2E 스모크 하네스(dev 플레이버 고정, 콜드 스타트 시나리오)로 검증한다.

## Release Notes

### v1.3.10 (Current)
- 위상 사건(삭·상현·망·하현) 시각에 서버 푸시와 같은 ΔT 보정을 적용해 달력·홈·하늘이 같은 값을 표기
- 예보 요청에 8초 상한을 두고, 실패하면 같은 높이의 실패 카드와 다시 시도 버튼 표시
- 설정 알림 스위치를 행 단위 한 덩어리로 정리, 스플래시 달 접근성 이름을 오늘 위상 기준으로 고정
- 달력·하늘·홈 날씨 카드·달 탐구 카드 높이를 시안 값에 고정

### v1.3.9
- 본문 글꼴을 Pretendard, 숫자를 Roboto Mono 로 변경
- 글자 배율 2.0 에서 겹침·잘림 수정, 홈 날씨 카드 높이 고정
- 알림 권한 요청을 권한 안내가 닫힌 뒤 한 번만 실행

### v1.3.4 ~ v1.3.5
- 같은 달에 두 번 드는 위상이 홈·달력에서 빠지던 문제 수정
- 위치를 대략 위치로 낮추고 마지막 위치 대체 규칙 추가
- 알림함 빈 상태, 업데이트 팝업 문구, 홈 위치 칩 정렬 수정

### v1.3.3
- 오프라인 새 실행 차단(스플래시 확인 실패 시 차단, 온라인 재시도 시 통과)
- 위치·알림 권한 요청을 새 권한 SDK 로 이전
- 달 이벤트 조회 자동 재시도(연결 실패·시간 초과·5xx 만 최대 2회)

### v1.3.2
- 달 위상 표시를 Meeus 계산 하나로 일원화, 공공데이터 API 경로 제거
- 이달의 하늘 탭(KASI 해설·일정, 위키백과 용어 설명) 추가
- 홈에 월출·월몰과 달 보기 날씨 추가
- 첫 실행 달 알림 구독과 알림함 추가
- 달력 적재 실패를 화면에 알리도록 수정

## Notes

- 앱 이름은 `src/dev`·`src/prod` 의 문자열 리소스로 갈린다. `src/main` 에는 `app_name` 을 두지 않는다
- 테마는 밤하늘(Midnight) 팔레트 고정이며 시스템 라이트/다크 설정을 따르지 않는다
- 자동 백업은 꺼져 있어 새 기기에서는 알림 설정이 기본값(꺼짐)으로 시작한다
- ProGuard/R8 은 릴리스에서 켜져 있고, JNI 보존 규칙은 규약 플러그인이 생성한다
