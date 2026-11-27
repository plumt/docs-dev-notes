# QR Scanner (큐링, Q-Ring)

## Basic Information

- **App Name**: 큐링 (Q-Ring)
- **Package ID**: `com.plumt.qrscanner`
- **Version**: 1.0.1 (versionCode 2)
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: Google Play 내부 테스트 트랙 배포(2026-11-26). 프로덕션 트랙은 아직 출시 전

상세 설계는 [architecture.md](architecture.md) 를 본다.

## Description

큐링은 QR 코드 스캔·생성·히스토리를 4개 탭으로 묶은 독립 앱이다. 스캔·생성·판별은 직접 만든 공개 QR SDK(`net.plumt.qr`)로 하고, 앱은 화면과 기록 저장만 맡는다. 앱 모듈은 조립만 하고 화면은 앱 전용 feature 모듈에 있다.

## Features

- **홈**: 스캔 버튼·QR 생성·사진 스캔이 있는 바이올렛 판, 복사한 링크 줄, 최근 스캔 카드, 즐겨찾기
- **스캔**: 홈 버튼에서 원이 커지며 열리는 스캔 화면. 여러 코드를 동시에 추적하고, 누른 코드의 결과 시트만 연다. 사진에서 코드를 읽는 것도 같은 형식 규칙을 쓴다
- **생성**: 링크·텍스트·Wi-Fi·연락처 QR 을 크기·색·로고를 정해 만들고, 갤러리 저장(저장소 권한 불필요)과 공유
- **히스토리**: 날짜별 묶음 카드, 상세 시트(타입별 액션·즐겨찾기·다시 만들기·복사·삭제), 즐겨찾기 화면, 조회 실패 재시도
- **더보기**: 앱 버전과 업데이트 표시, 공지사항, 문의, 약관·처리방침, 오픈소스 라이선스, 광고 제거·구매 복원, 기록 전체 삭제
- **광고**: AdMob 배너·목록 네이티브·종료 팝업. 결제로 광고를 끌 수 있다
- **시작 관문**: 스플래시의 버전 확인(실패 시 진행 차단), 접근권한 안내, 정책 변경 팝업

## Dependencies

### App-specific Modules (9개)
- `:apps:qrscanner` — 조립 전용
- `feature:splash` · `home` · `history` · `more` · `generator`
- `core:history` (기록 모델·암호화 Room 저장소) · `core:ui` (기록 줄·종류 아이콘·타입별 액션) · `core:demo-seed` (촬영용 데이터, debug 전용)

### Shared Modules
- `app-shell`, `app-update`, `notice`, `permission-notice`, `remove-ads`, `settings-ui`, `oss-licenses`
- `image-picker`, `image-saver`, `share`, `webview`, `legal`
- `common:permission`, `common:database`, `common:admob`, `common:billing`, `designsystem`(Paper)
- `firebase:crashlytics`, `firebase:app-check`, `fundamental:common`

### 외부 SDK·서비스
- **QR SDK** `net.plumt.qr` 0.0.2 (스캔·생성·판별), **권한 SDK** `net.plumt.permission`
- **Firebase**(앱 전용 프로젝트): Crashlytics, App Check, 버전 확인 API. 서버 쪽은 별도 저장소에서 관리한다
- **AdMob**, **Google Play 결제**(광고 제거)

## Build Configuration

플레이버(`dev`, `prod`)·서명·빌드 타입은 Convention Plugin 이 만들며 앱은 재선언하지 않는다. 설정 값은 저장소에 두지 않고 빌드 때 NDK 로 구워 넣는다. 운영 릴리스는 약관·처리방침 주소나 운영 광고 키가 비면 빌드 전 검증에서 멈춘다.

```bash
./gradlew :apps:qrscanner:assembleDevDebug     # dev 디버그
./gradlew :apps:qrscanner:assembleProdRelease  # 운영 릴리스
./gradlew :apps:qrscanner:installDevDebug      # 기기 설치
```

Play 업로드는 Jenkins 가 운영 플레이버 AAB 를 내부 테스트 트랙에 올린다.

## Testing

```bash
./gradlew :apps:qrscanner:test                 # 유닛·Robolectric 테스트
```

딥링크 화이트리스트와 라우터 매핑의 일치, 기존 기록 DB 를 그대로 여는지(스키마 호환), 화면별 레이아웃 안정성과 시안 대조용 렌더를 테스트로 고정한다. E2E 는 testTag 기반 Appium 하네스를 쓰며 dev 플레이버 고정이다.

## Architecture 요약

- **UI**: Jetpack Compose, 공용 Paper 디자인 시스템 위의 밝은 단일 테마(시스템 다크 모드를 따르지 않음)
- **DI**: Hilt
- **Navigation**: 공용 앱 셸(`ShellHostActivity`)이 테마·관문·딥링크·바텀바를 파생하고 앱은 NavGraph 만 넘긴다
- **Data**: SQLCipher 로 암호화한 Room
- **Build System**: Convention Plugins + Version Catalog

## 스크린샷

스크린샷은 [screenshots](../../screenshots/qrscanner/) 폴더를 참고한다.
