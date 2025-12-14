---
title: iOS SDK_Version 3.10.0_WIP
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
This section shows you how to how to track custom events, enrich user profiles, and send push notifications with the CleverTap SDK in your iOS app. If you have not already completed the [iOS SDK Quick Start Guide](doc:ios-quickstart-guide), complete that first before following the steps in this section. The quick start guide shows you how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard.

# User Profiles

CleverTap stores the user’s demographic data (gender, age, location), app and website interaction events, campaign visits, and transaction history to give you a complete picture of every user.

A user profile is automatically created for every user launching your mobile application – whether logged in or not.

Initially, the user profile starts out as “Anonymous” – which means that the profile does not yet contain any identifiable information about the user. You can choose to enrich the profile with attributes such as name, age, and customer id.

CleverTap provides pre-defined profile properties such as name, phone, gender, age and so on to represent well know properties associated with a profile. It is strongly recommended to use these standard property names. A list of all pre-defined property names is available here. In addition, we also support arbitrary single and multi-value profile properties.

Examples of updating profile properties for a project written in Objective-C are documented below.
[block:code]
{
  "codes": [
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nNSDateComponents *dob = [[NSDateComponents alloc] init];\ndob.day = 24;\ndob.month = 5;\ndob.year = 1992;\nNSDate *d = [[NSCalendar currentCalendar] dateFromComponents:dob];\nNSDictionary *profile = @{\n    @\"Name\": @\"Jack Montana\",               // String\n    @\"Identity\": @61026032,                 // String or number\n    @\"Email\": @\"jack@gmail.com\",            // Email address of the user\n    @\"Phone\": @\"+14155551234\",              // Phone (with the country code, starting with +)\n    @\"Gender\": @\"M\",                        // Can be either M or F\n    @\"Employed\": @\"Y\",                      // Can be either Y or N\n    @\"Education\": @\"Graduate\",              // Can be either Graduate, College or School\n    @\"Married\": @\"Y\",                       // Can be either Y or N\n    @\"DOB\": d,                              // Date of Birth. An NSDate object\n    @\"Age\": @28,                            // Not required if DOB is set\n    @\"Tz\": @\"Asia/Kolkata\",                 //an abbreviation such as \"PST\", a full name such as \"America/Los_Angeles\", \n                                            //or a custom ID such as \"GMT-8:00\"\n    @\"Photo\": @\"www.foobar.com/image.jpeg\", // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\n    @\"MSG-email\": @NO,                      // Disable email notifications\n    @\"MSG-push\": @YES,                      // Enable push notifications\n    @\"MSG-sms\": @NO                         // Disable SMS notifications\n    @\"MSG-dndPhone\": @YES,                  // Opt out phone number from SMS                                                    notifications\n    @\"MSG-dndEmail\": @YES,                  // Opt out email from email                                                    notifications\n};\n\n[[CleverTap sharedInstance] profilePush:profile];",
      "language": "objectivec",
      "name": "Predefined"
    },
    {
      "code": "NSDictionary *profile = @{\n    @\"Customer Type\": @\"Silver\",\n    @\"Prefered Language\": @\"English\",\n};\n\n[[CleverTap sharedInstance] profilePush:profile];\n\n/**\n * Data types:\n * The value of a property can be of type NSDate, a NSNumber, a NSString, or a BOOL.\n */",
      "language": "objectivec",
      "name": "Arbitrary Single Value"
    },
    {
      "code": "// To set a multi-value property\n[[CleverTap sharedInstance] profileSetMultiValues:@[@\"bag\", @\"shoes\"] forKey:@\"myStuff\"];\n\n// To add an additional value(s) to a multi-value property\n[[CleverTap sharedInstance] profileAddMultiValue:@\"coat\" forKey:@\"myStuff\"];\n// or\n[[CleverTap sharedInstance] profileAddMultiValues:@[@\"socks\", @\"scarf\"] forKey:@\"myStuff\"];\n\n//To remove a value(s) from a multi-value property\n[[CleverTap sharedInstance] profileRemoveMultiValue:@\"bag\" forKey:@\"myStuff\"];\n[[CleverTap sharedInstance] profileRemoveMultiValues:@[@\"shoes\", @\"coat\"] forKey:@\"myStuff\"];\n\n//To remove the value of a property (scalar or multi-value)\n[[CleverTap sharedInstance] profileRemoveValueForKey:@\"myStuff\"];",
      "language": "objectivec",
      "name": "Arbitrary Multi Value"
    }
  ]
}
[/block]
Examples of updating profile properties for a project written in Swift are documented below.
[block:code]
{
  "codes": [
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nlet dob = NSDateComponents()\ndob.day = 24\ndob.month = 5\ndob.year = 1992\nlet d = NSCalendar.currentCalendar().dateFromComponents(dob)\nlet profile: Dictionary<String, AnyObject> = [\n    \"Name\": \"Jack Montana\",                 // String\n    \"Identity\": 61026032,                   // String or number\n    \"Email\": \"jack@gmail.com\",              // Email address of the user\n    \"Phone\": \"+14155551234\",                // Phone (with the country code, starting with +)\n    \"Gender\": \"M\",                          // Can be either M or F\n    \"Employed\": \"Y\",                        // Can be either Y or N\n    \"Education\": \"Graduate\",                // Can be either School, College or Graduate\n    \"Married\": \"Y\",                         // Can be either Y or N\n    \"DOB\": d!,                              // Date of Birth. An NSDate object\n    \"Age\": 28,                              // Not required if DOB is set\n    \"Tz\":\"Asia/Kolkata\",                    //an abbreviation such as \"PST\", a full name such as \"America/Los_Angeles\", \n                                            //or a custom ID such as \"GMT-8:00\"\n    \"Photo\": \"www.foobar.com/image.jpeg\",   // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\n    \"MSG-email\": false,                     // Disable email notifications\n    \"MSG-push\": true,                       // Enable push notifications\n    \"MSG-sms\": false                        // Disable SMS notifications\n    \"MSG-dndPhone\": true,                  // Opt out phone number from SMS                                                    notifications\n    \"MSG-dndEmail\": @YES,                  // Opt out email from email                                                    notifications\n]\n\nCleverTap.sharedInstance()?.profilePush(profile)",
      "language": "swift",
      "name": "Predefined"
    },
    {
      "code": "let profile: Dictionary<String, AnyObject> = [\n    \"Customer Type\": \"Silver\",\n    \"Prefered Language\": \"English\"\n]\n\nCleverTap.sharedInstance()?.profilePush(profile)\n\n/**\n * Data types:\n * The value of a property can be of type NSDate, a Number, a String, or a Bool.\n */",
      "language": "swift",
      "name": "Arbitrary Single Value"
    },
    {
      "code": "// To set a multi-value property\nCleverTap.sharedInstance()?.profileSetMultiValues([\"bag\", \"shoes\"], forKey: \"myStuff\")\n\n// To add an additional value(s) to a multi-value property\nCleverTap.sharedInstance()?.profileAddMultiValue(\"coat\", forKey: \"myStuff\")\n// or\nCleverTap.sharedInstance()?.profileAddMultiValues([\"socks\", \"scarf\"], forKey: \"myStuff\")\n\n//To remove a value(s) from a multi-value property\nCleverTap.sharedInstance()?.profileRemoveMultiValue(\"bag\", forKey: \"myStuff\")\nCleverTap.sharedInstance()?.profileRemoveMultiValues([\"shoes\", \"coat\"], forKey: \"myStuff\")\n\n//To remove the value of a property (scalar or multi-value)\nCleverTap.sharedInstance()?.profileRemoveValueForKey(\"myStuff\")",
      "language": "swift",
      "name": "Arbitrary Multi Value"
    }
  ]
}
[/block]
CleverTap provides easy ways to enrich the user profile with data from sources, such as Facebook. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

