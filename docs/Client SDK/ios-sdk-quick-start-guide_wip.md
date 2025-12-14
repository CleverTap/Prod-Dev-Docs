---
title: iOS SDK Quick Start Guide_WIP
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
[block:api-header]
{
  "title": "Overview"
}
[/block]
CleverTap provides an iOS SDK that enables app developers to track, segment, and engage their users. This guide shows how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard in less than ten minutes. 

To get started, sign up [here](https://clevertap.com/live-product-demo/).

# Requirements

The following are required for using the CleverTap iOS SDK:
  * iOS 9.0 or later
  * tvOS 9.0 or later
  * Xcode 10.0 or later

# Installation Methods

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

#Install SDK

To use the CleverTap iOS SDK, you have two options. You can install it with CocoaPods or manually install it by including the SDK source code in your Xcode project. 

##Option A: Install using CocoaPods (Recommended) 
Follow the steps below to install using CocoaPods:
1. Add the CleverTap SDK to your Podfile as shown below.
[block:code]
{
  "codes": [
    {
      "code": "pod \"CleverTap-iOS-SDK\"",
      "language": "text"
    }
  ]
}
[/block]
2. Once you have updated your Podfile run pod install in your terminal to automatically download.
3. Install the SDK in your project.

##Option B: Manual Install
Follow the steps below for a manual install:
1. Download and unzip the [CleverTap SDK](https://github.com/CleverTap/clevertap-ios-sdk/releases) 
2. Drag the CleverTapSDK.xcodeproj inside your project under the main project file.
3. Embed the framework.
4. Select your app.xcodeproj file. 
5. Under *General*, add the CleverTapSDK framework as an embedded binary.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/29dddd1-sdkimage.png",
        "sdkimage.png",
        512,
        235,
        "#eff0f2"
      ],
      "border": true
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Additional SDWebImage Library Required for Manual Integration",
  "body": "For a manual integration, you need to integrate SDWebImage library for supporting images and GIFs in App Inbox and Native In-Apps."
}
[/block]
#Add CleverTap Credentials

To associate your iOS app with your CleverTap account, you will need to add your CleverTap credentials in the Info.plist file in your application.

Perform the following steps:

1. Navigate to the Info.plist file in your project navigator.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/19c6c39-image4.png",
        "image4.png",
        574,
        358,
        "#e9eeed"
      ],
      "border": true
    }
  ]
}
[/block]
2. Create a key called *CleverTapAccountID* with type string. 
3. Create a key called *CleverTapToken* with type string.
4. Insert the account ID and account token values from your [CleverTap account](https://eu1.dashboard.clevertap.com). These values are available on the *Settings* page. To navigate to the *Settings* page, log into [your CleverTap account](https://eu1.dashboard.clevertap.com/login.html), click on the gear icon on the bottom left navigation, and select *Settings* dashboard. 
[block:callout]
{
  "type": "warning",
  "title": "Using IFA to Identify Unique Users",
  "body": "GDPR mandates that if you are using IFA to identify unique users, there should be prominent disclosure in your application that explains this to your end customer.  For more information,  refer to the [GDPR compliance](https://clevertap.com/blog/in-preparation-of-gdpr-compliance/) on our blog. \n\nIf you still want to use IFA to generate a user's identity, so that you can identify users across re-installs, add the key *CleverTapUseIFA* with the type *Boolean* and value as *Yes* to your info.plist file. \n\nIn addiiton, you must add [AdSupport](https://developer.apple.com/documentation/adsupport) framework to the Link Binary With Libraries. The SDK cannot access the advertising identifier without this framework."
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/dcf2211-Screen_Shot_2018-05-07_at_6.04.26_PM.png",
        "Screen Shot 2018-05-07 at 6.04.26 PM.png",
        1238,
        532,
        "#eff0f1"
      ]
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": []
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f1ef452-2.png",
        "2.png",
        1449,
        1419,
        "#f9f9fb"
      ],
      "sizing": "80"
    }
  ]
}
[/block]



# Swift Package Manager for CleverTap

