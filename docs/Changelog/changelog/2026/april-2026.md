---
title: April 2026
deprecated: false
hidden: false
metadata:
  robots: index
---
We are excited to share the April SDK changelog for our product!

<Callout icon="📘" theme="info">
  #### Note

  Click each SDK header to view the corresponding GitHub repository for release details.
</Callout>

# April 30

## [Cordova 5.0.0](https://github.com/CleverTap/clevertap-cordova/releases/tag/5.0.0)

### What's new

#### Android

* Supports CleverTap [Android SDK 8.1.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-810-april-17-2026).
* Raises the minimum supported Android API level from 21 (Android 5.0) to 23 (Android 6.0).

#### iOS

* Supports CleverTap [iOS SDK 7.6.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-760-april-17-2026).

### API changes

#### Android and iOS

* Adds new `variants(successCallback)` API to fetch the active A/B experiment variants for the current user. Each variant object contains an `id` key mapping to the numeric ID of the variant.
* Updates `discardInAppNotifications()` with an optional boolean parameter `dismissInAppIfVisible`. When `true`, it immediately dismisses any currently visible In-App notification in addition to clearing the pending queue.
* Adds new `unmute()` API to resume event tracking, overriding any active mute period.

### Bug Fixes

#### iOS

* Fixes a bug where `onUserLogin` was creating additional guids for high level encryption.

## [React Native 4.1.0](https://github.com/CleverTap/clevertap-react-native/releases)

### What's New

#### Android and iOS

* Supports CleverTap [Android SDK 8.1.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-810-april-17-2026).
* Supports CleverTap[ iOS SDK 7.6.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-760-april-17-2026).
* Adds Picture-in-Picture In-App Notifications support.
* Adds GIF and video media support in In-App templates.

### API changes

#### Android and iOS

* Adds a new API `unmute()` to clear a backend-imposed mute state, resuming network operations immediately.

> ❗️ Critical Update
>
> The minimum supported Android API level has been raised to **23 (Android 6.0)**. Apps targeting API levels below 23 can no longer integrate this SDK version.
>
> ExoPlayer integration is deprecated. Migrate to Media3 before upgrading to v5.0.0.

# April 28

## [Xamarin DotNet 4.0.0](https://github.com/CleverTap/clevertap-xamarin/blob/master/CHANGELOG.md#version-400-android-dotnet-april-28-2026)

### What's New

* Supports CleverTap [Android SDK v8.1.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev8.1.0).

# April 23

## [Flutter 4.0.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/4.0.0)

### What's New

#### Android

* Supports CleverTap [Android SDK 8.1.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-810-april-17-2026).
* Deprecates ExoPlayer integration. Migration to Media3 is required before Android SDK 9.0.0.
* Upgrades `compileSDKVersion` to 36.

#### iOS

* Supports CleverTap [iOS SDK 7.6.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-760-april-17-2026).

#### Android and iOS

* Adds Picture-in-Picture In-App Notifications support.
* Adds GIF and video media support in In-App templates.
* Adds support for App Inbox default media view.
* Adds support for server-controlled mute durations via backend header.

### API changes

#### Android and iOS

* Adds new `unmute()` API to resume event tracking, overriding any active mute period.

### Bug Fixes

#### iOS

* Fixes a bug where `onUserLogin` was creating additional guids for high level encryption.

> ❗️ Critical Update
>
> The minimum supported Android API level has been raised to **23 (Android 6.0)**. Devices running Android 5.x (API levels 21–22) are no longer supported.

# April 17

## [CTNotificationContent 1.3.0 ](https://github.com/CleverTap/CTNotificationContent/releases/tag/1.3.0)

### What's New

* Adds a new Vertical Image push notification template which supports images and GIFs with CTA.

## [Android 8.1.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-810-april-17-2026)

> ❗️ Critical Update
>
> The minimum supported Android API level has been raised from **21 (Android 5.0)** to **23 (Android 6.0)**. Apps targeting API levels below 23 can no longer integrate this SDK version.

### What's New

* **Picture-in-Picture (PIP) In-App Notifications**: Adds support for PIP in-app notifications with compact, draggable floating windows that overlay app content. Supports image, GIF, and video with expand, collapse, drag-to-reposition (9-point snap grid), and configurable controls. Video playback requires AndroidX Media3.
* **In-App Media Support**: Adds GIF and video support across all in-app notification templates for richer in-app experiences.

### Enhancements

* **App Inbox Media Fallback**: Adds a fallback media view for App Inbox when the backend does not provide a media orientation, rendering images, GIFs, video posters, and audio thumbnails at their natural aspect ratio.
* **Network Monitoring**: Replaces deprecated network APIs with a modern `NetworkMonitor` for more accurate connectivity detection, including captive portal support.
* **ExoPlayer Deprecated**: Deprecates ExoPlayer (`com.google.android.exoplayer2`) support. Developers using ExoPlayer for video playback should migrate to AndroidX Media3 (`androidx.media3`). Support will be removed after Android v9.0.0. IDE tooling provides automatic migration suggestions.
* **Unmute API**: Adds `CleverTapAPI#unmute()` to resume normal event tracking and network operations without waiting for the backend-set mute period to expire.

### Bug Fixes

* Fixes an `IndexOutOfBoundsException` in file cleanup by using a thread-safe collection.

# April 14

## [Unity 5.5.3](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/CHANGELOG.md#version-553-14-april-2026)

### What's new

* Supports [CleverTap Android SDK v8.0.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev8.0.0).
* Supports [CleverTap iOS SDK v7.5.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.5.1).

### Enhancements

* Automates Android build fixes via `AndroidProjectPostProcessor` for AGP 8.x compatibility.

### Bug Fixes

* Fixes `SetDebugLevel` to correctly route to `CleverTapLogger` in desktop/editor builds.
* Fixes `.meta` file platform assignments to ensure proper OS targeting for SQLite (Unity 2021).

# April 13

## [Push Template 2.4.0 ](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTPUSHTEMPLATESCHANGELOG.md#version-240-april-13-2026)

### What’s New

* **Image with CTA**: Adds a new Vertical Image with CTA push notification template with support for multiple images, action buttons, gradient backgrounds, and rounded corners. Button border radius `pt_btn_border_radius` and width `pt_btn_border_width` are also configurable.
* **Timer Template Enhancements**: Adds configurable border radius `pt_chrono_border_radius`, border width `pt_chrono_border_width`, border color `pt_chrono_border_clr`, and gradient background support `pt_chrono_style, pt_chrono_grad_clr1, pt_chrono_grad_clr2, pt_chrono_grad_dir` for the chronometer display.
