---
title: October 2024
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
We are excited to share CleverTap's October SDK changelog!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# October 30

## [Web 1.11.5](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/1.11.5)

* Adds support for personalization in visual editor.

# October 25

## [React Native 3.1.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/3.1.0)

### What's New

* **Android**
  * Supports [CleverTap Android SDK v7.0.2](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-702-october-10-2024).
  * Supports the custom handshake domain configuration in the Android manifest.
* **iOS**
  * Supports [CleverTap iOS SDK v7.0.2](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-702-october-10-2024).
  * Supports the custom handshake domain configuration.
* **Android and iOS**
  * Supports File Type Variables in Remote Config. For more information about the integration, refer to the [Remote Config Variables](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/docs/Variables.md) documentation.
  * Supports Custom Code Templates. For more information about the integration, refer to the [CustomCodeTemplates.md](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CustomCodeTemplates.md) documentation.
  * Supports Custom Code In-App template definitions through a JSON scheme.

### Bug Fixes

* **Android and iOS**
  * Fixes a missing import statement in the `index.js` file [#431](https://github.com/CleverTap/clevertap-react-native/issues/431).
  * Fixes issue [#426](https://github.com/CleverTap/clevertap-react-native/issues/426).

# October 24

## [Web 1.11.4](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.4)

* Adds support for the following non-case-sensitive input for gender values: `male`, `female`, `unknown`, `others`, `m`, `f`, `u`, and `o` .

# October 23

## [Web 1.11.3](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.3)

* Supports the new payload structure of Visual Editor.
* Moves the CleverTap Web SDK version check from within the SDK to the CleverTap dashboard.

# October 19

## [Flutter 2.5.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/2.5.0)

### What's New

* **Android**
  * Adds support for [CleverTap Android SDK v7.0.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-701-september-2-2024).
  * Adds support for triggering In-Apps messages based on user attribute changes.
  * Removes the three-line character limit from the App Inbox messages.
  * Adds support for AndroidX Media3 in lieu of the ExoPlayer [deprecation](https://developer.android.com/media/media3/exoplayer/migration-guide). CleverTap recommends [migrating](https://github.com/CleverTap/clevertap-flutter/blob/develop/doc/Integrate-Android.md#migrateExoplayer) to Media3, though it continues to support ExoPlayer.
* **iOS**
  * Adds support for [CleverTap iOS SDK v7.0.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.0.1).
  * Adds support for triggering In-App messages based on user attribute changes.
* **Web**
  * Adds support for [CleverTap Web SDK v1.11.2](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.2).
  * Adds the `enableWebPushNotifications()` method to support custom Web Push Prompt.

### Bug Fixes

* **Android and iOS**
  * Fixes the [GitHub](https://github.com/CleverTap/clevertap-flutter/issues/260) issue that occurs when running `pub get` command on a Windows machine.
* **Android**
  * Fixes an Application Not Responding (ANR) error triggered by extremely old In-App campaigns.
  * Fixes incorrect callbacks for In-App messages when the phone is rotated.
  * Fixes an issue where In-App messages continued to display after stopping all campaigns.
  * Fixes an issue where In-App images failed to display when the device was rotated to landscape mode.
  * Fixes incorrect URL loading behavior in custom HTML In-App templates.
* **iOS**
  * Fixes an issue where the `CTInAppHTMLViewController` rendered before the app's screen became active. The `CTInAppDisplayViewController` now utilizes the `keyWindow` supported orientations for accurate display.

# October 17

## [Web 1.11.2](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.2)

* Fixes an issue where the Web Popup Campaign failed to function when other `iframes` were present on the Document Object Model (DOM).

# October 16

## [Web 1.11.1](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.1)

* Fixes an issue where the delay functionality was not working for Web Popup Campaigns.

# October 14

## [Web 1.11.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.0)

* Adds Additional triggers and clicks handling for Web Pop-up template.
* Adds a fix for Account ID undefined for the Visual Editor
* Adds a fix for CSS in the soft prompt.

# October 10

## [Android 7.0.2](https://github.com/CleverTap/clevertap-android-sdk/tree/corev7.0.2)

### New Features

* Supports the custom handshake domain configuration in the Android manifest.
* Supports Custom Code In-App Template Definitions through a JSON scheme. For more information, refer to the [CustomCodeTemplates.md](https://github.com/CleverTap/clevertap-android-sdk/blob/corev7.0.2/docs/CustomCodeTemplates.md) documentation.

## [iOS 7.0.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.0.2)

### New Features

* Support the custom handshake domain configuration.
* Supports Custom Code In-App Template Definitions through a JSON scheme. For more information, refer to the [CustomCodeTemplates.md](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/docs/CustomCodeTemplates.md) documentation.

# October 8

## [Web 1.10.1](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.10.1)

* Adds a fix for the version check in the Visual Editor.

# October 8

## [React Native 3.0.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/3.0.0)

### What's New

* **Android**
  * Supports [CleverTap Android SDK v7.0.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-701-september-2-2024).
  * Removes the three-line character limit from the App Inbox messages.
  * Deprecates `CleverTapModule.setInitialUri()` in favor of `CleverTapRnAPI.setInitialUri()`. For more information, refer to [Step 4 of the Integration Guide](https://github.com/CleverTap/clevertap-react-native/blob/3.0.0/docs/integration.md#step4).
* **iOS**
  * Supports [CleverTap iOS SDK v7.0.1](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-701-august-22-2024).
* **Android and iOS**
  * Adds support for triggering In-App messages based on user attribute changes.
  * Migrates the bridge to a backward-compatible Turbo Module as part of the [New Architecture](https://github.com/CleverTap/clevertap-react-native/blob/3.0.0/docs/integration.md). 
  * The CleverTap React Native SDK continues to be compatible with both the Old and the New Architecture.

### Bug Fixes

* **Android**
  * Fixes an ANR (Application Not Responding) error triggered by extremely old In-App campaigns.
  * Fixes incorrect callbacks for In-App messages when the phone is rotated.
  * Fixes a bug where In-App messages continued to display after stopping all campaigns.
  * Fixes a bug where In-App images failed to display when the device was rotated to landscape mode.
  * Fixes incorrect URL loading behavior in custom HTML In-App templates.

### Breaking Changes

* **Action Required for Android**\
  Ensure that your custom `Application` class either:
  * Extends `CleverTapApplication` or 
  * Explicitly calls `CleverTapRnAPI.initReactNativeIntegration(this);` to enable the functionality of `ClevertapPushNotificationClicked` and other callbacks that are triggered when the app is in a killed state. For more information, refer to [Step 3 of the Integration Guide](https://github.com/CleverTap/clevertap-react-native/blob/3.0.0/docs/integration.md#step3a).

# October 8

## [Web 1.10.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.10.0)

* Adds a new API to handle the customized web push prompts rendering.
