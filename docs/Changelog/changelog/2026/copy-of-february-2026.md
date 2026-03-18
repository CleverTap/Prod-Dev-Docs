---
title: March 2026
deprecated: false
hidden: false
metadata:
  robots: index
---
We are excited to share the March SDK changelog for our product!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# March 17

## [React Native 4.0.0](https://github.com/CleverTap/clevertap-react-native/blob/master/CHANGELOG.md#version-400-march-17-2026)

### Android

Supports CleverTap Android SDK v8.0.0.

### iOS

Supports CleverTap iOS SDK v7.5.1.

#### Bug Fixes

* Fixes a bug where `CleverTapInAppNotificationShowed` was not called.

# March 4

## [iOS 7.5.1](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-751-march-4-2026).

**What's new**

* Adds deep link URL capture in InApp notification click events, ensuring consistent attribution with Push notifications. Supports button-level deep links for CTA clicks and template-level deep links for image-only and HTML templates.

**Bug Fixes**

* Fixes a bug where `onUserLogin` was creating additional GUIDs only for encryption level high.

# March 2

## [Flutter 3.8.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/3.8.0) / [React Native 3.9.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/3.9.0)

### Android

Supports CleverTap Android SDK v7.8.0.

### iOS

Supports CleverTap iOS SDK v7.5.0.

### Both Android and iOS

#### What's new

* Adds support for in-action In-App notifications. This server-side feature allows triggering In-App notifications to users who do not perform specific actions within a defined timeframe.
* Adds support for nested object ingestion in event and profile properties, enabling structured data modeling. This enables more granular segmentation, improved personalization, and deeper analytics of complex user behavior.
