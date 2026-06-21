# Lunar Phase

## Basic Information

- **App Name**: Lunar Phase
- **Package ID**: `com.yun.lunarphase`
- **Version Code**: 7
- **Version Name**: 1.3.2
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: Active

## Description

Lunar Phase is an app that displays moon phase information using external lunar data API and the Meeus astronomical algorithm. Built with Jetpack Compose following Clean Architecture with multi-module structure.

## API Keys & Credentials

API keys are managed via `clients/lunar/config.json` and encrypted into NDK binaries at build time. The following keys are required:

| Key Name | Purpose |
|----------|---------|
| `lunar_api_key` | Access lunar phase data API |
| `lunar_base_url` | Lunar API base URL |
| `admob_app_id` | AdMob application ID |
| `admob_banner_id` | Banner ad unit ID |

Keystore secrets are stored separately in `local.properties`:
- `KEYSTORE_FILE`, `KEYSTORE_PASSWORD`, `KEY_ALIAS`, `KEY_PASSWORD`

## Features

- **Moon Phase Display**: Current moon phase visualization
- **Lunar Phase Calculator**: Astronomical calculation using Meeus algorithm + external API
- **Phase Database**: Local caching of moon phase data (Room)
- **FCM Notifications**: Moon-related push notifications
- **App Update**: Force update via Remote Config
- **OSS Licenses**: Open source licenses screen
- **AdMob**: Banner advertisements

## Dependencies

### App-specific Modules
- `:apps:lunar:core:common` - Shared constants and utilities within the app
- `:apps:lunar:core:database` - Room DB for moon phase data caching
- `:apps:lunar:feature:moon-phase` - Moon phase display screen (external API)
- `:apps:lunar:feature:lunar-phase-calculator` - Astronomical calculation screen

### Shared Feature Modules
- `:shared:feature:app-update` - Force update dialog via Remote Config
- `:shared:feature:oss-licenses` - Open source licenses screen

### Shared Core Modules
- `:shared:core:fundamental:common` - App preferences, logger, version utils
- `:shared:core:fundamental:permission` - Permission contract interfaces
- `:shared:core:fundamental:local-notification` - Local notification channel and dispatch
- `:shared:core:permissions:permission-notification` - Notification permission
- `:shared:core:common:ui` - Shared Compose UI components
- `:shared:core:common:designsystem` - Material 3 theme and design system
- `:shared:core:common:admob` - AdMob banner ad integration
- `:shared:core:firebase:messaging` - FCM message router (Firebase project: `lunarphase-7535e`)
- `:shared:core:lunar:calculator` - Shared Meeus astronomical calculation library

### Key Libraries
- **Hilt**: Dependency injection
- **Joda-Time**: Date and time handling
- **Kotlin Coroutines**: Asynchronous operations
- **Jetpack Compose**: Modern UI toolkit
- **DataStore Preferences**: Notification settings persistence

### External Services
- **Firebase**: Cloud Messaging (FCM) — separate Firebase project `lunarphase-7535e` (distinct from Lottomate's `lottomate-ce9f6`)

## Build Configuration

### Build Commands

```bash
# Debug build
./gradlew :apps:lunar:assembleDebug

# Release build
./gradlew :apps:lunar:assembleRelease

# Install on device
./gradlew :apps:lunar:installDebug
```

## Testing

```bash
# Unit tests
./gradlew :apps:lunar:test

# Instrumented tests
./gradlew :apps:lunar:connectedAndroidTest
```

## Architecture

- **Pattern**: Clean Architecture + MVVM
- **UI Framework**: Jetpack Compose with Material 3
- **DI**: Hilt
- **Async**: Kotlin Coroutines
- **Date Library**: Joda-Time for reliable date calculations
- **Security**: NDK/JNI XOR encryption for API keys
- **Build System**: Convention Plugins

## Release Notes

### v1.3.2 (Current)
- Dual moon phase calculation: external API + Meeus algorithm
- Multi-module architecture with app-specific core and feature modules
- FCM push notifications for moon events
- Local notification scheduling

### Previous Versions
- Version history from v1.0.0 to v1.3.1

## Notes

- Uses a **separate Firebase project** (`lunarphase-7535e`) from Lottomate — do not mix `google-services.json` files
- All API keys are stored in `clients/lunar/config.json` and encrypted at build time
- ProGuard enabled for release builds
- DataStore is used for notification settings persistence (not Room)
