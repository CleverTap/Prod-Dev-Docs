---
title: August 2024
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

# August 29

## [Web 1.9.1](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.9.1)

* Adds origin check to Visual Editor template.

# August 22

## [iOS 7.0.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.0.1)

### Bug Fixes

* Fixes an issue where certain user properties were being sent with an incorrect prefix.

# August 12

## [Cordova 3.2.0](https://github.com/CleverTap/clevertap-cordova/releases/tag/3.2.0)

### New Features

#### Android

* Supports [CleverTap Android SDK 6.2.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev6.2.1).
* Compliant with Android 14, meeting all [Android 14 requirements](https://developer.android.com/about/versions/14/summary).
* Extends the push primer callback to notify when permission is denied by clicking the cancel button on the `PromptForSettings` alert dialog.
* Adds Accessibility ids for UI components of SDK.
* Migrates `JobScheduler` to `WorkManager` for [Pull Notifications](https://developer.clevertap.com/docs/android-push#pull-notification).

### Bug Fixes

#### Android

* Fixes [#239](https://github.com/CleverTap/clevertap-cordova/issues/239), an issue where the `onPushNotification` callback was not triggered when the notification was tapped from the killed state on capacitor apps.
* Fixes a crash in In-Apps caused by a rare race condition where an activity was destroyed.
* Fixes a potential Application Not Responding (ANR) issue due to a race condition during SDK initialization in a multithreaded setup.
* Fixes a bug in client-side In-Apps with regard to frequency limits.
* Fixes a crash due to `NullPointerException` related to `deviceInfo.deviceId`.
* Fixes an ANR issue related to the `isMainProcess` check.
* Fixes an ANR issue caused by eager initialization of `CtApi` triggered by `DeviceId` generation.

#### Breaking API Changes

* Removes all Xiaomi-related public methods, as the Xiaomi SDK has been discontinued. For more information, refer to the [Xiaomi documentation](https://developer.clevertap.com/docs/discontinuation-of-xiaomi-push-service).

# August 7

## [iOS 7.0.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.0.0)

> 🚧 Note
>
> We identified a critical bug in iOS SDK 7.0.0 and recommend skipping this version and upgrading to  [iOS SDK 7.0.1](doc:august-2024#ios-701) instead.

### New Features

* Adds support for custom code templates in In-App notifications.
* Adds support for File Type Variables within Remote Config.
* Adds support for triggering In-App notifications when a user attribute changes.
* Includes the CleverTap SDK version in the JS interface for HTML In-App notifications, enabling debugging and version management.

### Bug Fixes

* Fixes an issue where the HTML view controller `CTInAppHTMLViewController` is presented before the scene becomes active. This improves app stability.
* Fixes an issue where the `CTInAppDisplayViewController` now uses the `keyWindow` supported orientations.

# August 5

## [Android 7.0.0](https://github.com/CleverTap/clevertap-android-sdk/tree/corev7.0.0_ptv1.2.4)

### New Features

* Adds support for triggering In-App notifications when a user attribute changes.
* Adds support for custom code templates in In-App notifications.
* Adds support for File Types in Variables.
* Removes the character limit of a maximum of 3 lines in App Inbox messages.
* Adds a new API called `clearFileResources(boolean expiredOnly)`. This API lets you delete all preloaded files used in SDK features, such as Custom In-App Templates, App Functions, Variables, etc.

### Bug Fixes

* Fixes a race-condition bug where the App Launch event is triggered twice when using a custom CleverTap ID.
* Fixes an Application Not Responding (ANR) issue caused by old In-App campaigns.
* Fixes an issue where incorrect callbacks are sent for In-App when the device orientation changes.
* Fixes an issue where an In-App notification is displayed even after all the campaigns are stopped.
* Fixes an issue where the In-App image is not shown when the device orientation changes to the landscape.
* Fixes an issue where certain URLs are loaded incorrectly in custom HTML In-App templates.

### Dependency Update

* Adds support for `AndroidX Media3` in lieu of the deprecation of `ExoPlayer`. While CleverTap continues to support `ExoPlayer`, [migration](https://developer.android.com/media/media3/exoplayer/migration-guide) to `Media3` is is recommended.

## [Push Template 1.2.4](https://github.com/CleverTap/clevertap-android-sdk/tree/corev7.0.0_ptv1.2.4)

### New features

* Removes the cross button from the Five Icons push template. Consequently,  the Five Icons notification will no longer be sticky.
