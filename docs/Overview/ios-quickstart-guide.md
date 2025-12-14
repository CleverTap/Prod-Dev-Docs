---
title: iOS Quickstart Guide
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

**Option A - Install using CocoaPods (Recommended) **

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

#**Option B - Manual Install**

[Download](https://github.com/CleverTap/clevertap-ios-sdk/releases/download/3.1.6/CleverTapSDK-v3.1.6-20171013.framework.zip) and unzip the CleverTap SDK. 

Add the CleverTap SDK to your Xcode Project by dragging the file into your Project Navigator.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/bc32076-image9.png",
        "image9.png",
        257,
        359,
        "#ecf0f1"
      ],
      "border": true
    }
  ]
}
[/block]
Add the SystemConfiguration, CoreTelephony, Security, and UIKit frameworks to your Xcode project. You can do this by selecting the project file in the navigator on the left side of the Xcode window, and then navigating to the Linked Frameworks and Libraries section. From there, click the + button to add the SystemConfiguration, CoreTelephony, Security, and UIKit frameworks.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/098a2fb-image3.png",
        "image3.png",
        612,
        195,
        "#313955"
      ],
      "border": true
    }
  ]
}
[/block]
#Step 2: Add CleverTap Credentials

To associate your iOS app with your CleverTap account, you will need to add your CleverTap credentials in the Info.plist file in your application.

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

Insert the Account ID and Account Token values from your [CleverTap account](https://eu1.dashboard.clevertap.com). These values are available on the Settings page. To navigate to the Settings page, log into [your CleverTap account](https://eu1.dashboard.clevertap.com/login.html), click on the gear icon on the top right navigation, and select Settings dashboard. 
[block:callout]
{
  "type": "warning",
  "title": "Using IFA to uniquely identify users",
  "body": "GDPR mandates that if you are using IFA to uniquely identify users, there should be prominent disclosure in your application that explains this to your end customer.  You can read more about  GDPR [here](https://clevertap.com/blog/in-preparation-of-gdpr-compliance/). \n\nNote: if you still want to use IFA to generate a users identity, so that you can identify users across re-installs, add the key *CleverTapUseIFA* with the type *Boolean* and value as *Yes*, to your info.plist file. \n\nContact support@clevertap.com for any questions."
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
        "https://files.readme.io/480a26b-image2.png",
        "image2.png",
        489,
        258,
        "#e9e9ef"
      ],
      "border": true
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/24ecb73-image1.png",
        "image1.png",
        769,
        425,
        "#eaebf2"
      ],
      "border": true
    }
  ]
}
[/block]
#Step 3: Installation Steps for Swift

For a project written in Swift, you are required to follow the below. If you are using Objective-C, please continue to step 4.

Starting with v3.1.4, the SDK includes a modulemap, allowing you to import the SDK as a module rather than using a bridging header. See the example implementation here.

If you are unable to do that you can use a bridging header instead. In that case:
  * Add the example [bridging-header.h](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftStarterProjectManualInstall/CleverTapSDK-Bridging-Header.h) (rename to YOUR-PROJECT-NAME-Bridging-Header.h, if you like) to your project.
  * Add the path to that [bridging-header.h](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftStarterProjectManualInstall/CleverTapSDK-Bridging-Header.h) in the Objective-C Bridging Header section of your project Build Settings. 

Alternatively, you can also append the contents of the [bridging-header.h](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftStarterProjectManualInstall/CleverTapSDK-Bridging-Header.h) to your existing Bridging Header file.

#Step 4: SDK Integration

Import CleverTap.h to your AppDelegate.h file.
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
  "title": "Manual Integration",
  "body": "In a single line of code, the autoIntegrate method lets you automatically track app launches, receive in-app notifications, and enable deep-link tracking. \n\nAs an alternative to using the automatic integration method, you can manually turn on different CleverTap SDK features. The manual integration steps are documented on [this page](https://developer.clevertap.com/docs/ios)."
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
        "https://files.readme.io/ddced1b-screen5.png",
        "screen5.png",
        1254,
        345,
        "#e2e2e5"
      ],
      "border": true
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
        "https://files.readme.io/c8e27b6-screen4.png",
        "screen4.png",
        1080,
        597,
        "#f2f0f2"
      ],
      "border": true
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
        "https://files.readme.io/d2cb773-screen7.png",
        "screen7.png",
        1248,
        692,
        "#e1e1e5"
      ],
      "border": true
    }
  ]
}
[/block]
# Next Steps 

Congrats on successfully integrating CleverTap into your iOS application! 

You are now automatically tracking user events like app launches, and associating that information with profiles for each of your users. You also learned how to add information to a user profile and how to track custom events.

In the next [iOS guide](doc:ios), you will learn more advanced options for tracking custom events and enriching user profiles.