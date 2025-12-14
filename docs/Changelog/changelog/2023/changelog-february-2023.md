---
title: February 2023
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
We are excited to share the CleverTap SDK changelog for February!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# February 28

## [Web 1.4.1](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.4.1)

- **Fixed** rendering issues for Web Native Display banner and carousel.
- **Fixed** user interface issues for Web Inbox. 

# February 14

## [Flutter 1.6.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/1.6.0)

- **Added ** the following new push permission public APIs to support core [Android SDK v4.7.4](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.7.4) and [iOS SDK v4.2.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/4.2.0) versions: 

  - `getPushNotificationPermissionStatus()`
  - `promptPushPrimer(object)`
  - `promptForPushNotification(boolean)`

   For more information, refer to the [Flutter Push Primer](doc:flutter-push-notification#push-primer) document.

- **Added** the `setCleverTapPushPermissionResponseReceivedHandler` push permission callback method, which returns _true_ or _false_ depending on the user's notification permissions.

- **Added** the `setCleverTapInAppNotificationShowHandler` method to handle InApp notifications for Android.

- **Updated** the format of the Native Display payload for both Android and iOS platforms.

- **Fixed** the FCM Plugin's [onBackgroundMessage handler](https://github.com/CleverTap/clevertap-flutter/commit/8db6f34eec83e7f14990359f88c65a50e966acb3) bug that failed sending the method calls from Android.