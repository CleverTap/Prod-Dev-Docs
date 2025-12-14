---
title: November 2024
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
We are excited to share CleverTap's November SDK changelog!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# November 29

## [Web 1.11.10](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.10)

- Fixes bug with `dismissSpamControl` flag, where popups are not displayed despite enabling the feature.

## [iOS 7.0.3](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.0.3)

### What's New

- Supports the `setCredentials` method for setting up custom handshake domains.
- Supports previewing In-App messages created using the advanced builder.
- Supports parsing of URLs for the `open-url` action to track parameters in the URL for `Notification Clicked `events within HTML In-App messages.
- Supports the `promptForPushPermission` method in the JavaScript interface and HTML In-App messages.

### Bug Fixes

- Fixes a potential crash related to the `CTValidationResultStack` class.  
- Fixes a potential crash for the `[CTInAppHTMLViewController hideFromWindow:]` method.  
- Updates campaign triggering evaluation to ignore letter case and whitespace in event names, event properties, and profile properties.
- Fixes the issue where the `wzrk_c2a` value is passed as `null` to the backend when the `callToAction` value is received as `null` in a WebView message handler.

## [Android 7.0.3](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.0.3)

### What's New

- Supports previewing In-App messages created using the advanced builder.

### Bug Fixes

- Updates campaign triggering evaluation to ignore letter case and whitespace in event names, event properties, and profile properties.

# November 14

## [Web 1.11.9](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.9)

- Fixes issues with deleting and reading Web Inbox messages for custom inbox implementations.

# November 13

## [Web 1.11.8](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/1.11.8)

- Fixes an anti-flicker issue in the Visual Editor.

# November 11

## [Web 1.11.7](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.7)

- Fixes an issue with saving and fetching of Web Inbox messages for custom inbox.
- Adds an undefined check for the `Notification` object in Web Push.

# November 8

## [iOS Segment 1.4.0](https://github.com/CleverTap/clevertap-segment-ios/releases/tag/1.4.0)

- Supports [CleverTap iOS SDK v7.0.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.0.2)
- Supports custom code In-App template definitions through a JSON scheme.

# November 7

- [Web 1.11.6](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.11.6)Fixes an issue where the Web Inbox Open functionality failed in Single Page Applications (SPAs) due to stale nodes.

## [Android Segment 1.7.0](https://github.com/CleverTap/clevertap-segment-android/releases/tag/1.7.0)

- Supports [CleverTap Android SDK v7.0.2](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-702-october-10-2024)
- Supports custom code In-App template definitions through a JSON scheme.

## [React Native 3.1.1](https://github.com/CleverTap/clevertap-react-native/releases/tag/3.1.1)

### Bug Fixes

- **iOS**
  - Fixes an issue where the push notification callback gets triggered twice when the app is in the killed state.