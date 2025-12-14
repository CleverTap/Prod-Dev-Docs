---
title: November 2025
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
We are excited to share the November SDK changelog for our product!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# November 25

## [Unity 5.4.0](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.4.0)

### What’s New

- Supports Unity 2021 by adding the required `using System.Text;` directive in `UnityNativeRequest`.
- Detects platform string variables containing JSON data and returns them as JSON objects instead of plain quoted strings.
- Supports [CleverTap Android SDK v7.7.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-770-november-21-2025).
- Supports [CleverTap iOS SDK v7.4.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-740-november-21-2025).

# November 21

## [Android 7.7.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.7.0)

### What’s New

- Data Encryption Support: Adds support for full encryption of all data stored by the SDK, enhancing security and privacy. For setup instructions, refer to the guide on [Data Encryption](doc:android-advanced-features#encryption-of-pii-data).

- Delayed In-App Notifications: New configuration option to delay In-App messages by _1–1200_ seconds after the trigger event.

- Universal In-App Previews: Adds support for previewing all types of In-App messages directly from the dashboard.

- Foreground Push Notification Control: Adds support for silent push notifications when the app is in the foreground using `wzrk_sif: true`.

- In-App Dismissal Control: Extended the `discardInAppNotifications()` method to include an optional `dismissInAppIfVisible: Boolean` parameter. Passing `true` immediately dismisses any currently visible In-App notification and clears the pending queue.

### Bug Fixes

- Fixes `StringIndexOutOfBoundsException` caused by invalid color values in In-App payloads.

## [iOS 7.4.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.4.0)

### What's New

- Data Encryption Support: Adds support for full encryption of all data stored by the SDK, enhancing security and privacy. For setup instructions, refer to the guide on [Data Encryption](doc:advanced-options-ios#encryption-of-pii-data).
- Universal In-App Previews: Adds support for previewing all types of In-App messages directly from the dashboard.
- In-App Dismissal Control: Extended the `discardInAppNotifications()` method with an optional boolean parameter. This parameter controls whether an active In-App notification is dismissed immediately.
- Delayed In-App Notifications: New configuration option to delay In-App messages by _1–1200_ seconds after the trigger event.

> 🚧 Downgrade Advisory
> 
> After upgrading the SDK to version 7.4.0 or later, do not downgrade in subsequent app releases if encryption is enabled. If you encounter issues, contact the [CleverTap Support](https://help.clevertap.com/hc/en-us/requests/new) team.

# November 03

## [Flutter 3.6.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/3.6.0)

### What's New

- Supports [CleverTap Android SDK v7.6.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-760-october-17-2025).

## [Unity 5.3.7](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/5.3.7)

### What's New

- Adds _Preserve_ attribute to the property setters in `UnityNativeEventDBEntry`. This ensures that Unity retains these setters during IL2CPP (Intermediate Language to `C++`) builds, so event data is stored and retrieved correctly.
- Supports [CleverTap Android SDK v7.6.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-760-october-17-2025).
- Supports [CleverTap iOS SDK v7.3.4](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-734-october-29-2025)