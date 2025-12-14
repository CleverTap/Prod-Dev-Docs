---
title: March 2023
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
We are excited to share the CleverTap SDK changelog for March!

> 📘 Note
>
> Click each SDK header to view the corresponding GitHub repository for release details.

# March 31

## [Android 4.6.9](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.9)

* **Updated** the `itemIndex` parameter name to `contentPageIndex` in the `onInboxItemClicked `callback.
* **Updated** the value of `contentPageIndex` in the `onInboxItemClicked` callback. Previously, it is used to indicate the position of the clicked item within the list container of the App Inbox. Now, it indicates the page index of the content, which ranges from 0 to the total number of pages for carousel templates. For non-carousel templates, the value is always 0, as they only have one page of content. 

     For more details, refer to the [Android App Inbox](https://developer.clevertap.com/docs/android-app-inbox#app-inbox-item-and-button-click-callbacks).

# March 28

## [React Native 0.9.5](https://github.com/CleverTap/clevertap-react-native/releases/tag/0.9.5)

This release is being done for **Android 12** targeted users.

* **Added** the support for [CleverTap Android SDK v4.6.8](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.8), [CleverTap iOS SDK v4.2.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/4.2.1), [CleverTap Android RenderMax SDK v1.0.3](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.7.5_rmv1.0.3), and [CleverTap Push Templates SDK v1.0.5.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.7_ptv1.0.5.1). You must update to all these versions for compatibility and smooth working. 
* **Updated** the `CleverTap.CleverTapInboxMessageTapped` callback. For more information, refer to the [React Native App Inbox](https://developer.clevertap.com/docs/react-native-app-inbox#app-inbox-item-and-button-click-callbacks).
  * [Android and iOS Platforms] Added the `itemIndex` and `buttonIndex` arguments to the payload sent via App Inbox Message tapped listener: `CleverTap.CleverTapInboxMessageTapped`. The `itemIndex` corresponds to the index of the item clicked in the list, whereas the `buttonIndex` is for the App Inbox button clicked (0, 1, or 2). A value of -1 in `buttonIndex` indicates the App Inbox item is clicked.
  * [Android Platform] Previously, the `CleverTap.CleverTapInboxMessageTapped` callback was raised when the App Inbox item was clicked. Now, it is also raised when the App Inbox button is clicked. It matches the behavior in the iOS platform.

# March 24

## [Web 1.5.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.5.0)

* **Added** the new template *Image Only* in Web Pop-up. For more details, refer to the [Web Pop-up](https://developer.clevertap.com/docs/web-popup#web-pop-ups) documentation. 

# March 22

## [iOS 4.2.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/4.2.1)

* **Added** the public method `dismissAppInbox` to dismiss iOS App Inbox. For more information, refer to [iOS App Inbox](https://developer.clevertap.com/docs/app-inbox-ios#dismiss-app-inbox).
* **Added** the public method `enableLocation` for enabling location API in case of Swift Package Manager(SPM). For more details, refer to [iOS Quick Start Guide](https://developer.clevertap.com/docs/ios-quickstart-guide#get-location-information-in-application).
* **Added** the public method `markReadInboxMessagesForIDs` for marking multiple App Inbox messages as read by passing a collection of `messageIds`.  For more details, refer to [Create your Own App Inbox](https://developer.clevertap.com/docs/app-inbox-ios#step-3-creating-your-own-app-inbox).
* **Fixed** a bug where CoreData would crash with threading inconsistency exceptions.
* **Fixed** the crash caused due to the `deleteInboxMessagesForIDs` method when the `messageId` is null or invalid.

## [Android 4.6.8](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.8)

* **Updated** the `onInboxItemClicked` callback. For more information, refer to [Android App Inbox](https://developer.clevertap.com/docs/android-app-inbox#app-inbox-item-and-button-click-callbacks).
  * The method `onInboxItemClicked(CTInboxMessage message)` is changed to `onInboxItemClicked(CTInboxMessage message, int itemIndex, int buttonIndex)`. The `itemIndex` corresponds the index of the item clicked in the list whereas the `buttonIndex` is for the App Inbox button clicked (0, 1, or 2). A value of -1 in `buttonIndex` indicates the App Inbox item is clicked.
  * Previously, `onInboxItemClicked` callback was raised when the App Inbox Item is clicked. But, now, it is raised when both the App Inbox button and item are clicked.
* **Added** the new public API `dismissAppInbox()` via `CleverTapAPI` class to dismiss the Android App Inbox. For more information, refer to [Dismiss App Inbox](https://developer.clevertap.com/docs/android-app-inbox#dismiss-app-inbox).

# March 16

## [Unity 2.3.0](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/2.3.0)

* **Added** the Unity SDK compatibility for [CleverTap Android SDK v4.6.7](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.7_ptv1.0.5.1)\
  and [CleverTap iOS SDK v4.1.6](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/4.1.6).
* **Added** the support for *ExoPlayer v2.17.1*. This is applicable only for Android.
* **Added** the new callback `CleverTapInboxItemClicked` to receive the inbox item click. For more details, refer to [Unity App Inbox](https://developer.clevertap.com/docs/unity-app-inbox#unityc) documentation.
* **Added** the `deleteInboxMessagesForIds()` public method for deleting multiple App Inbox messages by passing a collection of `messageID`s. This is applicable only for iOS, and NO-OP (No Operation) for Android as of now. For more details, refer to [Create Your Own Unity App Inbox](https://developer.clevertap.com/docs/unity-app-inbox#create-your-own-app-inbox) documentation.

# March 15

## [Web 1.4.2](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.4.2)

* **Fixed** repeated request firings from the `processBackupEvents` functionality.
* **Fixed** notification rendering by Delivery Preferences. For more information, refer to [Delivery Preference](https://docs.clevertap.com/docs/create-message-web-popup#delivery-preferences).
* **Updated** charged item count limit from **16** to **50**. 

## [Android 4.6.7](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.7_ptv1.0.5.1)

* **Improved** overall SDK performance by SDK code optimization and bug fix.

## [Push Template 1.0.5.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.7_ptv1.0.5.1)

* **Added** the Push Templates SDK compatibility for [CleverTap Android SDK version 4.6.7](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.7_ptv1.0.5.1). *CleverTap Push Templates SDK v1.0.5.1* requires *CleverTap Android SDK v4.6.7* to work properly.

> 📘 Note
>
> [Android SDK version 4.6.7](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.7_ptv1.0.5.1) and [ Push Templates SDK v1.0.5.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.7_ptv1.0.5.1) releases are intended for **Android 12** targeted users, satisfying the below points:
>
> 1. Targeting Android 12 
> 2. Using RenderMax and/or using Push Templates

# March 08

## [Push Templates 1.0.8](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/ptv1.0.8)

* **Added** the Push Templates SDK compatibility for [CleverTap Android SDK version 4.7.5](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.7.5_rmv1.0.3). *CleverTap Push Templates SDK v1.0.8* requires *CleverTap Android SDK v4.7.5* to work properly.

# March 06

## [Core Android 4.7.5](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.7.5_rmv1.0.3)

* **Improved** overall SDK performance by SDK code optimization and bug fix.

## [RenderMax 1.0.3](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.7.5_rmv1.0.3)

* **Fixed** the Application Not Responding (ANR) issue while receiving push notifications through RenderMax.
* **Fixed** the crash caused due to the `java.lang.IllegalStateException` exception.

# March 2

## [mParticle Web 1.0.2](https://github.com/CleverTap/mparticle-web-integration-clevertap/releases/tag/1.0.2)

* **Fixed** the initialization condition issue for mParticle Webkit.
