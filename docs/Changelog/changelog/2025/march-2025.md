---
title: March 2025
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
We are excited to share the March SDK changelog for our product!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# March 31

## [Unity 5.2.0](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.2.0)

### Enhancements

* Supports Unity Native Variables, allowing users to define and sync variables directly within the Unity Editor and across builds. This simplifies configuration management between development and production environments.
* Enhances compatibility with Unity version 2021 for native features.

### Bug Fixes

* Fixes WebGL compatibility issues for Unity version 6000.

# March 27

## [React Native 3.4.0](https://github.com/CleverTap/clevertap-react-native/releases)

### What's New

#### Android

* Supports [CleverTap Android SDK v7.3.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-731-march-27-2025).

#### iOS

* Supports [CleverTap iOS SDK v7.1.1](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-711-march-17-2024).

### Important API Changes

* Adds the following new APIs to register push tokens:
  * Use `pushRegistrationToken(your_token, pushType)` to register tokens for providers other than FCM. For more information, refer to [Registering Baidu Huawei Token](https://github.com/CleverTap/clevertap-react-native/blob/3.4.0/docs/usage.md#registering-hpsbaidu-token) for example usage.
    > 🚧 Note
    >
    > For revised integration steps on Android, refer to [Huawei Push integration](doc:clevertap-huawei-push-integration).
  * Use `setFCMPushToken(your_token)` specifically for FCM token registration. For more information, refer to [Registering FCM Token ](https://github.com/CleverTap/clevertap-react-native/blob/3.4.0/docs/usage.md#registering-fcm-token) for example usage.

* Removes the legacy API:
  * Removes `setPushToken(value, type)` to support injectable push providers. Use the updated APIs listed above instead.

## [Flutter 3.3.0](https://github.com/CleverTap/clevertap-flutter/releases)

### What's New

* Supports [CleverTap Android SDK v7.3.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-731-march-27-2025).
* Adds the following new API to register to push tokens for non-FCM providers:
  * `pushRegistrationToken(String value, Map<String, String> pushType)` registers the token based on the push provider. For example usage, refer to [Registering FCM Baidu Huawei Token](https://github.com/CleverTap/clevertap-flutter/blob/3.3.0/doc/Usage.md#registering-fcm-baidu-huawei-token).
    > 🚧 Note
    >
    > For revised integration steps on Android, refer to [Huawei Push integration](doc:clevertap-huawei-push-integration).

### Important API Changes

#### Android

* Removes the following legacy push token registration APIs to enable injectable push providers:
  * `setBaiduPushToken(String value)`
  * `setHuaweiPushToken(String value)`

## [Android 7.3.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.3.1)

### Bug Fixes

* Fixes an issue that prevented In-App messages from displaying after the app was upgraded.

# March 20

## [Unity 5.1.0](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.1.0)

### What’s New

#### Android

* Adds support for setting a custom CleverTapAPI instance to iOS `CleverTapUnityManager`.
* Replaces `Handler` with `Timer` in the Android plugin for more efficient scheduling.

#### iOS

* Supports [CleverTap iOS SDK v7.1.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.1.1).
* Adds support for setting a custom CleverTap instance to Android `CleverTapUnityPlugin`.
* Adds macro to disable `UnityAppController` subclass for iOS.

### Enhancements

* Improves variable callback handling for better reliability.

# March 17

## [iOS 7.1.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.1.1)

### What's New

* Adds support for the `dismissInAppNotification` action to enable smooth dismissal of custom HTML In-App notifications.

### Bug Fixes

* Fixes an issue with device orientation checks for custom In-App notifications to ensure accurate rendering.

## [React Native 3.3.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/3.3.0)

> ❗️ Upgrade Alert
>
> We recommend upgrading to [CleverTap React Native 3.4.0](doc:march-2025#react-native-340) or above if using In-App.

### What's New

#### Android Platform

* Supports [CleverTap Android SDK v7.2.2](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-722-january-21-2025).
* Adds support for Android 15, ensuring compliance with the latest platform requirements. For more information, refer to [Android 15](https://developer.android.com/about/versions/15/summary).
* Enhances the [encryption algorithm for PII data](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/EXAMPLES.md#encryption-of-pii-data-), aligning it with [OWASP Mobile Application Security Guidelines](https://mas.owasp.org/MASTG/0x04g-Testing-Cryptography/). On devices running API 23 and above, the encryption key is now securely backed up using [Android Key Store](https://developer.android.com/privacy-and-security/keystore).
* Updates the ` minSdkVersion` to API 21 (Android 5.0).

> 🚧 Upgrade Help for 3.3.0
>
> If you run into any issues during the upgrade, refer to the [troubleshooting guide](https://github.com/CleverTap/clevertap-react-native/blob/master/docs/install.md#troubleshooting-1) for step-by-step help.

# March 12

## [Web 1.13.5](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.13.5)

### Bug Fixes

* Adds validations to prevent errors caused by `null` values in campaign objects.

* Addresses runtime issues affecting WebInbox functionality.

* Skips WebInbox retries when configuration is not present to prevent unnecessary API calls.

* Corrects inconsistencies in the custom event payload structure for Web Native Display.

* Improves session storage validation for `WZRK_D` with fallback mechanism and error handling.

# March 11

## [Android 7.3.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md)

> ❗️ Upgrade Alert
>
> We recommend upgrading to [CleverTap Android SDK 7.3.1](doc:march-2025#android-731) or above if using In-App.

### Important API Changes

* **Updates Huawei Push Integration**
  * Updates Huawei Push integration process to improve reliability and simplify setup. If you have integrated CleverTap Huawei Push SDK, you must follow the new steps listed under [Huawei Push Integration](doc:clevertap-huawei-push-integration) to ensure continued functionality.
* **Updates Baidu Push Integration**  
  Updates Baidu Push integration steps for better performance. To ensure compatibility, refer to the new steps listed under [Baidu Push Integration](doc:baidu-push-notifications).
* **Updates Unified Push Registration**  
  Removes `pushBaiduRegistrationId()` and `pushHuaweiRegistrationId()` methods removed from `CleverTapAPI` to simplify push registration. Use the appropriate `PushType` constant to identify the push provider (Huawei or Baidu). For more information about implementation updates, refer to [Huawei Push Integration](doc:clevertap-huawei-push-integration) and [Baidu Push Integration](doc:baidu-push-notifications).

> ❗️ Critical Update
>
> Migrate to `pushRegistrationToken()` to ensure Push Notifications continue to function.

### Bug Fixes

* Fixes an issue in `CleverTapAPI.getCleverTapID()` where a missing listener caused a Null Pointer Exception (NPE).
* Fixed an issue where device font size settings caused In-App message text to overflow. Messages now resize dynamically for improved layout.

## [Push Templates 1.4.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTPUSHTEMPLATESCHANGELOG.md)

### What's new

* Supports [CleverTap Android SDK v7.3.0](doc:march-2025#android-sdk-730).

## [Huawei Push 1.5.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTHUAWEIPUSHCHANGELOG.md)

### Important API Changes

* **Updates Huawei Push Integration**  
  The integration process for Huawei Push has been significantly updated. If you have previously integrated the CleverTap Huawei Push SDK, you must now follow the new steps outlined under [CleverTap Huawei Push Integration](doc:clevertap-huawei-push-integration) to ensure continued functionality.
* **Pluggable Huawei Push Integration**  
  Huawei Push is now available as a pluggable component, providing more flexibility and removing reliance on reflection-based initialization in the core SDK.

## [Expo 0.0.1](https://github.com/CleverTap/clevertap-expo-plugin/releases/tag/0.0.1)

> 📘 Public Beta
>
> This feature is released in Public Beta. it is fully functional, and we are actively fine-tuning it based on your feedback to ensure the best experience.

### What’s New

* Introduces the new Expo plugin to integrate the `clevertap-react-native` SDK in Expo managed workflow apps.
* Adds support for CleverTap core features in Expo-managed environments.
* Configures native code for both Android and iOS, so no manual changes are needed.

# March 10

## [Web 1.13.4](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.13.4)

### What's New

* Supports preview in custom HTML templates for Web Native Display campaigns, using a `post message proxy` for seamless preview functionality.

### Bug Fixes

* Fixes an issue where web pop-ups displayed an unintended white background, ensuring a cleaner and consistent visual experience.

# March 06

## [Web 1.13.3](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.13.3)

### What's New

* Introduces priority support for Web Native Display campaigns, allowing better control over message selection and delivery.

### Bug Fixes

* Combines multiple Web Native Display events into a single event with different `campaignSource` values.

### Important API Changes

* Changes in the structure of `CustomEvents` payloads and `listener keys`, requiring modifications in user implementations.

# March 05

## [Web 1.13.2](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.13.2)

### What's New

* **VAPID Migration and APNS Support**
  * Supports Apple Push Notification Service (APNS) in the new soft prompt.
  * Sends prompt configurations on every page refresh.

### Bug Fixes

* Fixes race conditions when `notifications.push` is called before web push configuration is available.
* Improves error handling for notification subscriptions.
* Ensures proper prompt closure when users grant permissions via the bell icon.
* Enhances prompt rendering logic to handle missing configurations or initialization delays.
* **Improved Web Push Handling**
  * Enhances web push notification handling with improved configuration management.
  * Introduces structured methods for setting up and processing soft prompts.

### Important API Changes

This update improves stability, simplifies API usage, and enhances push notification reliability.

* **Deprecated `notifications.enable` API**
  * `notifications.push` API now handles both old and new soft prompts, including the bell icon rendering.
* **Updated `notifications.push` API**
  * Manages the entire web push prompt flow.
  * Ensures correct rendering based on the received web push configuration.
  * Handles race conditions when the SDK is not yet initialized, or the application server key is unavailable.
* **Refactored Notification Handler Methods**
  * Adds `setupWebPush`, `processSoftPrompt`, `parseDisplayArgs`, and `setNotificationHandlerValues` for better modularity.
  * Updates `processWebPushConfig`, `enable`, and `push` methods to improve prompt logic.
  * Improves notification subscription flow and error handling.

# March 03

## [Flutter 3.2.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/3.2.0)

> ❗️ Upgrade Alert
>
> We recommend upgrading to [CleverTap Flutter 3.3.0](doc:march-2025#flutter-330) or above if using In-App.

### What's New

This release brings Android 15 support, enhanced encryption, and updated SDK compatibility for a more secure and optimized experience.

> ⚠️ Downgrade Advisory
>
> After upgrading to Flutter 3.2.0, avoid downgrading in subsequent app releases. If you face any issue after upgrading to this version, contact the [CleverTap Support](https://help.clevertap.com/hc/en-us/requests/new) team for assistance.

#### Android

* Supports [CleverTap Android SDK v7.2.2](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-722-january-21-2025).
* Supports Android 15. For more information, refer to [Android 15](https://developer.android.com/about/versions/15/summary).
* Updates the minimum supported Android SDK version to API level 21 (Android 5.0).
* Enhances the [encryption algorithm for PII data](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/EXAMPLES.md#encryption-of-pii-data-), aligning it with [OWASP guidelines](https://mas.owasp.org/MASTG/0x04g-Testing-Cryptography/). The encryption key is securely backed up using [Android Key Store](https://developer.android.com/privacy-and-security/keystore). This encryption is supported for API level 23 (Android 6.0 and above).
* Upgrades Android Gradle Plugin (A.G.P) to 8.6.1 as [recommended for Android 15](https://developer.android.com/about/versions/15/setup-sdk#:~:text=Update%20your%20app's%20build%20configuration,-Warning%3A%20If%20your\&text=1%20or%20higher%2C%20first%20run,1.).

## [Flutter 3.1.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/3.1.0)

### What's New

This release introduces key improvements to Android and iOS, including enhanced event tracking and In-App campaign triggers.

#### Android

* Supports [CleverTap Android SDK v7.1.2](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-712-january-29-2025).
* Supports Flutter 3.29 by removing deprecated Embedding v1. If your project uses Embedding v1, update it using the [migration guide](https://github.com/flutter/flutter/blob/main/docs/platforms/android/Upgrading-pre-1.12-Android-projects.md).
* Supports hiding the large icon in Android notifications using the `wzrk_hide_large_icon` key in the payload.

#### iOS

* Supports [CleverTap iOS SDK v7.1.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-710-january-21-2024).

#### Android and iOS

* Supports triggering In-App campaigns based on a combination of recurring and first-time events. For example, trigger a campaign every time the _Charged_ event occurs or the _App Launched_ event occurs for the first time.
* Supports user-level event log tracking system with the help of the following new APIs:
  * `getUserEventLog()`: Retrieves specific event details.
  * `getUserEventLogCount()`: Retrieves the count of times an event occurred.
  * `getUserLastVisitTs()`: Retrieves the timestamp of a user's most recent app visit.
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
