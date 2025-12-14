---
title: August 2023
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
We are excited to share the August SDK changelog for our product!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# August 28

## [Flutter 1.9.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/1.9.0)

### New Features

* **Android Platform**
  * **Added** support for [CleverTap Android SDK v5.2.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-520-august-10-2023)
* **iOS Platform**
  * **Added** support for [CleverTap iOS SDK v5.2.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/5.2.0)
* **Android and iOS Platform**
  * **Added** support for encryption of PII data such as Email, Identity, Name, and Phone. For more information on enabling or disabling encryption, refer to the [Usage.md](https://github.com/CleverTap/clevertap-flutter/blob/master/doc/Usage.md#encryption-of-pii-data-android-and-ios) file.
  * **Added** support for custom key-value pairs common to all inbox messages in App Inbox.

### Bug Fixes

* **Android Platform**
  * Fixed [#393](https://github.com/CleverTap/clevertap-android-sdk/issues/393). A push permission flow crash when the context in the`CoreMetadata` is null.
  * Fixed a bug where `addMultiValueForKey` and `addMultiValuesForKey` were overwriting the current values of the user properties instead of appending them.

# August 24

## [Web 1.6.5](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.6.5)

* **Fixed** a bug related to the message sequence in Web Inbox.

# August 18

## [React Native 1.2.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/1.2.0)

### New Features

* **Android Platform**
  * **Added** support for [CleverTap Android SDK v5.2.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-520-august-10-2023).
    > 📘 Note
    >
    > CleverTap Core SDK now directly supports RenderMax Push SDK within the CleverTap SDK. Remove the integrated [RenderMax SDK](doc:react-native-push-notification#integrate-rendermax-push-sdk-with-react-native) before you upgrade to CleverTap React Native SDK for this version.
  * **Added** support for developer-defined default notification channel. For more information about setting up a default channel in your app, refer to the [Usage.md](https://github.com/CleverTap/clevertap-react-native/blob/master/docs/usage.md#default-notification-channel) file.
    > 📘 Note
    >
    > This feature is only supported for CleverTap core notifications. Support for push templates will be released soon.
* **iOS Platform**
  * **Added** support for [CleverTap iOS SDK v5.2.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/5.2.0).
* **Android and iOS Platforms**
  * **Added** support for encryption of PII data such as Email, Identity, Name, and Phone. For more information on enabling or disabling encryption, refer to the [Encryption.md](https://github.com/CleverTap/clevertap-react-native/blob/master/docs/usage.md#encryption-of-pii-data) file.
  * **Added** support for custom key-value pairs common to all inbox messages in App Inbox.

### API Changes

* **Android Platform**
  * **Added** the `SCCampaignOptOut` event to Restricted Events Name list for internal use.
  * **Added** custom SDK versions to `af` field for internal use.

### Breaking API Changes

* **Android Platform**
  * `CTFlushPushImpressionsWork` breaks custom `WorkerFactory` implementation of an App as follows:
    * If you use a custom `WorkFactory` implementation of `WorkManager` for Android platform, ensure you correctly handle workers defined by CleverTap SDK and other third-party dependencies.
    * You must return null from `createWorker()` for any unknown `workerClassName`. For more information on its implementation, refer to the [documentation](https://medium.com/androiddevelopers/customizing-workmanager-fundamentals-fdaa17c46dd2).

### Bug Fixes

* **Android Platform**
  * **Fixed** [#393](https://github.com/CleverTap/clevertap-android-sdk/issues/393). A push permission flow crash when the context in the`CoreMetadata` is null.
  * **Fixed** [#428](https://github.com/CleverTap/clevertap-android-sdk/issues/428). A race condition when detecting whether an In-App message should appear.
  * **Fixed** Push primer alert dialog freeze behavior, which became unresponsive when clicked outside the window.
  * **Fixed** a bug where `addMultiValueForKey` and `addMultiValuesForKey` were overwriting the current values of the user properties instead of appending them.

# August 17

## [Cordova 2.7.1](https://github.com/CleverTap/clevertap-cordova/releases/tag/2.7.1)

### **New Features**

* **Android Platform**
  * **Added** support for [CleverTap Android SDK v5.2.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev5.2.0_hmsv1.3.3_xpsv1.5.3). This supported version includes an encryption feature for PII data. For more information, refer to [Encryption usage for Cordova Android](https://github.com/CleverTap/clevertap-cordova/blob/2.7.1/docs/Integrate-Android.md#setup-encryption-for-pii-data).
* **iOS specific**
  * **Added** support for [CleverTap iOS SDK v5.2.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/5.2.0). This supported version includes an encryption feature for PII data. For more information, refer to [Encryption usage for Cordova iOS](https://github.com/CleverTap/clevertap-cordova/blob/2.7.1/docs/Integrate-iOS.md#setup-encryption-for-pii-data).
* **Bug Fixes**
  * **Fixed** `NSInvalidArgumentException` for `getDisplayUnitForId` and `getInboxMessageForId` in iOS.

# August 16

## [iOS 5.2.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/5.2.0)

* **Added** support for encryption of PII data such as Email, Identity, Name, and Phone. For more information on enabling or disabling encryption, refer to [Encryption.md](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/docs/Encryption.md) file.
* **Added** support for custom key-value pairs common to all inbox messages in App Inbox.
* **Added** sample `SwiftUIStarter` app to support CleverTap iOS SDK for SwiftUI.
* **Added** steps to track screen views in SwiftUI. For more information, refer to [SwiftUI](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/docs/SwiftUI.md) documentation.

# August 10

## [Android 5.2.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md)

### New Features

* **Added** support for encryption of PII data such as Email, Identity, Name, and Phone. For more information on enabling or disabling encryption, refer to the [Encryption of PII Data](doc:android-advanced-features#encryption-of-pii-data).
* **Added** support for custom key-value pairs common to all inbox messages in AppInbox.

### Bug Fixes

* **Fixed** a bug where `addMultiValueForKey` and `addMultiValuesForKey` were overwriting the current values of the user properties instead of appending them.
* **Fixed** [#393](https://github.com/CleverTap/clevertap-android-sdk/issues/393). A push permission flow crash when the context in the`CoreMetadata` is null.

## [Xiaomi Push 1.5.3](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTXIAOMIPUSHCHANGELOG.md)

### Bug Fixes

* **Updated** Xiaomi Push SDK to v5.1.5
* **Added** support for CleverTap Android SDK v5.2.0

## [Huawei Push 1.3.3](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTHUAWEIPUSHCHANGELOG.md)

### Bug Fixes

* **Updated** Huawei Push SDK to v6.11.0.300
* **Added** support for CleverTap Android SDK v5.2.0

# August 8

## [Cordova 2.7.0](https://github.com/CleverTap/clevertap-cordova/releases/tag/2.7.0)

### New Features

* **Android Platform**
  * **Added** support for [CleverTap Android SDK v5.1.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev5.1.0_ptv1.1.0)
  * **Added** support for [Cordova Android SDK v12.0.0](https://cordova.apache.org/announcements/2023/05/22/cordova-android-12.0.0.html)
  * **Added** support for `deleteInboxMessagesForIds(messageIDs)`
  * **Added** a new callback `onCleverTapInAppNotificationShow(JSONObject)`
  * **Added** the below behavioral changes of `onCleverTapInboxItemClick`
    * Previously, the callback was raised when the App Inbox Item was clicked.
    * Now, it is also raised when the App Inbox button and Item are clicked.
  * **Fixed** an [XSS vulnerability](https://fluidattacks.com/advisories/maiden/). We recommend you update to v2.7.0 or above.
* **iOS Platform**
  * **Added** support for [CleverTap iOS SDK v5.1.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/5.1.2)
  * **Added** support for [Cordova iOS v7.0.0](https://cordova.apache.org/announcements/2023/07/10/cordova-ios-7.0.0.html)
* **For Android and iOS**
  * **Added** the below new public APIs to support [Android 13 notification runtime permission:](https://developer.android.com/develop/ui/views/notifications/notification-permission)
    * `isPushPermissionGranted(successCallback)`. For more information, refer to [Usage](https://github.com/CleverTap/clevertap-cordova/blob/2.7.0/docs/pushprimer.md#get-the-push-notification-permission-status).
    * `promptPushPrimer(JSONObject)`. For more information, refer to [Usage](https://github.com/CleverTap/clevertap-cordova/blob/2.7.0/docs/pushprimer.md#push-primer-using-half-interstitial-local-in-app).
    * `promptForPushPermission(boolean)`. For more information, refer to [Usage](https://github.com/CleverTap/clevertap-cordova/blob/2.7.0/docs/pushprimer.md#prompt-the-notification-permission-dialog-without-push-primer).
    * New callback `onCleverTapPushPermissionResponseReceived` is available. It returns after the user allows/denies notification permission. For more information, refer to [Usage](https://github.com/CleverTap/clevertap-cordova/blob/2.7.0/docs/pushprimer.md#available-callbacks-for-push-primer).
  * **Added** a new API, `markReadInboxMessagesForIds(messageIDs)` to mark read an array of Inbox Messages.
  * **Added** a new API, `dismissInbox()` to dismiss the App Inbox.
* **API Changes**
  * Obsolete methods and callbacks associated with Product Config and Feature Flags are now labeled as deprecated.
* **Breaking API Changes**
  * Modifications in return values for the `onCleverTapInboxItemClick` callback in Android and the `messageDidSelect` callback in iOS.
