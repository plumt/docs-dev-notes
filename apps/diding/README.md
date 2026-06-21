# Diding

## Basic Information

- **App Name**: Diding
- **Package ID**: `com.yunseong.diding`
- **Version Code**: 1
- **Version Name**: 1.0.0
- **Min SDK**: 29
- **Target SDK**: 36
- **Status**: Active Development

## Description

Diding is a TODO management app with alarm scheduling. Built with Jetpack Compose and Clean Architecture, it provides a calendar view, home screen for today's tasks, and full CRUD task management with local notifications.

## API Keys & Credentials

No external API keys required. Diding is a fully offline app with no external service dependencies.

## Features

- **Home**: Today's TODO list at a glance
- **Calendar**: Monthly calendar view with task indicators
- **Management**: Create, read, update, and delete TODO items
- **Settings**: App settings (theme, notification preferences)
- **Alarm Scheduling**: AlarmManager-based reminder notifications for tasks
- **Local Notifications**: In-app notification channel for task reminders

## Dependencies

### App-specific Modules
- `:apps:diding:core:database` - Room DB for TODO items and alarm scheduling
- `:apps:diding:feature:home` - Home screen (today's TODOs)
- `:apps:diding:feature:calendar` - Calendar screen
- `:apps:diding:feature:management` - TODO CRUD management screen
- `:apps:diding:feature:setting` - Settings screen

### Shared Core Modules
- `:shared:core:fundamental:common` - App preferences, logger, version utils
- `:shared:core:fundamental:permission` - Permission contract interfaces
- `:shared:core:fundamental:local-notification` - Notification channel and dispatch
- `:shared:core:fundamental:alarm` - AlarmManager compatibility utilities
- `:shared:core:common:ui` - Shared Compose UI components
- `:shared:core:common:designsystem` - Material 3 theme and design system

## Build Configuration

### Build Commands

```bash
# Debug build
./gradlew :apps:diding:assembleDebug

# Release build
./gradlew :apps:diding:assembleRelease

# Install on device
./gradlew :apps:diding:installDebug
```

## Testing

```bash
# Unit tests
./gradlew :apps:diding:test

# Instrumented tests
./gradlew :apps:diding:connectedAndroidTest
```

## Architecture

- **UI Framework**: Jetpack Compose with Material 3
- **DI**: Hilt
- **Navigation**: Jetpack Navigation Compose
- **Local Data**: Room DB (SQLite)
- **Notifications**: AlarmManager + NotificationManager
- **Build System**: Convention Plugins

## Notes

- Fully offline — no Firebase or external API dependencies
- Uses `AlarmManager` with `USE_EXACT_ALARM` permission for reliable task reminders
- ProGuard enabled for release builds
