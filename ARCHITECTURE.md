# yunseongApps 아키텍처

## 개요

yunseongApps는 Android 멀티 앱 모노레포입니다. Clean Architecture와 Modular Architecture를 기반으로 코드 재사용성과 확장성을 극대화했습니다.

## 모듈 계층 구조

```
┌─────────────────────────────────────┐
│         App Layer (app/*)           │  ← 엔트리 포인트, 네비게이션
│  lottomate, lunar, qrscanner        │
└────────────┬────────────────────────┘
             │ depends on
┌────────────▼────────────────────────┐
│      Feature Layer (feature/*)      │  ← 비즈니스 로직 + UI
│  lotto/*, moon/*, qr/*, lock/...    │
└────────────┬────────────────────────┘
             │ depends on
┌────────────▼────────────────────────┐
│       Core Layer (core/*)           │  ← 공통 인프라
│  common, ui, network, database...   │
└─────────────────────────────────────┘
```

## 의존성 규칙

### 허용 ✅
- `app` → `feature` + `core`
- `feature` → `core`
- `core` → 다른 `core` (주의 필요)

### 금지 ❌
- `core` → `feature` 또는 `app`
- `feature` → 다른 `feature` (공유 필요 시 `core`로 이동)
- 순환 의존성

## 모듈 타입

### 1. App 모듈 (`app/*`)

**역할**: 앱의 엔트리 포인트
- Application 클래스
- MainActivity
- 네비게이션 그래프
- Feature 모듈 조합

**예시**:
```kotlin
// app/lottomate/build.gradle.kts
plugins {
    id("convention.android.application")
    id("convention.android.compose")
    id("convention.android.hilt")
}

dependencies {
    // Core 의존성
    implementation(project(":core:common"))
    implementation(project(":core:ui"))
    implementation(project(":core:design-system"))
    
    // Feature 의존성
    implementation(project(":feature:lotto:winning-numbers"))
    implementation(project(":feature:lotto:my-numbers"))
    implementation(project(":feature:oss-licenses"))
}
```

### 2. Feature 모듈 (`feature/*`)

**역할**: 비즈니스 로직 + UI
- Screen Composables
- ViewModel
- Use Cases
- Repository 구현

**종류**:
- **App-specific**: 특정 앱 전용 (예: `feature/lotto/*`)
- **Shared**: 여러 앱에서 공유 (예: `feature/qr/*`, `feature/lock`)

**예시**:
```kotlin
// feature/lotto/winning-numbers/build.gradle.kts
plugins {
    id("convention.android.library")
    id("convention.android.compose")
    id("convention.android.hilt")
}

dependencies {
    implementation(project(":core:common"))
    implementation(project(":core:ui"))
    implementation(project(":feature:lotto:common"))
}
```

### 3. Core 모듈 (`core/*`)

**역할**: 재사용 가능한 인프라
- 네트워크 설정
- 데이터베이스 설정
- UI 컴포넌트
- 유틸리티

**주요 모듈**:
- `core/common`: 공통 유틸리티, 확장 함수
- `core/ui`: 재사용 가능한 UI 컴포넌트
- `core/design-system`: Material 3 테마, 색상, 타이포그래피
- `core/network`: Retrofit, OkHttp 설정
- `core/database`: Room 기본 설정
- `core/firebase`: Firebase 통합
- `core/admob`: AdMob 통합
- `core/permission`: 권한 처리

## Convention Plugins

Gradle Convention Plugins로 빌드 설정을 표준화합니다.

### 사용 가능한 플러그인

```kotlin
// 앱 모듈
id("convention.android.application")

// 라이브러리 모듈
id("convention.android.library")

// Compose 사용
id("convention.android.compose")

// Hilt DI 사용
id("convention.android.hilt")

// BuildConfig 생성
id("convention.buildconfig")
```

### 장점
- 중복 설정 제거
- 일관된 빌드 설정
- 유지보수 용이

## 버전 관리 (Version Catalog)

모든 의존성은 `gradle/libs.versions.toml`에서 중앙 관리합니다.

```toml
[versions]
kotlin = "2.2.10"
compose-bom = "2024.09.00"
hilt = "2.56.2"

[libraries]
androidx-core-ktx = { module = "androidx.core:core-ktx", version.ref = "core-ktx" }
hilt-android = { module = "com.google.dagger:hilt-android", version.ref = "hilt" }

[plugins]
android-application = { id = "com.android.application", version.ref = "agp" }
```

**사용**:
```kotlin
dependencies {
    implementation(libs.androidx.core.ktx)  // ✅ 올바름
    // implementation("androidx.core:core-ktx:1.17.0")  // ❌ 금지
}
```

## 새 모듈 추가 프로세스

### 1. 재사용 조사 (필수!)
```bash
# 기존 모듈 확인
grep -r "기능명" core/
grep -r "기능명" feature/
ls -la core/
ls -la feature/
```

### 2. 모듈 위치 결정

| 필요 기능 | 위치 |
|----------|------|
| 2개 이상 앱에서 사용 | `core/*` |
| 2개 이상 앱의 특정 기능 | `feature/shared/*` |
| 특정 앱 전용 | `feature/앱명/*` |
| 엔트리 포인트 | `app/*` |

### 3. 모듈 생성

```kotlin
// 예: feature/newfeature/build.gradle.kts
plugins {
    id("convention.android.library")
    id("convention.android.compose")
    id("convention.android.hilt")
}

android {
    namespace = "com.yunseong.feature.newfeature"
}

dependencies {
    implementation(project(":core:common"))
    implementation(project(":core:ui"))
    
    implementation(libs.androidx.core.ktx)
    implementation(libs.androidx.compose.ui)
}
```

### 4. settings.gradle.kts 등록

```kotlin
include(":feature:newfeature")
```

## 재사용률 측정

```
재사용률 = (재사용 모듈 수 / 전체 모듈 수) × 100
```

- ✅ 80%+: 우수
- ⚠️ 50-80%: 양호  
- ❌ <50%: 중복 확인 필요

## 모범 사례

### DO ✅
- 새 기능 전 기존 모듈 확인
- Version Catalog 사용
- Convention Plugin 사용
- 계층 구조 준수
- 단일 책임 원칙

### DON'T ❌
- 의존성 하드코딩
- 순환 의존성
- Feature 간 직접 의존
- Core에서 Feature 의존
- 과도한 추상화

## 기술 스택

### Android
- **Language**: Kotlin 2.2.10
- **Build**: Gradle 8.13-rc-1, AGP 9.0.0-alpha11
- **UI**: Jetpack Compose (BOM 2024.09.00)
- **DI**: Hilt 2.56.2
- **Database**: Room 2.7.0-alpha12
- **SDK**: min 29, target 36, compile 36

### Backend (Firebase Functions)
- **Runtime**: Node.js 20
- **Language**: TypeScript 5.3
- **Framework**: Firebase Functions v2

### Frontend (Web)
- **Framework**: React 18
- **Language**: TypeScript
- **Build**: Vite

## 더 알아보기

- [프로젝트 구조](README.md)
- [디자인 시스템](docs/PILLYZE_DESIGN_SYSTEM.md)
