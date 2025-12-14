---
title: August 2025
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

## [Flutter 3.5.2](https://github.com/CleverTap/clevertap-flutter/releases/tag/3.5.2)

### What's New

- Supports [CleverTap Android SDK 7.5.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-751-august-28-2025).

## [Android 7.5.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.5.1)

### Bug Fixes

- Fixes an `OutOfMemoryError` in `AESCrypt.parseCipherText()` that occurred when parsing large encrypted inputs. The error was caused by using `Regex.split()` on large byte array strings, which led to excessive memory allocations.

# August 26

## [Web 2.2.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v2.2.0)

- Adds subdomain-specific cookie support controlled by the `isolateSubDomain` flag.

# August 22

## [Flutter 3.5.1](https://github.com/CleverTap/clevertap-flutter/releases/tag/3.5.1)

### What's New

- Supports [CleverTap Web SDK 2.1.2](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v2.1.2).
- Supports [CleverTap iOS SDK 7.3.2](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-732-august-08-2025).

## [Web 2.1.2](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v2.1.2)

- Fixes the request handling flow for multi-user login.

# August 20

## [Web 2.1.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v2.1.0)

### What's New

- Adds support for creating and editing multiple pages in the Visual Editor.
- Enables preview functionality for campaigns built using the Advanced Web-popup Builder.

### Bug Fixes

- Fixes an issue where the iframe preview did not close as expected in Advanced Web-popup Builder.
- Ensures that exit intent callbacks are triggered only once per Linked Content event, preventing repeated executions.

# August 19

## [Unity 5.3.1](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.3.1)

### Bug Fixes

- Fixes JSON serialization to use `InvariantCulture`, which ensures consistent number formatting.

# August 08

## [iOS 7.3.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.3.2)

### What's New

- Added Swift Package Manager (SPM) support for CleverTap watchOS.

### Bug Fixes

- Fixes a crash in `CTInAppHTMLViewController` when the application presents an HTML In-App message.
- Fixes an issue that prevents Push Primer class methods from being exposed when integrated via SPM.
- Mitigates a potential error caused by dereferencing a garbage pointer in `persistOrClearQueues` after the application terminates.

# August 07

## [Unity 5.3.0](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.3.0)

### What's New

Enhances the CleverTap Unity SDK with expanded platform compatibility, geofencing capabilities, and new customization options.

- Supports [CleverTap Android SDK v7.5.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-750-july-11-2025).
- Supports [CleverTap iOS SDK v7.3.1](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-731-july-15-2025).
- Adds [Unity Native App Inbox](doc:unity-app-inbox) support.
- Adds compatibility with Unity 6.
- Enables geofencing with example activities for Android and iOS, including permissions and initialization.
- Enables iOS push notification service and content extensions with sample implementations.
- Adds support for custom In-App templates in Unity Editor.