## DND 
###Phone
You can opt-out a user or a phone number. 

  * To opt-out a specific user when multiple users share the same phone number,  you can disable SMS for a specific user. Set the MSG-sms flag to false and the specified user stops receiving SMS. However, all other users sharing the number continue to receive messages if they have not opted out.  
  
  * To opt-out a phone number and all the users associated with it, set the MSG-dndPhone flag to true.

###Email
You can opt-out a user or an email address.  

  * To opt-out a specific user when multiple users share the same email address,  you can disable email for a specific user. Set the MSG-email flag to false and the specified user stops receiving email. However, all other users sharing the email address continue to receive messages if they have not opted out.  
  
  * To opt-out an email and all the users associated with it, set the MSG-dndEmail flag to true.

# User Events

A User Event is an event that a user takes in your mobile application. CleverTap records the event on the User Profile, using an Event Name and optional associated key:value-based Event Properties. You can then segment users, target and personalize messaging based on both the Event Name and specific Event Properties.

An example of recording a User Event called Product Viewed.
[block:code]
{
  "codes": [
    {
      "code": "// event without properties\n[[CleverTap sharedInstance] recordEvent:@\"Product viewed\"];",
      "language": "objectivec"
    },
    {
      "code": "// event without properties\nCleverTap.sharedInstance()?.recordEvent(\"Product viewed\")",
      "language": "swift"
    }
  ]
}
[/block]
An example of recording a User Event called Product Viewed with Properties.
[block:code]
{
  "codes": [
    {
      "code": "// event with properties\nNSDictionary *props = @{\n    @\"Product name\": @\"Casio Chronograph Watch\",\n    @\"Category\": @\"Mens Accessories\",\n    @\"Price\": @59.99,\n    @\"Date\": [NSDate date]\n};\n\n[[CleverTap sharedInstance] recordEvent:@\"Product viewed\" withProps:props];\n\n/**\n * Data types:\n * The value of a property can be of type NSDate, a NSNumber, a NSString, or a BOOL.\n *\n * NSDate object:\n * When a property value is of type NSDate, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "objectivec"
    },
    {
      "code": "// event with properties\nlet props = [\n    \"Product name\": \"Casio Chronograph Watch\",\n    \"Category\": \"Mens Accessories\",\n    \"Price\": 59.99,\n    \"Date\": NSDate()\n]\n\nCleverTap.sharedInstance()?.recordEvent(\"Product viewed\", withProps: props)\n\n/**\n * Data types:\n * The value of a property can be of type NSDate, a Number, a String, or a Bool.\n *\n * NSDate object:\n * When a property value is of type NSDate, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "swift"
    }
  ]
}
[/block]
For an example of how to record a custom event from a watchOS app, see [here](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftStarter/SwiftWatchOS%20Extension/InterfaceController.swift).

For an example of recording an event from an App Extension, see [here](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftStarter/NotificationService/NotificationService.swift).

Events help you understand how your users interact with your app. CleverTap tracks certain common User Events automatically, while giving you the flexibility to record business specific events.

# Push Notifications

If you automatically integrated the SDK in the quick start guide, you have already enabled push notification support. If you have not yet, follow the steps below. 

CleverTap using the SDK is capable of sending push notifications to your users using the dashboard. To enable this support, when your app delegate receives the application:didRegisterForRemoteNotificationsWithDeviceToken: message, include a call to CleverTap setPushToken: as follows.
[block:code]
{
  "codes": [
    {
      "code": "- (void) application:(UIApplication *)application\n    didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {\n      [[CleverTap sharedInstance] setPushToken:deviceToken];\n}",
      "language": "objectivec"
    },
    {
      "code": "func application(application: UIApplication,\n                 didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData) {\n    CleverTap.sharedInstance()?.setPushToken(deviceToken)\n}",
      "language": "swift"
    }
  ]
}
[/block]
The above will save the user’s APNS token with CleverTap. This token is used to send push notifications.

Next, include a call to handleNotificationWithData: when your app delegate is sent the following messages:
  * application:didReceiveRemoteNotification:
  * application:didReceiveLocalNotification:
  * application:didReceiveRemoteNotification:fetchCompletionHandler:
  * application:handleActionWithIdentifier:forRemoteNotification:completionHandler:
  * application:handleActionWithIdentifier:forLocalNotification:completionHandler: 
[block:code]
{
  "codes": [
    {
      "code": "// iOS 8 and below\n- (void) application:(UIApplication *)application\n    didReceiveRemoteNotification:(NSDictionary *)userInfo {\n      [[CleverTap sharedInstance] handleNotificationWithData:userInfo];\n    }\n- (void) application:(UIApplication *)application\n    didReceiveLocalNotification:(UILocalNotification *)notification {\n      [[CleverTap sharedInstance] handleNotificationWithData:notification];\n    }\n    \n// For iOS 8 and iOS 9\n- (void) application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier\n    forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {\n        [[CleverTap sharedInstance] handleNotificationWithData:notification];\n        if (completionHandler) completionHandler();\n    }\n    \n- (void) application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier\n    forRemoteNotification:(NSDictionary *)userInfo completionHandler:(void (^)())completionHandler {\n        [[CleverTap sharedInstance] handleNotificationWithData:userInfo];\n        if (completionHandler) completionHandler();\n}\n\n- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo \nfetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {\n\t[[CleverTap sharedInstance] handleNotificationWithData:userInfo];\n    completionHandler(UIBackgroundFetchResultNoData);\n}",
      "language": "objectivec"
    },
    {
      "code": "func application(application: UIApplication, didReceiveRemoteNotification\n    userInfo: [NSObject : AnyObject]) {\n    CleverTap.sharedInstance()?.handleNotificationWithData(userInfo)\n}\n\nfunc application(application: UIApplication, didReceiveLocalNotification\n    notification: UILocalNotification) {\n    CleverTap.sharedInstance()?.handleNotificationWithData(notification)\n}\n\n// As of iOS 8 and above\nfunc application(application: UIApplication, handleActionWithIdentifier identifier: String?,\n                 forLocalNotification notification: UILocalNotification, completionHandler: () -> Void) {\n    CleverTap.sharedInstance()?.handleNotificationWithData(notification)\n    completionHandler()\n}\n\nfunc application(application: UIApplication, handleActionWithIdentifier identifier: String?,\n                 forRemoteNotification userInfo: [NSObject : AnyObject], completionHandler: () -> Void) {\n    CleverTap.sharedInstance()?.handleNotificationWithData(userInfo)\n    completionHandler()\n}\n\nfunc application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], \nfetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {\n\tCleverTap.sharedInstance()?.handleNotification(withData: userInfo)\n    completionHandler(.noData)\n}",
      "language": "swift"
    }
  ]
}
[/block]
**Required for CleverTap iOS SDK v.3.1.7 and below and Manual Integration:**

