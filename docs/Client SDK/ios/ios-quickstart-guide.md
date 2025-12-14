---
title: iOS Quick Start Guide
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
# Overview

CleverTap provides an iOS SDK that enables app developers to track, segment, and engage users. This guide shows how to install the CleverTap SDK in your application, track your first user event, and quickly see user profiles and user events within the CleverTap dashboard.

# Install SDK

To use the CleverTap iOS SDK, you have the following options:

* [Install it with CocoaPods](doc:ios-quickstart-guide#install-using-cocoapods-recommended) 
* [Install it with SPM](doc:ios-quickstart-guide#install-using-swift-package-manager)
* [Install it manually by including the SDK source code in your Xcode project](doc:ios-quickstart-guide#install-manually) 

## Install using CocoaPods (Recommended)

To install SDK using CocoaPods:

1. Add the CleverTap SDK to your Podfile, as shown below:

```coffeescript
pod "CleverTap-iOS-SDK"
```

2. After you have updated your Podfile, run `pod install` in your terminal to download automatically.
3. Install the SDK in your project.

## Install using Swift Package Manager

1. Open the `cleverTap-ios-sdk` project page on GitHub and copy the clone URL [https://github.com/CleverTap/clevertap-ios-sdk](https://github.com/CleverTap/clevertap-ios-sdk) of the project.
2. Now integrate CleverTap SDK in your application in Xcode. Go to *File* > *Add Packages* to add a new dependency.
3. Specify the clone URL of the cleverTap-ios-sdk library. Keep the default version settings - the library will be updated after the next major release.

<Image title="Clone URL of the CleverTap iOS SDK Library" alt={2557} align="center" border={true} src="https://files.readme.io/2912bb5-Screenshot_2022-08-01_at_4.14.26_PM.png">
  Clone URL of the CleverTap iOS SDK Library
</Image>

<Image title="Add Package" alt={2298} align="center" border={true} src="https://files.readme.io/195a15b-Screenshot_2022-08-01_at_4.15.11_PM.png">
  Add Package
</Image>

## Install Manually

To install the SDK manually:

1. Download and unzip the [CleverTap SDK](https://github.com/CleverTap/clevertap-ios-sdk/releases).
2. Drag the `CleverTapSDK.xcodeproj` inside your project under the main project file.
3. Embed the framework.
4. Select your `app.xcodeproj` file. 
5. Under *General*, add the CleverTapSDK framework as an embedded binary.

<Image title="Add CleverTap SDK Framework" alt={2665} align="center" border={true} src="https://files.readme.io/e01f4c1-Screenshot_2022-08-01_at_4.25.13_PM.png">
  Add CleverTap SDK Framework
</Image>

> 📘 Additional `SDWebImage` Library Required for Manual Integration
>
> You must integrate the `SDWebImage` library for supporting images and GIFs in App Inbox and Native In-Apps for manual integration. Follow the steps for integrating SDWebImage:
>
> 1. Navigate to the \_Vendors/SDWebImage \_directory found under the cloned CleverTap iOS SDK repository.
> 2. Drag and drop `SDWebImage.xcodeproj` into the main project file.
> 3. Navigate to the project application’s target settings, open *General*, click the *+* button under the *Frameworks, Libraries, and Embedded Content*, add `SDWebImage.framework` as an embedded binary.

6. Add the CleverTapSDK package product to the app target.

## Add CleverTap Credentials

To associate your iOS app with your CleverTap account, you need to add your CleverTap credentials in the `Info.plist` file in your application. To do so:

1. Navigate to the `Info.plist file` from your project navigator.

<Image title="Key value Pairs in Info.plist File" alt={574} align="center" border={true} src="https://files.readme.io/19c6c39-image4.png">
  Key value Pairs in Info.plist File
</Image>

2. Create a key called `CleverTapAccountID` with a type string. 
3. Create a key called `CleverTapToken` with a type string.
4. Insert the *Project ID* and account *Project Token* values from your [CleverTap account](https://eu1.dashboard.clevertap.com). These values are available under the *Settings* page.

> 🚧 Credentials and Access
>
> The `CleverTapAccountId` and `CleverTapToken` are available as *Project ID* and *Project Token* respectively on the CleverTap dashboard.\
> Member and Creator roles in the project don’t have access to view the *Passcode* and *Project Token* of the account on the dashboard.

<Image title="CleverTap Project Credentials" alt={2708} align="center" border={true} src="https://files.readme.io/9fc6ea4-Screenshot_2022-08-01_at_3.46.42_PM.png">
  CleverTap Project Credentials
</Image>

> 📘 Disable IDFV Usage
>
> Optionally, starting from CleverTap iOS SDK 3.9.4, you can disable the generation of CleverTap ID basis IDFV value. Add `CleverTapDisableIDFV` with type *Boolean* and set its value as *1* in your `Info.plist` file.\
> This is recommended for apps that send data from different iOS apps to a common CleverTap account.

## Add Region Code

To know how to add region code in iOS SDK, refer to [Region Codes.](doc:idc#ios)

# Integrate CleverTap SDK in iOS Applications

## Objective-C Installation

To integrate the CleverTap SDK for Objective-C:

1. Import `CleverTap.h` to your `AppDelegate.h` file.

<Image title="Import CleverTap.h File" alt={988} align="center" border={true} src="https://files.readme.io/3050f84-image6.png">
  Import CleverTap.h File
</Image>

2. Import `CleverTap.h` into every class where you plan to record user events.\
   For example, in the application below, we record user events in the `ViewController` class by importing `CleverTap.h` in the `ViewController.h` file.

<Image title="Record User Events in the `ViewController` Class" alt={822} align="center" border={true} src="https://files.readme.io/3f06535-image8.png">
  Record User Events in the `ViewController` Class
</Image>

### Autointegrate CleverTap in your Obj-C Application

This creates an instance of the CleverTap class used to track app launches, receive in-app notifications, and enable deep-link tracking.\
In your `AppDelegate.m` file, add [CleverTap autoIntegrate] within the `application:didFinishLaunchingWithOptions:` method. 

```objectivec
- (BOOL) application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    ...
    [CleverTap autoIntegrate];
    ...
}
```

<Image title="Auto Integrate CleverTap in Obj-C Application" alt={1488} align="center" border={true} src="https://files.readme.io/d69c7d4-Screenshot_2022-09-07_at_9.34.30_PM.png">
  Auto Integrate CleverTap in Obj-C Application
</Image>

### Debug CleverTap SDK in Obj-C application

In your `AppDelegate.m` file, add `[CleverTap setDebugLevel:CleverTapLogDebug];` within the `application:didFinishLaunchingWithOptions:` method. This is an optional step.\
By default, CleverTap logs are set to CleverTapLogLevel.info. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level to CleverTapLogLevel.debug. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTapLogLevel.off.

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    [CleverTap autoIntegrate];
    [CleverTap setDebugLevel:CleverTapLogDebug];
}
```

## Swift Installation

To integrate CleverTap SDK for Swift:

1. Import `CleverTapSDK` to your `AppDelegate.swift` file as given in the application below. Similarly import `CleverTapSDK` in every class file where you plan to record user events.

> ❗️ Bridging Headers for CleverTap v3.1.4 and below for Swift installation:
>
> For bridging header on the [CleverTap iOS SDK github](https://github.com/CleverTap/clevertap-ios-sdk/blob/3.1.4/SwiftStarterProject/CleverTapSDK-Bridging-Header.h), refer to the example implementation.\
> Starting with v3.1.4, the SDK now includes a module map, which allows importing the SDK as a module rather than using a bridging header.

### Autointegrate CleverTap in your Swift Application

In your `AppDelegate.swift` file, add CleverTap.autoIntegrate() within the\
`application:didFinishLaunchingWithOptions:` method.\
  This creates an instance of the CleverTap class used to track app launches, receive in-app notifications, and enable deep-link tracking.

```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject:AnyObject]?) -> Bool {
    ...
    CleverTap.autoIntegrate()
    ...
}
```

<Image title="Auto Integrate CleverTap in Swift Application" alt={2214} align="center" border={true} src="https://files.readme.io/a3bf648-Screenshot_2019-04-09_at_3.02.27_PM.png">
  Auto Integrate CleverTap in Swift Application
</Image>

### Debug CleverTap SDK in your Swift Application

In your `AppDelegate.swift` file, add `CleverTap.setDebugLevel(CleverTapLogLevel.debug.rawValue)` within the\
`application:didFinishLaunchingWithOptions:` method. This is an optional step. By default, CleverTap logs are set to CleverTapLogLevel.info. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level to CleverTapLogLevel.debug. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTapLogLevel.off.rawValue.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        // Configure and init the default shared CleverTap instance (add CleverTap Account ID and Account Token in your .plist file)
        CleverTap.autoIntegrate()
        CleverTap.setDebugLevel(CleverTapLogLevel.debug.rawValue)
}
```

<Image title="Debug CleverTap SDK in Swift Application" alt={1544} align="center" border={true} src="https://files.readme.io/0fafa44-Screenshot_2022-09-07_at_9.35.52_PM.png">
  Debug CleverTap SDK in Swift Application
</Image>

## Get Location Information in Application

You can use the `CleverTapLocation` module to get the user's location (and possibly track the location for events). The `CTLocationManager` class has a `getLocationWithSuccess` method to track the location of users. You must install and integrate `CleverTapLocation` via Cocoapods, SPM or manually for this purpose.

Please refer to the [Location doc](https://github.com/CleverTap/clevertap-ios-sdk/blob/location-api/docs/CleverTapLocation.md) for more details.

### Installation

* Cocoapods
* Swift Package Manager
* Manual Installation

#### Cocoapods

Add the following to your Podfile:

```
target 'YOUR_TARGET_NAME' do  
    pod 'CleverTapLocation'  
end     
```

Then run pod install.

#### Swift Package Manager

1. Open your project and navigate to the project's settings. Select the tab named Swift Packages and click on the add button (+) at the bottom left.
2. Enter the URL of CleverTap GitHub repository - [https://github.com/CleverTap/clevertap-ios-sdk.git](https://github.com/CleverTap/clevertap-ios-sdk.git) and click Next.
3. On the next screen, select the preferred SDK version and click Next.
4. Click finish and ensure that the CleverTapLocation has been added to the appropriate target.

#### Manual Installation

* Clone the CleverTap iOS SDK repository recursively:
  ```
   git clone --recursive https://github.com/CleverTap/clevertap-ios-sdk.git
  ```

* Navigate to `CleverTapLocation` folder and add the `CleverTapLocation.xcodeproj` to your Xcode Project, by dragging the `CleverTapLocation.xcodeproj` under the main project file.

* Navigate to the project application’s target settings, open "General", click the "+" button under the `Frameworks, Libraries, and Embedded Content`, add `CleverTapLocation.framework` as an embedded binary.

> 🚧 Manual Integration
>
> In a single line of code, the `autoIntegrate` method lets you automatically track app launches, receive in-app notifications, and enable deep-link tracking. 
>
> You can manually turn on different CleverTap SDK features as an alternative to the automatic integration method. For more information on the manual integration steps for setting up push notifications, refer to [iOS Push Notifications](doc:push-notifications-ios).

# Run Your Application

To run your application, navigate to your CleverTap dashboard. If you integrate the CleverTap SDK successfully, you see a new active user on the dashboard.

<Image title="View Active Users on CleverTap Dashboard" alt={1405} align="center" border={true} src="https://files.readme.io/135b863-1_New_mobile_users.png">
  View Active Users on CleverTap Dashboard
</Image>

# Identify Users

A user profile is automatically created in CleverTap for each user launching your application. 

Initially, the user profile starts out as anonymous which means the profile does not contain any identifiable information about the user. You can enrich the profile with pre-defined attributes from the CleverTap data model, such as name and email. You can also add custom attributes that you define to extend the CleverTap data model.

Sending user profile information to CleverTap using our iOS SDK requires two steps:

1. Build a `NSDictionary` object with the profile properties. 
2. Call the SDK's `Onuserlogin` method and pass the `NSDictionary` object you created as a parameter. 

The following example shows how to do this in Objective-C and Swift:

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
```objectivec
NSDictionary *profile = @{
                              //Update pre-defined profile properties
                              @"Name": @"Jack Montana" as AnyObject,
                              @"Email": @"jack.montana@gmail.com" as AnyObject,
                              //Update custom profile properties
                              @"Plan Type": @"Silver" as AnyObject,
                              @"Favorite Food": @"Pizza" as AnyObject
                              };
    
[[CleverTap sharedInstance] onUserLogin:profile];
```

When the `Onuserlogin` method is called, the user profile information is sent to CleverTap. 

To see how this information displays within the CleverTap dashboard:

1. Log in to the CleverTap dashboard.
2. Click **Find People** under the *Segment* tab. In the *By Identity* box, enter the email you set on the user profile record and click **Find**.

<Image title="Find People on CleverTap Dashboard" alt={1407} align="center" border={true} src="https://files.readme.io/eac3675-2_By_identity_search_box.png">
  Find People on CleverTap Dashboard
</Image>

If CleverTap finds a user profile with this email, the user record is displayed. On that page, you will see name and email as pre-defined fields and any other custom fields.

<Image title="User Profile" alt={2912} align="center" border={true} src="https://files.readme.io/7bdfec6-3__User_Profile.png">
  User Profile
</Image>

## Update the User Profile

The `Onuserlogin` method identifies the individual users on the device. However, you may need to add additional user properties to the user profile such as gender, date of birth, and so on. You can update these properties with the `profilePush()` method. For more information, refer to our [User Documentation](https://developer.clevertap.com/docs/concepts-user-profiles).

# Track Custom Events

After you integrate the CleverTap SDK, we automatically start tracking events, such as *App Launch* and *Notification Viewed*. In addition to the [default (system) events tracked by CleverTap](https://developer.clevertap.com/docs/events#system-events), you can also track custom events.

To send custom events to CleverTap using our iOS SDK, you will have to call the `recordEvent` method with the name of the custom event you want to track. 

The following example shows how to do this in both Objective-C and Swift:

```swift
CleverTap.sharedInstance()?.recordEvent("Product viewed")
```
```objectivec
[[CleverTap sharedInstance] recordEvent:@"Product Viewed"];
```

Now, looking at the same user profile in CleverTap from the previous step, click **Activity** within the user profile record, which shows a list of activities associated with the user profile. If the custom event is successfully sent from your application to CleverTap, you can see it in this list. 

<Image title="User Activity" alt={2722} align="center" border={true} src="https://files.readme.io/ae28962-Screenshot_2022-08-02_at_1.44.51_PM.png">
  User Activity
</Image>

## GDPR Compliance

From CleverTap iOS SDK 3.1.7 onwards, CleverTap provides APIs to support GDPR compliance. For more information, refer to [iOS GDPR Compliance](https://developer.clevertap.com/docs/sdk-changes-for-gdpr-compliance#ios).

# Sample Projects demonstrating direct implementation of CleverTap features:

1. For an example project of integrating CleverTap SDK and using all its features in your Swift application, refer to the [Github Swift Starter Project](https://github.com/CleverTap/clevertap-ios-sdk/tree/master/SwiftStarter).

2. For an example project of integrating CleverTap SDK and using all its features in your ObjC application, refer to the [Github Objective-C Starter Project](https://github.com/CleverTap/clevertap-ios-sdk/tree/master/ObjCStarter). Follow the same steps.

3. For an example project of implementing CleverTap advanced push notifications and various push templates refer to [CleverTap Notification Content](https://github.com/CleverTap/CTNotificationContent). We have Swift starter and Obj-C starter applications in the repository for reference. 

# Next Steps

By completing this guide, you are now automatically tracking user events such as app launches and associating that information with profiles for each of your users. You have also learned how to debug, how to add information to a user profile and how to track custom events.

In the next [iOS User Profiles](doc:user-profiles-ios), you will learn more advanced options for enriching user profiles.
