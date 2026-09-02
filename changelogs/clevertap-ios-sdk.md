---
title: CleverTap iOS SDK
author: Bajrang
hidden: true
published_at: '2022-12-07T16:16:00.000Z'
---
Welcome to the CleverTap iOS SDK changelog. All notable changes to the iOS SDK are documented in this file.

# Version 4.1.4 (October 24, 2022)

**Changed**

* Allows additional special characters when setting a custom CleverTap ID.

# Version 4.1.3 (October 11, 2022)

**Fixed**

* Fixes the value of `wzrk_c2a` key for image-only in-app notification CTAs.
* Possible fix for crashes related to profile caches.
* Updates support for CleverTap Signed Call iOS SDK.

# Version 4.1.2 (September 16, 2022)

**Fixed**

* Fixes possible App Inbox crashes.
* Fixes NSKeyedUnarchiver console warnings

# Version 4.1.1 (July 06, 2022)

**Added**

* Adds support to `call onUserLogin`, `profileIncrementValueBy`, `profileDecrementValueBy` methods in a WebView.
* Updates certificates for SSL Pinning.

**Fixed**

* Fixes possible App Inbox crashes.

# Version 4.1.0 (June 16, 2022)

**Added**

* Adds analytics support for upcoming CleverTap Direct Call iOS SDK.\
  `CleverTap.sharedInstance()?.profileRemoveValue(forKey: )` can now remove PII data like Email, Phone, and Date Of Birth.

**Fixed**

* Fixes possible crashes by applying locks to make shared instances thread-safe.
* Mitigates FileManager deprecated APIs.
* Mitigates UIKit deprecated APIs.

# Version 4.0.1 (April 12, 2022)

**Fixed**

* Fixes `deviceID` so that it will be fetched from storage instead of memory variables.
* Fixes compile time errors when building SDK with `CLEVERTAP_NO_INBOX_SUPPORT`.
* Adds a check for `uname()` method when fetching platform name to avoid possible runtime crashes.

# Version 4.0.0 (February 7, 2022)

**Added**

* Adds Custom Proxy Domain functionality for Push Impressions and Events.
* Adds support for configurable CleverTap Profile identifiers.

**Changed**

* Use `resource_bundle` instead of resources in podspec.

# Version 3.10.0 (Aug 23, 2021)

**Added**

* Adds public methods for suspending/discarding & resuming InApp Notifications.
* Adds public methods to increment/decrement values set via User properties.
* Custom Deep-link handling for App inbox, push notifications, and in-app notifications.

**Changed**

* Usage `clevertap-prod.com` instead of `wzrkt.com`.
* Usage `spiky.clevertap-prod.com` instead of `spiky.wzrkt.com`.
* Refactor and Addresses iOS 15 beta fixes related to the App inbox.
* Synchronize access for `deviceName` and `model` property.

**Fixed**

* [#103](https://github.com/CleverTap/clevertap-ios-sdk/issues/103)
* [#137](https://github.com/CleverTap/clevertap-ios-sdk/issues/137)