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

iOS 10 has introduced [Rich Push Notifications](https://developer.apple.com/videos/play/wwdc2016/707/), which allow you to add image, video, audio or gif attachments to your push notifications.

Rich Push Notifications are enabled via a [Notification Service Extension](https://developer.apple.com/reference/usernotifications/unnotificationserviceextension), a separate and distinct binary embedded in your app bundle. Prior to displaying a new push notification, the system will call your Notification Service Extension to allow you to modify the payload as well as add media attachments to be displayed.

# Instructions

First, enable push notifications for your app.

Second, create a Notification Service Extension in your project. To do that, in Xcode, select File -> New -> Target and choose the Notification Service Extension template.

<Image title="ios10.png" alt={1438} className="border" border={true} src="https://files.readme.io/99f83fd-ios10.png" />

Then, when sending notifications via [APNS](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html):

* Include the mutable-content flag in your aps payload (this key must be present in the aps payload or the system will not call your app extension)
* Add custom key-value pair(s) to the payload with the necessary data to construct the download url for the media you want to display (your app extension code will then read these key-value pairs to initiate the media download on the device). Apple supports images, video, audio and gif.

When using the CleverTap Dashboard to send push:

* Select Advanced.
* Set the mutable-content flag check box.
* Add your key-value pair(s).

![2004](https://files.readme.io/30be372-10.png "10.png")

![2682](https://files.readme.io/5eeaf51-11.png "11.png")

When using the CleverTap Server API to send push: include "mutable-content": "true" in the platform\_specific: ios section of the request payload.

# Notes

* See an example [Swift project here](https://github.com/CleverTap/notification-examples-ios10/blob/master/notif10swift/NotificationService/NotificationService.swift).
* See an example [Objective-C project here](https://github.com/CleverTap/notification-examples-ios10/blob/master/notif10objc/NotificationService/NotificationService.m).
