---
title: October 2025
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
We are excited to share the October SDK changelog for our product!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# October 30

## [Expo 0.0.3](https://github.com/CleverTap/clevertap-expo-plugin/releases/tag/0.0.3)

> 🚧 Public Beta
>
> This is a beta release. The build is fully functional but may contain minor issues that will be addressed in future updates.

### What’s new

* Adds support for [Expo SDK 53.0.0](https://expo.dev/changelog/sdk-53) and [React Native 0.79](https://reactnative.dev/blog/2025/04/08/react-native-0.79), ensuring compatibility with the latest framework versions.
* Adds support for [CleverTap React Native SDK v3.7.0](https://github.com/CleverTap/clevertap-react-native/blob/master/CHANGELOG.md#version-370-october-3-2025) to enable improved integration and enhanced performance.
* Adds the `encryptionInTransit` parameter to secure all event data sent over the network through end-to-end encryption.
* Adds the `enablePlayReview` feature for Android, allowing users to submit Google Play In-App Reviews as a System In-App Function.

# October 29

## [iOS 7.3.4](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.3.4)

### Bug Fixes

* Fixes a potential crash related to Core Data concurrency in App Inbox.
* Fixes an issue where multiple profiles were created when `onUserLogin` was called through a custom CleverTap instance.
* Fixes conflicts with SDWebImage when other Swift Package Manager dependencies also use SDWebImage.

# October 17

## [Android Push Templates 2.2.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTPUSHTEMPLATESCHANGELOG.md#version-220-october-17-2025)

### What’s New

* Adds support for displaying GIFs in *Basic*, *Timer*, *Zero Bezel* and *Rating* templates.
* Adds an option to configure a notification as sticky on *Android 14+* devices using `pt_sticky: true/false`.
* Adds an option to configure automatic dismissal of the notification after a set time on *Android 8+* devices using `pt_dismiss (value in seconds)`.
* Adds support to show different media in the expanded and collapsed views of the *Zero Bezel* template.
* Optimizes image download to prevent duplicate fetches.

### Bug Fixes

* Fixes an issue to ensure the *chronometer* and the *title/message* don't overlap in the *Timer* template.
* Fixes an issue where the *Zero Bezel* and *Carousel* templates did not correctly fall back to text-only notifications when media download failed.

## [Android 7.6.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-760-october-17-2025)

### What’s New

* Adds an option to configure a notification as sticky on *Android 14+* devices using `wzrk_sticky: true/false`.
* Adds an option to configure automatic dismissal of the notification after a set time on *Android 8+* devices using `wzrk_dismiss (value in seconds)`.

### Bug Fixes

* Fixes an issue that caused unintended padding around the Toolbar in `CTInboxActivity` for edge-to-edge layouts.

# October 08

## [Unity 5.3.6](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.3.6)

### What’s New

* Links Native Display methods from `androidlib` to `C#` in `AndroidPlatformBinding` to ensure consistent CleverTap SDK behavior across platforms.

## [Cordova 4.3.0](https://github.com/CleverTap/clevertap-cordova/releases/tag/4.3.0)

### What’s New

* Supports [CleverTap Android SDK v7.5.2](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-752-september-11-2025).
* Supports [CleverTap iOS SDK v7.3.3](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-733-september-11-2025).

### Bug Fixes

* Fixes an issue where the `profileGetProperty` method did not return `null` for undefined keys.

# October 07

## [Unity 5.3.5](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.3.5)

### Bug Fixes

* Adds an internal `Path` property in `UnityNativeRequest` to correctly resolve the request path.

# October 03

## [React Native 3.7.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/3.7.0)

### What’s New

* Supports [CleverTap Android SDK v7.5.2](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-752-september-11-2025).
* Supports [CleverTap iOS SDK v7.3.3](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-733-september-11-2025).

### Bug Fixes

* Fixes an issue where dates were incorrectly parsed for the case-sensitive DOB key.
