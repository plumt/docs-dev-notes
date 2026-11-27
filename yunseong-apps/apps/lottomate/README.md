# Lottomate

## Basic Information

- **App Name**: 로픽 (Lottomate)
- **Package ID**: `com.plumt.lottomate`
- **Version**: 1.0.8 (9)
- **Min SDK / Target SDK**: 29 / 36
- **Status**: Released (Google Play 프로덕션)
- **Google Play**: [로픽](https://play.google.com/store/apps/details?id=com.plumt.lottomate)

상세 설계는 [architecture.md](architecture.md), 모듈별 책임은 [modules.md](modules.md) 를 본다.

## Description

Jetpack Compose 로 만든 로또 관리 앱이다. 당첨번호 조회·동기화, 내 번호와 즐겨찾기 관리, 통계, 당첨 확인, 추첨 카운트다운, 번호 불러오기(카메라·사진의 티켓 QR 과 구매내역 글자 인식), 푸시 알림, 공지사항을 제공한다. 모든 화면은 종이 티켓 느낌의 공용 Paper 디자인 시스템으로 통일돼 있다.

앱 모듈은 조립만 한다. Application, MainActivity, DI 와 내비게이션 배선만 두고 화면과 도메인은 `core`·`feature` 하위 모듈에 있다.

## Features

- **당첨번호**: 최신·회차별·전 회차 당첨번호를 Firestore 에서 받아 로컬 DB 에 동기화. 외부 로또 사이트는 앱이 직접 부르지 않는다
- **내 번호**: 번호 저장·삭제·즐겨찾기, 이번주 구매/달력/즐겨찾기 3탭, 수동 입력 시트
- **통계**: 번호 빈도, 당첨금 기록, 월별 추이, 번호별 성과 (Vico 차트)
- **당첨 확인**: 저장 번호를 당첨번호와 대조해 등수 판정
- **카운트다운**: 토요일 추첨까지 남은 시간과 추첨 중/결과 발표 상태
- **번호 불러오기**: 카메라나 사진에서 로또 QR 을 읽고, QR 이 아니면 구매내역 글자를 인식해 번호를 저장한다. 저장 전에 회차를 검증한다
- **푸시 알림**: 당첨 결과와 구매 알림. 알림 설정에서 구독을 켜고 끈다
- **알림함**: 앱 내 알림 이력. 항목을 누르면 해당 화면으로 이동
- **업데이트 관문**: 시작 시 버전을 확인하고, 확인에 실패하면 진행을 막는다
- **광고**: AdMob 배너·네이티브·종료 팝업. 결제 판매 스위치(기본 꺼짐)가 꺼진 빌드는 결제 구현과 결제 권한을 빼고 광고를 항상 노출한다
- **공지사항**: 공지 목록과 N 배지, 정책 변경 팝업

## 화면 구성

스플래시 -> 홈 · 내 번호 · 통계 · 더보기 4개 탭. 알림함, 공지사항, 개인정보 처리방침, 이용약관은 바텀바를 숨기는 보조 목적지다.

## Dependencies

자세한 책임은 [modules.md](modules.md) 에 있다.

### App-specific Modules
- core: `common`, `database`, `winning-numbers`, `winning-checker`, `countdown`, `my-numbers`, `statistics`, `receipt-import`, `demo-seed`(debug 전용)
- feature: `splash`, `home`, `my-numbers-overview`, `my-numbers`, `statistics`, `more`, `messaging`, `notifications`

### Shared Modules
- feature: `chart`, `notification-inbox`(impl/ui), `notification-settings`, `app-update`, `notice`, `permission-notice`, `remove-ads`, `oss-licenses`, `settings-ui`, `webview`, `image-picker`
- core: `common:app-shell`, `common:designsystem`, `common:database`, `common:time-sync`, `common:admob`, `common:billing`, `common:permission`, `fundamental:common`, `firebase:{firestore,crashlytics,remote-config,messaging,app-check}`
- 공용 순수 로직: `recurrence`(카운트다운), `ttl-cache`(최신 회차 캐시), `transaction`(여러 건 저장), `period-stats`(구매 연속·월별 집계)

### 공개 SDK
- `net.plumt.qr`: 카메라·사진 QR 인식
- `net.plumt.permission`: 카메라·알림 권한 요청
- `net.plumt.ads`: 공용 `admob` 모듈이 감싸는 광고 SDK

### External Services
- **Firebase**: Crashlytics, Cloud Messaging, Remote Config, Firestore, App Check. 당첨번호 수집과 푸시 발송은 별도 저장소의 서버가 맡는다
- **AdMob**: 배너·네이티브 광고
- **ML Kit**: 구매내역 한국어 글자 인식

## Build Configuration

플레이버(`dev`, `prod`)·서명·빌드 타입은 Convention Plugin(`convention.android.application`)이 만들고 앱 모듈은 값만 더한다.

```bash
./gradlew :apps:lottomate:assembleDevDebug     # dev 디버그
./gradlew :apps:lottomate:assembleProdRelease  # 운영 릴리스
```

릴리스 빌드 전에는 `preBuildVerify` 가 필수 설정 값 누락, 운영 광고 키에 테스트 id 가 섞였는지, 딥링크 스킴과 매니페스트의 일치를 검사한다.

## Testing

```bash
./gradlew :apps:lottomate:test                 # 유닛·스크린샷 테스트
```

화면 스크린샷 테스트(Roborazzi)와 testTag 기반 E2E 스모크 시나리오가 있다. 스토어 스크린샷은 debug 빌드에만 들어가는 `core:demo-seed` 로 "몇 주 쓴 사용자" 데이터를 심어 찍는다.

## Architecture 요약

- **UI**: Jetpack Compose + Material 3, 공용 Paper 디자인 시스템 위의 라이트 고정 브랜드 테마
- **DI**: Hilt
- **Navigation**: Navigation Compose, 목적지가 바텀바 표시를 직접 켜고 끈다
- **패턴**: Clean Architecture + MVVM/MVI(`onIntent`), core(data/domain)와 feature(UI)로 계층 분리
- **Security**: NDK/JNI 암호화 설정, SQLCipher 암호화 Room DB, 릴리스에서 `Log` 호출 제거
- **Build System**: Convention Plugins + Version Catalog

## Notes

- API 키와 URL 은 저장소 밖 설정 파일에서 빌드 때 읽고 소스 리터럴로 두지 않는다
- Firebase 설정 파일과 서명 키는 공개하지 않는다

## Release Notes

### v1.0.8 (Current)
- 결제 판매 스위치(기본 꺼짐) 도입, 꺼짐 빌드는 결제 구현·결제 권한 제거
- 스플래시 관문 전 딥링크를 보류했다가 관문 통과 후 전달

### v1.0.7
- 사진 인식 2단계 전처리와 불일치 칸 재시도, 촬영 실패 시 사진 선택으로 대체
- 금액·수익률 표기와 큰 글자 레이아웃 정리, 2등 보너스 공 표시

### v1.0.3 ~ v1.0.6
- 번호 불러오기(카메라 QR, 사진 QR, 구매내역 스크린샷 글자 인식) 신설
- 구매내역 확정이 막히던 문제와 회차 데이터 병합 오류 수정

## 스크린샷

스크린샷은 [screenshots](../../screenshots/lottomate/) 폴더를 참고한다.
