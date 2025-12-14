---
title: December 2024
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
We are excited to share CleverTap's December SDK changelog!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# December 24

## [Android 7.1.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.1.0)

> ❗️ Upgrade Alert
> 
> We strongly recommend upgrading to the [CleverTap Android SDK 7.2.2](doc:january-2025#january-21) version because it contains fixes for critical issues in 7.1.0, 7.2.0, and 7.2.1 versions.

### What's New

- Supports triggering In-App campaigns based on a combination of recurring and first-time events. For example, trigger a campaign every time the _App Launched_ event occurs or the _Charged_ event occurs for the first time.
- Supports hiding the large icon in Android notifications by including the `wzrk_hide_large_icon` key in the notification payload.
- Supports user-level event log tracking system with the help of the following APIs:
  - `getUserEventLog()`: Retrieves specific event details.
  - `getUserEventLogCount()`: Retrieves the count of times an event occurred.
  - `getUserLastVisitTs()`: Retrieves the timestamp of the most recent app visit by a user.
  - `getUserAppLaunchCount()`: Retrieves the total number of times a user launched the app. 
  - `getUserEventLogHistory()`: Retrieves the full event history for the current user.

### Important API Changes

The following event tracking APIs previously tracked events at the device level, which made it difficult to maintain accurate user-specific event logs, especially in multi-user scenarios. These APIs have now been deprecated in favor of new user-specific APIs. The deprecated methods will be removed in future versions with prior notice:

- `getDetails()`: Use `getUserEventLog()`instead.
- `getFirstTime()`: Use `getUserEventLog()` instead.
- `getLastTime()`: Use `getUserEventLog()` instead.
- `getCount()`: Use `getUserEventLogCount()` instead.
- `getPreviousVisitTime()`: Use `getUserLastVisitTs()` instead.
- `getTotalVisits()`: Use `getUserAppLaunchCount()` instead.
- `getHistory()`: Use `getUserEventLogHistory()` instead.

### Bug Fixes

- Fixes error [#671](https://github.com/CleverTap/clevertap-android-sdk/issues/671), which caused an `AbstractMethodError` in the App Inbox feature when using Audio/Video.
- Fixes an issue where File Type Variables changed from `validValue` to `null` unexpectedly.

# December 19

## [Web 1.11.14](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.14)

- Fixes an issue with the `markReadInboxMessage` API, which was not functioning as expected for custom Web Inbox implementation.

## [Flutter 3.0.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/3.0.0)

### What's New

#### Android

- Supports [CleverTap Android SDK v7.0.3](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-703-november-29-2024).
- Supports the custom handshake domain configuration in Android manifest.

#### iOS

- Supports [CleverTap iOS SDK v7.0.3](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-703-november-29-2024).
- Supports the custom handshake domain configuration.

#### Android and iOS

- Supports File Type Variables in Remote Config. For more information on integration, refer to [Remote Config Variables](https://github.com/CleverTap/clevertap-flutter/blob/master/doc/Variables.md).
- Supports Custom Code Templates. For more information on integration, refer to [Custom Code Templates ](https://github.com/CleverTap/clevertap-flutter/blob/master/doc/CustomCodeTemplates.md).
- Supports Custom Code In-App template definitions through a JSON scheme.

### Bug Fixes

#### Android

- Fixed a Null Pointer Exception (NPE) in the PN clicked event caused by casting an object to a string [#275](https://github.com/CleverTap/clevertap-flutter/issues/275).

# December 18

## [Web 1.11.13](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.13)

- Fixes a web push soft prompt bug that caused it to render repeatedly upon page refresh, even after users enabled browser-native permissions.

# December 11

## [Web 1.11.12](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.12)

- Initialize the Web Inbox when the `readyState` of the `document` object changes to `complete`.

# December 6

## [Web 1.11.11](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.11)

- Adds support for Personalization in Visual Editor.

# December 3

## [Cordova 3.3.0](https://github.com/CleverTap/clevertap-cordova/releases/tag/3.3.0)

### What's New

#### Android

- Supports [CleverTap Android SDK v7.0.3](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-703-november-29-2024).
- Removes the 3-line character limit for App Inbox messages.
- Supports  `AndroidX Media3` due to the [`ExoPlayer` deprecation](https://developer.android.com/media/media3/exoplayer/migration-guide).  While `ExoPlayer` remains supported, migration is recommended. For more information, refer to [ExoPlayer Migration Guide](https://github.com/CleverTap/clevertap-cordova/blob/3.3.0/docs/Integrate-Android.md#migrating-from-exoplayer-to-androidx-media3-optional).

#### iOS

- Supports [CleverTap iOS SDK v7.0.3](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-703-november-29-2024).

#### Android and iOS

- Supports triggering In-App Messages based on user attribute changes.
- Supports File Type Variables in Remote Config. For more information on integration, refer to [Remote Config Variables](https://github.com/CleverTap/clevertap-cordova/blob/3.3.0/docs/Variables.md).
- Supports Custom Code Templates. For more information on integration, refer to [Custom Code Templates ](https://github.com/CleverTap/clevertap-cordova/blob/3.3.0/docs/CustomCodeTemplates.md).
- Updates campaign triggering evaluation to ignore letter case and whitespace in event names, event properties, and profile properties.
- Supports previewing In-App messages created using the advanced builder.
- Supports the custom handshake domain configuration.

### Bug Fixes

#### Android

- Fixes an Application Not Responding (ANR) error caused by extremely old In-App campaigns.
- Fixes an issue where incorrect callbacks were sent for In-App messages when the phone was rotated.
- Fixes an issue where an In-App message was displayed even after all the campaigns were stopped.
- Fixes an issue where the In-App image was not displayed when the phone was rotated to landscape.
- Fixes an issue where certain URLs are loaded incorrectly in Custom HTML In-App templates.