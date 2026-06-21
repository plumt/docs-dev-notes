# yunseongApps Monorepo

Android 멀티 앱 모노레포 프로젝트

## 프로젝트 구조

```
yunseongApps/
├── apps/                         # 앱 모듈
│   ├── lottomate/               # Lottomate Android 앱
│   ├── lunar/                   # LunarPhase Android 앱
│   ├── qrscanner/               # QR Scanner Android 앱
│   ├── diding/                  # Diding Android 앱
│   └── moduta/                  # Moduta Android 앱 (서울시 버스)
│
├── shared/
│   ├── feature/                  # Shared Feature 모듈 (16개)
│   │   ├── qr/                  # QR feature 모듈
│   │   │   ├── common/
│   │   │   ├── scanner/
│   │   │   ├── multi-scanner/
│   │   │   └── generator/
│   │   ├── lock/                # 앱 잠금 feature
│   │   ├── oss-licenses/        # 오픈소스 라이선스
│   │   ├── app-update/          # 앱 업데이트 안내
│   │   ├── chart/               # 차트 컴포넌트
│   │   ├── image-picker/        # 이미지 선택
│   │   ├── image-saver/         # 이미지 저장
│   │   ├── notification-inbox/  # 알림 수신함
│   │   ├── notification-settings/ # 알림 설정
│   │   ├── seoul-bus/           # 서울시 버스 실시간 정보
│   │   ├── share/               # 공유 기능
│   │   ├── social-login/        # 소셜 로그인
│   │   └── webview/             # 웹뷰
│   │
│   └── core/                    # Shared Core 모듈 (28개, infrastructure)
│       ├── common/              # 공통 인프라
│       │   ├── admob/           # AdMob 통합
│       │   ├── database/        # Room 기본 설정
│       │   ├── designsystem/    # Material 3 테마
│       │   ├── location-provider/ # 위치 제공자
│       │   ├── network/         # 네트워크 설정
│       │   └── ui/              # UI 컴포넌트
│       ├── firebase/            # Firebase 통합
│       │   ├── auth/
│       │   ├── core/
│       │   ├── crashlytics/
│       │   ├── firestore/
│       │   ├── messaging/
│       │   └── remote-config/
│       ├── fundamental/         # 기반 기능
│       │   ├── alarm/
│       │   ├── common/
│       │   ├── local-notification/
│       │   └── permission/
│       ├── lunar/               # 달 관련 공통
│       │   └── calculator/
│       └── permissions/         # 런타임 권한 (11개)
│           ├── permission-core/
│           ├── permission-camera/
│           ├── permission-gallery/
│           └── ...
│
├── backend/                      # 백엔드 서비스
│   └── lottomate/
│       └── functions/           # Firebase Cloud Functions (TypeScript)
│
├── frontend/                     # 웹 프론트엔드
│   └── lottomate-web/          # React + TypeScript + Vite
│
├── build-logic/                  # Gradle Convention Plugins
│   └── convention/
│
├── clients/                      # 앱별 설정 파일 (NDK 암호화 대상)
│   ├── lottomate/config.json
│   ├── lunar/config.json
│   ├── qrscanner/config.json
│   ├── diding/config.json
│   └── moduta/config.json
│
├── .github/workflows/            # GitHub Actions
│   ├── deploy-firebase.yml     # Firebase 배포 (수동)
│   └── build-android.yml       # Android 빌드 (자동)
│
├── gradle/libs.versions.toml    # Version Catalog
├── settings.gradle.kts          # Module 설정
└── CLAUDE.md                    # Claude Code 가이드
```

## 앱 목록

| 앱 | 패키지명 | 버전 | Flavor |
|----|---------|------|--------|
| Lottomate | com.yunseong.lottomate | v1.0.0 | dev / prod |
| LunarPhase | com.yun.lunarphase | v1.3.2 | - |
| QRScanner | com.yunseong.qring | v1.0.0 | dev / prod |
| Diding | com.yunseong.diding | v1.0.0 | - |
| Moduta | com.yunseong.moduta | v1.0.0 | - |

