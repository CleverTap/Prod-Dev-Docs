---
title: Flutter Resources
excerpt: View our sample app, usage document, and changelog.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Sample App

Check out our [Example Flutter project](https://github.com/CleverTap/clevertap-flutter/tree/master/example)

# Changelog

Refer to the [CleverTap Flutter SDK Change Log](https://github.com/CleverTap/clevertap-flutter/tree/master/CHANGELOG.md).

# FAQ

### How to manage the `onSelectNotification` callback function issue while using the `flutter_local_notifications` package in iOS?

For iOS, you can remove `[CleverTap autoIntegrate]` from the `AppDelegate` file and manually handle the notifications. This solves the `onSelectNotification` callback function issue.