---
title: January 2025
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
We are excited to share the January SDK changelog for our product!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# January 31

## [Unity 5.0.0](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/CHANGELOG.md#version-500-31-january-2025)

### Enhancements

* Improved integration and SDK initialization for Android and iOS.

#### Android

* Supports CleverTap [Android SDK v7.1.2](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.1.2).
* Adds missing bindings for Android `GetUnreadInboxMessages` and `GetInboxMessageForId`.

#### iOS

* Supports CleverTap [iOS SDK v7.1.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.1.0).
* Adds `OnCleverTapPushNotificationPermissionStatusCallback` callback for iOS.
* Implements `CleverTapInAppNotificationShowCallback` callback on iOS.

#### Android and iOS

* Supports triggering In-App campaigns based on a combination of recurring and first-time events. For example, trigger a campaign every time the *App Launched* event occurs or the *Charged* event occurs for the first time.
* Supports [Custom Code Templates](https://developer.clevertap.com/docs/unity-custom-code-in-app-templates).
* Adds `CleverTapInboxMessage` model and new methods: `GetAllInboxMessagesParsed`, `GetUnreadInboxMessagesParsed`, and `GetInboxMessageForIdParsed`.
* Adds UserEventLog methods: `GetUserEventLog`, `GetUserEventLogCount`, `GetUserAppLaunchCount`, `GetUserEventLogHistory`, and `GetUserLastVisitTs`.
* Deprecates the following methods: `EventGetDetail`, `EventGetFirstTime`, `EventGetLastTime`, `EventGetOccurrences`, `UserGetEventHistory`, `UserGetPreviousVisitTime`, and `UserGetTotalVisits`.
* Deprecates Product Config and Feature Flags methods.

### Bug Fixes

* Fixes iOS Push Permission Response Received message.

# January 28

## [Web 1.12.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.12.0)

### What's New

* **Safari Web Push Support**
  * Supports Web Push notifications on Safari browsers using Voluntary Application Server Identification (VAPID). 
  * Enables Web Push on Safari iOS Browsers.

### Enhancements

* Replaces hard-coded browser version checks with API-based logic for Web Push, ensuring compatibility across all browsers.
* Adds a sticky header to Web Inbox for improved navigation, making it easier to close the inbox while scrolling through message content.

### Bug Fixes

* Fixes an issue in the Web Inbox where unread message counts were inaccurate for mobile users, ensuring only opened messages are marked as read.
* Resolves Web Inbox badge update issues in Single Page Applications (SPAs) by implementing a timeout-based mechanism to handle route changes more reliably, including native browser navigation.

# January 21

## [iOS 7.1.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.1.0)

### What's New

* Supports triggering In-App campaigns based on a combination of recurring and first-time events. For example, trigger a campaign every time the *App Launched* event occurs or the *Charged* event occurs for the first time.
* Supports user-level event log tracking system with the help of the following new APIs:
  * `getUserEventLog()`: Retrieves specific event details.
  * `getUserEventLogCount()`: Retrieves the count of times an event occurred.
  * `getUserLastVisitTs()`: Retrieves the timestamp of the most recent app visit by a user.
  * `getUserAppLaunchCount()`: Retrieves the total number of times a user launched the app.
  * `getUserEventLogHistory()`: Retrieves the complete event history for the current user.
* Adds `inAppNotificationDidShow` to the `CleverTapInAppNotificationDelegate`.

### Important API Changes

The following event tracking APIs previously tracked events at the device level, which made it difficult to maintain accurate user-specific event logs, especially in multi-user scenarios. These APIs have now been deprecated in favor of new user-specific APIs. The deprecated methods will be removed in future versions with a prior notice:

* `eventGetDetail()`: Use `getUserEventLog()`instead.
* `eventGetFirstTime()`: Use `getUserEventLog()` instead.
* `eventGetLastTime()`: Use `getUserEventLog()` instead.
* `eventGetOccurrences()`: Use `getUserEventLogCount()` instead.
* `userGetPreviousVisitTime()`: Use `getUserLastVisitTs()` instead.
* `userGetTotalVisits()`: Use `getUserAppLaunchCount()` instead.
* `userGetEventHistory()`: Use `getUserEventLogHistory()` instead.

## [Android 7.2.2](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.2.2)

> ❗️ Upgrade Alert
>
> We recommend upgrading to [CleverTap Android SDK 7.3.1](doc:march-2025#android-731) or above if using In-App.

> 📘 Hotfix Release
>
> This hotfix release fixes the critical issue in CleverTap Android SDK [7.1.0](doc:december-2024#android-710), [7.2.0](doc:january-2025#android-720), and [7.2.1](doc:january-2025#january-16).

### Bug Fixes

* Fixes an issue that prevents the `Notification Clicked` event from being raised.

# January 16

## [Android 7.2.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.2.1)

> ❗️ Upgrade Alert
>
> We strongly recommend upgrading to the [CleverTap Android SDK 7.2.2](doc:january-2025#january-21) version because it contains fixes for critical issues in 7.1.0, 7.2.0, and 7.2.1 versions.

### Bug Fixes

* Fixes the `ClassCastException` error that occurs during server-side In-App delivery.

# January 14

## [Web 1.11.15](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.15)

### Bug Fixes

* **Improves click tracking for Web Push Notifications**: Separates event tracking from the redirection URL, ensuring silent event logging without affecting the user’s redirection flow.
* **Resolves Web Inbox initialization failure**: Adds a retry mechanism to check for the Inbox element and initializes it when available.

## [Android 7.2.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.2.0_ptv1.3.0_hmsv1.4.0_geofencev1.4.0)

> ❗️ Upgrade Alert
>
> We strongly recommend upgrading to the [CleverTap Android SDK 7.2.2](doc:january-2025#january-21) version because it contains fixes for critical issues in 7.1.0, 7.2.0, and 7.2.1 versions.

### What's New

* Supports Android 15. For more information, refer to [Android 15](https://developer.android.com/about/versions/15/summary).
* Updates the minimum supported Android SDK version to API level 21 (Android 5.0).
* Enhances the [encryption algorithm for PII data](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/EXAMPLES.md#encryption-of-pii-data-), aligning it with [OWASP guidelines](https://mas.owasp.org/MASTG/0x04g-Testing-Cryptography/). The encryption key is securely backed up using [Android Key Store](https://developer.android.com/privacy-and-security/keystore). This encryption is supported for API level 23 (Android 6.0 and above).

> 🚧 Downgrade Advisory
>
> After upgrading to Android SDK v7.2.0, avoid downgrading in subsequent app releases. If you face any issue after upgrading to this version, contact the [CleverTap Support](https://help.clevertap.com/hc/en-us/requests/new) team for assistance.

## [Android Push Templates 1.3.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.2.0_ptv1.3.0_hmsv1.4.0_geofencev1.4.0)

### What's New

* Supports Android 15. For more information, refer to [Android 15](https://developer.android.com/about/versions/15/summary).
* Updates the minimum supported Android SDK version to API level 21 (Android 5.0).

## [Android Geofence 1.4.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.2.0_ptv1.3.0_hmsv1.4.0_geofencev1.4.0)

### What's New

* Supports Android 15. For more information, refer to [Android 15](https://developer.android.com/about/versions/15/summary).
* Updates the minimum supported Android SDK version to API level 21 (Android 5.0).

## [Android Huawei Push 1.4.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.2.0_ptv1.3.0_hmsv1.4.0_geofencev1.4.0)

### What's New

* Supports Android 15. For more information, refer to [Android 15](https://developer.android.com/about/versions/15/summary).
* Updates the minimum supported Android SDK version to API level 21 (Android 5.0).