## 앱 스크린샷

### Lottomate

<p align="center">
  <img src="screenshots/lottomate/home.png" width="250" alt="홈 화면">
  <img src="screenshots/lottomate/numbers.png" width="250" alt="번호 관리">
  <img src="screenshots/lottomate/statistics.png" width="250" alt="통계">
</p>

> 스크린샷은 추후 업데이트 예정입니다.

### Lunar Phase

<p align="center">
  <img src="screenshots/lunar/phase.png" width="250" alt="달 위상">
  <img src="screenshots/lunar/calendar.png" width="250" alt="달력">
</p>

> 스크린샷은 추후 업데이트 예정입니다.

### QR Scanner

<p align="center">
  <img src="screenshots/qrscanner/scan.png" width="250" alt="QR 스캔">
  <img src="screenshots/qrscanner/result.png" width="250" alt="스캔 결과">
</p>

> 스크린샷은 추후 업데이트 예정입니다.

## 기술 스택

### Android
- Kotlin 2.2.10
- AGP 9.0.1
- Jetpack Compose (BOM 2025.02.00, Material 3)
- Hilt 2.56.2
- Room 2.7.0-alpha12
- Firebase BOM 34.0.0
- KSP 2.3.2
- minSdk: 29, targetSdk: 36, compileSdk: 36, Java: 17

### Backend (Firebase Functions)
- TypeScript 5.3
- Node.js 20
- Firebase Functions v2
- Firebase Admin SDK
- Firestore (lottomate-db)
- Cloud Scheduler (매주 토요일 20:55)

### Frontend (Web)
- React 19.2
- TypeScript
- Vite 7.2
- React Router
- Firebase Hosting

## 배포된 서비스

### Firebase Functions
- **scheduledLottoCheck**: 매주 토요일 20:55 자동 실행
- **sendLottoCheckNotification**: `https://sendlottochecknotification-t5op6c3wmq-du.a.run.app`
- **testLottoCheckNotification**: `https://testlottochecknotification-t5op6c3wmq-du.a.run.app`

### Web Hosting
- **URL**: https://lottomate-ce9f6.web.app
- **Routes**:
  - `/` - 홈페이지
  - `/privacy` - 개인정보처리방침
  - `/terms` - 이용약관
  - `/admin/*` - 관리자 페이지

## 개발

### 백엔드 개발
```bash
cd backend/lottomate/functions
npm install
npm run build        # TypeScript 빌드
npm run serve        # 로컬 에뮬레이터 실행
```

### 프론트엔드 개발
```bash
cd frontend/lottomate-web
npm install
npm run dev          # 개발 서버 실행
npm run build        # 프로덕션 빌드
```

### Android 개발
```bash
./gradlew :apps:lottomate:build
./gradlew :apps:lottomate:installDebug
```

## 모노레포 아키텍처

### 모듈 계층 구조
```
apps → shared/feature → shared/core
```

- **apps**: 앱 구성 및 네비게이션
- **shared/feature**: 비즈니스 로직 + UI (앱 전용 또는 공유)
- **shared/core**: 공통 인프라 (네트워크, UI, DB 등)

### 모듈 수
- Shared Core: 28개
- Shared Feature: 16개
- App 전용: 24개
- **합계: 68개**

### 의존성 규칙
- apps → shared/feature + shared/core
- shared/feature → shared/core
- shared/core → shared/core (신중하게)
- shared/core → shared/feature (금지)
- shared/feature → shared/feature (금지, core 추상화 필요)

### Convention Plugins
- `convention.android.application` - 앱 기본 설정
- `convention.android.library` - 라이브러리 기본 설정
- `convention.android.compose` - Compose 설정
- `convention.android.hilt` - Hilt DI 설정
- `convention.buildconfig` - BuildConfig 설정