iOS 10 has introduced the UNUserNotificationCenterDelegate for handling notifications. Implementation is optional, but if you do implement the Delegate, please note that, whether or not you have used automatic integration, if you wish CleverTap to track notification opens and fire attached deep links, you must manually call the SDK as follows.
[block:code]
{
  "codes": [
    {
      "code": "/** For iOS 10 and above - Background **/\n- (void)userNotificationCenter:(UNUserNotificationCenter *)center\ndidReceiveNotificationResponse:(UNNotificationResponse *)response\n         withCompletionHandler:(void (^)())completionHandler {\n    \n    /** For iOS 10 and above.\n     Use this method to perform the tasks associated with your app’s custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.\n     \n     You specify your app’s notification types and custom actions using UNNotificationCategory and UNNotificationAction objects. You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.\n     \n     If you do not implement this method, your app never responds to custom actions.\n     \n     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter?language=objc\n     \n    **/\n    \n    // Skip this line if you have opted for auto-integrate. If you wish CleverTap to record the notification open and fire any deep links contained in the payload.  \n    [[CleverTap sharedInstance] handleNotificationWithData:response.notification.request.content.userInfo];\n    \n    completionHandler();\n}",
      "language": "objectivec"
    },
    {
      "code": "/** For iOS 10 and above - Background **/\nfunc userNotificationCenter(_ center: UNUserNotificationCenter,\n                                didReceive response: UNNotificationResponse,\n                                withCompletionHandler completionHandler: @escaping () -> Void) {\n        \n\t/**\n\t Use this method to perform the tasks associated with your app’s custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.\n\t \n\t You specify your app’s notification types and custom actions using UNNotificationCategory and UNNotificationAction objects. You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.\n\t \n\t If you do not implement this method, your app never responds to custom actions.\n\t \n\t see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter\n\t \n\t **/\n\t\n\t// if you wish CleverTap to record the notification open and fire any deep links contained in the payload. Skip this line if you have opted for auto-integrate. \n\tCleverTap.sharedInstance().handleNotification(withData: response.notification.request.content.userInfo)\n\t\n\tcompletionHandler()\n        \n}",
      "language": "swift"
    }
  ]
}
[/block]
When your app is not running or is in the background, the above method will track notification opens and fire attached deep links. But if you want CleverTap to continue to fire attached deep links when in the foreground, you must manually call the SDK as follows.
[block:code]
{
  "codes": [
    {
      "code": "/** For iOS 10 and above - Foreground **/\n- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler{\n \n    \n   /**\n     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.\n     \n     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.\n     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.\n     \n     If you do not implement this method, your app never responds to custom actions.\n     \n     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter?language=objc\n     **/\n\n    // if you wish CleverTap to record the notification open and fire any deep links contained in the payload\n     [[CleverTap sharedInstance]handleNotificationWithData:notification.request.content.userInfo openDeepLinksInForeground: YES];\n     completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);\n}",
      "language": "objectivec"
    },
    {
      "code": "/** For iOS 10 and above - Foreground**/\n\nfunc userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: (UNNotificationPresentationOptions) -> Void) {\n\n       /**\n     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.\n     \n     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.\n     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.\n     \n     If you do not implement this method, your app never responds to custom actions.\n     \n     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter\n     **/\n\n    print(\"APPDELEGATE: didReceiveResponseWithCompletionHandler \\(response.notification.request.content.userInfo)\")\n\n    // If you wish CleverTap to record the notification click and fire any deep links contained in the payload. \n  \n     CleverTap.sharedInstance()?.handleNotification(withData: notification.request.content.userInfo, openDeepLinksInForeground: true)\n     completionHandler([.badge, .sound, .alert])\n}",
      "language": "swift"
    }
  ]
}
[/block]

##Push Notification Callback
To get the callback on the click of the Push Notifications, check  that your class implements the `CleverTapPushNotificationDelegate` delegate and then use the following method to handle the user-selected actions from Push notifications:
[block:code]
{
  "codes": [
    {
      "code": "\n- (void)pushNotificationTappedWithCustomExtras:(NSDictionary *)customExtras {\n    \n      NSLog(@\"Push Notification Tapped with Custom Extras: %@\", customExtras);\n      ...\n }\n",
      "language": "objectivec"
    },
    {
      "code": "\nfunc pushNotificationTapped(withCustomExtras customExtras: [AnyHashable : Any]!) {\n      print(\"Push Notification Tapped with Custom Extras: \\(customExtras)\")\n      ...\n}\n",
      "language": "swift"
    }
  ]
}
[/block]

## FAQs - Push Notification Errors in iOS

 ###1. APNS Device Token Does Not Match The Specified Topic 

Apple Push Notification service (APNs) returns this error when the device token doesn't match the specified topic.

In the context of CleverTap, you will get this error when you try to send the notification with a wrong certificate. Check that you use a production certificate for the production environment. This error is caused because of incorrect configurations. 

Check the provisioning profile used to deploy the app and send the device token to the CleverTap dashboard. To avoid this error, the "App bundle ID" of the provisioning profile and the CleverTap push certificate (p12 certificate) must match.

###2. APNS Unregistered

The Push notification delivery to the target fails because the device token is inactive for the specified topic. If the device token is not valid/inactive you may have to wait till the user installs the app again. You can then start sending notifications. 


###3. APNS Topic Disallowed

The topic is currently the bundle identifier of the target application on an iOS device. The "APNS Topic Disallowed" error appears when you specify the incorrect "App Bundle ID" in the Account Settings. For more information, see [Apple documentation](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CommunicatingwithAPNs.html).

To resolve this error, check the value of the "App Bundle ID" and "APNs push mode" that you have configured in the CleverTap Dashboard under Push Settings.

###4. APNS Temporarily Blacklisted

You are temporarily blacklisted from sending push notifications to the device, which can be due to sending bad tokens. The push notification will not be delivered for some duration.

##5. APNS Failed Delivery 

The notification to the device sent from CleverTap failed to deliver due to either of the following reasons
  * The app was uninstalled and the token is no longer valid.
  * You have sent too many notifications in short duration to the same device.

If the user has uninstalled the app then you may have to wait until the user installs the app again before the user can receive your notifications.  
For more info about APNs, see [Handling Notification Responses from APNs](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/handling_notification_responses_from_apns).

###6. APNs Bad Device Token

