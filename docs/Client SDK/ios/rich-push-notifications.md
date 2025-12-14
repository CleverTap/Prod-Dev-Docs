---
title: Rich Push Notifications
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
# Overview

iOS 10 has introduced [rich push notifications](https://developer.apple.com/videos/play/wwdc2016/707/) which provides the capability to add image, video, audio, or GIF attachments to your push notifications.

Rich push notifications are enabled via a [notification service extension](https://developer.apple.com/reference/usernotifications/unnotificationserviceextension), a separate and distinct binary embedded in your app bundle. Prior to displaying a new push notification, the system will call your notification service extension to modify the payload as well as add media attachments to be displayed.

# Enable Rich Push Notifications

The following section covers how to enable rich push notifications:

1. Enable push notifications for your app.
2. To create a notification service extension in your project, in Xcode, select *File* > *New* > *Target*.
3. Choose the *Notification Service Extension* template.

<Image title="ios10.png" alt={1438} className="border" border={true} src="https://files.readme.io/99f83fd-ios10.png" />

4. When sending notifications via [APNs](https://developer.apple.com/videos/play/wwdc2016/724/), include the mutable-content flag in your app payload.

> 🚧 Required Key
>
> This key must be present in the app payload or the system will not call your app extension.

# Send Rich Push Notifications

When using the CleverTap Dashboard to send rich push notifications:

1. Select the *Advanced* checkbox.
2. Select the *Mutable Content* checkbox.

![2004](https://files.readme.io/30be372-10.png "10.png")

3. Add your custom key-value pair(s).

![1554](https://files.readme.io/5a27c86-Custom_key-value_pairs.png "Custom key-value pairs.png")

When using the CleverTap Server API to send push: include "mutable-content": "true" in the platform\_specific: iOS section of the request payload.

# Examples

Below are some project examples:

* [Swift project](https://github.com/CleverTap/notification-examples-ios10/blob/master/notif10swift/NotificationService/NotificationService.swift)
* [Objective-C project](https://github.com/CleverTap/notification-examples-ios10/blob/master/notif10objc/NotificationService/NotificationService.m)
