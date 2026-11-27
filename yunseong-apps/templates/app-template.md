# [App Name]

## Basic Information

- **App Name**: 
- **Package ID**: (출시 앱만. 미출시 앱은 적지 않는다)
- **Version Code**: 
- **Version Name**: 
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: Active / In Development / Deprecated

## Description

[Brief description of what this app does]

## 키와 설정값

키 이름, 설정 파일 경로, 토픽·컬렉션 이름, 딥링크 스킴 같은 앱 상수는 이 공개 문서에 적지 않는다. "키는 빌드 때 NDK 로 숨긴다" 수준만 쓴다.

## Features

- Feature 1
- Feature 2
- Feature 3

## Dependencies

### Core Modules
- List core dependencies

### External Services
- Firebase (if applicable)
- AdMob (if applicable)
- Other APIs

## Build Configuration

### Build Commands

```bash
# Debug build
./gradlew :app:[app-name]:assembleDebug

# Release build
./gradlew :app:[app-name]:assembleRelease

# Install on device
./gradlew :app:[app-name]:installDebug
```

### Flavors

[If applicable, describe product flavors]

## Testing

```bash
# Unit tests
./gradlew :app:[app-name]:test

# Instrumented tests
./gradlew :app:[app-name]:connectedAndroidTest
```

## Release Notes

### v1.0.0 (YYYY-MM-DD)
- Initial release

## Notes

[Any additional notes, gotchas, or important information]