The push notification delivery to the target device has failed due to an invalid token. The APNs bad device token error occurs when there is a mismatch between the APNs push mode on your app and the CleverTap dashboard. 
For example, you have set the APNs push mode in your app to the "development" stage and in the "CleverTap" dashboard you have set up to "production" stage. To check your CleverTap dashboard APNs push mode: Navigate to Settings > Engage > Mobile Push> iOS.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/20ff216-Push_Notification_iOS_API_Key.png",
        "Push_Notification_iOS_API_Key.png",
        1166,
        761,
        "#f9f9fb"
      ],
      "border": true
    }
  ]
}
[/block]
# Push Impressions
You can now raise and record push notifications delivered onto your users’ iOS devices.
CleverTap SDK v3.5.0 and above supports raising Push Impressions for push campaigns. 
To raise Push Impressions for iOS, the [Notification Service Extension](https://developer.apple.com/reference/usernotifications/unnotificationserviceextension) must be included.
Notification Service Extension is a separate and distinct binary embedded in your app bundle. Prior to displaying a new push notification, the system will call your Notification Service Extension to allow you to call the Notification Viewed method for your Application.

[block:callout]
{
  "type": "warning",
  "title": "Note",
  "body": "Push Impressions are not raised if the Notification Service Extension is not included in your app bundle."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "- (void)didReceiveNotificationRequest:(UNNotificationRequest *)request withContentHandler:(void (^)(UNNotificationContent * _Nonnull))contentHandler {\n    \n    self.contentHandler = contentHandler;\n    self.bestAttemptContent = [request.content mutableCopy];\n\n// While running the Application add CleverTap Account ID and Account token in your .plist file    \n\n// call to record the Notification viewed \n    [[CleverTap sharedInstance] recordNotificationViewedEventWithData:request.content.userInfo];\n    [super didReceiveNotificationRequest:request withContentHandler:contentHandler];\n    \n}",
      "language": "c",
      "name": "Objective C"
    },
    {
      "code": "override func didReceive(_ request: UNNotificationRequest, withContentHandler contentHandler: @escaping (UNNotificationContent) -> Void) {\n        // While running the Application add CleverTap Account ID and Account token in your .plist file\n        \n        // call to record the Notification viewed\n        CleverTap.sharedInstance()?.recordNotificationViewedEvent(withData: request.content.userInfo)\n        super.didReceive(request, withContentHandler: contentHandler)\n    }",
      "language": "swift",
      "name": "Swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "danger",
  "body": "If the `Notification viewed` method is not called correctly, it will create a new profile in CleverTap with only this event. This will create a dummy profile with no other event and will not be mapped to the profile from which the Push Impressions event was raised.\n\nTo avoid the creation of a new user profile while calling the Notification Viewed method from your Application, you must pass the Identity/ Email to CleverTap in the Notification Service Class or you can call recordNotificationViewedEvent API from your Main Application.",
  "title": "Dummy Profile Creation"
}
[/block]
Sending notifications via APNS
* Include the mutable-content flag in your apns payload (this key must be present in the apns payload or the system will not call your app extension)

# In-App Notifications

The CleverTap SDK allows you to show In-App notifications to your users. You can design in-app notifications right from the dashboard, without writing a single line of code. There is no code required for this feature

Note: not applicable inside App Extensions and watchOS apps.

**Javascript support in In-App Notifications**
CleverTap iOS SDK v3.5.0 and above supports embedding Javascript code inside Custom In-Apps. To make sure your Javascript code works on the app, while creating the InApp campaign, select the checkbox for Enabling javascript during the In-App campaign creation

All methods are explained with examples below:
[block:code]
{
  "codes": [
    {
      "code": "//Recording a User Event called Product Viewed in JS enabled custom in-apps.\nvar props = {foo: 'xyz', lang: 'French'};\nvar message = { action:'recordEventWithProps', event:'Product Viewed', props: props};\n\nif(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){\n // Call iOS interface                  \n   window.webkit.messageHandlers.clevertap.postMessage(message);         \n} \n\n//Updating profile properties of the User in JS enabled custom in-apps.\nvar props = {foo: 'xyz', lang: 'French'};\nvar message = { action:'profilePush', properties: props};\n\nif(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){\n // Call iOS interface\n  window.webkit.messageHandlers.clevertap.postMessage(message);        \n}\n\n//Setting a user profile\nvar message = {\n                action: 'profileSetMultiValues',\n                values: ['bag', 'kitkat'],\n                key: 'myStuff'\n            };\n            \nif(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){\n    window.webkit.messageHandlers.clevertap.postMessage(message);\n}\n\n\n//Add multi value\nvar message = {action: 'profileAddMultiValue',\n             value: 'coat', \n             key: 'myStuff'};\n\nif(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){\n    window.webkit.messageHandlers.clevertap.postMessage(message);\n}\n\n\n//Add multi values\n var message = {action: 'profileAddMultiValues',\n                values: ['bag', 'kitkat', 'Wine'],\n                key: 'myStuff'};\n\nif(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){\n    window.webkit.messageHandlers.clevertap.postMessage(message);\n}\n\n\n//Remove value for key\n\nvar message = {action: 'profileRemoveValueForKey',\n                    key: 'myStuff' };\n\nif(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){\n    window.webkit.messageHandlers.clevertap.postMessage(message);\n}\n\n\n//Remove multi values for key\n var message = {action: 'profileRemoveMultiValues',\n                values: ['scarf', 'knife'],\n                key: 'myStuff'};\n\nif(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){\n    window.webkit.messageHandlers.clevertap.postMessage(message);\n}\n\n\n//Remove multi Value\nvar message = {action: 'profileRemoveMultiValue',\n                value: 'bag',\n                key: 'myStuff'};\n\nif(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){\n    window.webkit.messageHandlers.clevertap.postMessage(message);\n}\n\n//Record Charged Event\nconst chargeDetails = {\n                \"Amount\" : 300,\n                \"Payment mode\": \"Credit Card\",\n                \"Charged ID\": 24052013\n            };\nconst items = [{\n                \"Category\": \"books\",\n                \"Book name\": \"The Millionaire next door\",\n                \"Quantity\": 1\n            },{\n                \"Category\": \"books\",\n                \"Book name\": \"The Millionaire previous door\",\n                \"Quantity\": 5\n            }];\nconst message = {\n                action:'recordChargedEvent',\n                chargeDetails: chargeDetails,\n                items: items\n            };\nif (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {\n\twindow.webkit.messageHandlers.clevertap.postMessage(message);\n}",
      "language": "javascript"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "To add inline media support in custom HTML for in-app notifications, add the key playsinline with value as 1 or true as a query parameter in your media URL For example, https://www.youtube.com/watch?v=RUvrypIAPbg?playsinline=1. This functionality is available for CleverTap iOS SDK 3.7.2 and above.",
  "title": "Inline Media"
}
[/block]
## **InApp Notification Button onClick Callbacks**
iOS SDK v3.7.1 and above supports callback on the click of InApp Notification Buttons by returning a Map of Key-Value pairs. To use this, make sure your class implements the CleverTapInAppNotificationDelegate and use the following method to handle user-selected actions from InApp notifications:
[block:code]
{
  "codes": [
    {
      "code": "- (void)inAppNotificationButtonTappedWithCustomExtras:(NSDictionary *)customExtras {\n     NSLog(@\"inAppNotificationButtonTappedWithCustomExtras:%@\", customExtras);\n  ...\n}",
      "language": "objectivec"
    },
    {
      "code": "func inAppNotificationButtonTapped(withCustomExtras customExtras: [AnyHashable : Any]!) {\n      print(\"In-App Button Tapped with custom extras:\", customExtras ?? \"\");\n  }",
      "language": "swift"
    }
  ]
}
[/block]

## Control In-App Notifications

For CleverTap iOS SDK 3.10.0 and above, you can control the rendering of in-app notifications by calling the following methods:
### `suspendInAppNotifications`
Suspends and saves in-app notifications until `resumeInAppNotifications` <<@aditi, this is the method name ?>> is called for the current session.
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.sharedInstance()?.suspendInAppNotifications()\n\n\n\n",
      "language": "swift"
    },
    {
      "code": "#import <CleverTapSDK/CleverTap+InAppNotifications.h>\n[[CleverTap sharedInstance] suspendInAppNotifications];\n\n  ",
      "language": "objectivec"
    }
  ]
}
[/block]
### `discardInAppNotifications`
Discards in-app notifications until `resumeInAppNotifications` is called for the current session.


