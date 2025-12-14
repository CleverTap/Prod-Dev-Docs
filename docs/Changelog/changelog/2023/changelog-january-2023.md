---
title: January 2023
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
We are excited to share the CleverTap SDK changelog for January!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# January 31

## [Web 1.4.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.4.0)

- **Added** the new **Web Inbox** messaging channel on the CleverTap dashboard. For more details, refer to the [Developer documentation](doc:web-inbox).

> 📘 Private Beta
> 
> Web Inbox feature is released in private Beta. Contact your Customer Success Manager to request early access to this feature.

- **Added** the `clevertap.getLocation()` API to enrich the user profile with location details and enable the location-based segmentation from the CleverTap dashboard. For more details, refer to [User Location Handling for Web](doc:concepts-user-profiles#user-location-handling).
- **Fixed** the issue for the `addMultiValue` user profile property, where you can enrich anonymous user profiles with different properties.

# January 27

## [Android 4.7.4](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.7.4)

- **Improved** overall SDK performance by SDK code optimization and cleanup.

# January 25

## [RenderMax 1.0.2](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.7.3_rmv1.0.2)

- **Fixed** the [runtime exception](https://github.com/CleverTap/clevertap-android-sdk/issues/379) error occurring with the RenderMax library in minified app versions.

## [Android 4.7.3](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.7.3_rmv1.0.2)

- **Fixed** UI message issue in the _Footer_ template on CleverTap's _In-App_ campaigns.
- **Fixed** Null Pointer Exception (NPE) crash that occurs when clicking the Inbox Message body with the deep link in the `CTInboxListFragment` class of core SDK.
- **Improved** overall SDK performance by SDK code optimization and cleanup.

## [React Native 1.0.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/1.0.0)

- **Added** push permission callback method, which returns _true_ or _false_ depending on the user's notification permissions.
- **Added ** the following new push permission public APIs to support core **Android SDK v4.7.2** and **iOS SDK v4.2.0** versions: 

  - `isPushPermissionGranted()`
  - `promptPushPrimer(object)`
  - `promptForPushPermission(boolean)` 

   For more information, refer to the [Push Primer](doc:react-native-push-notification#push-notifications-permission) document.

# January 23

## [Flutter 1.5.5](https://github.com/CleverTap/clevertap-flutter/releases/tag/1.5.5)

- **Fixed** closing App Inbox controller issue when a deep link is present in iOS.
- **Added** the [Add-to-App](https://docs.flutter.dev/development/add-to-app) feature for Android platform to embed the CleverTap plugin in a flutter module.