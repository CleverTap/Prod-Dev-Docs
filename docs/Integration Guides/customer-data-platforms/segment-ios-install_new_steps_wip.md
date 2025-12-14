---
title: Segment Integration in React Native - iOS
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
Once the Segment library is integrated, toggle CleverTap on in your Segment destinations, and add your CleverTap Account ID and CleverTap Account Token which you can find in the CleverTap Dashboard under Settings.

You can integrate CleverTap via a server-side or mobile destination (iOS or Andriod). If you are interested in using CleverTap’s push notifications or in-app notifications products, you should use mobile destinations.

For Adding Account Id and Account Token ->


##Step 1:
Go to Destination in Segment -> Select CleverTap -> Configure CleverTap.
[block:code]
{
  "codes": [
    {
      "code": "npm add  @segment/analytics-react-native",
      "language": "javascript"
    }
  ]
}
[/block]
##Step 2:

Add CleverTap Account Id, Account Token and Enable the Switch from the top. For CleverTap Account Id and Account Token, Go to CleverTap Dashboard > Your Application > Settings.

[block:code]
{
  "codes": [
    {
      "code": "pod install",
      "language": "javascript"
    }
  ]
}
[/block]
##Step 3: Then in your application file App.js, setup the SDK like so:


[block:code]
{
  "codes": [
    {
      "code": "// replace with your write key\nanalytics.setup('YOUR_WRITE_KEY', {\n  // Record screen views automatically!\n  recordScreenViews: true,\n  // Record certain application events automatically!\n  trackAppLifecycleEvents: true\n})",
      "language": "javascript"
    }
  ]
}
[/block]
And of course, import the SDK in the files that you use it with:
[block:code]
{
  "codes": [
    {
      "code": "import analytics from '@segment/analytics-react-native'",
      "language": "javascript"
    }
  ]
}
[/block]
##Step 4: Now add CleverTap-Segment SDK in react native project.
[block:code]
{
  "codes": [
    {
      "code": "npm add  @segment/analytics-react-native-clevertap",
      "language": "javascript"
    }
  ]
}
[/block]
And of course, install the pods navigating inside the iOS directory.
[block:code]
{
  "codes": [
    {
      "code": "pod install",
      "language": "javascript"
    }
  ]
}
[/block]
##Step 4: Now Run the project to check for compile errors. If there are no errors then continue further steps, else check below steps to resolve the errors

##Resolve Compile Error (CleverTap.h file not found):

  * Open SEGCleverTapIntegration.m file from pods directory under segment-clevertap pods.
  * Change  #import <CleverTapSDK/CleverTap.h> to #import <CleverTap-iOS-SDK/CleverTap.h> and hit unlock button to edit and save the changes.
  * Run the project again and this should compile successfully without error.

##OR (Recommended Approach) : 

1. Add **use_modular_headers! **in your PodFile to enable the stricter search paths and module map generation for all of your pods.
2. Run pod install
3. Navigate to the SEGCleverTapIntegration.m class, and replace `#import<CleverTapSDK/CleverTap.h>` with `@import CleverTapSDK;`.
4. Also, in this scenario you need to import header as `@import CleverTapSDK;` instead of `<CleverTapSDK/CleverTap.h>` in the App Delegate class as well.

##Step 5: Then in your application file App.js, configure CleverTap like so:

[block:code]
{
  "codes": [
    {
      "code": "analytics.setup('YOUR_WRITE_KEY', {\n  // Record screen views automatically!\n  recordScreenViews: true,\n  // Record certain application events automatically!\n  trackAppLifecycleEvents: true\n  using : [CleverTap]\n})\n",
      "language": "javascript"
    }
  ]
}
[/block]
And of course, import the ClevertTap SDK in the application file:
[block:code]
{
  "codes": [
    {
      "code": "import CleverTap from '@segment/analytics-react-native-clevertap'",
      "language": "javascript"
    }
  ]
}
[/block]
##Step 6: Enabling Push Notifications.

In AppDelegate.m file, add code to register for Push Notification. (Assuming PN capability is added to the project)

[block:code]
{
  "codes": [
    {
      "code": "UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];\n    center.delegate = self;\n    [center requestAuthorizationWithOptions:(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge) completionHandler:^(BOOL granted, NSError * _Nullable error){\n    if(!error){\n      dispatch_async(dispatch_get_main_queue(), ^{\n         [[UIApplication sharedApplication] registerForRemoteNotifications];\n      });\n    }\n    }];",
      "language": "javascript"
    }
  ]
}
[/block]
Initialise CleverTap SDK to register the device token within didFinishLaunchingWithOptions method in AppDelegate.m file.
[block:code]
{
  "codes": [
    {
      "code": "[CleverTap autoIntegrate];",
      "language": "javascript"
    }
  ]
}
[/block]
##Step 7: Pushing Profile and Events.

Add below code to push profile in application file App.js

[block:code]
{
  "codes": [
    {
      "code": "analytics.identify(\"User_ID\", {\n  \t\temail: \"test@example.com\",\n\t\tname : \"Test User\"\n});\n//User_ID - Unique Identity of the user\n//This method maps to OnUserLogin() method of CleverTap. ",
      "language": "javascript"
    }
  ]
}
[/block]
Add below code to push custom event against a user profile.
[block:code]
{
  "codes": [
    {
      "code": "analytics.track('Sample Event', {\n  \t\t'name': 'Sword of Heracles',\n  \t\t'eventId': '1234'\n});",
      "language": "javascript"
    }
  ]
}
[/block]
You can see a [sample project](https://github.com/jaysmehta/CleverTapSegmentReactNative) here.

#Related Articles

https://segment.com/docs/connections/destinations/catalog/clevertap/
https://segment.com/docs/connections/sources/catalog/libraries/mobile/react-native/
https://developer.clevertap.com/docs/react-native