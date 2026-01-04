# Lottomate

## Basic Information

- **App Name**: Lottomate
- **Package ID**: `com.yunseong.lottomate`
- **Version Code**: 1
- **Version Name**: 1.0.0
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: Active Development

## Description

Lottomate is a comprehensive lottery management app built with Jetpack Compose. It helps users manage lottery numbers, check winning numbers, view statistics, and scan QR codes for lottery tickets.

## API Keys & Credentials

### Required Environment Variables

Add these to `local.properties` in the root project:

```properties
# Lottomate AdMob Keys
ADMOB_APP_ID=ca-app-pub-xxxxxxxxxxxxxxxx~xxxxxxxxxx
ADMOB_BANNER_ID=ca-app-pub-xxxxxxxxxxxxxxxx/xxxxxxxxxx
```

### Key Details

| Key Name | Purpose | Where to get it | Notes |
|----------|---------|-----------------|-------|
| ADMOB_APP_ID | AdMob application ID | [Google AdMob Console](https://apps.admob.com/) | Required for ad serving |
| ADMOB_BANNER_ID | Banner ad unit ID | [Google AdMob Console](https://apps.admob.com/) | Used for banner advertisements |

## Features

- **Winning Numbers**: View and track lottery winning numbers
- **My Numbers**: Manage personal lottery number combinations
- **Statistics**: Analyze lottery number patterns and statistics
- **Winning Checker**: Check if your numbers have won
- **QR Scanner**: Scan lottery ticket QR codes
- **FCM Notifications**:
  - Winning result notifications (Saturday 20:55)
  - Purchase reminder notifications (Saturday 18:00)

## Dependencies

### Core Modules
- `:core:common` - Common utilities
- `:core:ui` - UI components and theming
- `:core:permission` - Permission handling
- `:core:firebase` - Firebase integration
- `:core:admob` - AdMob integration

### Feature Modules
- `:feature:lotto:common` - Shared lotto functionality
- `:feature:lotto:winning-numbers` - Winning numbers feature
- `:feature:lotto:my-numbers` - Personal numbers management
- `:feature:lotto:statistics` - Statistics and analytics
- `:feature:lotto:winning-checker` - Number checking functionality
- `:feature:qr-scanner` - QR code scanning

### External Services
- **Firebase**:
  - Crashlytics for crash reporting
  - Cloud Messaging (FCM) for push notifications
  - Cloud Functions for scheduled notification triggers
  - Firestore for lottery data storage
- **AdMob**: Mobile advertising platform

## Build Configuration

### Product Flavors

- **dev**: Development flavor with `.dev` suffix
- **prod**: Production flavor

### Build Commands

```bash
# Debug build (dev flavor)
./gradlew :app:lottomate:assembleDevDebug

# Debug build (prod flavor)
./gradlew :app:lottomate:assembleProdDebug

# Release build
./gradlew :app:lottomate:assembleProdRelease

# Install on device
./gradlew :app:lottomate:installDevDebug
```

## Testing

```bash
# Unit tests
./gradlew :app:lottomate:test

# Specific test variant
./gradlew :app:lottomate:testDevDebugUnitTest

# Instrumented tests
./gradlew :app:lottomate:connectedAndroidTest
```

## Architecture

- **UI Framework**: Jetpack Compose with Material 3
- **DI**: Hilt for dependency injection
- **Navigation**: Jetpack Navigation Compose
- **Build System**: Convention Plugins for build logic

## Release Notes

### v1.0.0 (In Development)
- Initial development with core features
- Multi-module architecture implementation
- Firebase and AdMob integration

## Notes

- Uses Convention Plugins for consistent build configuration
- Requires Google Services configuration file (`google-services.json`)
- AdMob keys must be set in `local.properties` before building
- ProGuard enabled for release builds