[block:code]
{
  "codes": [
    {
      "code": "CleverTap.sharedInstance()?.discardInAppNotifications()\n\n\n",
      "language": "swift"
    },
    {
      "code": "#import <CleverTapSDK/CleverTap+InAppNotifications.h>\n[[CleverTap sharedInstance] discardInAppNotifications];\n",
      "language": "objectivec"
    }
  ]
}
[/block]
### `resumeInAppNotifications`

Resumes displaying in-app notifications.

If you call this API after the `discardInAppNotifications()` method, it resumes the in-app notifications for events raised after the call is performed. <<@aditi is this applicable for iOS ?>>

However, if you call this API after the `suspendInAppNotifications()` method, then it displays all queued in-app notifications and also resumes in-app notifications for events raised after the call is performed.  <<@aditi is this applicable for iOS?>>
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.sharedInstance()?.resumeInAppNotifications()\n  \n\n",
      "language": "java"
    },
    {
      "code": "#import <CleverTapSDK/CleverTap+InAppNotifications.h>\n[[CleverTap sharedInstance] resumeInAppNotifications];\n\n\n  ",
      "language": "objectivec"
    }
  ]
}
[/block]
# Custom Handling Deeplink

For CleverTap iOS SDK 3.10.0 and above, you can implement custom handling for URLs of in-app notification CTAs, push notifications, and app Inbox messages. 
Check that your class conforms to the `CleverTapURLDelegate` protocol by first calling `setURLDelegate` <<@aditi, is this a method?>>. Use the following protocol method  <<@aditi what is the method name here>> to handle URLs received from channels such as in-app notification CTAs, push notifications, and app Inbox messages:
[block:code]
{
  "codes": [
    {
      "code": "// Set the URL Delegate\nCleverTap.sharedInstance()?.setUrlDelegate(self)\n\n\n\n // CleverTapURLDelegate method\npublic func shouldHandleCleverTap(_ url: URL?, for channel: CleverTapChannel) -> Bool {  \n    print(\"Handling URL: \\(url!) for channel: \\(channel)\") \n    return true \n}\n\n\n\n",
      "language": "swift"
    },
    {
      "code": "#import <CleverTapSDK/CleverTapURLDelegate.h>\n\n// Set the URL Delegate\n[[CleverTap sharedInstance]setUrlDelegate:self];\n\n// CleverTapURLDelegate method \n- (BOOL)shouldHandleCleverTapURL:(NSURL *)url forChannel:(CleverTapChannel)channel {\n    NSLog(@\"Handling URL: \\(%@) for channel: \\(%d)\", url, channel);\n    return YES;\n}\n\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The CleverTap SDK automatically resumes in-app notifications display on CleverTap shared instance creation. This ensures that the feature <<@aditi what is the feature name>> remains available after temporary suspend/ discard <<@aditi - of the in-app notification? >.\nThe `showInAppNotificationIfAny` method will be deprecated soon<<@aditi, do we have a date for this ?>>.  Use the `resumeInAppNotifications` method to manually display the pending in-app notifications."
}
[/block]
# Manually Enable Support for Universal (Deep) Link Tracking

Deep links are a way of launching a native app and providing additional information telling it do some specific event or show specific content. CleverTap automatically tracks universal links that open your application. If you have universal (deep) links coming to your app, you can capture the incoming UTM parameters easily. Call handleOpenURL:sourceApplication: when the application:openURL:sourceApplication:annotation: message is sent to your app delegate.
[block:code]
{
  "codes": [
    {
      "code": "- (BOOL) application:(UIApplication *)application\n    openURL:(NSURL *)url\n    sourceApplication:(NSString *)sourceApplication\n    annotation:(id)annotation {\n      [[CleverTap sharedInstance] handleOpenURL:url sourceApplication:sourceApplication];\n      return YES;\n}\n\n- (BOOL)application:(UIApplication *)app\n            openURL:(NSURL *)url\n            options:(NSDictionary *)options {\n    [[CleverTap sharedInstance] handleOpenURL:url sourceApplication:nil];\n    return YES;\n}\n\n- (void)openURL:(NSURL*)url options:(NSDictionary<NSString *, id> *)options\ncompletionHandler:(void (^ __nullable)(BOOL success))completion {\n    [[CleverTap sharedInstance] handleOpenURL:url sourceApplication:nil];\n    if (completion) {\n        completion(YES);\n    }\n}",
      "language": "objectivec"
    },
    {
      "code": "func application(application: UIApplication, openURL url: NSURL,\n                 sourceApplication: String?, annotation: AnyObject) -> Bool {\n    CleverTap.sharedInstance()?.handleOpenURL(url, sourceApplication: sourceApplication)\n    return true\n}\n\n// Swift 3\nfunc application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {\n   CleverTap.sharedInstance()?.handleOpen(url, sourceApplication: nil)\n   return true\n}\n\nfunc open(_ url: URL, options: [String : Any] = [:],\n                   completionHandler completion: ((Bool) -> Swift.Void)? = nil) {\n\tCleverTap.sharedInstance()?.handleOpen(url, sourceApplication: nil)\n\tcompletion?(false)\n}",
      "language": "swift"
    }
  ]
}
[/block]
# Send Location Information to CleverTap
[block:code]
{
  "codes": [
    {
      "code": "- (void)locationManager:(CLLocationManager *)manager \n    didUpdateToLocation:(CLLocation *)newLocation \n           fromLocation:(CLLocation *)oldLocation {\n    [CleverTap setLocation: newLocation.coordinate];\n}",
      "language": "objectivec"
    },
    {
      "code": "func locationManager(manager: CLLocationManager, didUpdateToLocation newLocation: CLLocation,\n                     fromLocation oldLocation: CLLocation) {\n    CleverTap.setLocation(newLocation.coordinate)\n}",
      "language": "swift"
    }
  ]
}
[/block]
# App Inbox
  * The CleverTap SDK 3.4.0 and above, allows you to create App Inbox notifications for your users. 
  * You can use the App Inbox provided by CleverTap or create your own
  * You can design App Inbox notifications right from the dashboard

Note: not applicable for tvOS and watchOS apps.

##Step 1: Integrating the CleverTap App Inbox
  * You have to import the CleverTap Inbox Header
  * Initialize the CleverTap App Inbox Method
[block:code]
{
  "codes": [
    {
      "code": "#import <CleverTapSDK/CleverTap+Inbox.h> \n\n//Initialize the CleverTap App Inbox\n    [[CleverTap sharedInstance] initializeInboxWithCallback:^(BOOL success) {\n        int messageCount = (int)[[CleverTap sharedInstance] getInboxMessageCount];\n        int unreadCount = (int)[[CleverTap sharedInstance] getInboxMessageUnreadCount];\n        NSLog(@\"Inbox Message: %d/%d\", messageCount, unreadCount);\n    }];",
      "language": "objectivec",
      "name": "Objective C"
    },
    {
      "code": "import CleverTapSDK\n\n//Initialize the CleverTap App Inbox\nCleverTap.sharedInstance()?.initializeInbox(callback: ({ (success) in\n        let messageCount = CleverTap.sharedInstance()?.getInboxMessageCount()\n        let unreadCount = CleverTap.sharedInstance()?.getInboxMessageUnreadCount()\n        print(\"Inbox Message:\\(String(describing: messageCount))/\\(String(describing: unreadCount)) unread\")\n }))",
      "language": "swift",
      "name": "Swift"
    }
  ]
}
[/block]
##Step 2: Configure App Inbox Styling
  * By default, App Inbox styling will be CleverTap's default colors and font
  * You can choose to change the styling as shown below
[block:code]
{
  "codes": [
    {
      "code": "// config the style of App Inbox Controller\nCleverTapInboxStyleConfig *style = [[CleverTapInboxStyleConfig alloc] init];\nstyle.backgroundColor = [UIColor blueColor];\nstyle.messageTags = @[@\"tag1\", @\"tag2\"];\nstyle.navigationBarTintColor = [UIColor blueColor];\nstyle.navigationTintColor = [UIColor blueColor];\nstyle.tabUnSelectedTextColor = [UIColor blueColor];\nstyle.tabSelectedTextColor = [UIColor blueColor];\nstyle.tabSelectedBgColor = [UIColor blueColor];\nstyle.firstTabTitle = @\"My First Tab\";\n\nCleverTapInboxViewController *inboxController = [[CleverTap sharedInstance] \nnewInboxViewControllerWithConfig:style andDelegate:self];\n    if (inboxController) {\n        UINavigationController *navigationController = [[UINavigationController alloc] initWithRootViewController:inboxController];\n        [self presentViewController:navigationController animated:YES completion:nil];\n    }",
      "language": "objectivec"
    },
    {
      "code": "// config the style of App Inbox Controller\n    let style = CleverTapInboxStyleConfig.init()\n    style.title = \"App Inbox\"\n    style.backgroundColor = UIColor.blue()\n    style.messageTags = [\"tag1\", \"tag2\"]\n    style.navigationBarTintColor = UIColor.blue()\n    style.navigationTintColor = UIColor.blue()\n    style.tabUnSelectedTextColor = UIColor.blue()\n    style.tabSelectedTextColor = UIColor.blue()\n    style.tabSelectedBgColor = UIColor.blue()\n    style.firstTabTitle = \"My First Tab\"\n    \n    if let inboxController = CleverTap.sharedInstance()?.newInboxViewController(with: style, andDelegate: self) {\n        let navigationController = UINavigationController.init(rootViewController: inboxController)\n        self.present(navigationController, animated: true, completion: nil)\n  }",
      "language": "swift"
    }
  ]
}
[/block]
## Step 3: Creating your own App Inbox
  * You can choose to create your own App Inbox by calling the following APIs
[block:code]
{
  "codes": [
    {
      "code": "//Initialize App Inbox\n- (void)initializeInboxWithCallback:(CleverTapInboxSuccessBlock _Nonnull)callback;\n\n//Get Inbox Message Count\n- (NSUInteger)getInboxMessageCount;\n\n//Get Inbox Unread Count\n- (NSUInteger)getInboxMessageUnreadCount;\n\n//Get All messages \n- (NSArray<CleverTapInboxMessage *> * _Nonnull )getAllInboxMessages;\n\n//Get only Unread messages\n- (NSArray<CleverTapInboxMessage *> * _Nonnull )getUnreadInboxMessages;\n\n//Get message object belonging to the given message id only. Message id must be a String\n- (CleverTapInboxMessage * _Nullable )getInboxMessageForId:(NSString *)messageId;\n\n//Delete message from the Inbox. Message id must be a String\n- (void)deleteInboxMessage:(CleverTapInboxMessage * _Nonnull )message;\n\n//Mark Message as Read\n- (void)markReadInboxMessage:(CleverTapInboxMessage * _Nonnull) message;\n\n//Callback on Inbox Message update/delete/read (any activity)\n- (void)registerInboxUpdatedBlock:(CleverTapInboxUpdatedBlock _Nonnull)block;\n\n//Record Notification Viewed for App Inbox.\n- (void)recordInboxNotificationViewedEventForID:(NSString * _Nonnull)messageId;\n\n// Record Notification Clicked for App Inbox.\n- (void)recordInboxNotificationClickedEventForID:(NSString * _Nonnull)messageId;\n\n//Mark Message as Read. Message id must be a String\n- (void)markReadInboxMessageForID:(NSString * _Nonnull)messageId;\n\n//Delete message from the Inbox. Message id must be a String\n- (void)deleteInboxMessageForID:(NSString * _Nonnull)messageId;\n\n",
      "language": "objectivec",
      "name": "Public APIs"
    }
  ]
}
[/block]

###App Inbox Button onClick Callbacks

To get the callback on the click of the App Inbox Notification, check that your class implements the CleverTapInboxViewControllerDelegate and use the following method to handle user-selected actions from InApp notifications:
[block:code]
{
  "codes": [
    {
      "code": "-(void)messageDidSelect:(CleverTapInboxMessage *)message atIndex:(int)index withButtonIndex:(int)buttonIndex {\n      // This is called when an inbox message is clicked(tapped or call to action)\n    CleverTapInboxMessageContent *content = (CleverTapInboxMessageContent*)message.content[index];\n    ...\n}",
      "language": "objectivec"
    },
    {
      "code": "func messageDidSelect(_ message: CleverTapInboxMessage, at index: Int32, withButtonIndex buttonIndex: Int32) {\n    // This is called when an inbox message is clicked(tapped or call to action)\n  ...\n  }",
      "language": "swift",
      "name": null
    }
  ]
}
[/block]
iOS SDK v3.7.1 and above supports callback on the click of App Inbox Notification Buttons by returning a map of Key-Value pairs. Use the following method:
[block:code]
{
  "codes": [
    {
      "code": "- (void)messageButtonTappedWithCustomExtras:(NSDictionary *)customExtras {\n          NSLog(@\"App Inbox Button Tapped with custom extras::%@\", customExtras);\n  \n}\n\n",
      "language": "objectivec"
    },
    {
      "code": "func messageButtonTapped(withCustomExtras customExtras: [AnyHashable : Any]?) {\n        print(\"App Inbox Button Tapped with custom extras: \", customExtras ?? \"\");\n    }\n\n",
      "language": "swift"
    }
  ]
}
[/block]
# Debugging

By default, CleverTap logs are set to CleverTapLogLevel.info. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level to CleverTapLogLevel.debug. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTapLogLevel.off.
[block:code]
{
  "codes": [
    {
      "code": "#ifdef DEBUG\n   [CleverTap setDebugLevel:CleverTapLogDebug];\n#else\n   [CleverTap setDebugLevel:CleverTapLogOff];\n#endif",
      "language": "objectivec"
    },
    {
      "code": " #if DEBUG\n    CleverTap.setDebugLevel(CleverTapLogLevel.debug.rawValue)\n#else\n    CleverTap.setDebugLevel(CleverTapLogLevel.off.rawValue)\n#endif",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Advanced iOS Push Notifications"
}
[/block]
Apart from Title and Message, you have the below-mentioned options to add to your iOS push notification. Please note that each of these is optional.

**Rich Media**
Rich media requires iOS 10.

Single Media
  * You will have to install our <a href="https://github.com/CleverTap/CTNotificationService">single media</a> library to enable sending media iOS notifications.
  * If a media link is specified, the media will be displayed as a static thumbnail in the default view, and as the actual media (image/video/gif/audio) in the expanded view. You will have to install our single media library to enable sending media iOS notifications.
  * Supported file formats are .png, .jpeg, .jpg, .gif, .mp3, .mp4.
  * Aspect ratio: 16:9
 
Image Slideshow
  * You will have to install our <a href="https://github.com/CleverTap/CTNotificationContent">image carousel</a> and <a href="https://github.com/CleverTap/CTNotificationService">single media</a> libraries to enable sending slideshow media iOS notifications.
  * You can add up to 5 slides. The first image will be shown as a thumbnail in the default view. A slideshow of all the images will be shown in the expanded view. You will have to install our image carousel and single media libraries to enable sending slideshow media iOS notifications.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4a4fe52-ios1.png",
        "ios1.png",
        760,
        674,
        "#cccdd3"
      ]
    }
  ]
}
[/block]
You have 2 options to set the behavior of the notification after the user clicks on the notification. The notification can either be dismissed, or the notification will stay in the notification tray until explicitly swiped/dismissed.

