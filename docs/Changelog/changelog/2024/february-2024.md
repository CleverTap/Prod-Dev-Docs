---
title: February 2024
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

# February 28

## [Android 6.1.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev6.1.1)

> 📘 Note
>
> For developers that have BACKGROUND\_SYNC ([Android Pull Notification](https://developer.clevertap.com/docs/android-push#pull-notification)) enabled in their older apps and are now upgrading to CleverTap Android SDK v6.1.0 or CleverTap Android SDK v6.1.1, please add the below code snippet to your AndroidManifest.xml to avoid ClassNotFoundException error.
>
> ```xml
> < service
> 	android: name = "com.clevertap.android.sdk.pushnotification.amp.CTBackgroundJobService"
> 	android: exported = "false"
> 	android: enabled = "false"
> 	tools: ignore = "MissingClass" />
> ```

### Bug Fixes

* **Fixed** an incorrect endpoint issue in the network handshake case.
* **Fixed** an issue in client-side In-Apps with regard to frequency limits.

# February 26

## [Flutter 2.1.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/2.1.0)

### New Features

* **Android Platform**
  * Supports [CleverTap Android SDK v6.0.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-600-january-15-2024).
* **iOS Platform**
  * Supports [CleverTap iOS SDK v6.0.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/6.0.0).
* **Android and iOS Platform**
  * Adds support for client-side In-Apps.
  * Adds new API: `clearInAppResources(boolean)`. It deletes images and GIFs preloaded for In-Apps in CS mode.
  * Adds new API: `fetchInApps()`. It explicitly fetches In-Apps from the server.

### Bug Fixes

* **Android Platform**
  * Fixes a bug where JavaScript does not work for custom-html InApp header/footer templates.
  * Fixes a Null Pointer Exception (NPE) related to App Inbox APIs.
  * Fixes a `ClassCastException` in `defineVariable` API of Product Experiences.
  * Fixes a resource name conflict with the firebase library in `fcm_fallback_notification_channel_label`.
  * Fixes a StrictMode Violation spawning from `ctVariables.init()`.
  * Removes the use of lossy conversions leading to an issue in Push Templates.
  * Handles an edge case related to the migration of encryption level when the local database is out of memory.
* **iOS Platform**
  * Fixes a bug where some In-Apps were not being dismissed.

# February 22

## [iOS 6.1.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-610-february-22-2024)

* **Added** privacy manifests for CleverTap-iOS-SDK and CleverTapLocation.
* **Fixed** a crash due to out-of-bounds in `NSLocale` implementation.

## [Android 6.1.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev6.1.0_ptv1.2.3_hmsv1.3.4_geofencev1.3.0)

> 🚧 Warning
>
> We have identified a critical bug in Android SDK 6.1.0 occurring because of an incorrect endpoint while performing the network handshake. We recommend skipping it and upgrading to Android SDK 6.1.1 instead.

### New Features

* Supports Android 14. It is compliant with Android 14 requirements. For more information, refer to the [Android](https://developer.android.com/about/versions/14/summary) documentation. 
* Upgrades the Android Gradle Plugin (AGP) to v8.2.2 for building the SDK and adds related consumer pro guard rules.
* Deprecates Xiaomi's public methods as we sunset the Xiaomi SDK. For more information, refer to the [Xiaomi](https://dev.mi.com/distribute/doc/details?pId=1555) document. 
* Adds accessibility IDs for UI components of SDK.
* Migrates `JobScheduler` to `WorkManager` for Pull Notifications.

### Breaking API Changes

* `CTPushAmpWorker` breaks custom `WorkerFactory` implementation of an Application. If you are using a custom `WorkFactory` implementation of `WorkManager`, ensure you: 
  * Correctly handle workers defined by CleverTap SDK and other third-party dependencies. 
  * Return `null` from `createWorker()` for any unknown `workerClassName`. For more information, refer to the [Customizing WorkManager — fundamentals](https://medium.com/androiddevelopers/customizing-workmanager-fundamentals-fdaa17c46dd2) blog. 

### Bug Fixes

* Fixes a rare activity destroyed race condition causing crashes in In-App.
* Fixes a potential Application Not Responding (ANR) issue arising from a race condition during SDK initialization in a multi-threaded environment. 

## [Geofence 1.3.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev6.1.0_ptv1.2.3_hmsv1.3.4_geofencev1.3.0)

* Supports Android 14. It is compliant with Android 14 requirements. For more information, refer to the [Android](https://developer.android.com/about/versions/14/summary) documentation.
* Upgrades the Android Gradle Plugin (AGP) to 8.2.2 for building the SDK and adds related consumer pro guard rules.

## [Huawei Push 1.3.4](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev6.1.0_ptv1.2.3_hmsv1.3.4_geofencev1.3.0)

* Supports Android 14. It is compliant with Android 14 requirements. For more information, refer to the [Android](https://developer.android.com/about/versions/14/summary) documentation.
* Upgrades the Android Gradle Plugin (AGP) to 8.2.2 for building the SDK and adds related consumer pro guard rules.

## [Push Templates 1.3.3](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev6.1.0_ptv1.2.3_hmsv1.3.4_geofencev1.3.0)

* Supports Android 14. It is compliant with Android 14 requirements. For more information, refer to the [Android](https://developer.android.com/about/versions/14/summary) documentation.
* Upgrades the Android Gradle Plugin (AGP) to 8.2.2 for building the SDK and adds related consumer pro guard rules.

# February 16

## [React Native 2.0.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/2.0.0)

### What's New

* **Android Platform**
  * Supports [CleverTap Android SDK 6.0.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-600-january-15-2024).
* **iOS Platform**
  * Supports [CleverTap iOS SDK 6.0.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/6.0.0).
* **Android and iOS Platform**
  * Adds support for client-side In-Apps.
  * Adds new API: `clearInAppResources(boolean)`. It deletes images and GIFs preloaded for In-Apps in CS mode.
  * Adds new API: `fetchInApps() `. It explicitly fetches In-Apps from the server.

### Bug Fixes

* **Android Platform**
  * Fixes a bug where JavaScript does not work for custom HTML in In-App header/footer templates.
  * Fixes an NPE related to App Inbox APIs.
  * Fixes a `ClassCastException` in `defineVariable` API of Product Experiences.
  * Fixes a resource name conflict with the firebase library in `fcm_fallback_notification_channel_label`.
  * Fixes a StrictMode Violation spawning from `ctVariables.init()`.
  * Removes the use of lossy conversions leading to an issue in Push Templates.
  * Handles an edge case related to the migration of encryption level when the local database is out of memory.
* **iOS Platform**
  * Fixes a bug where some In-Apps were not being dismissed.

# February 1

## [Flutter 2.0.1](https://github.com/CleverTap/clevertap-flutter/releases/tag/2.0.1)

**What's New**

* **Web Platform**
  * Supports [CleverTap Web SDK v1.6.10.](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.6.10)

**Bug Fixes**

* **Web Platform**
  * Fixes [#213](https://github.com/CleverTap/clevertap-flutter/issues/213): Invalid error in case of event structure.
