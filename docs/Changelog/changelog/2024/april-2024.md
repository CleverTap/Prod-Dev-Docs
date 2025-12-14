---
title: April 2024
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
We are excited to share the April SDK changelog for our product!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

<br />

# April 19

## [iOS Push Templates - CTNotificationContent 0.2.7](https://github.com/CleverTap/CTNotificationContent/releases/tag/0.2.7)

* Fixes a privacy manifests-related build error. This error occurs when incorporating the SDK via CocoaPods with static linking.
* Fixes the `Missing an expected key: ‘NSPrivacyCollectedDataTypes’` error when generating a privacy report.
* Fixes a location mismatch between the podspec and the privacy manifest location.

## [iOS Push Notification - CTNotificationService 0.1.7](https://github.com/CleverTap/CTNotificationService/releases/tag/0.1.7)

* Fixes a privacy manifests-related build error. This error occurs when incorporating the SDK via CocoaPods with static linking.
* Fixes the `Missing an expected key: ‘NSPrivacyCollectedDataTypes’` error when generating a privacy report.

# April 17

## [Cordova 3.0.0](https://github.com/CleverTap/clevertap-cordova/releases/tag/3.0.0)

### New Features

* **Android Platform**
  * Supports [CleverTap Android SDK v6.0.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev6.0.0_ptv1.2.2).
  * Supports [ExoPlayer v2.19.1](https://github.com/google/ExoPlayer/releases/tag/r2.19.1).
* **iOS Platform**
  * Supports [CleverTap iOS SDK v6.0.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/6.0.0).
* **Android and iOS Platform**
  * Adds new public APIs, `fetchInApps` and `clearInAppResources`, to support client-side In-Apps.

### Bug Fixes

* **Android Platform**
  * Fixes no empty message for App Inbox without tabs.
  * Removes `onClickListener` for the cover In-App's image.
* **iOS Platform**
  * Fixes a bug where some In-Apps were not being dismissed.

# April 12

## [React Native 2.2.1](https://github.com/CleverTap/clevertap-react-native/releases/tag/2.2.1)

### New Features

* **Android Platform**
  * Supports [CleverTap Android SDK v6.2.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-621-april-11-2024).
* **iOS Platform**
  * Supports [CleverTap iOS SDK v6.2.1](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-621-april-12-2024).

### Bug Fixes

* **Android Platform**
  * Fixes a crash due to `IllegalArgumentException` caused by `allowedPushType` XPS enum.
* **iOS Platform**
  * Fixes a build error related to privacy manifests. This issue occurs when incorporating the SDK via CocoaPods with static linking.

## [iOS 6.2.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/6.2.1)

* Fixes a build error related to privacy manifests. This issue occurs when incorporating the SDK via CocoaPods with static linking.

# April 11

## [Android 6.2.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev6.2.1)

This hotfix release addresses the following issue in [CleverTap Android SDK v6.2.0](doc:april-2024#android-620):

### Bug Fixes

* Fixes a crash `IllegalArgumentException` caused by `allowedPushType` Xiaomi Push Service (XPS) enum.

# April 4

## [React Native 2.2.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/2.2.0)

> 📘 Note
>
> We recommend skipping React Native 2.2.0 and update to [React Native 2.2.1](https://developer.clevertap.com/docs/april-2024#react-native-221) or higher.

### New Features

* **Android Platform**
  * Supports [CleverTap Android SDK v6.2.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-620-april-3-2024).
* **iOS Platform**
  * Supports [CleverTap iOS SDK v6.2.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/6.2.0).
  * Updates privacy manifests for iOS SDK.

### API Changes

* Removed all Xiaomi-related public methods, as the Xiaomi Push Service has been discontinued. For more information, refer to the [Xiaomi](https://developer.clevertap.com/docs/discontinuation-of-xiaomi-push-service) document.
* Changes the function definition of `setPushToken` to `setPushToken: function (token, type)`, i.e., it no longer accepts `region` as a parameter.

### Bug Fixes

* **Android Platform**
  * Extends the push primer callback to notify permission denial. This occurs when the cancel button is clicked on the `PromptForSettings` alert dialog.
  * Fixes a crash due to the `NullPointerException` error related to `deviceInfo.deviceId` property.
  * Fixes an ANR related to the`isMainProcess` check.
  * Fixes an Application Not Responding (ANR) due to `lazy` initialization of `CtApi` triggered by `DeviceId` generation.
  * Fixes an Android build issue. This issue is related to package name not being found for apps with `ReactNative` version 0.70 or lower.
* **iOS Platform**
  * Fixes a bug where client-side In-Apps were not discarded. This occurs when the rendering status is set to **discard**.

## [iOS 6.2.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/6.2.0)

* Updates privacy manifests for iOS SDK.
* Fixes a bug where client-side In-Apps were not discarded. This occurs when the rendering status is set to **discard**.

## [Android 6.2.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev6.2.0)

> 📘 Note
>
> Android 6.2.0 produces a crash. Upgrade to Android 6.2.1 or higher.

* **Bug Fixes**
  * Extends the push primer callback to notify permission denial. Implement this extension to trigger when the cancel button is clicked on the `PromptForSettings` alert dialog.
  * Fixes a crash issue [#576](https://github.com/CleverTap/clevertap-android-sdk/issues/576) occurred due to `ClassNotFoundException` error for `CTBackgroundJobService` class. 
  * Fixes a crash due to the `NullPointerException` error related to `deviceInfo.deviceId` property. 
  * Fixes an Application Not Responding (ANR) issue related to the`isMainProcess` check.
  * Fixes an ANR due to lazy initialization of `CtApi` triggered by `DeviceId` generation.
* **Breaking API Changes**
  * Deprecates all Xiaomi-related public methods, as the Xiaomi Push Service has been discontinued. For more information, refer to the [Xiaomi](https://developer.clevertap.com/docs/discontinuation-of-xiaomi-push-service) document.
