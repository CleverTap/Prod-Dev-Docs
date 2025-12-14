---
title: April 2023
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
We are excited to share the April SDK changelog for our product!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# April 19

## [Node 1.3.0](https://github.com/CleverTap/clevertap-node/releases/tag/v1.3.0)

- **Added** the missing APIs for Profile, Events, Reports and Campaign in [CleverTap Node](https://github.com/CleverTap/clevertap-node/) repository.
- **Fixed** the [Invalid JavaScript ](https://github.com/CleverTap/clevertap-node/issues/11) issue.

# April 18

## [Cordova 2.6.2](https://github.com/CleverTap/clevertap-cordova/releases/tag/2.6.2)

- **Fixed** compilation errors in Xcode 14.3 and higher versions for iOS.
- **Added** the support for [iOS SDK v4.2.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/4.2.2).

# April 11

## [Unity 2.3.1](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/2.3.1)

- **Added** the support for [CleverTap Android v4.6.9](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.9) and [iOS v4.2.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/4.2.2) SDKs.
- **Fixed** the mapping issues related to the `CreateNotificationChannelWithSound`, `CreateNotificationChannelWithGroup`, and `CreateNotificationChannelWithGroupAndSound` methods in Android.
- **Updated** the callback `CleverTapInboxItemClicked` to receive inbox item click with item payload. The `ContentPageIndex` corresponds to the index of the item clicked in the list whereas the `ButtonIndex` corresponds to the App Inbox button clicked (0, 1, or 2). A value of -1 indicates the App Inbox item is clicked.
- **Added** the `DismissAppInbox` API to dismiss the App Inbox. For more details, refer to [Unity App Inbox](https://developer.clevertap.com/docs/unity-app-inbox#create-your-own-app-inbox) document.

# April 06

## [Web 1.5.1](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.5.1)

- **Fixed** the issue for Web Inbox, where Document Object Model (DOM) was not loaded during inbox-selector search.

# April 05

## [Flutter 1.5.6](https://github.com/CleverTap/clevertap-flutter/releases/tag/1.5.6)

This release is being done for **Android 12** targeted users.

- **Added** the support for [CleverTap Android v4.6.9](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.9) and [iOS v4.2.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/4.2.2) SDKs.
- **Added** the new public API `dismissInbox()` to dismiss the App Inbox.
- **Updated** the `CleverTapInboxNotificationMessageClickedHandler(Map\<String, dynamic>? data)` callback. For more details, refer to the [Flutter App Inbox](https://developer.clevertap.com/docs/flutter-app-inbox#app-inbox-item-and-button-click-callbacks).
  - The `CleverTapInboxNotificationMessageClickedHandler(Map<String, dynamic>? data)` callback is changed to `CleverTapInboxNotificationMessageClickedHandler(Map<String, dynamic>? data, int contentPageIndex, int buttonIndex)`. The `contentPageIndex` corresponds to the page index of the content, which ranges from 0 to the total number of pages for carousel templates. For non-carousel templates, the `contentPageIndex `value is always 0, as they only have one page of content. The `buttonIndex` corresponds to the the App Inbox button clicked (0, 1, or 2). A value of -1 in `buttonIndex` field indicates the entire App Inbox Item is clicked.
  - Previously, the callback was raised when the App Inbox item is clicked. Now, it is also raised when the App Inbox button is clicked besides the item click.
- **Updated** the format of native display payload across Android and iOS.
- **Updated** the argument of `onDisplayUnitsLoaded` callback method in iOS to pass the list of `displayUnits`.
- **Fixed** the FCM Plugin's [onBackgroundMessage handler](https://github.com/CleverTap/clevertap-flutter/commit/8db6f34eec83e7f14990359f88c65a50e966acb3) bug which was breaking the CleverTap Plugin's platform channel for sending method calls from Android to Dart platform.
- **Fixed** compilation errors in Xcode 14.3 and higher for iOS.

# April 04

## [Flutter 1.6.1](https://github.com/CleverTap/clevertap-flutter/releases/tag/1.6.1)

- **Fixed** compilation errors in Xcode v14.3 and higher for iOS.
- **Added** support to pass the display unit array directly by streamlining the argument of the `onDisplayUnitsLoaded` callback method in iOS.
- **Added** support for [CleverTap iOS SDK v4.2.2](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-422-april-03-2023).

# April 03

## [iOS 4.2.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/4.2.2)

- **Fixed ** the compilation errors in Xcode v14.3 and higher for iOS. For more details, refer to the [iOS App Inbox ](https://developer.clevertap.com/docs/app-inbox-ios#app-inbox-item-and-button-click-callbacks).
- **Added** the guard rails to prevent crashes for background state tasks.

## [React Native 0.9.6](https://github.com/CleverTap/clevertap-react-native/releases/tag/0.9.6)

- **Added** the support for [CleverTap Android v4.6.9](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev4.6.9) and [iOS v4.2.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/4.2.2) SDKs.
- **Updated** the name of the `itemIndex` parameter to `contentPageIndex` in the `CleverTap.CleverTapInboxMessageTapped` callback payload.
- **Updated** the value of `contentPageIndex` in the `CleverTap.CleverTapInboxMessageTapped` callback. Previously, it indicated the clicked item's position within the App Inbox list container. Now, it indicates the page index of the content, which ranges from 0 to the total number of pages for carousel templates. For non-carousel templates, the value is always 0, as they only have one page of content. For more details, refer to the [React Native App Inbox](https://developer.clevertap.com/docs/react-native-app-inbox#app-inbox-item-and-button-click-callbacks).
- **Added** the support for type definitions for the event names available for Javascript.