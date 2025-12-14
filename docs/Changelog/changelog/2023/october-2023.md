---
title: October 2023
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
We are excited to share the October SDK changelog for our product!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# October 27

## [Android Push Templates 1.2.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/ptv1.2.0)

### New Features

* **Added** support for developer-defined default notification channel for Push Templates.

### Bug Fixes

* **Fixed** a bug in the Rating Push Template where clicking a star resulted in no action.
* **Fixed** [#488](https://github.com/CleverTap/clevertap-android-sdk/issues/488), a bug related to the image sequence in Manual Carousel PushTemplate.

# October 25

## [React Native 1.1.2](https://github.com/CleverTap/clevertap-react-native/releases/tag/1.2.1)

### New Features

* **Android Platform**
  * **Added** support for [CleverTap Android SDK v5.2.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-521-october-12-2023).
  * **Added** Custom Proxy Domain functionality for Push Impressions and Events raised from CleverTap Android SDK. For more information on configuring custom proxy domains in Android, refer to [Usage.md](https://github.com/CleverTap/clevertap-react-native/blob/master/docs/usage.md) file.
* **iOS Platform**
  * **Added** support for [CleverTap iOS SDK v5.2.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/5.2.1).
  * **Added** support to enable `NSFileProtectionComplete` to secure the App’s document directory.
* **Android and iOS Platform**
  * **Added** in-built support to send the default locale (that is, language and country) data to the dashboard and exposed public API `CleverTapPlugin.setLocale(Locale locale)` to set the custom locale for LP Parity.
  * **Added** support for Integration Debugger. It allows you to view errors and events on the dashboard when the `debugLevel `is set to 3 using `CleverTapPlugin.setDebugLevel(3)`.
  * **Added** support to configure the first tab title in App Inbox.

### Changes

**iOS Platform**

* **Updated** the logic to retrieve country code using `NSLocale` above iOS 16 as `CTCarrier` is deprecated above iOS 16 with no replacements. For more information, refer to the [Apple](https://developer.apple.com/documentation/coretelephony/ctcarrier) documentation.
* **Updated** the logic to not send carrier name in the `CTCarrier` field for all versions above iOS 16.

### Bug Fixes

* **iOS Platform**
  * Fixed an iOS 17/Xcode 15 crash related to alerts for In App notifications.

# October 20

## [iOS Push Templates (CTNotificationContent) 0.2.5](https://github.com/CleverTap/CTNotificationContent/releases/tag/0.2.5)

* Fixed an issue where push notifications remained in the Notification Center when clicked on expanded view.
* Fixed an issue where the carousel did not display correctly the first time the app was installed.

## [Flutter 1.9.1](https://github.com/CleverTap/clevertap-flutter/releases/tag/1.9.1)

### New Features

* **Android Platform**
  * **Added** support for [CleverTap Android SDK v5.2.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-521-october-12-2023).
  * **Added** Custom Proxy Domain functionality for Push Impressions and Events raised from CleverTap Android SDK. For more information on configuring custom proxy domains in Android, refer to the [Usage.md](https://github.com/CleverTap/clevertap-flutter/blob/master/doc/Usage.md#integrate-custom-proxy-domain) file.
* **iOS Platform**
  * **Added** support for [CleverTap iOS SDK v5.2.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/5.2.1).
  * **Added** support to enable `NSFileProtectionComplete`. It secures the App’s document directory.
* **Android and iOS Platform**
  * **Added** in-built support to send the default locale (i.e., language and country) data to the dashboard and exposed public API `CleverTapPlugin.setLocale(Locale locale)` to set the custom locale, for LP Parity.
  * **Added** support for Integration Debugger to view errors and events on the dashboard when the `debugLevel` is set to 3 using `CleverTapPlugin.setDebugLevel(3)`.

### Changes

* **iOS Platform**
  * **Updated** logic to retrieve country code using NSLocale above iOS 16 as `CTCarrier` is deprecated above iOS 16 with no replacements. For more information, refer to [Apple](https://developer.apple.com/documentation/coretelephony/ctcarrier) documents.
  * **Updated** logic to not send carrier name above iOS 16 in the `CTCarrier` field.

### Bug Fixes

* iOS Platform
  * **Fixed** an iOS 17/Xcode 15 crash related to alerts In Apps.

## [Web 1.6.7](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.6.7)

* **Fixed** a bug related to Web Inbox preview.
* **Fixed** a bug related to Web Popup Image Only resizing.
* **Added** the debug flag in requests when the log level is 3 for Integration Debugger.
* **Added** handling for *Web Popup Drag* and *Drop* template.

# October 13

## [Android 5.2.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev5.2.1_xpsv1.5.4)

* **Added** Custom Proxy Domain functionality for Push Impressions and Events raised from CleverTap SDK. For more information on configuring custom proxy domains, refer to the [EXAMPLES.md](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/EXAMPLES.md#integrate-custom-proxy-domain) file.
* **Added** new API `setLocale(String locale)`. It allows you to set a custom locale for the required CleverTap instance. Different instances can have different locales.
* **Added** support for Integration Debugger by sending a debug flag in the header.
* **Fixed** a security and trust vulnerability related to implicit `PendingIntent`.

## [Xiaomi 1.5.4](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev5.2.1_xpsv1.5.4)

**Fixed** an issue related to push impressions leading to a profile split.
