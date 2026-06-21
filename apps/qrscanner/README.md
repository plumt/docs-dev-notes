# QR Scanner (QRing)

## Basic Information

- **App Name**: QR Scanner
- **Package ID**: `com.yunseong.qring`
- **Version Code**: 1
- **Version Name**: 1.0.0
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: Active Development

## Description

QRing is a QR code scanner and generator app built with Jetpack Compose. It supports single QR scanning (ZXing), multi QR scanning (ML Kit), QR code generation with custom logo/color, and scan history management.

## API Keys & Credentials

Firebase configuration via `google-services.json`:
- Crashlytics for crash reporting
- Remote Config for force update

No additional API keys are required. Firebase setup is the only external dependency.

## Features

- **QR Scanner**: Single QR code scanning using ZXing (no ML Kit required)
- **Multi QR Scanner**: Simultaneous multi-QR detection using ML Kit
- **QR Generator**: Generate QR codes with custom logo and color options
- **Scan History**: Local scan history with type classification (URL, text, etc.)
- **App Update**: Force update dialog via Remote Config
- **OSS Licenses**: Open source licenses screen
- **WebView**: In-app browser for URLs from QR results

## Dependencies

### Shared Feature Modules
- `:shared:feature:qr:common` - QR history Room DB and type classification
- `:shared:feature:qr:multi-scanner` - ML Kit multi-QR scanning
- `:shared:feature:qr:generator` - ZXing QR code generation
- `:shared:feature:app-update` - Force update dialog
- `:shared:feature:oss-licenses` - Open source licenses screen
- `:shared:feature:webview` - WebView screen for URL results

### Shared Core Modules
- `:shared:core:fundamental:common` - App preferences, logger, version utils
- `:shared:core:fundamental:permission` - Permission contract interfaces
- `:shared:core:common:ui` - Shared Compose UI components
- `:shared:core:common:designsystem` - Material 3 theme and design system
- `:shared:core:firebase:core` - Firebase BOM
- `:shared:core:firebase:crashlytics` - Crash reporting
- `:shared:core:firebase:remote-config` - Remote Config and version check

### External Services
- **Firebase**: Crashlytics, Remote Config

## Build Configuration

### Product Flavors

- **dev**: Development flavor with `.dev` applicationId suffix
- **prod**: Production flavor

### Build Commands

```bash
# Debug build (dev flavor)
./gradlew :apps:qrscanner:assembleDevDebug

# Release build
./gradlew :apps:qrscanner:assembleProdRelease

# Install on device
./gradlew :apps:qrscanner:installDevDebug
```

## Testing

```bash
# Unit tests
./gradlew :apps:qrscanner:test

# Instrumented tests
./gradlew :apps:qrscanner:connectedAndroidTest
```

## Architecture

- **UI Framework**: Jetpack Compose with Material 3
- **DI**: Hilt
- **Navigation**: Jetpack Navigation Compose
- **Build System**: Convention Plugins

## Notes

- Requires `google-services.json` for Firebase
- Single scanner uses ZXing (lighter) — ML Kit only for multi-scanner
- ProGuard enabled for release builds
