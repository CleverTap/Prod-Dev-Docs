---
title: March 2024
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
We are excited to share the March 2024 SDK changelog for our product!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# March 28

## [Web 1.7.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.7.0)

- Introduced Product Experiences in the Web SDK.
- Fixed a carousel preview bug.

# March 20

## [iOS Geofence 1.0.5](https://github.com/CleverTap/clevertap-geofence-ios/releases/tag/1.0.5)

- Supports [CleverTap iOS SDK v6.1.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/6.1.0).
- Adds privacy manifest for iOS Geofence SDK.

## [iOS Segment 1.3.0](https://github.com/CleverTap/clevertap-segment-ios/releases/tag/1.3.0)

- Supports [CleverTap iOS SDK v6.1.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/6.1.0).
- Adds privacy manifest for iOS Segment SDK.

## [iOS Push Templates for CTNotificationContent 0.2.6](https://github.com/CleverTap/CTNotificationContent/releases/tag/0.2.6)

- Adds privacy manifest for `CTNotificationContent` SDK.

## [iOS Push Notification for CTNotificationService 0.1.6](https://github.com/CleverTap/CTNotificationService/releases/tag/0.1.6)

- Adds privacy manifest for `CTNotificationService` SDK.

## [Flutter 2.2.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/2.2.0)

### New Features

- **Android Platform**
  - Supports [CleverTap Android SDK v6.1.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-611-february-27-2024).
  - Supports Android 14. It is compliant with Android 14 requirements. For more information, refer to the [Android](https://developer.android.com/about/versions/14/summary) documentation. 
  - Upgrades Android Gradle Plugin (AGP) to v8.3.0 for building the SDK and adds related consumer proguard rules.
  - Deprecates Xiaomi public methods as we are sunsetting the Xiaomi SDK. For more information, refer to the [Xiaomi](https://dev.mi.com/distribute/doc/details?pId=1555) document.
  - Adds accessibility IDs for UI components of the Android SDK.
  - Migrates `JobScheduler` to `WorkManager` for Pull Notifications.
- **iOS Platform**
  - Supports [CleverTap iOS SDK v6.1.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/6.1.0).
  - Adds privacy manifests for `CleverTap-iOS-SDK` and `CleverTapLocation`.
  ### Bug Fixes
- **Android Platform**
  - Fixes a rare activity destroyed race condition causing crashes in In-App.
  - Fixes a potential Application Not Responding (ANR) issue arising from a race condition during SDK initialization in a multi-threaded environment.
  - Fixes [#211](https://github.com/CleverTap/clevertap-flutter/issues/211).
- **iOS Platform**
  - Fixes a crash due to out-of-bounds in `NSLocale` implementation.

# March 8

## [React Native 2.1.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/2.1.0)

### New Features

**Android Platform**

- Supports CleverTap [Android SDK 6.1.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-611-january-15-2024).
- Supports Android 14.  It is compliant with Android 14 requirements. For more information, refer to the [Android](https://developer.android.com/about/versions/14/summary) documentation.
- Upgrades the Android Gradle Plugin (AGP) to 8.2.2 for building the SDK and adds related consumer proguard rules.
- Deprecates Xiaomi public methods as we are sunsetting SDK. For more information, refer to the [Xiaomi](https://dev.mi.com/distribute/doc/details?pId=1555) document.
- Adds accessibility IDs for UI components of SDK.
- Migrates `JobScheduler` to `WorkManager` for Pull Notifications.

**iOS Platform**

- Supports [CleverTap iOS SDK 6.1.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/6.1.0).
- Adds privacy manifests for CleverTap-iOS-SDK and CleverTapLocation.

### Bug Fixes

**Android Platform**

- Fixes a rare activity destroyed race condition causing crashes in In-App.
- Fixes a potential Application Not Responding (ANR) issue arising from a race condition during SDK initialization in a multi-threaded environment.

**iOS Platform**

- Fixes a crash due to out-of-bounds in `NSLocale` implementation.