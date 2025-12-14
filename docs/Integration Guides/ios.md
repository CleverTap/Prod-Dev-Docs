---
title: iOS
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
This guide will show you how to how to track custom events, enrich user profiles, and send push notifications with the CleverTap SDK in your iOS app. 

If you haven’t already completed the [iOS quickstart guide](doc:ios-quickstart-guide), please complete that first before following this guide. The quickstart guide will show you how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard.

# User Profiles

CleverTap stores the user’s demographic data (gender, age, location), app and website interaction events, campaign visits and transaction history to give you a complete picture for every user.

A User Profile is automatically created for every user launching your mobile application – whether logged in or not.

Initially, the User Profile starts out as “Anonymous” – which means that the profile does not yet contain any identifiable information about the user. You can choose to enrich the profile with attributes like name, age, customer id, etc.

CleverTap provides pre-defined profile properties such as name, phone, gender, age etc to represent well know properties associated with a profile. It is strongly recommended to use these standard property names. A list of all pre-defined property names is available here. In addition, we also support arbitrary single and multi value profile properties.

Examples of updating profile properties for a project written in Objective-C are documented below.
[block:code]
{
  "codes": [
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nNSDateComponents *dob = [[NSDateComponents alloc] init];\ndob.day = 24;\ndob.month = 5;\ndob.year = 1992;\nNSDate *d = [[NSCalendar currentCalendar] dateFromComponents:dob];\nNSDictionary *profile = @{\n    @\"Name\": @\"Jack Montana\",               // String\n    @\"Identity\": @61026032,                 // String or number\n    @\"Email\": @\"jack@gmail.com\",            // Email address of the user\n    @\"Phone\": @\"+14155551234\",              // Phone (with the country code, starting with +)\n    @\"Gender\": @\"M\",                        // Can be either M or F\n    @\"Employed\": @\"Y\",                      // Can be either Y or N\n    @\"Education\": @\"Graduate\",              // Can be either Graduate, College or School\n    @\"Married\": @\"Y\",                       // Can be either Y or N\n    @\"DOB\": d,                              // Date of Birth. An NSDate object\n    @\"Age\": @28,                            // Not required if DOB is set\n    @\"Tz\": @\"Asia/Kolkata\",                 //an abbreviation such as \"PST\", a full name such as \"America/Los_Angeles\", \n                                            //or a custom ID such as \"GMT-8:00\"\n    @\"Photo\": @\"www.foobar.com/image.jpeg\", // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\n    @\"MSG-email\": @NO,                      // Disable email notifications\n    @\"MSG-push\": @YES,                      // Enable push notifications\n    @\"MSG-sms\": @NO                         // Disable SMS notifications\n};\n\n[[CleverTap sharedInstance] profilePush:profile];",
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
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nlet dob = NSDateComponents()\ndob.day = 24\ndob.month = 5\ndob.year = 1992\nlet d = NSCalendar.currentCalendar().dateFromComponents(dob)\nlet profile: Dictionary<String, AnyObject> = [\n    \"Name\": \"Jack Montana\",                 // String\n    \"Identity\": 61026032,                   // String or number\n    \"Email\": \"jack@gmail.com\",              // Email address of the user\n    \"Phone\": \"+14155551234\",                // Phone (with the country code, starting with +)\n    \"Gender\": \"M\",                          // Can be either M or F\n    \"Employed\": \"Y\",                        // Can be either Y or N\n    \"Education\": \"Graduate\",                // Can be either School, College or Graduate\n    \"Married\": \"Y\",                         // Can be either Y or N\n    \"DOB\": d!,                              // Date of Birth. An NSDate object\n    \"Age\": 28,                              // Not required if DOB is set\n    \"Tz\":\"Asia/Kolkata\",                    //an abbreviation such as \"PST\", a full name such as \"America/Los_Angeles\", \n                                            //or a custom ID such as \"GMT-8:00\"\n    \"Photo\": \"www.foobar.com/image.jpeg\",   // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\n    \"MSG-email\": false,                     // Disable email notifications\n    \"MSG-push\": true,                       // Enable push notifications\n    \"MSG-sms\": false                        // Disable SMS notifications\n]\n\nCleverTap.sharedInstance()?.profilePush(profile)",
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
CleverTap provides easy ways to enrich the user profile with data from sources like Facebook or Google Plus. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

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
For an example of how to record a custom event from a watchOS app, see here and here.

For an example of recording an event from an App Extension, see here.

Events help you understand how your users interact with your app. CleverTap tracks certain common User Events automatically, while giving you the flexibility to record business specific events.

# Push Notifications

If you automatically integrated the SDK in the quickstart guide, you have already enabled push notification support. If you did not, please follow the steps below. 

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
      "code": "- (void) application:(UIApplication *)application\n    didReceiveRemoteNotification:(NSDictionary *)userInfo {\n      [[CleverTap sharedInstance] handleNotificationWithData:userInfo];\n    }\n- (void) application:(UIApplication *)application\n    didReceiveLocalNotification:(UILocalNotification *)notification {\n      [[CleverTap sharedInstance] handleNotificationWithData:notification];\n    }\n    \n// As of iOS 8 and above\n- (void) application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier\n    forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {\n        [[CleverTap sharedInstance] handleNotificationWithData:notification];\n        if (completionHandler) completionHandler();\n    }\n    \n- (void) application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier\n    forRemoteNotification:(NSDictionary *)userInfo completionHandler:(void (^)())completionHandler {\n        [[CleverTap sharedInstance] handleNotificationWithData:userInfo];\n        if (completionHandler) completionHandler();\n}\n\n- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo \nfetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {\n\t[[CleverTap sharedInstance] handleNotificationWithData:userInfo];\n    completionHandler(UIBackgroundFetchResultNoData);\n}",
      "language": "objectivec"
    },
    {
      "code": "func application(application: UIApplication, didReceiveRemoteNotification\n    userInfo: [NSObject : AnyObject]) {\n    CleverTap.sharedInstance()?.handleNotificationWithData(userInfo)\n}\n\nfunc application(application: UIApplication, didReceiveLocalNotification\n    notification: UILocalNotification) {\n    CleverTap.sharedInstance()?.handleNotificationWithData(notification)\n}\n\n// As of iOS 8 and above\nfunc application(application: UIApplication, handleActionWithIdentifier identifier: String?,\n                 forLocalNotification notification: UILocalNotification, completionHandler: () -> Void) {\n    CleverTap.sharedInstance()?.handleNotificationWithData(notification)\n    completionHandler()\n}\n\nfunc application(application: UIApplication, handleActionWithIdentifier identifier: String?,\n                 forRemoteNotification userInfo: [NSObject : AnyObject], completionHandler: () -> Void) {\n    CleverTap.sharedInstance()?.handleNotificationWithData(userInfo)\n    completionHandler()\n}\n\nfunc application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], \nfetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {\n\tCleverTap.sharedInstance()?.handleNotification(withData: userInfo)\n    completionHandler(.noData)\n}",
      "language": "swift"
    }
  ]
}
[/block]
iOS 10 has introduced the UNUserNotificationCenterDelegate for handling notifications. Implementation is optional, but if you do implement the Delegate, please note that, whether or not you have used automatic integration, if you wish CleverTap to track notification opens and fire attached deep links, you must manually call the SDK as follows.
[block:code]
{
  "codes": [
    {
      "code": "- (void)userNotificationCenter:(UNUserNotificationCenter *)center\ndidReceiveNotificationResponse:(UNNotificationResponse *)response\n         withCompletionHandler:(void (^)())completionHandler {\n    \n    /**\n     Use this method to perform the tasks associated with your app’s custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.\n     \n     You specify your app’s notification types and custom actions using UNNotificationCategory and UNNotificationAction objects. You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.\n     \n     If you do not implement this method, your app never responds to custom actions.\n     \n     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter?language=objc\n     \n    **/\n    \n    // if you wish CleverTap to record the notification open and fire any deep links contained in the payload\n    [[CleverTap sharedInstance] handleNotificationWithData:response.notification.request.content.userInfo];\n    \n    completionHandler();\n}",
      "language": "objectivec"
    },
    {
      "code": "func userNotificationCenter(_ center: UNUserNotificationCenter,\n                                didReceive response: UNNotificationResponse,\n                                withCompletionHandler completionHandler: @escaping () -> Void) {\n        \n\t/**\n\t Use this method to perform the tasks associated with your app’s custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.\n\t \n\t You specify your app’s notification types and custom actions using UNNotificationCategory and UNNotificationAction objects. You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.\n\t \n\t If you do not implement this method, your app never responds to custom actions.\n\t \n\t see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter\n\t \n\t **/\n\t\n\t// if you wish CleverTap to record the notification open and fire any deep links contained in the payload\n\tCleverTap.sharedInstance().handleNotification(withData: response.notification.request.content.userInfo)\n\t\n\tcompletionHandler()\n        \n}",
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
      "code": "- (void) userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)(void))completionHandler {\n    \n    /**\n     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.\n     \n     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.\n     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.\n     \n     If you do not implement this method, your app never responds to custom actions.\n     \n     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter?language=objc\n     **/\n\n    // if you wish CleverTap to record the notification open and fire any deep links contained in the payload\n    [[CleverTap sharedInstance]handleNotificationWithData:response.notification.request.content.userInfo openDeepLinksInForeground: YES];\n    \n    completionHandler();\n}",
      "language": "objectivec"
    },
    {
      "code": "func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {\n\n       /**\n     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.\n     \n     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.\n     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.\n     \n     If you do not implement this method, your app never responds to custom actions.\n     \n     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter\n     **/\n\n    print(\"APPDELEGATE: didReceiveResponseWithCompletionHandler \\(response.notification.request.content.userInfo)\")\n\n    // if you wish CleverTap to record the notification click and fire any deep links contained in the payload\n    CleverTap.sharedInstance().handleNotification(withData: response.notification.request.content.userInfo, openDeepLinksInForeground: true)\n\n    completionHandler()\n} ",
      "language": "swift"
    }
  ]
}
[/block]
# In-App Notifications

The CleverTap SDK allows you to show In-App notifications to your users. You can design in-app notifications right from the dashboard, without writing a single line of code. There is no code required for this feature

Note: not applicable inside App Extensions and watchOS apps.

**Manually Enable Support for Universal (Deep) Link Tracking**
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

# Changing Account Credentials

If you’d not like to insert your account credentials in your app’s Info.plist, or would like to change your account ID programmatically, you can do so in your app delegate, in application:didFinishLaunchingWithOptions:
[block:code]
{
  "codes": [
    {
      "code": "- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {\n    [CleverTap changeCredentialsWithAccountID:@\"Your account ID here\" andToken:@\"Your account token here\"];\n    [CleverTap autoIntegrate];\n    ...\n    return YES;\n}",
      "language": "text",
      "name": "Objective-C"
    },
    {
      "code": "func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject:AnyObject]?) -> Bool {\n    CleverTap.changeCredentialsWithAccountID(\"Your account ID here\", andToken: \"Your account token here\")\n    CleverTap.autoIntegrate()\n    ...\n    return true\n}",
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