Starting with the 3.9.3 release, CleverTap supports installation via [Swift
Package Manager](https://swift.org/package-manager/). Before starting the installation please ensure that you are using Xcode 12 or greater.


## Setup

1. Open your project and navigate to the project's settings. Select the tab named *Swift Packages* and click on the add button (+) at the bottom left.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b188c81-spm-image-1.png",
        "spm-image-1.png",
        2237,
        1172,
        "#fbfbfc"
      ]
    }
  ]
}
[/block]

  
2. Enter the URL of CleverTap GitHub repository - https://github.com/CleverTap/clevertap-ios-sdk.git and click **Next**.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1ec33fd-spm-image-2.png",
        "spm-image-2.png",
        2166,
        1194,
        "#d6d7d7"
      ]
    }
  ]
}
[/block]

3. On the next screen, select the preferred SDK version and click **Next**.
  
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a307412-spm-image-3.png",
        "spm-image-3.png",
        2246,
        1244,
        "#d6d7d7"
      ]
    }
  ]
}
[/block]
4. Click finish and ensure that the CleverTap iOS SDK has been added to the appropriate target.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0cc87db-spm-image-4.png",
        "spm-image-4.png",
        2250,
        1244,
        "#d6d7d7"
      ]
    }
  ]
}
[/block]
#SDK Integration
Use one of the following sections to integrate the appropriate SDK.

##Objective-C
Follow the steps below for Objective-C:
1. Import CleverTap.h to your AppDelegate.h file.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3050f84-image6.png",
        "image6.png",
        988,
        367,
        "#080808"
      ],
      "border": true
    }
  ]
}
[/block]
2. Import CleverTap.h into every class where you plan to record user events. 

For example in the application below, we record user events in the ViewController class by importing CleverTap.h in the ViewController.h file.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3f06535-image8.png",
        "image8.png",
        822,
        363,
        "#eaeeeb"
      ],
      "border": true
    }
  ]
}
[/block]
##Swift
Follow the steps below for Swift:
1. Import CleverTapSDK to your AppDelegate.swift file.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a3bf648-Screenshot_2019-04-09_at_3.02.27_PM.png",
        "Screenshot 2019-04-09 at 3.02.27 PM.png",
        2214,
        728,
        "#3c3d43"
      ]
    }
  ]
}
[/block]
2. In your AppDelegate.m file, add [CleverTap autoIntegrate] within the application:didFinishLaunchingWithOptions: method. This creates an instance of the CleverTap class used to track app launches, receive in-app notifications, and enable deep-link tracking.
[block:code]
{
  "codes": [
    {
      "code": "- (BOOL) application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {\n    ...\n    [CleverTap autoIntegrate];\n    ...\n}",
      "language": "objectivec"
    },
    {
      "code": "func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject:AnyObject]?) -> Bool {\n    ...\n    CleverTap.autoIntegrate()\n    ...\n}",
      "language": "swift"
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/58ff095-image5.png",
        "image5.png",
        1067,
        430,
        "#070707"
      ],
      "border": true
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "CleverTap has a getLocation method to track the location of users. In case you are not using the getLocation method and get a warning while uploading your application to iTunes connect, please add key NSLocationWhenInUseUsageDescription in the info.PList with a string value.",
  "title": "Purpose String in Info.plist"
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Manual Integration",
  "body": "In a single line of code, the autoIntegrate method lets you automatically track app launches, receive in-app notifications, and enable deep-link tracking. \n\nAs an alternative to using the automatic integration method, you can manually turn on different CleverTap SDK features. For more information on the manual integration steps, refer to [iOS Push Notifications](https://developer.clevertap.com/docs/ios#section-push-notifications)."
}
[/block]
#Run Your Application

To run your application, navigate to your CleverTap dashboard. If you successfully integrated the CleverTap SDK, you will see a new active user. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/135b863-1_New_mobile_users.png",
        "1 New mobile users.png",
        1405,
        421,
        "#f9f9fa"
      ],
      "border": true
    }
  ]
}
[/block]
# Identify Users

