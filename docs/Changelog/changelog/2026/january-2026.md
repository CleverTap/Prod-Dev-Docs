---
title: January 2026
deprecated: false
hidden: false
metadata:
  robots: index
---
We are excited to share the January SDK changelog for our product!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# January 22

## [Android 7.8.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.8.0)

### What's New

* Support for ingestion of _Nested objects_ in event properties and profile properties, enabling richer engagement through more structured analytics.

> 🚧 Account-Controlled Feature
>
> This feature is account-controlled. Contact [CleverTap Support](https://help.clevertap.com/en-US/clevertapsupport/directories) to enable support for nested objects on your account.

* Support for inaction-based In-App notifications. You can configure an inaction duration between _01_ and _1,200_ seconds to trigger an In-App notification when a user does not perform an expected action within the specified time window.

### Bug Fixes

* Fixes `AbstractMethodError` related to `setTheme` in `InAppNotificationActivity`.

## [Unity 5.5.0](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.5.0)

### What’s New

* Supports [CleverTap Android SDK v7.7.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.7.1)
* Supports [CleverTap iOS SDK v7.4.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.4.1)
* Supports managing multiple CleverTap environments in a single Unity project. Refer to [Multi-Environment Credential Support](doc:unity-multi-environment-credential-support).
* Updated _x64_ and _x86_ `sqlite3.dll` binaries and their associated `.meta` files.
  SQLite has been updated to version 3.51.1.
* Supports tracking the app launch event processing state for _Queue Custom Events_.

# January 14

## [React Native 3.8.1](https://github.com/CleverTap/clevertap-react-native/releases/tag/3.8.1)

### What's new

* Supports [CleverTap iOS SDK v7.4.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.4.2).

### Bug Fixes

* Fixes an issue where the SDK passed the Date of Birth (DOB) value incorrectly during profile events.
* Improves app startup performance by loading variable and variant caches earlier during initialization.

## [iOS SDK 7.4.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.4.2)

### Bug Fixes

* Fixes an issue where the SDK passed the Date of Birth (DOB) value incorrectly during profile events.
* Improves app startup performance by loading variable and variant caches earlier during initialization.
