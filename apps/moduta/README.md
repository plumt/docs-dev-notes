# Moduta

## Basic Information

- **App Name**: Moduta
- **Package ID**: `com.yunseong.moduta`
- **Version Code**: 1
- **Version Name**: 1.0.0
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: Active Development

## Description

Moduta is a Seoul public bus information app built with Jetpack Compose. It provides real-time bus route search using the Seoul Open Data API and a Kakao Map-based live bus location viewer.

## API Keys & Credentials

API keys are stored in `local.properties` and injected via BuildConfig:

| Key Name | Purpose | Where to get it |
|----------|---------|-----------------|
| `SEOUL_BUS_SERVICE_KEY` | Seoul Open Data API service key | [Seoul Open Data Plaza](https://data.seoul.go.kr/) |
| `KAKAO_MAP_APP_KEY` | Kakao Map SDK app key | [Kakao Developers](https://developers.kakao.com/) |

> Note: These keys are loaded from `local.properties` at build time, not from `clients/moduta/config.json`.

## Features

- **Bus Search**: Search Seoul bus routes by route name or number
- **Bus Map**: Real-time bus location viewer on Kakao Map
- **Route Details**: Bus stop list and vehicle positions for a given route

## Dependencies

### App-specific Modules
- `:apps:moduta:core:database` - Room DB for bus route data caching
- `:apps:moduta:feature:bus-search` - Bus route search screen
- `:apps:moduta:feature:bus-map` - Kakao Map bus location screen

### Shared Feature Modules
- `:shared:feature:seoul-bus` - Seoul bus API Clean Architecture (domain + data layer)

### Shared Core Modules
- `:shared:core:fundamental:common` - App preferences, logger, version utils
- `:shared:core:fundamental:permission` - Permission contract interfaces
- `:shared:core:permissions:permission-location` - Location permission
- `:shared:core:common:ui` - Shared Compose UI components
- `:shared:core:common:designsystem` - Material 3 theme and design system
- `:shared:core:common:network` - OkHttp + Retrofit configuration
- `:shared:core:common:location-provider` - FusedLocationProviderClient wrapper

### External SDKs
- **Kakao Map SDK**: Map rendering and bus vehicle marker overlay
- **Retrofit + Gson**: Seoul Open Data API HTTP client

## Build Configuration

### Build Commands

```bash
# Debug build
./gradlew :apps:moduta:assembleDebug

# Release build
./gradlew :apps:moduta:assembleRelease

# Install on device
./gradlew :apps:moduta:installDebug
```

## Testing

```bash
# Unit tests
./gradlew :apps:moduta:test

# Instrumented tests
./gradlew :apps:moduta:connectedAndroidTest
```

## Architecture

- **UI Framework**: Jetpack Compose with Material 3
- **DI**: Hilt
- **Navigation**: Jetpack Navigation Compose (bus_search → bus_map/{busRouteId}/{routeName})
- **Map**: Kakao Map SDK (initialized in Application class)
- **Network**: Retrofit + Gson for Seoul Open Data API
- **Build System**: Convention Plugins

## Notes

- Kakao Map SDK requires `KakaoMapSdk.init()` in the Application class before use
- Route name is URL-encoded when passed as a navigation argument
- Weather feature is intentionally excluded (planned separately with Korea Meteorological Agency API)
- ProGuard enabled for release builds
