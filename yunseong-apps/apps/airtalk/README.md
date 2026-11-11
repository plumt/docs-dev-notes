# AirTalk

## Basic Information

- **App Name**: AirTalk
- **Package ID**: `com.yunseong.airtalk` (dev 플레이버는 `.dev` 접미사)
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: In Development
- **Gradle 경로**: `:apps:airtalk`
- **플레이버**: `dev` · `prod` (규약 플러그인의 allowlist 가 생성). 환경마다 Firebase 프로젝트를 따로 쓴다
- **딥링크 스킴**: `yunseong-airtalk`

## Description

종단간 암호화(E2EE) 채팅과 공개 게시판을 합친 앱이다. 로그인은 소셜(Google·카카오·네이버)과 이메일을 지원한다. 하단 탭 4개(홈·채팅·게시판·설정)로 구성되고, 채팅 메시지와 그룹 이름은 기기에서 암호화한 뒤 서버로 보내며 게시판은 공개 평문이다. 앱 모듈은 조립만 하고 화면은 feature 모듈, 공용 부품은 `shared` 에 둔다.

## Features

- **친구와 채팅**: 친구 추가·검색, 1:1 대화와 그룹 채팅, 메시지 페이징·날짜 구분선·읽음 표시, 푸시 알림
- **게시판**: 글 목록·상세·작성·수정, 댓글
- **안전 장치**: 가입 시 약관 동의와 운영정책 링크, 친구·채팅 사용자 신고
- **설정**: 알림, 약관·처리방침, 오픈소스 라이선스
- **단일 활성 기기**: 한 번에 한 기기만 활성이며, 재설치하면 이전 대화는 복구되지 않는다

## 구조

```
:apps:airtalk                 조립 (Hilt 키 바인딩, 딥링크, 내비 그래프, 플레이버)
 ├─ core:domain               모델·리포지토리 계약·유스케이스 (Android·Room·Firebase 를 모르는 순수 계층)
 ├─ core:database             Room + SQLCipher 로컬 저장소, 동기화용 Entity·DAO
 ├─ core:crypto               앱별 E2EE 설정과 봉투 종류
 ├─ core:remote               원격 데이터 소스 (Firebase SDK 타입은 이 모듈만 쓴다)
 ├─ core:data                 리포지토리 구현, 채팅 동기화, 발신 큐, 푸시 수신
 └─ feature:{splash, login, home, chat, board, settings}
```

의존 방향은 `feature -> core:domain`, `core:data -> domain·database·crypto·remote` 한쪽뿐이다.

### 쓰는 공용 모듈

- `core:common`: `admob`, `billing`, `crypto-e2ee`(Tink 기반 봉투·키 금고), `delta-sync`(커서 증분 조회), `outbox`(멱등 전송 대기열), `write-coalescer`(읽음 표시 합치기), `app-shell`, `navigation-base`
- `core:firebase`: `core`, `auth`, `firestore`, `functions`, `messaging`, `storage`(프로필 사진만), `app-check`, `crashlytics`
- `feature`: `app-update`, `social-login-*`, `settings-ui`, `image-picker`, `legal`, `oss-licenses`, `share`, `notice`, `remove-ads`, `permission-notice`
- 소셜 로그인 그룹은 공개 SDK [sdk-kotlin-social-login](https://github.com/plumt/sdk-kotlin-social-login) 으로 이전 중이다

## 데이터와 보안

- 서버: Firebase Auth·Firestore·Functions·FCM. 로그인 토큰 발급, 프로필, 친구, 방, 메시지 전송은 호출형 Functions 로 처리한다
- 로컬: Room + SQLCipher. 백업·기기 이전 규칙은 DB 와 설정을 통째로 제외한다
- 메시지는 멱등 ID 로 발신 큐에 쌓고, 커서 기반 증분 동기화로 받는다
- 키·클라이언트 ID 는 `clients/airtalk/config.json` 으로만 관리하고 NDK `NativeConfig`(XOR + JNI)로 구워 넣는다. 값은 저장소에 싣지 않는다

## Build Configuration

```bash
./gradlew :apps:airtalk:assembleDevDebug
./gradlew :apps:airtalk:assembleProdRelease
./gradlew :apps:airtalk:installDevDebug
```

운영 릴리스는 `preBuildVerify` 가 필수 설정 키(약관·처리방침 URL, 소셜 로그인 키 등)가 비었으면 빌드 전에 멈춘다.

## Notes

- 약관·처리방침 링크는 가입 전에도 접근할 수 있어야 하므로 로그인 화면에서 메뉴를 숨기지 않는다
- 서버 함수의 호출 계약은 서버 저장소의 `contract` 를 정본으로 하고 앱 쪽에 사본을 둔다
