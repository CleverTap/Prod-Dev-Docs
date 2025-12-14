---
title: iOS Quick Start Guide
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
CleverTap provides an iOS SDK that enables app developers to track, segment, and engage their users. 

This guide will show you how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard in less than ten minutes. 

#Step 1: Install SDK

To use the CleverTap iOS SDK, you have two options. You can install it with CocoaPods or manually install it by including the SDK source code in your Xcode project. 

##Option A - Install using CocoaPods (Recommended) 

Add the CleverTap SDK to your Podfile as shown below.
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
Once you have updated your Podfile run pod install in your terminal to automatically download and install the SDK in your project.

##Option B - Manual Install

 Step 1: Download and unzip the [CleverTap SDK](https://github.com/CleverTap/clevertap-ios-sdk/releases) 
 
Step 2: Drag the CleverTapSDK.xcodeproj inside your project under the main project file

Step 3: Embed the framework. Select your app.xcodeproj file. Under "General", add the CleverTapSDK framework as an embedded binary
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
  "title": "Note : Additional SDWebImage library required for Manual Integration",
  "body": "Note : For Manual Integration, you need to integrate SDWebImage library for supporting images and gifs in App Inbox and Native InApps"
}
[/block]
#Step 2: Add CleverTap Credentials

To associate your iOS app with your CleverTap account, you will need to add your CleverTap credentials in the Info.plist file in your *application*.

Navigate to the Info.plist file in your project navigator.
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
First, create a key called *CleverTapAccountID* with type string. 
Second, create a key called *CleverTapToken* with type string.

Insert the Account ID and Account Token values from your [CleverTap account](https://eu1.dashboard.clevertap.com). These values are available on the Settings page. To navigate to the Settings page, log into [your CleverTap account](https://eu1.dashboard.clevertap.com/login.html), click on the gear icon on the bottom left navigation, and select Settings dashboard. 
[block:callout]
{
  "type": "warning",
  "title": "Using IFA to uniquely identify users",
  "body": "GDPR mandates that if you are using IFA to uniquely identify users, there should be prominent disclosure in your application that explains this to your end customer.  You can read more about  GDPR [here](https://clevertap.com/blog/in-preparation-of-gdpr-compliance/). \n\nNote: if you still want to use IFA to generate a users identity, so that you can identify users across re-installs, add the key *CleverTapUseIFA* with the type *Boolean* and value as *Yes*, to your info.plist file. \n\nAdditionally, you must add [AdSupport](https://developer.apple.com/documentation/adsupport) framework to the Link Binary With Libraries. The SDK cannot access the advertising identifier without this framework."
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
      "image": [
        "https://files.readme.io/6640bc2-1.png",
        "1.png",
        1088,
        840,
        "#efeff0"
      ]
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
#Step 3: Installation Steps for Swift

Starting with v3.1.4, the SDK includes a modulemap, allowing you to import the SDK as a module rather than using a bridging header.

See the example implementation for bridging header here: https://github.com/CleverTap/clevertap-ios-sdk/blob/3.1.4/SwiftStarterProject/CleverTapSDK-Bridging-Header.h

#Step 4: SDK Integration

For Objective-C, Import CleverTap.h to your AppDelegate.h file.
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
Import CleverTap.h into every class where you plan to record user events. For example, in the application below, we will record user events in the ViewController class by importing CleverTap.h in the ViewController.h file.
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
For Swift, Import CleverTapSDK to your AppDelegate.swift file.
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
In your AppDelegate.m file, add [CleverTap autoIntegrate] within the application:didFinishLaunchingWithOptions: method. This creates an instance of the CleverTap class used to track app launches, receive in-app notifications, and enable deep-link tracking.
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
  "body": "CleverTap has a getLocation method to track the location of users. In case you are not using the getLocation method and get of a warning while uploading your application to iTunes connect, please add key NSLocationWhenInUseUsageDescription in the info.PList with a string value",
  "title": "Purpose String in Info.plist"
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Manual Integration",
  "body": "In a single line of code, the autoIntegrate method lets you automatically track app launches, receive in-app notifications, and enable deep-link tracking. \n\nAs an alternative to using the automatic integration method, you can manually turn on different CleverTap SDK features. The manual integration steps are documented on [this page](https://developer.clevertap.com/docs/ios#section-push-notifications)."
}
[/block]
#Step 5: Run Your Application

Build and run your application. 

Navigate to your CleverTap dashboard. If you successfully integrated the CleverTap SDK, you will see a new active user. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7a32bc7-image7.png",
        "image7.png",
        1320,
        520,
        "#1f2f45"
      ],
      "border": true
    }
  ]
}
[/block]
# Step 6: Add Information to a User Profile

