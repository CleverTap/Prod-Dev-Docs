---
title: iOS Geofence SDK
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
## Overview

CleverTap Geofence SDK provides geofencing capabilities to CleverTap iOS SDK by using Core Location framework. This document lists down steps to utilize CleverTap Geofence in your apps. You can further customize your integration of CleverTap Geofence by using the [Geofence Customization document](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/Docs/Customization.md).

## Requirements

The following are required for using CleverTap Geofence SDK:

* [CleverTap iOS SDK version 3.9.0 or above](https://github.com/CleverTap/clevertap-ios-sdk/releases)
* Swift version 5.1 or above
* iOS version 10.0 or above
* CoreLocation iOS Framework

# Installation

## Via CocoaPods

[CocoaPods](https://cocoapods.org) is a dependency manager for iOS projects. To integrate CleverTap Geofence SDK into your Xcode project using CocoaPods, specify the following in your `Podfile`:

```
pod 'CleverTap-Geofence-SDK'
```

For installing CleverTap Geofence via [Carthage](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/Docs/Customization.md#carthage) or [Manually](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/Docs/Customization.md#manual), check out the steps in [Geofence Customization document](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/Docs/Customization.md#alternative-installation).

# Integration

CleverTap Geofence utilizes Core Location APIs to setup up Geofences Region monitoring.\
 The CleverTap Geofence will **NOT** request Location permissions from the user. Location Permission has to requested by the app as deemed fit while onboarding the user to the app.

1. In Xcode, enable `Location Updates` in `Background Modes` for your App Target. You can enable this in Xcode by:

* Click on your `AppTarget` in Xcode Project Navigator
* Click on `Signing & Capabilities` tab 
* Click on `+ Capability` button
* Select the `Background Modes` option
* Enable `Location Updates` by selecting the checkbox

<Image title="Enable Location Updates in Background Modes" alt={1600} align="center" border={true} src="https://files.readme.io/7f599de-Integration.png">
  Enable Location Updates in Background Modes
</Image>

2. In your `Info.plist` file, add the following keys:

* `NSLocationAlwaysAndWhenInUseUsageDescription` also known as `Privacy - Location Always and When In Use Usage Description`\
  This is a key that accepts a String description to be used by iOS while requesting Location permission from the user.

* `NSLocationWhenInUseUsageDescription` also known as `Privacy - Location When In Use Usage Description`\
  This is a key that accepts a String description to be used by iOS while requesting Location permission from the user.

* `UIBackgroundModes` also known as `Required background modes`\
  This is a key that accepts an Array of items. This should include the `location` also known as `App registers for location updates` item to enable Background Location monitoring.

  *For iOS 11 only*: If your app targets iOS 11 then along with the above-mentioned three Info.plist keys, the following key is also required. If your app target is iOS 12 or above, the following is not required:

* `NSLocationAlwaysUsageDescription` also know as `Privacy - Location Always Usage Description`\
  This is a key that accepts a String description to be used by iOS while requesting Location permission from the user. Only required for iOS 11.

  Following is an example of all the Info.plist keys in Source Code format:

```
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>App needs your location to provide enhanced features.</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>App needs your location to provide enhanced features.</string>
<key>NSLocationAlwaysUsageDescription</key>
<string>App needs your location to provide enhanced features.</string>
<key>UIBackgroundModes</key>
<array>
  <string>location</string>
  <string>remote-notification</string>
  <string>fetch</string>
  <string>processing</string>
</array>
```

<Image title="Information Property List in Info.plist" alt={2508} align="center" border={true} src="https://files.readme.io/f73e447-Info-plist.png">
  Information Property List in Info.plist
</Image>

3. In your `AppDelegate` file, import the CleverTapGeofence module:

```swift
import CleverTapGeofence
```
```objectivec
@import CleverTapGeofence;
```

4. In your AppDelegate's `application:didFinishLaunchingWithOptions:` function, add the following the code snippet. Ensure that Geofence SDK initialization is done **after** CleverTap Main SDK initialization.

```swift
CleverTapGeofence.monitor.start(didFinishLaunchingWithOptions: launchOptions)
```
```objectivec
[[CleverTapGeofence monitor] startWithDidFinishLaunchingWithOptions:launchOptions];
```

5. CleverTap Geofence SDK requires location permission from users to provide Geofencing capabilities. The App is expected to request Location permission from the user at an appropriate time. Once CleverTap Geofence SDK detects that Location permission has been given by the user, only then the module starts to perform its functions.

An example of how an app can request location permission is below:

```swift
let locationManager = CLLocationManager()
  locationManager.requestAlwaysAuthorization()
```
```objectivec
CLLocationManager *locationManager = [[CLLocationManager alloc] init];
  [locationManager requestAlwaysAuthorization];
```

<Image title="Request Location Permission Code" alt={2820} align="center" border={true} src="https://files.readme.io/6b0a51f-StartGeofence.png">
  Request Location Permission Code
</Image>

# Customization

You can further *customize* your integration of CleverTap Geofence by following the [Geofence Customization document](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/Docs/Customization.md).

## Example Usage

* A [demo application](https://github.com/CleverTap/clevertap-geofence-ios/tree/master/Examples/CocoapodsExample) showing CocoaPods Installation.
* A [demo application](https://github.com/CleverTap/clevertap-geofence-ios/tree/master/Examples/CarthageExample) showing Carthage Installation.

# Change Log

Refer to the [CleverTap Geofence SDK Change Log](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/CHANGELOG.md).

# Help and Questions

If you have questions or concerns, you can reach out to the CleverTap Support Team from your CleverTap Dashboard.

# License

CleverTap Geofence is MIT licensed, as found in the [LICENSE](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/LICENSE) file.
