---
title: CleverTap GTM iOS Integration
excerpt: Learn how to send iOS app events to CleverTap via GTM
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

Using Google Tag Manager for iOS, you can control which of your third-party vendors receive events and create tags specific to CleverTap.

Clevertap iOS SDK can be initialized and controlled by tags configured within [Google Tag Manager](https://tagmanager.google.com/). But, before using Google Tag Manager, ensure that you complete the initial [Android SDK Setup](doc:android-quickstart-guide).

# Integration

The following are the steps to integrate CleverTap with GTM in your project: 

1. To install the CleverTap iOS SDK,  refer to the [Install SDK](doc:ios-quickstart-guide#install-sdk) section.

2. To associate your iOS app with your CleverTap account, you must add your CleverTap credentials in the `Info.plist` file in your application. To do so:

   a. Create a key called `CleverTapAccountID` of type string.\
   b. Create a key called `CleverTapToken` of type string.\
   c. Insert the *Account ID* and *Account Token* values from your CleverTap account by navigating to the *Settings* page. Here, the *Account ID* is equivalent to the *Project ID*.

3. Initialize the SDK.

* **Objective-C**

     a. Import `CleverTap.h` to your `AppDelegate.h` file.\
     b. Import `CleverTap.h` into every class where you plan to record user events. For instance, in the application code shown below, we record user events in the `ViewController` class by importing `CleverTap.h` into the ViewController.h file.

```objectivec
(BOOL) application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
...
[CleverTap autoIntegrate];
...
}
```

* **Swift**

    a. Import `CleverTapSDK` to your `AppDelegate.swift` file.\
    b. Add [CleverTap autoIntegrate] within the `application:didfinishlaunchingwithoptions:` method of your `AppDelegate.m` file. This creates an instance of the CleverTap class used to track app launches, receive in-app notifications, and enable deep-link tracking.

```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject:AnyObject]?) -> Bool {
...
CleverTap.autoIntegrate()
...
}
```

4. To enable location tracking, and network information, you must call the following method in your app build:

```objectivec
ObjC: [[CleverTap sharedInstance] enableDeviceNetworkInfoReporting:YES];
```
```swift
clevSwift: CleverTap.sharedInstance().enableDeviceNetworkInfoReporting(true)
```