**Sound File**
You will have to specify the name of the sound file which is included in your app bundle. Apple supports .aiff, .caf and .wav extensions. For more information on how you can generate such sound files, please read [this article](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/SupportingNotificationsinYourApp.html).

**Badge Count**
This updates the badge count for your app to the one specified while creating the campaign. Please note that this doesn’t increment the badge count or set the value to 0 (zero) to hide the badge number. That should be handled manually at the application level.

**Category**
You can input the name of the category while creating the campaign. Each category has to be registered with the app. Each such category is associated with actions that the user can perform when a notification of rich media type is delivered. Each category can have up to four actions associated with it, although the number of actions actually displayed depends on the type of notification delivered. This enables users to take multiple actions for the notification. For example, a single media push notification can have two buttons – ‘Buy Now’ and ‘Save for Later’.

**Deep Link/External URL**
Deep links allow you to land the user to a particular part of your app. Your app’s OpenURL method is called with the deep link specified here. If you wish to use external URLs, then you will have to whitelist the IPs or provide http/https before the URL so that they can be handled properly by the SDK.

**Mutable Content**
Please check this box if you would like to send the mutable-content flag along with your payload. This will invoke your app’s notification service extension. For more information click [here](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ModifyingNotifications.html).

**Content Available**
If you include the content-available key with a value of 1 to send out a silent notification to your users. It will not alert the user in any way (update badge count/play a sound/show a notification), but it will wake your app up in the background, allowing you to fetch new content and prepare it for the next time the user opens your app.
  * Key: content-available
  * Value: 1

