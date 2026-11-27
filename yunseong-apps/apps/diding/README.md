# Diding

반복 규칙이 있는 할 일을 관리하고 정해진 시각에 알림을 울려 주는 앱이다. 할 일 데이터는 서버 없이 기기 안에만 저장된다. Jetpack Compose 와 멀티모듈 구조를 쓰고, 앱 안에 순수 도메인 모듈(`core:domain`)을 따로 둔다.

상세 문서: [architecture.md](architecture.md) (구조·설계 결정) · [features.md](features.md) (화면·기능별 동작)

## Basic Information

- **App Name**: 던잇 (DONE!T, 이전 이름 디딩). 코드·모듈 이름은 `diding` 을 유지한다
- **Version**: 1.0.0 (1)
- **Min SDK / Target SDK**: 29 / 36
- **Status**: 미출시 (개발 중)
- **Gradle 경로**: `:apps:diding`
- **플레이버**: `dev` · `prod` (규약 플러그인 allowlist 가 생성)

## Description

시작 화면(버전 확인) 뒤에 하단 탭 네 개(오늘, 관리, 캘린더, 설정)로 이루어진다. 오늘 해당하는 할 일을 체크하고, 반복(매일/매주/매월) 할 일을 추가·수정·보관하며, 월 달력에서 날짜별 수행률을 색으로 확인하고, 할 일별 알림 시각에 `AlarmManager` 정확 알람으로 알림을 받는다.

## Features

- **오늘**: 오늘 해당하는 할 일 목록과 진행 카드, 완료 토글. 자정마다 목록이 갱신된다
- **관리**: 반복 할 일 추가·수정·보관(바텀시트), 매일/매주(요일 선택)/매월(일자 선택) 반복, 할 일별 알림 시각
- **캘린더**: 월별 달력에 날짜마다 할 일 수행률을 색으로 표시
- **설정**: 알림 마스터 스위치, 정확 알람 권한 안내, 앱 버전, 공지사항, 광고 제거, 문의, 처리방침·약관, 오픈소스 라이선스
- **알람**: 정확 알람 예약, 발화 후 다음 알람 재예약, 재부팅·앱 교체 뒤 자동 복구
- **딥링크**: 탭 네 개로 바로 이동

## 외부 서비스

할 일 기능은 외부 API 에 의존하지 않는다(그래서 선행 조건 없이 dev/prod 플레이버 체계에 세 번째로 합류했다). 공용 서비스로 Firebase(Crashlytics, Remote Config, App Check), AdMob 광고(배너·관리 목록 네이티브·종료 팝업), Google Play 결제(광고 제거)를 붙였다. 문의 메일 주소 같은 설정 값은 NDK 로 암호화해 빌드에 넣는다.

## Dependencies

### 앱 전용 모듈
- `core:domain` - 할 일·완료 모델, 저장소 계약, 반복 판정, 달성률, 유스케이스 (Room·Hilt·Compose 를 모르는 순수 계층)
- `core:database` - 암호화 Room DB, 저장소 구현, 알람 스케줄러 포트, 알림 마스터 스위치
- `core:alarm` - 정확 알람 스케줄러 구현과 발화·재예약 리시버
- `core:demo-seed` - 스크린샷 촬영용 데이터 시드 (debug 전용)
- `feature:splash` - 버전 확인 관문
- `feature:home` / `management` / `calendar` / `setting` - 탭 화면

### 공유 모듈
- `shared:core:common:app-shell` - 셸 호스트·딥링크 배선·바텀바
- `shared:core:common:recurrence` - 반복 규칙 판정 (`core:domain` 이 사용)
- `shared:core:common:permission` + 공개 SDK `net.plumt.permission` - 알림·정확 알람 권한 요청
- `shared:core:fundamental:common` / `alarm` / `local-notification` - 시각 공급·설정 저장소, 정확 알람 호환 계층, 알림 발송
- `shared:core:common:database`(암호화 DB 팩토리), `admob`, `billing`, `ui`, `navigation-base`
- `shared:core:firebase:{crashlytics,remote-config,app-check}`
- `shared:feature:app-update`, `notice`, `permission-notice`, `remove-ads`, `legal`, `webview`, `oss-licenses`, `settings-ui`

### 주요 라이브러리
Hilt, Jetpack Compose + Material 3, Navigation Compose, Room, Kotlin Coroutines/Flow, `AlarmManager`

## Build Configuration

```bash
./gradlew :apps:diding:assembleDevDebug     # dev 디버그
./gradlew :apps:diding:assembleProdRelease  # 운영 릴리스
```

규약 플러그인 일곱 개(`android.application`, `hilt`, `compose`, `buildconfig`, `ndk.nativeconfig`, `verify.prebuild`, `screenshot`)가 빌드 구성의 뼈대다. 서명은 buildType 이 아니라 flavor 로 갈리고 서명 정보는 저장소 밖에서 읽는다.

## Testing

```bash
./gradlew :apps:diding:test                  # 앱 모듈(딥링크 매핑·바텀바 규칙·광고 스크린샷)
./gradlew :apps:diding:core:domain:test      # 반복 판정·달성률
./gradlew :apps:diding:core:alarm:test       # 다음 발화 시각 계산
```

도메인·저장소·알람·화면 모듈마다 단위 테스트가 있고, 화면은 Roborazzi 스크린샷 테스트로 광고 있음/없음을 함께 본다. 반복 판정을 공용 모듈로 바꿀 때는 원래 구현과 같은 답을 내는지 패리티 테스트로 확인했다. E2E 스모크 시나리오는 testTag 를 셀렉터로 쓴다.

## Notes

- 테마는 공용 Paper 디자인 시스템 위의 밝은 종이 톤 고정(accent 그린)이며 시스템 다크 설정을 읽지 않는다
- 스키마 v1 은 아직 파괴적 마이그레이션 허용 상태라, 스키마를 올리기 전에 `Migration` 을 먼저 써야 한다(안 그러면 할 일·완료 이력이 지워진다)
- 앱 이름은 플레이버 소스셋 리소스에만 두어 한쪽 정의가 빠지면 빌드가 실패하게 한다
- 자세한 한계와 함정은 [architecture.md](architecture.md) 의 마지막 절 참고
