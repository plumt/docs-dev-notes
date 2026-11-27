# Lottomate 모듈 구성

앱 전용 모듈의 책임과 경계를 정리한다. 모듈 목록의 정본은 저장소 루트의 `settings.gradle.kts` 이고, 모든 모듈은 Convention Plugin 과 Version Catalog 로만 빌드를 구성한다.

## core 모듈 (데이터·도메인)

| 모듈 | 책임 | 경계·설계 포인트 |
|---|---|---|
| `core:common` | 회차 모델, 등수 판정(`WinningRank`), 번호 계산, 번호 UI, 테마, 토픽 구독 | 외부 URL 같은 설정 값은 평문 상수 대신 네이티브 설정 접근자로 읽는다. feature 를 의존하지 않는다. |
| `core:database` | SQLCipher 암호화 Room DB | 테이블 사이 외래 키 없이 회차 값으로만 연결. 파괴적 마이그레이션 금지, 스키마 JSON 을 커밋. |
| `core:winning-numbers` | 당첨번호 조회·동기화 | Firestore 에서만 받아 로컬 DB 를 채운다. 외부 로또 사이트는 부르지 않는다. 최신 회차는 공용 `ttl-cache` 로 프로세스당 하나를 둔다. |
| `core:winning-checker` | 저장 번호와 당첨번호 대조, 당첨/구매 로컬 알림 발송 | 대조 트리거를 갖지 않는다(언제 대조할지는 호출부가 정한다). 알림 문구는 등수만 보고 고른다. 금액을 모르는 1등이 낙첨 문구로 나가면 안 되기 때문이다. |
| `core:countdown` | 추첨까지 남은 시간 판정 | 시각을 인자로만 받는 순수 계산. 기기 시간대가 아니라 한국 시간 기준으로 판정하고 반복 시각 계산은 공용 `recurrence` 에 맡긴다. |
| `core:my-numbers` | 저장 번호·즐겨찾기 도메인/저장소/집계 | 여러 건 변경은 공용 `transaction` 포트로 감싸고, 구매 연속 계산은 공용 `period-stats` 를 쓴다. |
| `core:statistics` | 당첨번호와 내 구매 게임 집계 | 저장소 계약은 domain, 구현은 data. 월별 추이의 달 판정은 `period-stats`, 현재 시각은 `TimeProvider`. |
| `core:receipt-import` | 번호 불러오기의 판별·파싱·회차 검증·저장 유스케이스 | QR 을 먼저 시도하고 아니면 구매내역 글자 인식으로 넘어간다. QR SDK 와 ML Kit 타입은 data 패키지 안에만 두고 domain 은 포트만 안다. 화면은 `feature:my-numbers` 에 있다. |
| `core:demo-seed` | 스토어 스크린샷 촬영용 데이터 시드 | debug 소스셋에만 코드가 있고 앱은 `debugImplementation` 으로만 의존해 운영 빌드에 들어가지 않는다. 당첨번호는 실제 값만 쓴다. |

## feature 모듈 (화면)

| 모듈 | 책임 |
|---|---|
| `feature:splash` | 공용 `SplashScaffold` 로 버전 관문을 지난 뒤 당첨번호를 동기화하고 홈으로 이동. 확인에 실패하면 진행을 막는다. |
| `feature:home` | 최신 당첨 결과, 카운트다운, 내 당첨 띠, 번호 불러오기·직접 저장 진입, 회차 조회와 번호 이력. 입력·불러오기 시트는 `feature:my-numbers` 것을 재사용한다. |
| `feature:my-numbers-overview` | 내 번호 탭. 이번주 구매 / 달력 / 즐겨찾기 3세그먼트와 네이티브 광고. |
| `feature:my-numbers` | 번호 입력 시트, 수동 입력, 번호 불러오기 흐름(카메라·사진 -> 미리보기). 카메라 QR 은 `net.plumt.qr`, 권한은 `net.plumt.permission` 을 쓴다. |
| `feature:statistics` | 통계 탭. 집계는 `core:statistics` 를 쓰고 숫자 서식은 한국 로캘을 명시. |
| `feature:more` | 더보기 탭. 버전 확인, 알림 토글, 저장 번호 삭제, 공지사항, 광고 제거, 약관 진입. |
| `feature:notifications` | 알림함. 공용 notification-inbox UI 에 앱 탭과 디자인 슬롯을 꽂은 진입점. |
| `feature:messaging` | FCM 메시지 핸들러. 수신 서비스는 공용이고 핸들러만 `@IntoSet` 으로 등록해 의존에 넣는 것만으로 배선된다. |

## 공유 모듈 사용 지점

- `shared:core:common:app-shell`: 셸 호스트 액티비티, 딥링크 배선, 바텀바 상태
- `shared:core:common:designsystem`: Paper 디자인 시스템(토큰·컴포넌트)
- `shared:core:common:permission`: 공개 권한 SDK 를 앱 시작에 연결
- `shared:core:common:{recurrence,ttl-cache,transaction,period-stats}`: 공용 순수 로직
- `shared:feature:notice`, `permission-notice`, `remove-ads`: 공지사항, 접근권한 사전 고지, 광고 제거 행
- `shared:feature:chart`: Vico 기반 차트
- `shared:feature:notification-inbox` / `notification-settings`: 알림함, 알림 구독 토글
- `shared:feature:app-update`, `image-picker`, `oss-licenses`, `webview`, `settings-ui`
- `shared:core:firebase:*`, `shared:core:common:admob`(공개 광고 SDK 어댑터), `shared:core:common:time-sync`, `shared:core:common:billing`

## 의존 방향 규칙

1. 앱 -> feature, core, shared
2. feature -> core, shared (다른 feature 참조는 계층 가드 예외 소수만)
3. core -> shared (feature 금지)
4. 서로 의존하는 두 feature 중 한쪽은 상위(`my-numbers-overview` 와 `home` 이 `my-numbers` 를 의존하고 반대는 없다)
