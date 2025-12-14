---
title: December 2025
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
We are excited to share the December SDK changelog for our product!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# December 02

## [Android 7.7.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev7.7.1)

### What’s New

- Adds a public `variants` method to fetch A/B experiment variants for the user. This makes it easier to access experiment data in custom implementations.

### Bug Fixes

- Fixes prevented gradient backgrounds and other _HTML_ send-test flows from rendering correctly.

## [iOS 7.4.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.4.1)

### What's New

- Adds a public `variants` method to fetch A/B experiment variants for the user. This makes it easier to access experiment data in custom implementations.

### Bug Fixes

- Fixes an issue where the SDK recorded _App Launched_ events when the app was woken up by silent push notifications.
- Fixes _HTML_ In-App previews that were not rendering gradient colors.