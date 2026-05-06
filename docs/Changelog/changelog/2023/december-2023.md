---
title: December 2023
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

# December 15

## [Flutter 2.0.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/2.0.0)

* **iOS Platform**
  * Supports [CleverTap iOS SDK v5.2.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/5.2.2).
* **Web Platform**
  * Supports [CleverTap Web SDK v1.6.9](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.6.9). For information on integrating and using CleverTap Web SDK in Flutter, refer to [Usage-Web.md](https://github.com/CleverTap/clevertap-flutter/blob/master/doc/Usage-Web.md).

# December 6

## [Cordova 2.7.2](https://github.com/CleverTap/clevertap-cordova/releases/tag/2.7.2)

## New Features

* **Android Platform**
  * Supports [CleverTap Android SDK v5.2.1. ](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev5.2.1_xpsv1.5.4)This supported version includes support for Custom Proxy Domain functionality. For more information on usage for Cordova Android, refer to the [document](https://github.com/CleverTap/clevertap-cordova/blob/2.7.2/docs/Integrate-Android.md#integrate-custom-proxy-domain).
* **iOS Platform**
  * Supports [CleverTap iOS SDK v5.2.2.](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/5.2.2) This supported version includes support for Custom Proxy Domain functionality. For more information on usage for Cordova iOS, refer to the [document](https://github.com/CleverTap/clevertap-cordova/blob/2.7.2/docs/Integrate-iOS.md#integrate-custom-proxy-domain).
* **Common for both Android and iOS**
  * **Added** a new public API `setLocale(String locale)` for in-built support to send the custom locale (i.e., language and country) data to the dashboard.
  * **Added** support for Integration Debugger. It allows you to view errors and events on the dashboard when the `debugLevel` is set to 3.
  ## Bug Fixes
  * Fixes a crash in iOS 17/Xcode 15 related to alerts in in-apps.
