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

# [Unity 5.5.1](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.5.1)

### What’s New

* Adds support for the `GetVariants` method.

### Bug Fixes

* Fixes an issue where `IOSPostBuildProcessor` was excluded in iOS builds.
