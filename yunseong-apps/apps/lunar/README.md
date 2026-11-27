# Lunar (루나)

달의 위상을 보여 주는 앱이다. Google Play 에 [출시](https://play.google.com/store/apps/details?id=com.plumt.lunar)했다. 위상·음력 일자·조도·다가오는 삭망은 서버 없이 Meeus 천문 알고리즘으로 기기에서 계산한다. Jetpack Compose 와 멀티모듈 구조(앱 조립 모듈 + 화면별 feature 모듈)로 만들었다.

상세 문서: [architecture.md](architecture.md) (구조·설계 결정) · [features.md](features.md) (화면·기능별 동작)

## Basic Information

- **App Name**: 루나 (dev 플레이버는 "루나 DEV")
- **Package ID**: `com.plumt.lunar`
- **Version**: 1.3.10 (15)
- **Min SDK / Target SDK**: 29 / 36
- **Status**: Released (Google Play 프로덕션, 2026-11-24)
- **Gradle 경로**: `:apps:lunar`
- **플레이버**: `dev` · `prod` (규약 플러그인이 생성)

## Description

앱은 하단 탭 다섯 개로 구성된다. 오늘의 달 대시보드(홈), 월별 달력, 그 해의 슈퍼문·미니문·블루문을 모은 달 탐구, 한국천문연구원(KASI) 천문현상 해설을 보여 주는 이달의 하늘, 설정이다. 앱 모듈은 진입점·내비게이션·전역 DI 만 갖고 화면은 전부 feature 모듈이 소유한다.

달 위상 표시는 2026-09 말부터 공공데이터 API 대신 Meeus 계산 하나로 일원화했다. 외부 API 는 하루 한 번 월령만 제공해 삭망을 보간하면 날짜가 최대 1일, 시각이 최대 3시간 어긋났기 때문이다. Meeus 계산은 2~3분 오차로 끝난다.

## Features

- **홈**: 오늘의 달 위상(Canvas 로 그린 달), 조도, 음력 일자, 다가오는 위상 타임라인, 보름달까지 카운트다운, 월출·월몰과 달 보기 날씨
- **달력**: 월별 격자/목록 탭, 날짜별 위상·삭망 시각·블루문 표시
- **달 탐구**: 그 해의 블루문·슈퍼문·미니문을 로컬 계산으로 시간순 나열
- **이달의 하늘**: KASI 월별 해설과 일정, 일정을 누르면 위키백과 용어 설명
- **설정**: 달 알림 토글 2종(달의 위상 / 특별한 달), 광고 제거 구매·복원, 공지사항, 이용약관·개인정보 처리방침, 광고 동의 변경, 문의, 오픈소스 라이선스
- **알림**: FCM data 메시지를 로컬 알림으로 변환(새 달·보름달·슈퍼문·미니문·블루문), 알림함, 첫 실행 알림 구독 안내
- **스플래시와 강제 업데이트**: 페이드인 뒤 버전 확인. 확인에 실패하면 연결 안내와 다시 시도에서 멈춘다(오프라인 새 실행 차단)
- **광고**: AdMob 배너(바텀바와 함께 표시), 달력·하늘 목록 광고, 종료 팝업 광고
- **접근권한 안내와 딥링크**: 첫 실행 접근권한 사전 고지, 탭과 알림함으로 가는 딥링크

## Dependencies

### 앱 전용 모듈
- `:apps:lunar:core:common` - 달 위상 그림, 포맷, 모델, 알림 설정(DataStore), 테마
- `:apps:lunar:core:demo-seed` - 스토어 스크린샷 촬영용 데이터(debug 빌드 전용)
- `:apps:lunar:feature:home` / `calendar` / `explore` / `sky` / `settings` - 탭 화면
- `:apps:lunar:feature:splash` - 시작 화면(앱 NavGraph 가 직접 연결)
- `:apps:lunar:feature:notification` - 달 이벤트 FCM 핸들러와 알림함 화면

### 공유 모듈
- 달 계산(위상 이벤트, 월령, 블루문, 슈퍼문)은 공개 SDK [sdk-kotlin-astro](https://github.com/plumt/sdk-kotlin-astro)(`net.plumt.astro:lunar`)를 쓴다
- 권한 요청은 공개 SDK `net.plumt.permission` 과 연결 모듈 `:shared:core:common:permission` 을 쓴다
- `:shared:core:common:app-shell` - 앱 셸(Scaffold, 바텀바, 딥링크 호스트 Activity)
- `:shared:core:common:admob`, `billing` - 광고(공개 SDK `net.plumt.ads` 위 어댑터)와 광고 제거 결제
- `:shared:core:common:astro-event`, `weather-kma`, `wikipedia`, `moon-viewing`, `location-provider` - 천문현상·날씨·용어 설명·달 보기 판단·현재 위치
- `:shared:core:firebase:messaging`, `crashlytics`, `app-check` - FCM 라우터, 크래시 리포터, 프록시 호출 검증
- `:shared:feature:app-update`, `notice`, `legal`, `webview`, `permission-notice`, `remove-ads`, `settings-ui`, `notification-settings`, `oss-licenses`, `notification-inbox:*`

### 주요 라이브러리
Hilt, Jetpack Compose + Material 3, Kotlin Coroutines/Flow, Joda-Time, DataStore Preferences, Firebase Cloud Messaging / Crashlytics / App Check

### 외부 서비스
- Firebase(FCM, Crashlytics, App Check): 이 앱 전용 프로젝트를 쓰며 다른 앱 설정과 섞지 않는다
- AdMob, Google Play 결제, KASI 천문현상, 기상청 단기예보, 위키백과
- KASI·기상청은 앱이 직접 부르지 않고 서버 프록시를 거친다. 프록시는 별도 저장소에서 관리한다

## 설정값과 키 관리

- 비밀 설정값은 소스에 적지 않는다. 빌드 때 암호화해 NDK 네이티브 라이브러리에 넣고 런타임에 JNI 로 복호화한다
- 공공데이터 API 키는 앱에 두지 않는다. 서버 프록시가 App Check 로 호출자를 확인한 뒤 키를 붙인다
- 광고 ID 는 빌드 스크립트·소스에 리터럴로 두지 않는다. prodRelease 에 테스트 광고 ID 가 남아 있으면 빌드 전 검증이 막는다
- 릴리스 서명 정보는 환경변수나 로컬 설정에서 읽고, 서명 키는 buildType 이 아니라 플레이버로 가른다

## Build Configuration

```bash
./gradlew :apps:lunar:assembleDevDebug      # 디버그 (dev)
./gradlew :apps:lunar:assembleProdRelease   # 릴리스 (prod)
./gradlew :apps:lunar:installDevDebug
```

앱 모듈 단위 테스트(DI·내비게이션·화면 사이 위상 이름 일치·셸 스크린샷), 화면 모듈별 ViewModel·스크린샷 테스트, Appium E2E 스모크(dev 플레이버, 콜드 스타트)로 검증한다. `androidTest` 는 없다.

## Release Notes

### v1.3.10 (Current)
- 위상 사건(삭·상현·망·하현) 시각에 서버 푸시와 같은 ΔT 보정을 적용해 달력·홈·하늘이 같은 값을 표기
- 예보 요청에 시간 상한을 두고, 실패하면 같은 높이의 실패 카드와 다시 시도 버튼 표시
- 설정 알림 스위치를 행 단위로 읽게 정리, 스플래시 달 접근성 이름을 오늘 위상 기준으로 고정
- 달력·하늘·홈 날씨 카드·달 탐구 카드 높이를 시안 값에 고정

### v1.3.9
- 본문 글꼴을 Pretendard, 숫자를 Roboto Mono 로 변경
- 글자 배율 2.0 에서 겹침·잘림 수정
- 알림 권한 요청을 권한 안내가 닫힌 뒤 한 번만 실행

### v1.3.4 ~ v1.3.5
- 같은 달에 두 번 드는 위상이 홈·달력에서 빠지던 문제 수정
- 위치를 대략 위치로 낮추고 마지막 위치 대체 규칙 추가
- 알림함 빈 상태, 업데이트 팝업 문구, 홈 위치 칩 정렬 수정

### v1.3.3
- 오프라인 새 실행 차단(스플래시 확인 실패 시 차단, 온라인 재시도 시 통과)
- 위치·알림 권한 요청을 권한 SDK 로 이전
- 달 이벤트 조회 자동 재시도(일시적 오류만)

### v1.3.2
- 달 위상 표시를 Meeus 계산 하나로 일원화, 공공데이터 API 경로 제거
- 이달의 하늘 탭, 홈 월출·월몰과 달 보기 날씨, 첫 실행 알림 구독과 알림함 추가

## Notes

- 앱 이름은 `src/dev`·`src/prod` 의 문자열 리소스로 갈린다
- 테마는 밤하늘(Midnight) 팔레트 고정이며 시스템 라이트/다크 설정을 따르지 않는다. 카드·점선·버튼 모양은 공용 Paper 디자인 시스템을 쓴다
- 자동 백업은 꺼져 있어 새 기기에서는 알림 설정이 기본값(꺼짐)으로 시작한다
- 릴리스는 R8 을 켠다
