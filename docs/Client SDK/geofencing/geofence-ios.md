---
title: Geofence iOS SDK
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
[block:api-header]
{
  "title": "Overview"
}
[/block]
CleverTap Geofence SDK provides geofencing capabilities to CleverTap iOS SDK by using Core Location framework. This document lists down steps to utilize CleverTap Geofence in your apps. You can further customize your integration of CleverTap Geofence by using the [Geofence Customization document](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/Docs/Customization.md).

## Requirements
The following are required for using CleverTap Geofence SDK:
- [CleverTap iOS SDK version 3.9.0 or above](https://github.com/CleverTap/clevertap-ios-sdk/releases)
- Swift version 5.1 or above
- iOS version 10.0 or above
- CoreLocation iOS Framework

# Installation

##Via CocoaPods

[CocoaPods](https://cocoapods.org) is a dependency manager for iOS projects. To integrate CleverTap Geofence SDK into your Xcode project using CocoaPods, specify the following in your `Podfile`:

```
pod 'CleverTapGeofence'
```
For installing CleverTap Geofence via [Carthage](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/Docs/Customization.md#carthage) or [Manually](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/Docs/Customization.md#manual), check out the steps in [Geofence Customization document](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/Docs/Customization.md#alternative-installation).

# Integration

CleverTap Geofence utilizes Core Location APIs to setup up Geofences Region monitoring.
 The CleverTap Geofence will **NOT** request Location permissions from the user. Location Permission has to requested by the app as deemed fit while onboarding the user to the app.

1. In Xcode, enable `Location Updates` in `Background Modes` for your App Target. You can enable this in Xcode by:
  - Click on your `AppTarget` in Xcode Project Navigator
  - Click on `Signing & Capabilities` tab 
  - Click on `+ Capability` button
  - Select the `Background Modes` option
  - Enable `Location Updates` by selecting the checkbox
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7f599de-Integration.png",
        "Integration.png",
        1600,
        1088,
        "#36373a"
      ]
    }
  ]
}
[/block]
  
2. In your `Info.plist` file, add the following keys:
  - `NSLocationAlwaysAndWhenInUseUsageDescription` also known as `Privacy - Location Always and When In Use Usage Description`
   This is a key that accepts a String description to be used by iOS while requesting Location permission from the user.

  - `NSLocationWhenInUseUsageDescription` also known as `Privacy - Location When In Use Usage Description`
  This is a key that accepts a String description to be used by iOS while requesting Location permission from the user.

  - `UIBackgroundModes` also known as `Required background modes`
  This is a key that accepts an Array of items. This should include the `location` also known as `App registers for location updates` item to enable Background Location monitoring.

  *For iOS 11 only*: If your app targets iOS 11 then along with the above-mentioned three Info.plist keys, the following key is also required. If your app target is iOS 12 or above, the following is not required:
  - `NSLocationAlwaysUsageDescription` also know as `Privacy - Location Always Usage Description`
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
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f73e447-Info-plist.png",
        "Info-plist.png",
        2508,
        1290,
        "#3c3d3f"
      ]
    }
  ]
}
[/block]
3. In your `AppDelegate` file, import the CleverTapGeofence module:
[block:code]
{
  "codes": [
    {
      "code": "  import CleverTapGeofence",
      "language": "swift"
    },
    {
      "code": "  @import CleverTapGeofence;",
      "language": "objectivec"
    }
  ]
}
[/block]
 
4. In your AppDelegate's `application:didFinishLaunchingWithOptions:` function, add the following the code snippet. Ensure that Geofence SDK initialization is done **after** CleverTap Main SDK initialization.
[block:code]
{
  "codes": [
    {
      "code": "  CleverTapGeofence.monitor.start(didFinishLaunchingWithOptions: launchOptions)",
      "language": "swift"
    },
    {
      "code": "  [[CleverTapGeofence monitor] startWithDidFinishLaunchingWithOptions:launchOptions];",
      "language": "objectivec"
    }
  ]
}
[/block]
5. CleverTap Geofence SDK requires location permission from users to provide Geofencing capabilities. The App is expected to request Location permission from the user at an appropriate time. Once CleverTap Geofence SDK detects that Location permission has been given by the user, only then the module starts to perform its functions.

An example of how an app can request location permission is below:
[block:code]
{
  "codes": [
    {
      "code": "  let locationManager = CLLocationManager()\n  locationManager.requestAlwaysAuthorization()",
      "language": "swift"
    },
    {
      "code": " CLLocationManager *locationManager = [[CLLocationManager alloc] init];\n  [locationManager requestAlwaysAuthorization];",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6b0a51f-StartGeofence.png",
        "StartGeofence.png",
        2820,
        1726,
        "#34353a"
      ]
    }
  ]
}
[/block]
# Customization

You can further *customize* your integration of CleverTap Geofence by following the [Geofence Customization document](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/Docs/Customization.md).

## Example Usage

- A [demo application](https://github.com/CleverTap/clevertap-geofence-ios/tree/master/Examples/CocoapodsExample) showing CocoaPods Installation.
- A [demo application](https://github.com/CleverTap/clevertap-geofence-ios/tree/master/Examples/CarthageExample) showing Carthage Installation.

# Change Log

Refer to the [CleverTap Geofence SDK Change Log](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/CHANGELOG.md).

#Help and Questions

If you have questions or concerns, you can reach out to the CleverTap Support Team from your CleverTap Dashboard.

# License

CleverTap Geofence is MIT licensed, as found in the [LICENSE](https://github.com/CleverTap/clevertap-geofence-ios/blob/master/LICENSE) file.