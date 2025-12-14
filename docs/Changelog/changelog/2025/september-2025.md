---
title: September 2025
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
We are excited to share the September SDK changelog for our product!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# September 29

## [Huawei Push 1.5.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/hmsv1.5.1)

### What's New

- Adds support for custom initialization of AGConnect (Huawei’s mobile services platform, similar to Firebase) using `agconnect-services.json` from the assets folder.

# September 23

## [Unity 5.3.4](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.3.4)

### What's New

- Adds an enhanced `setOptOut(boolean userOptOut, boolean allowSystemEvents)` method that provides finer control over system events.

# September 22

## [Flutter 3.5.3](https://github.com/CleverTap/clevertap-flutter/releases/tag/3.5.3)

### What's New

This release updates the Flutter SDK with the latest platform support and includes a bug fix for the Web SDK.

- Supports [CleverTap Web SDK v2.3.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v2.3.0)
- Supports [CleverTap Android SDK v7.5.2](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-752-september-11-2025)
- Supports [CleverTap iOS SDK v7.3.3](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-733-september-11-2025)

### Bug Fixes

- Fixed an issue where `addKVDataChangeListener()` did not return key-value data in the Web SDK.

# September 16

## [Web 2.3.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v2.3.0)

- Adds support to reorder elements in list using the Visual Builder. 

# September 15

## [Unity 5.3.3](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.3.3)

### What's New

This release updates the Unity SDK with the latest Android and iOS platform support.

- Supports [CleverTap Android SDK v7.5.2](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.5.2).
- Supports [CleverTap iOS SDK v7.3.3](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.3.3).

# September 11

## [Android 7.5.2](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.5.2)

### Improvements

- Optimizes the queuing behavior of different types of CleverTap events into a single batch.

### Bug Fixes

- Fixes an issue where updated variable values were not fetched immediately after a profile push.

## [iOS 7.3.3](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.3.3)

### Improvements

- Optimizes the queuing behavior of different types of CleverTap events into a single batch.

### Bug Fixes

- Fixes an issue where updated variable values were not fetched immediately after a profile push.

# September 08

## [CleverTap Segment Swift 1.0.0](https://github.com/CleverTap/clevertap-segment-swift/releases/tag/1.0.0)

### What’s New

This is the first release of the CleverTap Segment Swift integration for iOS.

- Supports [Analytics-Swift v1.8.0](https://github.com/segmentio/analytics-swift) and [CleverTap iOS SDK v7.3.2](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-732-august-08-2025).

## [CleverTap Segment Kotlin 1.0.0](https://github.com/CleverTap/analytics-kotlin-clevertap/releases/tag/1.0.0)

### What’s New

This is the first release of the CleverTap Segment Kotlin integration for Android.

- Supports [Analytics-Kotlin v1.21.0](https://github.com/segmentio/analytics-kotlin) and [CleverTap Android SDK v7.5.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-751-august-28-2025).

# September 02

## [Web 2.2.2](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v2.2.2)

### Bug Fixes

- Fixes Safari Web Push handling on iPhones.

## [Web 2.2.1](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v2.2.1)

### Bug Fixes

- Resolves an issue where image-only popups without a close icon could cause a runtime error. The SDK now attaches the close action only when a close icon is present, preventing errors and ensuring stability.

# September 01

## [Unity 5.3.2](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.3.2)

### What's New

- Supports [CleverTap Android SDK v7.5.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-751-august-28-2025).
- Supports [CleverTap iOS SDK v7.3.2](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-732-august-08-2025).