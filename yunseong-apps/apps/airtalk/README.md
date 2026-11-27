# AirTalk (에어톡)

## Basic Information

- **App Name**: 에어톡 (dev 빌드는 "에어톡 DEV")
- **Version Code**: 1
- **Version Name**: 1.0.0
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: In Development (미출시)
- **Gradle 경로**: `:apps:airtalk`
- **플레이버**: `dev` · `prod` (규약 플러그인의 allowlist 가 생성). 환경마다 Firebase 프로젝트를 따로 쓴다

## Description

종단간 암호화(E2EE) 채팅과 공개 게시판을 합친 앱이다. 로그인은 소셜(Google·카카오·네이버)과 이메일을 지원한다. 하단 탭 4개(홈·채팅·게시판·설정)로 구성되고, 채팅 메시지와 그룹 이름은 기기에서 암호화한 뒤 서버로 보내며 게시판은 공개 평문이다. 앱 모듈은 조립만 하고 화면은 feature 모듈, 공용 부품은 `shared` 에 둔다. 구조와 설계는 [architecture.md](architecture.md) 를 본다.

## Features

- **친구와 채팅**: 서버가 발급한 친구 ID 로 친구 추가·공유, 1:1 대화와 그룹 채팅, 메시지 페이징·날짜 구분선·읽음 표시, 푸시 알림(내용 표시 여부 선택)
- **게시판**: 글 목록·상세·작성·수정·삭제, 댓글
- **안전 장치**: 가입 시 약관 동의와 운영정책 링크, 사용자·글·댓글 신고, 사용자 차단(차단한 사람의 메시지·글은 숨김)
- **설정**: 프로필 사진·닉네임·상태 메시지, 알림, 차단 목록, 공지사항, 약관·처리방침, 오픈소스 라이선스, 광고 제거, 로그아웃·회원 탈퇴
- **단일 활성 기기**: 한 계정은 한 기기에서만 활성이다. 다른 기기에서 로그인하면 이전 기기는 로그아웃되고, 재설치하면 이전 대화는 복호화할 수 없다

## 구조

```
:apps:airtalk                 조립 (DI 바인딩, 딥링크, 내비 그래프, 플레이버)
 ├─ core:domain               모델·리포지토리 계약·유스케이스 (Android·Room·Firebase 를 모르는 순수 계층)
 ├─ core:database             Room + SQLCipher 로컬 저장소, 동기화용 Entity·DAO
 ├─ core:crypto               앱별 E2EE 설정과 봉투 종류
 ├─ core:remote               원격 데이터 소스 (Firebase SDK 타입은 이 모듈만 쓴다)
 ├─ core:data                 리포지토리 구현, 채팅 동기화, 발신 큐, 푸시 수신
 ├─ core:demo-seed            스크린샷 촬영용 상태 (debugImplementation)
 └─ feature:{splash, login, home, chat, board, settings}
```

의존 방향은 `feature -> core:domain`, `core:data -> domain·database·crypto·remote` 한쪽뿐이다.

### 쓰는 공용 모듈·SDK

- `core:common`: `crypto-e2ee`(Tink 기반 봉투·키 금고), `delta-sync`(커서 증분 조회), `outbox`(멱등 전송 대기열), `write-coalescer`(읽음 표시 합치기), `transaction`(트랜잭션 포트), `backoff`(재연결·재시도 간격), `ttl-cache`(부재 기억 캐시), `paging`, `connectivity`, `database`, `permission`(권한 SDK 연결), `demo-seed`, `designsystem`(Paper), `app-shell`, `navigation-base`, `admob`, `billing`
- `core:firebase`: `core`, `auth`, `firestore`, `functions`, `messaging`, `database`(읽음 위치), `storage`(프로필 사진만), `app-check`, `crashlytics`
- `feature`: `app-update`, `settings-ui`, `image-picker`, `legal`, `webview`, `oss-licenses`, `share`, `notice`, `remove-ads`, `permission-notice`
- 공개 SDK: 소셜 로그인 [sdk-kotlin-social-login](https://github.com/plumt/sdk-kotlin-social-login)(`net.plumt.sociallogin`, 이전 완료), 알림 권한 요청 `net.plumt.permission`, 프로필 사진 압축 `net.plumt.image`

## 데이터와 보안

- 서버: Firebase Auth·Firestore·Realtime Database·Functions·FCM. 로그인 토큰 발급, 프로필, 친구, 방, 메시지 전송은 호출형 Functions 로 처리한다
- 서버 코드는 별도 저장소에서 관리한다. 호출 계약은 서버 쪽이 정본이고 앱 저장소에 사본을 두어 테스트가 대조한다
- 로컬: Room + SQLCipher. 백업·기기 이전 규칙은 DB 와 설정을 통째로 제외한다
- 메시지는 멱등 ID 로 발신 큐에 쌓고, 커서 기반 증분 동기화로 받는다
- 키·클라이언트 ID 는 저장소 밖 설정 파일로만 관리하고 빌드 때 NDK 네이티브 라이브러리로 넣는다. `BuildConfig` 평문은 APK 에서 그대로 추출되기 때문이다

## Build Configuration

```bash
./gradlew :apps:airtalk:assembleDevDebug
./gradlew :apps:airtalk:assembleProdRelease
./gradlew :apps:airtalk:installDevDebug
```

운영 릴리스는 필수 설정 값(약관·처리방침 주소, 소셜 로그인 키 등)이 비었으면 빌드 전에 멈춘다.

## Notes

- 약관·처리방침 링크는 가입 전에도 접근할 수 있어야 하므로 로그인 화면에서 숨기지 않는다
- 스플래시는 버전 확인 관문이다. 오프라인 새 실행은 캐시가 있어도 막는다
- 채팅 이미지 전송은 기획만 있고 구현하지 않았다
