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

Lottomate is a comprehensive lottery management app built with Jetpack Compose. It helps users manage lottery numbers, check winning numbers, view statistics, and receive FCM push notifications for lottery results.

## API Keys & Credentials

API keys and URLs are managed via `clients/lottomate/config.json` and encrypted into NDK binaries at build time. The following keys are required:

| Key Name | Purpose |
|----------|---------|
| `admob_app_id` | AdMob application ID (dev) |
| `admob_prod_app_id` | AdMob application ID (prod) |
| `admob_banner_id` | Banner ad unit ID |
| `admob_native_id` | Native ad unit ID |
| `lotto_api_base_url` | Lottery API base URL |
| `lotto_official_intro_url` | Official lottery intro URL |
| `lotto_official_main_url` | Official lottery main URL |
| `firebase_privacy_url` | Firebase privacy policy URL |
| `app_privacy_policy_url` | App privacy policy URL |
| `app_terms_of_service_url` | Terms of service URL |
| `fcm_topic_lotto_check` | FCM topic for winning result notifications |
| `fcm_topic_purchase_reminder` | FCM topic for purchase reminder notifications |

Keystore secrets are stored separately in `local.properties`:
- `KEYSTORE_FILE`, `KEYSTORE_PASSWORD`, `KEY_ALIAS`, `KEY_PASSWORD`

## Features

- **Winning Numbers**: View and sync lottery winning numbers from API
- **My Numbers**: Save and manage personal lottery number combinations
- **Statistics**: Analyze lottery number patterns and statistics with charts
- **Winning Checker**: Check if your saved numbers have won
- **Countdown**: Countdown timer to next lottery draw
- **QR Scanner**: Scan lottery ticket QR codes
- **FCM Notifications**:
  - Winning result notifications (Saturday 20:55 KST)
  - Purchase reminder notifications (Saturday 18:00 KST)
- **App Update**: Force update via Remote Config
- **Notification Inbox**: In-app notification history

## Dependencies

### App-specific Modules
- `:apps:lottomate:core:common` - Lotto domain types and constants
- `:apps:lottomate:core:database` - Room DB for winning numbers and my numbers
- `:apps:lottomate:feature:winning-numbers` - Winning numbers display and sync
- `:apps:lottomate:feature:my-numbers` - Personal numbers management
- `:apps:lottomate:feature:statistics` - Statistics and analytics
- `:apps:lottomate:feature:winning-checker` - Number checking
- `:apps:lottomate:feature:countdown` - Draw countdown timer

### Shared Feature Modules
- `:shared:feature:qr:common` - QR history DB and type classification
- `:shared:feature:qr:scanner` - QR code scanning
- `:shared:feature:chart` - Vico chart components
- `:shared:feature:notification-inbox` - Notification history screen
- `:shared:feature:notification-settings` - FCM topic toggle settings
- `:shared:feature:app-update` - Force update dialog
- `:shared:feature:oss-licenses` - Open source licenses screen
- `:shared:feature:webview` - WebView for policy pages

### Shared Core Modules
- `:shared:core:fundamental:common` - App preferences, logger, version utils
- `:shared:core:fundamental:permission` - Permission contract interfaces
- `:shared:core:permissions:permission-notification` - Notification permission
- `:shared:core:common:ui` - Shared Compose UI components
- `:shared:core:common:designsystem` - Material 3 theme and design system
- `:shared:core:common:admob` - AdMob banner/native ad integration
- `:shared:core:firebase:core` - Firebase BOM
- `:shared:core:firebase:crashlytics` - Crash reporting
- `:shared:core:firebase:remote-config` - Remote Config and version check
- `:shared:core:firebase:messaging` - FCM topic management

### External Services
- **Firebase**: Crashlytics, Cloud Messaging (FCM), Remote Config
- **Firestore**: Lottery data storage (via Cloud Functions)
- **AdMob**: Banner and native advertisements

## Build Configuration

### Product Flavors

- **dev**: Development flavor with `.dev` applicationId suffix
- **prod**: Production flavor

### Build Commands

```bash
# Debug build (dev flavor)
./gradlew :apps:lottomate:assembleDevDebug

# Debug build (prod flavor)
./gradlew :apps:lottomate:assembleProdDebug

# Release build
./gradlew :apps:lottomate:assembleProdRelease

# Install on device
./gradlew :apps:lottomate:installDevDebug
```

## Testing

```bash
# Unit tests
./gradlew :apps:lottomate:test

# Specific test variant
./gradlew :apps:lottomate:testDevDebugUnitTest

# Instrumented tests
./gradlew :apps:lottomate:connectedAndroidTest
```

## Architecture

- **UI Framework**: Jetpack Compose with Material 3
- **DI**: Hilt
- **Navigation**: Jetpack Navigation Compose
- **Security**: NDK/JNI XOR encryption for API keys and URLs
- **Build System**: Convention Plugins

## Release Notes

### v1.0.0 (In Development)
- Initial release with winning numbers, my numbers, statistics, winning checker, countdown
- QR scanner for lottery tickets
- FCM push notifications for lottery results and purchase reminders
- Multi-module Clean Architecture

## Notes

- Uses Convention Plugins for consistent build configuration
- Requires `google-services.json` (Firebase project: `lottomate-ce9f6`)
- All API keys are stored in `clients/lottomate/config.json` and encrypted at build time — do not use `local.properties` for these
- ProGuard enabled for release builds
