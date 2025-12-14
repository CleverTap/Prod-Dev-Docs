---
title: iOS SDK Installation
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
# Overview

CleverTap brings together real-time user insights, an advanced segmentation engine, and easy-to-use marketing tools in one mobile marketing platform — giving your team the power to create amazing experiences that deepen customer relationships. Our intelligent mobile marketing platform provides the insights you need to keep users engaged and drive long-term retention and growth.

To get started, sign up [here](https://clevertap.com/live-product-demo/).

# Requirements

The following are required for using the CleverTap iOS SDK:

* iOS 9.0 or later
* tvOS 9.0 or later
* Xcode 10.0 or later

# Installation

The following provides details about the different installation methods:

1. [CocoaPods](/docs/CocoaPods.md)
2. [Swift Package Manager](/docs/SwiftPackageManager.md)
3. [Carthage](/docs/Carthage.md)
4. [Manual Installation](/docs/Manual.md)

# Integration

## Add your CleverTap account credentials

Update your .plist file:

* Create a key called **CleverTapAccountID** with a string value
* Create a key called **CleverTapToken** with a string value
* Insert the values from your CleverTap [Dashboard](https://dashboard.clevertap.com) > *Settings* > *Integration Details*.

For more details, refer to our [installation guide](https://developer.clevertap.com/docs/ios-quickstart-guide) for instructions on installing and using our iOS SDK in your project.

## Rich Push Notifications

Apart from the title and message, you have the following options to add to your iOS push notification. Note that each of these is optional.

* [CTNotificationService](https://github.com/CleverTap/CTNotificationService)
* [CTNotificationContent](https://github.com/CleverTap/CTNotificationContent)

For more details, refer to our [Advanced iOS Push Notifications](https://developer.clevertap.com/docs/ios#section-advanced-ios-push-notifications) guide.

## Geofence

CleverTap Geofence SDK provides Geofencing capabilities to CleverTap iOS SDK. To find the installation & integration steps for CleverTap Geofence SDK, click [here](https://github.com/CleverTap/clevertap-geofence-ios).

### Example Usage

* A [demo application](/ObjCStarter) showing the integration of our SDK in Objective-C language.
* A [demo application](/SwiftStarter) showing the integration of our SDK in Swift language.
* A [demo application](/SPMStarter) showing the installation of our SDK via Swift Package Manager.

## Change Log

Refer to the [CleverTap iOS SDK Change Log](/CHANGELOG.md).

## License

CleverTap iOS SDK is released under the MIT license. See [LICENSE](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/LICENSE) for details.