##Push Notification Clicked
You can now raise and record push notifications clicked on your users’ iOS devices.
CleverTap SDK v3.8.1 and above supports raising Push Notification Clicked for push campaigns.
[block:code]
{
  "codes": [
    {
      "code": "[CleverTap sharedInstance] recordNotificationClickedEventWithData:response.notification.request.content.userInfo\n",
      "language": "objectivec"
    },
    {
      "code": "CleverTap.sharedInstance()?.recordNotificationClickedEvent(withData: response.notification.request.content.userInfo)\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "Use this API only if you wish to raise a Notification Clicked event if the carousel action button is tapped.",
  "title": "Using the Notification Clicked public API"
}
[/block]

[block:callout]
{
  "type": "danger",
  "body": "If the `Notification Clicked` method is not called correctly, it will create a new profile in CleverTap with only this event. This will create a dummy profile with no other event and will not be mapped to the profile from which the Notification Clicked event was raised.\n\nTo avoid the creation of a new user profile while calling the Notification Clicked method from your Application, you must pass the Identity/ Email to CleverTap in the Notification Content Class.",
  "title": "Dummy Profile Creation"
}
[/block]
# Setting Native Display in iOS

We support Native Display for CleverTap iOS SDK version 3.7.2 and above. Follow the steps to integrate Native Display with your App. 

##Step 1: Register the CleverTap Display Unit

Import the CleverTap Display Unit Header.


[block:code]
{
  "codes": [
    {
      "code": "#import <CleverTapSDK/CleverTap+DisplayUnit.h>\n",
      "language": "objectivec"
    },
    {
      "code": "import CleverTapSDK;\n",
      "language": "swift"
    }
  ]
}
[/block]
To use this, check that your class implements the CleverTapAdUnitDelegate
and set the setAdUnitDelegate to the class.

[block:code]
{
  "codes": [
    {
      "code": "\n#import \"ViewController.h\"\n#import <CleverTapSDK/CleverTap.h>\n#import <CleverTapSDK/CleverTap+DisplayUnit.h>\n\n@interface ViewController () <CleverTapDisplayUnitDelegate>\n\n@end\n\n@implementation ViewController\n\n- (void)viewDidLoad {\n    [super viewDidLoad];\n    ...\n    [[CleverTap sharedInstance] setDisplayUnitDelegate:self];\n}\n\n@end\n\n\n",
      "language": "objectivec"
    },
    {
      "code": "import UIKit\nimport CleverTapSDK\n\nclass ViewController: UIViewController, CleverTapDisplayUnitDelegate  {\n\n    override func viewDidLoad() {\n        super.viewDidLoad()\n        ...\n        CleverTap.sharedInstance()?.setDisplayUnitDelegate(self)\n    }\n}\n",
      "language": "swift"
    }
  ]
}
[/block]
##Step 2: Handle Native Display Data
Use the following method:
[block:code]
{
  "codes": [
    {
      "code": "- (void)displayUnitsUpdated:(NSArray<CleverTapDisplayUnit *>*_Nonnull)displayUnits {\n       // you will get display units here\n    NSArray <CleverTapDisplayUnit *> *units = displayUnits;\n}\n\n",
      "language": "objectivec"
    },
    {
      "code": "func displayUnitsUpdated(_ displayUnits: [CleverTapDisplayUnit]) {   \n       // you will get display units here\n    var units:[CleverTapDisplayUnit] = displayUnits;\n}\n\n",
      "language": "swift"
    }
  ]
}
[/block]
###Public APIs

Definition of classes for CleverTapDisplayUnit and CleverTapDisplayUnitContent.
[block:code]
{
  "codes": [
    {
      "code": "@interface CleverTapDisplayUnit : NSObject\n/*!\n* json defines the display unit data in the form of NSDictionary.\n*/\n@property (nullable, nonatomic, copy, readonly) NSDictionary *json;\n/*!\n* unitID defines the display unit identifier.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *unitID;\n/*!\n* type defines the display unit type.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *type;\n/*!\n* bgColor defines the backgroundColor of the display unit.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *bgColor;\n/*!\n* customExtras defines the extra data in the form of an NSDictionary. The extra key/value pairs set in the CleverTap dashboard.\n*/\n@property (nullable, nonatomic, copy, readonly) NSDictionary *customExtras;\n/*!\n* content defines the content of the display unit.\n*/\n@property (nullable, nonatomic, copy, readonly) NSArray<CleverTapDisplayUnitContent *> *contents;\n@end\n  \n  /*!\n@method\n@abstract\nThis method returns all the display units.\n*/\n- (NSArray<CleverTapDisplayUnit *>*_Nonnull)getAllDisplayUnits;\n \n /*!\n @method\n @abstract\n This method return display unit for the provided unitID\n */\n- (CleverTapDisplayUnit *_Nullable)getDisplayUnitForID:(NSString *_Nonnull)unitID;",
      "language": "objectivec",
      "name": "CleverTapDisplayUnit"
    },
    {
      "code": "@interface CleverTapDisplayUnitContent : NSObject\n/*!\n* title defines the title section of the display unit content.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *title;\n/*!\n* titleColor defines hex-code value of the title color as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *titleColor;\n/*!\n* message defines the message section of the display unit content.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *message;\n/*!\n* messageColor defines hex-code value of the message color as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *messageColor;\n/*!\n* videoPosterUrl defines video URL of the display unit as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *videoPosterUrl;\n/*!\n* actionUrl defines action URL of the display unit as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *actionUrl;\n/*!\n* mediaUrl defines media URL of the display unit as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *mediaUrl;\n/*!\n* iconUrl defines icon URL of the display unit as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *iconUrl;\n/*!\n* mediaIsAudio check whether mediaUrl is an audio.\n*/\n@property (nonatomic, readonly, assign) BOOL mediaIsAudio;\n/*!\n* mediaIsVideo check whether mediaUrl is a video.\n*/\n@property (nonatomic, readonly, assign) BOOL mediaIsVideo;\n/*!\n* mediaIsImage check whether mediaUrl is an image.\n*/\n@property (nonatomic, readonly, assign) BOOL mediaIsImage;\n/*!\n* mediaIsGif check whether mediaUrl is a gif.\n*/\n@property (nonatomic, readonly, assign) BOOL mediaIsGif;\n@end\n\n/*!\n@method\n@abstract\nThis method returns all the display units.\n*/\n- (NSArray<CleverTapDisplayUnit *>*_Nonnull)getAllDisplayUnits;\n \n /*!\n @method\n @abstract\n This method return display unit for the provided unitID\n */\n- (CleverTapDisplayUnit *_Nullable)getDisplayUnitForID:(NSString *_Nonnull)unitID;\n",
      "language": "objectivec",
      "name": "CleverTapDisplayUnitContent"
    }
  ]
}
[/block]
##Step 3:  Raise Notification Viewed and Notification Clicked event 

Raise the events for the Native Display using the following methods:
[block:code]
{
  "codes": [
    {
      "code": "\nCleverTapDisplayUnit *displayUnit;\n[[CleverTap sharedInstance]recordDisplayUnitViewedEventForID:displayUnit.unitID];\n\n\n\n\nCleverTapDisplayUnit *displayUnit;\n[[CleverTap sharedInstance]recordDisplayUnitClickedEventForID:displayUnit.unitID];\n",
      "language": "objectivec"
    },
    {
      "code": "\nCleverTap.sharedInstance()?.recordAdUnitViewedEvent(forID: displayUnit.unitID)\n\n\nCleverTap.sharedInstance()?.recordAdUnitViewedEvent(forID: displayUnit.unitID)\n",
      "language": "swift"
    }
  ]
}
[/block]

# Setting Account Credentials

If you do not wish to insert your account credentials in your app’s Info.plist, or you want to set your account ID programmatically, you can do so in your app delegate, in application:didFinishLaunchingWithOptions:
[block:code]
{
  "codes": [
    {
      "code": "- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {\n    [CleverTap setCredentialsWithAccountID:@\"Your account ID here\" andToken:@\"Your account token here\"];\n    [CleverTap autoIntegrate];\n    ...\n    return YES;\n}",
      "language": "objectivec",
      "name": "Objective-C"
    },
    {
      "code": "func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject:AnyObject]?) -> Bool {\n    CleverTap.setCredentialsWithAccountID(\"Your account ID here\", andToken: \"Your account token here\")\n    CleverTap.autoIntegrate()\n    ...\n    return true\n}",
      "language": "swift"
    }
  ]
}
[/block]
If you have set used this method to set your CleverTap Account ID and Token, please ensure that you do not make an entry for these values in your info.plist files.

# Checking Push Notifications from CleverTap

If you wish to determine whether a notification originated from CleverTap, call this method:
- (BOOL)isCleverTapNotification:(NSDictionary *)payload;

You must manually call the SDK as follows/ Add following CleverTap code to your AppDelegate.
[block:code]
{
  "codes": [
    {
      "code": "- (void) userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)(void))completionHandler {\n\n    /**\n     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.\n\n     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.\n     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.\n\n     If you do not implement this method, your app never responds to custom actions.\n\n     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter?language=objc\n     **/\n\n    if ([[CleverTap sharedInstance] isCleverTapNotification:response.notification.request.content.userInfo]) {\n\n       ...\n    }\n    completionHandler();\n}",
      "language": "objectivec"
    },
    {
      "code": "func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {\n        \n     /**\n     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.\n     \n     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.\n     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.\n     \n     If you do not implement this method, your app never responds to custom actions.\n     \n     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter\n     **/\n\n      print(\"APPDELEGATE: didReceiveResponseWithCompletionHandler \\(response.notification.request.content.userInfo)\")\n\n      // If you wish to determine whether a notification originated from CleverTap, you must manually call the SDK as follows:\n      if CleverTap.sharedInstance().isCleverTapNotification(response.notification.request.content.userInfo) {\n\n          ...\n      }\n } ",
      "language": "swift",
      "name": "Swift"
    }
  ]
}
[/block]