## 보안 아키텍처

API 키·URL·설정값 등을 NDK/JNI로 보호합니다.

- `clients/<app>/config.json` → 빌드 타임에 Python 스크립트 실행
- XOR 암호화된 C 코드 자동 생성 → NDK로 `.so` 바이너리 컴파일
- 런타임에 JNI 호출로 복호화 (메모리에서만 평문 존재)
- BuildConfig에 평문 노출 없음 / `.so` 바이너리 내 평문 없음
- XOR 키: 필드명 SHA-256 파생 (결정론적, 증분 빌드 안정)

## 환경 설정

API 키와 설정값은 `clients/<앱명>/config.json`에서 관리됩니다.
빌드 시 자동으로 NDK 바이너리로 암호화됩니다.

Keystore 관련 시크릿만 `local.properties`에 등록합니다:
- KEYSTORE_FILE, KEYSTORE_PASSWORD, KEY_ALIAS, KEY_PASSWORD
- FIREBASE_SERVICE_ACCOUNT_FILE (App Distribution용)

## 배포

### Android APK (GitHub Actions)
수동 트리거:
```bash
GitHub Actions → Build Android APK → Run workflow
```

### Firebase (GitHub Actions)
수동 트리거:
```bash
GitHub Actions → Deploy Firebase → Run workflow
- 앱 선택: lottomate/qrscanner
- 배포 대상: all/functions/hosting
```

### 로컬 배포 (스크립트)
```bash
./scripts/deploy-all.sh          # 전체
./scripts/deploy-backend.sh      # Functions만
./scripts/deploy-frontend.sh     # Hosting만
```

## Firebase 설정

- **Project ID**: lottomate-ce9f6
- **Region**: asia-northeast3 (Seoul)
- **Firestore Database**: lottomate-db
- **FCM Topic**: lotto_check

## 주요 기능

### 백엔드
- 매주 토요일 20:55 자동 로또 당첨 번호 조회
- Firestore에 최신 회차 저장
- FCM 푸시 알림 발송
- 수동 알림 발송 API

### 웹 관리자
- 실시간 통계 대시보드
- 테스트 알림 발송
- 수동 알림 발송

### 모바일 앱
- 로또 번호 저장 및 관리
- 당첨 결과 푸시 알림
- 당첨 번호 확인

## 로그 확인

```bash
firebase functions:log
```

## 개발 가이드

### 새 모듈 추가
1. 모듈 위치 결정: apps/shared/feature/shared/core
2. `settings.gradle.kts`에 모듈 추가
3. `build.gradle.kts` 생성 (Convention Plugin 사용)
4. 필요한 의존성 추가 (`libs.versions.toml` 사용)

### 의존성 추가
1. `gradle/libs.versions.toml`에 버전 추가
2. 모듈의 `build.gradle.kts`에서 `libs.*` 사용
3. 하드코딩된 버전 금지

### 커밋 컨벤션
```
feat(camera): 카메라 모듈 xxx 기능 추가

 - 권한 체크 및 요청 기능 추가
 - 촬영 후 미리보기 화면 추가
```

- 타입/범위는 영문 (feat, fix, refactor, docs, build, test, perf, chore)
- 제목과 본문은 한글
- 본문은 불릿 포인트 (` - `) 사용

## 주의사항

### Android
- minSdk: 29, targetSdk: 36, compileSdk: 36, Java: 17
- Kotlin 2.2.10, AGP 9.0.1
- 모든 버전은 `libs.versions.toml`에서 관리

### Firebase
- Node.js 20 런타임 사용
- 스케줄: 매주 토요일 20:55 KST
- 모든 Functions는 asia-northeast3 리전에 배포

### GitHub Actions
- Firebase 배포는 수동 실행만 허용
- Android APK 빌드는 자동/수동 모두 가능
