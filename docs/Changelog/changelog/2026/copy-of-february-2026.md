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

# March 4

### iOS

Supports [CleverTap iOS SDK v7.5.1](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-751-march-4-2026).

**What's new**

* Adds deep link URL capture in InApp notification click events, ensuring consistent attribution with Push notifications. Supports button-level deep links for CTA clicks and template-level deep links for image-only and HTML templates.

**Bug Fixes**

* Fixes a bug where `onUserLogin` was creating additional GUIDs only for encryption level high.

# March 2

## React Native 3.9.0

### Android

Supports CleverTap Android SDK v7.8.0.

### iOS

Supports CleverTap iOS SDK v7.5.0.

### Both Android and iOS

* Adds support for in-action-based In-App notifications. This server-side feature allows triggering In-App notifications when users do not perform specific actions within a defined timeframe.
* Adds support for nested objects ingestion in event and profile properties, enabling structured data modeling for richer user profiles and more detailed event tracking.

## Flutter 3.8.0

### Android

Supports [CleverTap Android SDK v7.8.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-780-january-22-2026).

### iOS

Supports [CleverTap iOS SDK v7.5.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-750-february-9-2026).

### Both Android and iOS

* Adds support for in-action-based In-App notifications. This server-side feature allows triggering InApp notifications when users do not perform specific actions within a defined timeframe.
* Adds support for ingesting nested objects in event and profile properties, enabling more complex data structures for richer user profiling and event tracking.
