---
title: February 2025
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
We are excited to share the February SDK changelog for our product!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# February 26

## [Web 1.13.1](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.13.1)

### Enhancements

* Adds a `ctActionMode` query to handle all URL parameters for the Visual Editor, improving mode selection and query support. 

> 📘 Note
>
> This version is the minimum requirement for using the Visual Editor.

# February 25

## [Cordova 3.4.0](https://github.com/CleverTap/clevertap-cordova)

### What's New

This release introduces key improvements to Android and iOS, including enhanced event tracking and In-App campaign triggers.

#### Android

* Supports [CleverTap Android SDK v7.1.2](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-712-january-29-2025).
* Supports hiding the large icon in Android notifications using the `wzrk_hide_large_icon` key in the payload.

#### iOS

* Supports [CleverTap iOS SDK v7.1.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-710-january-21-2024).

#### Android and iOS

* Supports triggering In-App campaigns based on a combination of recurring and first-time events. For example, trigger a campaign every time the *Charged* event occurs or the *App Launched* event occurs for the first time.
* Supports user-level event log tracking system with the help of the following new APIs:
  * `getUserEventLog()`: Retrieves specific event details.
  * `getUserEventLogCount()`: Retrieves the count of times an event occurred.
  * `getUserLastVisitTs()`: Retrieves the timestamp of the most recent app visit by a user.
  * `getUserAppLaunchCount()`: Retrieves the total number of times a user launched the app.
  * `getUserEventLogHistory()`: Retrieves the complete event history for the current user.

### Important API Changes

The following event tracking APIs previously logged events at the device level, making user-specific tracking inaccurate, especially in multi-user scenarios. They are now deprecated in favor of new user-level APIs and will be removed in future versions with prior notice.

* `eventGetDetail()`: Use `getUserEventLog()`instead.
* `eventGetFirstTime()`: Use `getUserEventLog()` instead.
* `eventGetLastTime()`: Use `getUserEventLog()` instead.
* `eventGetOccurrences()`: Use `getUserEventLogCount()` instead.
* `sessionGetPreviousVisitTime()`: Use `getUserLastVisitTs()` instead.
* `sessionGetTotalVisits()`: Use `getUserAppLaunchCount()` instead.
* `getEventHistory()`: Use `getUserEventLogHistory()` instead.

# February 19

## [Web 1.13.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.13.0)

### What's New

* Adds support for encrypting local storage data via a new config option `clevertap.enableLocalStorageEncryption(true)`.
* Enables selective encryption for specific data keys to improve data protection.

### Bug Fixes

* Fixes a Cross-Site Scripting (XSS) vulnerability in web popups to mitigate security risks.
* For fixing build issues caused due the missing crypto module. update the build configuration to include a fallback:

```Text code
resolve:  
{  
  fallback:  
  {  
    crypto: false  
  }  
}
```

# February 11

## [Unity 5.0.1](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.0.1)

### Bug Fixes

* Fixes an issue that caused Android Gradle project build failures when generating APKs or app bundles.
* Fixes a bug that prevented In-App messages from displaying in full-screen mode on Android.
* Fixes an issue where the build process copied empty asset files on Android and iOS.

# February 06

## [Web 1.12.1](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.12.1)

### What's New

* **Support for Visual Builder**
  * Introduces changelog handling.
  * Introduces dynamic element insertion.

### Enhancements

* Supports custom HTML and JSON templates in Web Native display.  

### Bug Fixes

* Fixes a duplicate session issue on slow networks by appending session IDs from the first request response to subsequent backup event queries, ensuring consistency.
* Adds a check to prevent multiple soft prompts from displaying simultaneously, addressing issues with incorrect prompts appearing in Safari.  
* Improves event processing by adding a validation check to ensure that events and notifications are only triggered for the valid account ID.  
* Optimizes offline event processing by adjusting `setOffline` behavior to only process events, when the offline state changes from `true` to `false`, preventing unintended event triggers.  
* Fixes an issue with the Web Inbox badge in Single Page Applications (SPAs), where the badge was not displaying correctly on route changes.  

# February 05

## [React Native 3.2.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/3.2.0)

### What's New

#### Android

* Supports [CleverTap Android SDK v7.1.2](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-712-january-29-2025).
* Supports hiding the large icon in Android notifications by including the `wzrk_hide_large_icon` key in the notification payload.

#### iOS

* Supports [CleverTap iOS SDK v7.1.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-710-january-21-2024).

#### Android and iOS

* Supports triggering In-App campaigns based on a combination of recurring and first-time events. For example, trigger a campaign every time the *App Launched* event occurs or the *Charged* event occurs for the first time.
* Supports user-level event log tracking system with the help of the following new APIs:
  * `getUserEventLog()`: Retrieves specific event details.
  * `getUserEventLogCount()`: Retrieves the count of times an event occurred.
  * `getUserLastVisitTs()`: Retrieves the timestamp of the most recent app visit by a user.
  * `getUserAppLaunchCount()`: Retrieves the total number of times a user launched the app.
  * `getUserEventLogHistory()`: Retrieves the complete event history for the current user.

### Important API Changes

The following event tracking APIs previously tracked events at the device level, which made it difficult to maintain accurate user-specific event logs, especially in multi-user scenarios. These APIs have now been deprecated in favor of new user-specific APIs. The deprecated methods will be removed from future versions with a prior notice:

* `eventGetDetail()`: Use `getUserEventLog()`instead.
* `eventGetFirstTime()`: Use `getUserEventLog()` instead.
* `eventGetLastTime()`: Use `getUserEventLog()` instead.
* `eventGetOccurrences()`: Use `getUserEventLogCount()` instead.
* `sessionGetPreviousVisitTime()`: Use `getUserLastVisitTs()` instead.
* `sessionGetTotalVisits()`: Use `getUserAppLaunchCount()` instead.
* `getEventHistory()`: Use `getUserEventLogHistory()` instead.
