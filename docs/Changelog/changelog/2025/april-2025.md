---
title: April 2025
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

# April 29

## [Web 1.14.4](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.14.4)

### Enhancements

* **Improves Custom HTML Rendering in Web Native Display**: The render logic now updates only the inner content of the specified `div`, preserving the element identifier and improving content stability.

# April 21

## [Web 1.14.3](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.14.3)

### Bug Fixes

* **Fixes Incorrect Index Argument Handling in Web Native Rendering**: Wraps the `addScriptTo` call in an arrow function during `forEach` iteration to prevent the index argument from being unintentionally passed. This ensures correct script execution in Web Native Custom HTML rendering.

# April 18

## [Flutter 3.3.1](https://github.com/CleverTap/clevertap-flutter/releases/tag/3.3.1)

### What’s New

#### iOS Platform

* Supports [CleverTap iOS SDK 7.1.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.1.1).
* Adds the `dismissInAppNotification` action to allow programmatic dismissal of custom HTML In-App notifications.

### Bug Fixes

#### Android Platform

* Fixes an issue where images failed to render in push templates using a custom Firebase Cloud Messaging (FCM) integration.

#### iOS Platform

* Fixes an app-freezing issue that occurred when using `await` with `defineVariables()`.
* Fixes device orientation checks for custom In-App notifications.

# April 17

## [Unreal SDK 0.0.1](https://github.com/CleverTap/clevertap-unreal-sdk/releases/tag/v0.0.1)

### What's New

* Added support for User Profile APIs to set and update user properties.
* Added support for Event APIs to log custom events and event properties.

This is the first release of the CleverTap Unreal SDK, providing basic integration for user identity and event tracking.

# April 14

## [Web 1.14.2](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.14.2)

### What's New

* Supports inline `<script>` tags for Custom HTML Template in Web Native Display, offering greater flexibility in designing interactive experiences.
* Strengthens validation and security checks around `postMessage` handling to address Cross-Site Scripting (XSS) vulnerability.

# April 10

## [iOS Push Templates for CTNotificationContent 0.3.0](https://github.com/CleverTap/CTNotificationContent/releases/tag/0.3.0)

### What's New

* Adds support for basic HTML tags such as bold, italic, underline, and strikethrough in push template titles and messages.

# April 09

## [Web 1.14.1](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.14.1)

### What's New

* Supports inline `<script>` tags within HTML content, offering greater flexibility for customization directly from the Visual Editor.

* Enables richer, dynamic interactions for Web Native Display campaigns created using custom HTML.