A User Profile is automatically created in CleverTap for each user launching your application. 

Initially, the User Profile starts out as anonymous, which means the profile does not contain any identifiable information about the user. You can enrich the profile with pre-defined attributes from the CleverTap data model, such as name and email. You can also add custom attributes that you define to extend the CleverTap data model.

Sending user profile information to CleverTap using our iOS SDK requires two steps. First, you have to build a NSDictionary object with the profile properties. Second, you have to call the SDK's profilePush method and pass the NSDictionary object you created as a parameter. The example below shows how to do this in Objective-C and Swift.
[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *profile = @{\n                              //Update pre-defined profile properties\n                              @\"Name\": @\"Jack Montana\",\n                              @\"Email\": @\"jack.montana@gmail.com\",\n                              //Update custom profile properties\n                              @\"Plan Type\": @\"Silver\",\n                              @\"Favorite Food\": @\"Pizza\"\n                              };\n    \n[[CleverTap sharedInstance] profilePush:profile];",
      "language": "objectivec"
    },
    {
      "code": "let profile: Dictionary<String, AnyObject> = [\n    //Update pre-defined profile properties\n    \"Name\": \"Jack Montana\",\n    \"Email\": \"jack.montana@gmail.com\",\n    //Update custom profile properties\n    \"Plan type\": \"Silver\",\n    \"Favorite Food\": \"Pizza\"\n]\n\nCleverTap.sharedInstance()?.profilePush(profile)\n",
      "language": "swift",
      "name": "Swift"
    }
  ]
}
[/block]
Here is how to update the user profile information within the viewDidLoad method.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ef623e6-screen3.png",
        "screen3.png",
        836,
        508,
        "#565454"
      ],
      "border": true
    }
  ]
}
[/block]
When the profilePush method is called, the user profile information will be sent to CleverTap. 

Let's see how this information appears within the CleverTap dashboard. Log into the dashboard, and then click on the Find People button under the Segment tab. In the By Identity box, enter the email you set on the user profile record, and then click the Find button.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b3ced01-4.png",
        "4.png",
        2880,
        924,
        "#f8f7f8"
      ]
    }
  ]
}
[/block]
If CleverTap finds a user profile with this email, you will be taken to that user record. On that page, we'll see name and email as pre-defined fields, and plan type and favorite food as custom fields.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/20b5e2f-5.png",
        "5.png",
        2752,
        1674,
        "#f7f5f6"
      ]
    }
  ]
}
[/block]
# Step 7: Track Custom Events 

Once you integrate the CleverTap SDK, we automatically start tracking events, such as App Launch and Notification Viewed. In addition to the [default events tracked by CleverTap](https://developer.clevertap.com/docs/concepts-events), you can also track custom events.

To send custom events to CleverTap using our iOS SDK, you will have to call the recordEvent method with the name of the custom event you want to track. The example below shows how to do this in both Objective-C and Swift.
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
Here is how to record the custom event within the viewDidLoad method.
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
Let's look at the same user profile in CleverTap from step 6. Click on the Activity button within the user profile record. This will show a list of activities associated with the user profile. If the custom event was successfully sent from your application to CleverTap, you will see it in this list. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/51b7676-6.png",
        "6.png",
        2756,
        1676,
        "#f9f8f8"
      ]
    }
  ]
}
[/block]
# Next Steps 

Congrats on successfully integrating CleverTap into your iOS application! 

You are now automatically tracking user events like app launches, and associating that information with profiles for each of your users. You also learned how to add information to a user profile and how to track custom events.

In the next [iOS guide](doc:ios), you will learn more advanced options for tracking custom events and enriching user profiles.