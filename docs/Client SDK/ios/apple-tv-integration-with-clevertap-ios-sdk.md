---
title: Apple TV Integration with CleverTap iOS SDK
excerpt: Learn how to integrate CleverTap iOS SDK for Apple TV.
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

CleverTap iOS SDK integration with tvOS enables you to improve user engagement and personalize the experience for your Apple TV viewers. You can gain insights into user behavior, preferences, and interactions.

This guide will walk you through the step-by-step process of installing and integrating CleverTap's Web SDK with tvOS.

# Integrate CleverTap iOS SDK with Apple TV

This integration involves the following two major steps: 

1. [Install CleverTap iOS SDK](https://developer.clevertap.com/docs/apple-tv-integration-with-clevertap-ios-sdk#install-clevertap-ios-sdk)
2. [Integrate CleverTap iOS SDK](https://developer.clevertap.com/docs/apple-tv-integration-with-clevertap-ios-sdk#integrate-clevertap-ios-sdk)

## Install CleverTap iOS SDK

For your tvOS app, add the CleverTap iOS SDK to your `Podfile`:

```swift
target 'YOUR_TARGET_NAME' do  
    pod 'CleverTap-iOS-SDK'  
end     
```

  After you add the code, run the`pod install`.

## Integrate CleverTap iOS SDK

To integrate CleverTap iOS SDK with your Apple TV app:

1. Add your CleverTap account credentials.
2. Update the `.plist` file:
   - Create a key called **CleverTapAccountID** with a string value.
   - Create a key called **CleverTapToken** with a string value.
   - Insert the values from your CleverTap Dashboard. Go to **CleverTap Dashboard** > **Settings** > **Integration Details**.
3. Integrate CleverTap iOS SDK in `AppDelegate.swift` file:
   - Import CleverTapSDK to the `AppDelegate.swift` file.
   - Add the following method calls to the `AppDelegate.swift` file.
     ```swift
     func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
             
             // Configure and init the default shared CleverTap instance (add CleverTap Account ID and Account Token in your .plist file)
             CleverTap.autoIntegrate()
             CleverTap.setDebugLevel(CleverTapLogLevel.debug.rawValue)
     }
     ```
4. Integrate CleverTap iOS SDK in the ViewController file:

   - Import CleverTapSDK in `ViewController` class file.
   - Call the following methods to identify users and to raise events:

     ```swift
     // onUserLogin to identify users
     let profile: Dictionary<String, Any> = [
                 "Name": "Test User",
                 "Email": "test.user@gmail.com",
                 "Plan type": "Silver",
                 "Favorite Food": "Pizza"
             ]
      CleverTap.sharedInstance()?.onUserLogin(profile)
             
     // recordEvent to record an event with name
     CleverTap.sharedInstance()?.recordEvent("RecordButtonForTVOS_Pressed")

     // recordEventwithProps for recording an event with name and properties
     let props = [
                 "Product name": "Apple TV",
                 "Category": "Digital",
                 "Price": 167.00,
                 "Date": NSDate()
             ] as [String : Any]

     CleverTap.sharedInstance()?.recordEvent("RecordWithPropsButtonForTVOS_Pressed", withProps: props)

     ```