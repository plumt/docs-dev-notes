# yunseongApps Monorepo

Android 멀티 앱 모노레포 프로젝트

## 프로젝트 구조

```
yunseongApps/
├── app/                          # 앱 모듈
│   ├── lottomate/               # Lottomate Android 앱
│   ├── lunar/                   # Lunar Android 앱
│   └── qrscanner/               # QR Scanner Android 앱
│
├── feature/                      # Feature 모듈
│   ├── lotto/                   # Lottomate feature 모듈 (app-specific)
│   │   ├── common/
│   │   ├── database/
│   │   ├── winning-numbers/
│   │   ├── my-numbers/
│   │   ├── statistics/
│   │   └── winning-checker/
│   ├── qr/                      # QR feature 모듈 (shared)
│   │   ├── common/
│   │   ├── scanner/
│   │   └── multi-scanner/
│   ├── lock/                    # 앱 잠금 feature (shared)
│   └── oss-licenses/            # 오픈소스 라이선스 (shared)
│
├── core/                         # Core 모듈 (infrastructure)
│   ├── common/                  # 공통 유틸리티
│   ├── ui/                      # UI 컴포넌트
│   ├── design-system/           # Material 3 테마
│   ├── network/                 # 네트워크 설정
│   ├── database/                # Room 기본 설정
│   ├── permission/              # 권한 처리
│   ├── firebase/                # Firebase 설정
│   └── admob/                   # AdMob 통합
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
├── .github/workflows/            # GitHub Actions
│   ├── deploy-firebase.yml     # Firebase 배포 (수동)
│   └── build-android.yml       # Android 빌드 (자동)
│
├── gradle/libs.versions.toml    # Version Catalog
├── settings.gradle.kts          # Module 설정
└── CLAUDE.md                    # Claude Code 가이드
```

## 기술 스택

### Android
- Kotlin 2.0.21
- Jetpack Compose (Material 3)
- minSdk: 29, targetSdk: 36

### Backend (Firebase Functions)
- TypeScript 5.3
- Node.js 20
- Firebase Functions v2
- Firebase Admin SDK
- Firestore (lottomate-db)
- Cloud Scheduler (매주 토요일 20:55)

### Frontend (Web)
- React 18
- TypeScript
- Vite
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
./gradlew :app:lottomate:build
./gradlew :app:lottomate:installDebug
```

## 모노레포 아키텍처

### 모듈 계층 구조
```
app → feature → core
```

- **app**: 앱 구성 및 네비게이션
- **feature**: 비즈니스 로직 + UI (app-specific or shared)
- **core**: 공통 인프라 (네트워크, UI, DB 등)

### 의존성 규칙
- ✅ `app` → `feature` + `core`
- ✅ `feature` → `core`
- ✅ `core` → `core` (신중하게)
- ❌ `core` → `feature` or `app`
- ❌ `feature` → `feature` (core 추상화 필요)

### Convention Plugins
- `convention.android.application` - 앱 기본 설정
- `convention.android.library` - 라이브러리 기본 설정
- `convention.android.compose` - Compose 설정
- `convention.android.hilt` - Hilt DI 설정

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

## 환경 변수

현재는 환경 변수가 필요 없습니다. Firebase Admin SDK가 자동으로 인증합니다.

## 로그 확인

```bash
firebase functions:log
```

## 개발 가이드

### 새 모듈 추가
1. 모듈 위치 결정: app/feature/core
2. `settings.gradle.kts`에 모듈 추가
3. `build.gradle.kts` 생성 (Convention Plugin 사용)
4. 필요한 의존성 추가 (`libs.versions.toml` 사용)

### 의존성 추가
1. `gradle/libs.versions.toml`에 버전 추가
2. 모듈의 `build.gradle.kts`에서 `libs.*` 사용
3. ❌ 하드코딩된 버전 금지

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
- minSdk: 29, targetSdk: 36, compileSdk: 36
- Kotlin 2.0.21, AGP 9.0.0-alpha11
- 모든 버전은 `libs.versions.toml`에서 관리

### Firebase
- Node.js 20 런타임 사용
- 스케줄: 매주 토요일 20:55 KST
- 모든 Functions는 asia-northeast3 리전에 배포

### GitHub Actions
- Firebase 배포는 수동 실행만 허용
- Android APK 빌드는 자동/수동 모두 가능
