---
title: February 2026
deprecated: false
hidden: false
metadata:
  robots: index
---
We are excited to share the February SDK changelog for our product!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# February 19

## [CTNotificationContent 1.2.0](https://github.com/CleverTap/CTNotificationContent/releases/tag/1.2.0)

### What's new

* Supports displaying GIFs in `Basic`, `Timer`, `Zero Bezel`, and `Rating` templates.
* Supports configuring transparency for title, message, and background colors.

# February 18

## [Unity 5.5.2](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.5.2)

### What's new

* Supports [CleverTap Android SDK v7.8.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-780-january-22-2026).
* Supports [CleverTap iOS SDK v7.5.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-750-february-9-2026).
* Support for ingestion of _Nested Objects_ in event properties and profile properties, enabling richer engagement through more structured analytics.

# February 09

## [iOS 7.5.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.5.0)

### What's New

* Support for ingestion of _Nested Objects_ in event properties and profile properties, enabling richer engagement through more structured analytics.

> 🚧 Account-Controlled Feature
>
> This feature is account-controlled. Contact [CleverTap Support](https://help.clevertap.com/en-US/clevertapsupport/directories) to enable support for nested objects on your account.

* Support for inaction-based In-App notifications. You can configure an inaction duration between _01_ to _1,200_ seconds to trigger an In-App notification when a user does not perform an expected action within the specified time window.
* Adds Swift Package Manager (SPM) support for tvOS.

### Bug Fixes

* Refactored console logging to use `OSLog` instead of `NSLog`.

# February 06

## [Flutter 3.7.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/3.7.0)

### What’s New

* Supports [CleverTap Android SDK v7.7.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-771-december-2-2025).
* Supports [CleverTap iOS SDK v7.4.2](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-742-january-14-2026).

### API Changes

#### Android and iOS

* Adds a public `getVariants()` method to fetch A/B experiment variants for the user. This makes it easier to access experiment data when building custom implementations.
* Adds overloaded version of the `discardInAppNotifications()` method. When called with `true`, it immediately dismisses any currently visible In-App notification and clears all pending notifications from the queue.
  * `discardInAppNotifications({bool? dismissInAppIfVisible})`

# February 04

## [Unity 5.5.1](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.5.1)

### What’s New

* Adds support for the `GetVariants` method.

### Bug Fixes

* Fixes an issue where `IOSPostBuildProcessor` was excluded in iOS builds.