A user profile is automatically created in CleverTap for each user launching your application. 

Initially, the user profile starts out as anonymous which means the profile does not contain any identifiable information about the user. You can enrich the profile with pre-defined attributes from the CleverTap data model, such as name and email. You can also add custom attributes that you define to extend the CleverTap data model.

Sending user profile information to CleverTap using our iOS SDK requires two steps:
1. Build a NSDictionary object with the profile properties. 
2. Call the SDK's `Onuserlogin` method and pass the NSDictionary object you created as a parameter. 

The example below shows how to do this in Objective-C and Swift:
[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *profile = @{\n                              //Update pre-defined profile properties\n                              @\"Name\": @\"Jack Montana\",\n                              @\"Email\": @\"jack.montana@gmail.com\",\n                              //Update custom profile properties\n                              @\"Plan Type\": @\"Silver\",\n                              @\"Favorite Food\": @\"Pizza\"\n                              };\n    \n[[CleverTap sharedInstance] onUserLogin:profile];",
      "language": "objectivec"
    },
    {
      "code": "let profile: Dictionary<String, AnyObject> = [\n    //Update pre-defined profile properties\n    \"Name\": \"Jack Montana\",\n    \"Email\": \"jack.montana@gmail.com\",\n    //Update custom profile properties\n    \"Plan type\": \"Silver\",\n    \"Favorite Food\": \"Pizza\"\n]\n\nCleverTap.sharedInstance()?.onUserLogin(profile)",
      "language": "swift",
      "name": "Swift"
    }
  ]
}
[/block]
When the `Onuserlogin` method is called, the user profile information will be sent to CleverTap. 

Follow the steps below to see how this information displays within the CleverTap dashboard:
1. Log in to the dashboard.
2. Click on the **Find People** button under the *Segment* tab. In the *By Identity* box, enter the email you set on the user profile record, then click the **Find** button.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/eac3675-2_By_identity_search_box.png",
        "2 By identity search box.png",
        1407,
        396,
        "#fafafb"
      ]
    }
  ]
}
[/block]
If CleverTap finds a user profile with this email, you will be taken to that user record. On that page, you will see name and email as pre-defined fields and any other custom fields.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7bdfec6-3__User_Profile.png",
        "3 _User_Profile.png",
        2912,
        1674,
        "#f8f8f9"
      ]
    }
  ]
}
[/block]
#Track Custom Events 

Once you integrate the CleverTap SDK, we automatically start tracking events, such as App Launch and Notification Viewed. In addition to the [default events tracked by CleverTap](https://developer.clevertap.com/docs/concepts-events), you can also track custom events.

To send custom events to CleverTap using our iOS SDK, you will have to call the recordEvent method with the name of the custom event you want to track. 

The example below shows how to do this in both Objective-C and Swift:
[block:code]
{
  "codes": [
    {
      "code": "[[CleverTap sharedInstance] recordEvent:@\"Product Viewed\"];",
      "language": "objectivec"
    },
    {
      "code": "CleverTap.sharedInstance()?.recordEvent(\"Product viewed\")",
      "language": "swift"
    }
  ]
}
[/block]
Here is how to record the custom event within the viewDidLoad method:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7ba7269-screen6.png",
        "screen6.png",
        813,
        228,
        "#656464"
      ],
      "border": true
    }
  ]
}
[/block]
Now looking at the same user profile in CleverTap from the previous step, click on the **Activity** button within the user profile record. This shows a list of activities associated with the user profile. If the custom event was successfully sent from your application to CleverTap, you will see it in this list. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/83c029a-4__User_Activities.png",
        "4 _User_Activities.png",
        2756,
        1676,
        "#f9f9f9"
      ]
    }
  ]
}
[/block]
# Next Steps 

By completing this guide, you are now automatically tracking user events like app launches, and associating that information with profiles for each of your users. You also learned how to add information to a user profile and how to track custom events.

In the next [iOS guide](doc:ios), you will learn more advanced options for tracking custom events and enriching user profiles.