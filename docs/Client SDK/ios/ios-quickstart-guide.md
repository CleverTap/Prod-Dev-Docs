---
title: iOS SDK Quick Start Guide
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
## Overview

CleverTap provides an iOS SDK that enables app developers to track, segment, and engage their users. This guide shows how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard in less than ten minutes. 

# Install SDK

To use the CleverTap iOS SDK, you have two options. You can install it with CocoaPods or manually install it by including the SDK source code in your Xcode project. 

## Option A: Install using CocoaPods (Recommended) 

Follow the steps below to install using CocoaPods:

1. Add the CleverTap SDK to your Podfile as shown below.

```text
pod "CleverTap-iOS-SDK"
```

2. Once you have updated your Podfile run pod install in your terminal to automatically download.
3. Install the SDK in your project.

## Option B: Manual Install

Follow the steps below for a manual install:

1. Download and unzip the [CleverTap SDK](https://github.com/CleverTap/clevertap-ios-sdk/releases) 
2. Drag the CleverTapSDK.xcodeproj inside your project under the main project file.
3. Embed the framework.
4. Select your app.xcodeproj file. 
5. Under *General*, add the CleverTapSDK framework as an embedded binary.

<Image title="sdkimage.png" alt={512} className="border" border={true} src="https://files.readme.io/29dddd1-sdkimage.png" />

> 📘 Additional SDWebImage Library Required for Manual Integration
>
> For a manual integration, you need to integrate SDWebImage library for supporting images and GIFs in App Inbox and Native In-Apps.

# Add CleverTap Credentials

To associate your iOS app with your CleverTap account, you will need to add your CleverTap credentials in the Info.plist file in your application.

Perform the following steps:

1. Navigate to the Info.plist file in your project navigator.

<Image title="image4.png" alt={574} className="border" border={true} src="https://files.readme.io/19c6c39-image4.png" />

2. Create a key called *CleverTapAccountID* with type string. 
3. Create a key called *CleverTapToken* with type string.
4. Insert the account ID and account token values from your [CleverTap account](https://eu1.dashboard.clevertap.com). These values are available on the *Settings* page. To navigate to the *Settings* page, log into [your CleverTap account](https://eu1.dashboard.clevertap.com/login.html), click on the gear icon on the bottom left navigation, and select *Settings* dashboard. 

<Image title="2.png" alt={1449} width="80%" src="https://files.readme.io/f1ef452-2.png" />

> 📘 Disable IDFV Usage
>
> Optionally, starting from CleverTap iOS SDK 3.9.4, you can disable the generation of CleverTap ID basis IDFV value. Add `CleverTapDisableIDFV` with type *Boolean* and set its value as 1 in your *Info.plist* file.
>
> This is recommended for apps that send data from different iOS apps to a common CleverTap account.

# Installation Steps for Swift

Starting with v3.1.4, the SDK includes a modulemap which provides the ability to import the SDK as a module rather than using a bridging header.

Refer to the example implementation for bridging header on the [CleverTap iOS SDK github](https://github.com/CleverTap/clevertap-ios-sdk/blob/3.1.4/SwiftStarterProject/CleverTapSDK-Bridging-Header.h).

# SDK Integration

Use one of the following sections to integrate the appropriate SDK.

## Objective-C

Follow the steps below for Objective-C:

1. Import CleverTap.h to your AppDelegate.h file.

<Image title="image6.png" alt={988} className="border" border={true} src="https://files.readme.io/3050f84-image6.png" />

2. Import CleverTap.h into every class where you plan to record user events. 

For example in the application below, we record user events in the ViewController class by importing CleverTap.h in the ViewController.h file.

<Image title="image8.png" alt={822} className="border" border={true} src="https://files.readme.io/3f06535-image8.png" />

## Swift

Follow the steps below for Swift:

1. Import CleverTapSDK to your AppDelegate.swift file.

![2214](https://files.readme.io/a3bf648-Screenshot_2019-04-09_at_3.02.27_PM.png "Screenshot 2019-04-09 at 3.02.27 PM.png")

2. In your AppDelegate.m file, add [CleverTap autoIntegrate] within the application:didFinishLaunchingWithOptions: method. This creates an instance of the CleverTap class used to track app launches, receive in-app notifications, and enable deep-link tracking.

```objectivec
- (BOOL) application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    ...
    [CleverTap autoIntegrate];
    ...
}
```
```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject:AnyObject]?) -> Bool {
    ...
    CleverTap.autoIntegrate()
    ...
}
```

<Image title="image5.png" alt={1067} className="border" border={true} src="https://files.readme.io/58ff095-image5.png" />

> 🚧 Purpose String in Info.plist
>
> CleverTap has a getLocation method to track the location of users. In case you are not using the getLocation method and get a warning while uploading your application to iTunes connect, please add key NSLocationWhenInUseUsageDescription in the info.PList with a string value.

> 🚧 Manual Integration
>
> In a single line of code, the autoIntegrate method lets you automatically track app launches, receive in-app notifications, and enable deep-link tracking. 
>
> As an alternative to using the automatic integration method, you can manually turn on different CleverTap SDK features. For more information on the manual integration steps, refer to [iOS Push Notifications](https://developer.clevertap.com/docs/ios#section-push-notifications).

# Run Your Application

To run your application, navigate to your CleverTap dashboard. If you successfully integrated the CleverTap SDK, you will see a new active user. 

<Image title="1 New mobile users.png" alt={1405} className="border" border={true} src="https://files.readme.io/135b863-1_New_mobile_users.png" />

# Identify Users

A user profile is automatically created in CleverTap for each user launching your application. 

Initially, the user profile starts out as anonymous which means the profile does not contain any identifiable information about the user. You can enrich the profile with pre-defined attributes from the CleverTap data model, such as name and email. You can also add custom attributes that you define to extend the CleverTap data model.

Sending user profile information to CleverTap using our iOS SDK requires two steps:

1. Build a NSDictionary object with the profile properties. 
2. Call the SDK's `Onuserlogin` method and pass the NSDictionary object you created as a parameter. 

The example below shows how to do this in Objective-C and Swift:

```objectivec
NSDictionary *profile = @{
                              //Update pre-defined profile properties
                              @"Name": @"Jack Montana",
                              @"Email": @"jack.montana@gmail.com",
                              //Update custom profile properties
                              @"Plan Type": @"Silver",
                              @"Favorite Food": @"Pizza"
                              };
    
[[CleverTap sharedInstance] onUserLogin:profile];
```
```swift Swift
let profile: Dictionary<String, AnyObject> = [
    //Update pre-defined profile properties
    "Name": "Jack Montana",
    "Email": "jack.montana@gmail.com",
    //Update custom profile properties
    "Plan type": "Silver",
    "Favorite Food": "Pizza"
]

CleverTap.sharedInstance()?.onUserLogin(profile)
```

When the `Onuserlogin` method is called, the user profile information will be sent to CleverTap. 

Follow the steps below to see how this information displays within the CleverTap dashboard:

1. Log in to the dashboard.
2. Click on the **Find People** button under the *Segment* tab. In the *By Identity* box, enter the email you set on the user profile record, then click the **Find** button.

![1407](https://files.readme.io/eac3675-2_By_identity_search_box.png "2 By identity search box.png")

If CleverTap finds a user profile with this email, you will be taken to that user record. On that page, you will see name and email as pre-defined fields and any other custom fields.

![2912](https://files.readme.io/7bdfec6-3__User_Profile.png "3 _User_Profile.png")

# Track Custom Events 

Once you integrate the CleverTap SDK, we automatically start tracking events, such as App Launch and Notification Viewed. In addition to the [default events tracked by CleverTap](https://developer.clevertap.com/docs/concepts-events), you can also track custom events.

To send custom events to CleverTap using our iOS SDK, you will have to call the recordEvent method with the name of the custom event you want to track. 

The example below shows how to do this in both Objective-C and Swift:

```objectivec
[[CleverTap sharedInstance] recordEvent:@"Product Viewed"];
```
```swift
CleverTap.sharedInstance()?.recordEvent("Product viewed")
```

Here is how to record the custom event within the viewDidLoad method:

<Image title="screen6.png" alt={813} className="border" border={true} src="https://files.readme.io/7ba7269-screen6.png" />

Now looking at the same user profile in CleverTap from the previous step, click on the **Activity** button within the user profile record. This shows a list of activities associated with the user profile. If the custom event was successfully sent from your application to CleverTap, you will see it in this list. 

![2756](https://files.readme.io/83c029a-4__User_Activities.png "4 _User_Activities.png")

# Next Steps

By completing this guide, you are now automatically tracking user events like app launches, and associating that information with profiles for each of your users. You also learned how to add information to a user profile and how to track custom events.

In the next [iOS guide](doc:ios), you will learn more advanced options for tracking custom events and enriching user profiles.
