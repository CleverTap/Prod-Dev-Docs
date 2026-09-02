---
title: CleverTap Android SDK
author: Bajrang
hidden: true
published_at: '2022-12-07T16:17:00.000Z'
---
Welcome to the CleverTap Android SDK changelog. All notable changes to the Android SDK are documented in this file.

# v4.7.2

### December 16, 2022

![FIXED](https://files.readme.io/945ebe7-Tags.svg)

* Fixed a crash `ClassCastException` in header/footer In-App templates.

# Previous Versions

<details>
  <summary>Click to expand</summary>

# Version 4.7.1

### December 5, 2022

**FIXED**

* Fixes ANR on the main thread for static initialization of `SimpleDateFormat()`.

**ADDED**

* Add Proguard rules to keep `CREATOR` instance for Parcelable classes to prevent `ClassNotFoundException` when unmarshalling: `androidx.fragment.app.FragmentManagerState`.
* Made calls to `getInstallReferrer()` async to prevent ANR when called on the main thread.
* Made calls to `findCTPushProvider()` and `findCustomEnabledPushTypes()` async to prevent ANR when called on the main thread.
* Adds `unregisterPushPermissionNotificationResponseListener(PushPermissionResponseListener)` method in `CleverTapAPI` class to unregister the `PushPermissionResponseListener instance` to stop observing the Push Primer result.

**CHANGED**

* Used `ConcurrentHashMap` instead of `HashMap` for storing `CleverTapAPI` instances to prevent `ConcurrentModificationException` when trying to access the instances concurrently.
* Renames `setPushPermissionNotificationResponseListener(PushPermissionResponseListener)` to `registerPushPermissionNotificationResponseListener(PushPermissionResponseListener)`. Each `PushPermissionResponseListener` instance passed in this method is now maintained in a list of the `PushPermissionResponseListener` type, and the Push Primer result is notified to all the elements of this list.

# Version 4.7.0

### November 1, 2022

* Adds below new public APIs for supporting Android 13 notification runtime permission
  * `isPushPermissionGranted()` [Usage can be found here](https://developer.android.com/develop/ui/views/notifications/notification-permission).
  * `promptPushPrimer(JSONObject)` [Usage can be found here](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/EXAMPLES.md#push-primer-android-13-notification-runtime-permission).
  * `promptForPushPermission(boolean showFallbackSettings)` [Usage can be found here](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/EXAMPLES.md#push-primer-android-13-notification-runtime-permission).
* A new `CTLocalInApp` builder class is available to create half-interstitial & alert local in-apps to request notification permission [Usage can be found here](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/EXAMPLES.md#ctlocalinapp-builder-methods-description).
* New callback PushPermissionResponseListener is available, which returns after the user Allows/Denies notification permission [Usage can be found here](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/EXAMPLES.md#available-callbacks-for-push-primer).
* From 4.7.0+ existing callback `InAppNotificationListener` will now have `onShow(CTInAppNotification)` method, which needs to implement.
* The minimum Android SDK version has been bumped to API 19 (Android 4.4).

# Version 4.6.6

### October 31, 2022

* Fixes App Inbox bug where an Inbox message's video would not play when new Inbox messages were available.

# Version 4.6.5

### October 21, 2022

* Allows more special characters when setting custom CleverTap ID.

# Version 4.6.4

### October 11, 2022

* Bug fixes and improvements.

# Version 4.6.3

### September 20, 2022

* Fix crash in App inbox when No additional tabs are used.

# Version 4.6.3

### September 20, 2022

* Fix crash in App inbox when No additional tabs are used.

# Version 4.6.2

### September 13, 2022

* Support for exoplayer [v2.17.1](https://github.com/google/ExoPlayer/releases/tag/r2.17.1). Note: this upgrade will result in minor UI changes for interstitial in-app and inbox notifications that use exoplayer.

> 📘 Note
>
> Kindly upgrade to version CleverTap Android SDK v4.6.3 and above if you face any issues with the app inbox.

# Version 4.6.1

### September 6, 2022

* App inbox blue dot fix: This release fixes the bug where new entries in the inbox would continue showing a blue dot when scrolled up and down. the expected behavior is to stop showing a blue dot after 2 seconds.
* App inbox onMessage Click Callback: user can now pass an `InboxMessageListener` in addition to `InboxMessageButtonListener` to receive an inbox item click.

> 📘 Note
>
> Kindly upgrade to version CleverTap Android SDK v4.6.3 and above if you face any issues with the app inbox..

# Version 4.6.0

### August 4, 2022

* Improved push synchronization for multiple push services

</details>

<HTMLBlock>{`

`}</HTMLBlock>