---
title: iOS SDK
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

This section shows you how to how to track custom events, enrich user profiles, and send push notifications with the CleverTap SDK in your iOS app. If you have not already completed the [iOS SDK Quick Start Guide](doc:ios-quickstart-guide), complete that first before following the steps in this section. The quick start guide shows you how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard.

# User Profiles

CleverTap stores the user’s demographic data (gender, age, location), app and website interaction events, campaign visits, and transaction history to give you a complete picture of every user.

A user profile is automatically created for every user launching your mobile application – whether logged in or not.

Initially, the user profile starts out as “Anonymous” – which means that the profile does not yet contain any identifiable information about the user. You can choose to enrich the profile with attributes such as name, age, and customer id.

CleverTap provides pre-defined profile properties such as name, phone, gender, age and so on to represent well know properties associated with a profile. It is strongly recommended to use these standard property names. A list of all pre-defined property names is available here. In addition, we also support arbitrary single and multi-value profile properties.

Examples of updating profile properties for a project written in Objective-C are documented below.

```objectivec Predefined
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
NSDateComponents *dob = [[NSDateComponents alloc] init];
dob.day = 24;
dob.month = 5;
dob.year = 1992;
NSDate *d = [[NSCalendar currentCalendar] dateFromComponents:dob];
NSDictionary *profile = @{
    @"Name": @"Jack Montana",               // String
    @"Identity": @61026032,                 // String or number
    @"Email": @"jack@gmail.com",            // Email address of the user
    @"Phone": @"+14155551234",              // Phone (with the country code, starting with +)
    @"Gender": @"M",                        // Can be either M or F
    @"Employed": @"Y",                      // Can be either Y or N
    @"Education": @"Graduate",              // Can be either Graduate, College or School
    @"Married": @"Y",                       // Can be either Y or N
    @"DOB": d,                              // Date of Birth. An NSDate object
    @"Age": @28,                            // Not required if DOB is set
    @"Tz": @"Asia/Kolkata",                 //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                            //or a custom ID such as "GMT-8:00"
    @"Photo": @"www.foobar.com/image.jpeg", // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
    @"MSG-email": @NO,                      // Disable email notifications
    @"MSG-push": @YES,                      // Enable push notifications
    @"MSG-sms": @NO                         // Disable SMS notifications
    @"MSG-dndPhone": @YES,                  // Opt out phone number from SMS                                                    notifications
    @"MSG-dndEmail": @YES,                  // Opt out email from email                                                    notifications
};

[[CleverTap sharedInstance] profilePush:profile];
```
```objectivec Arbitrary Single Value
NSDictionary *profile = @{
    @"Customer Type": @"Silver",
    @"Prefered Language": @"English",
};

[[CleverTap sharedInstance] profilePush:profile];

/**
 * Data types:
 * The value of a property can be of type NSDate, a NSNumber, a NSString, or a BOOL.
 */
```
```objectivec Arbitrary Multi Value
// To set a multi-value property
[[CleverTap sharedInstance] profileSetMultiValues:@[@"bag", @"shoes"] forKey:@"myStuff"];

// To add an additional value(s) to a multi-value property
[[CleverTap sharedInstance] profileAddMultiValue:@"coat" forKey:@"myStuff"];
// or
[[CleverTap sharedInstance] profileAddMultiValues:@[@"socks", @"scarf"] forKey:@"myStuff"];

//To remove a value(s) from a multi-value property
[[CleverTap sharedInstance] profileRemoveMultiValue:@"bag" forKey:@"myStuff"];
[[CleverTap sharedInstance] profileRemoveMultiValues:@[@"shoes", @"coat"] forKey:@"myStuff"];

//To remove the value of a property (scalar or multi-value)
[[CleverTap sharedInstance] profileRemoveValueForKey:@"myStuff"];
```

Examples of updating profile properties for a project written in Swift are documented below.

```swift Predefined
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
let dob = NSDateComponents()
dob.day = 24
dob.month = 5
dob.year = 1992
let d = NSCalendar.currentCalendar().dateFromComponents(dob)
let profile: Dictionary<String, AnyObject> = [
    "Name": "Jack Montana",                 // String
    "Identity": 61026032,                   // String or number
    "Email": "jack@gmail.com",              // Email address of the user
    "Phone": "+14155551234",                // Phone (with the country code, starting with +)
    "Gender": "M",                          // Can be either M or F
    "Employed": "Y",                        // Can be either Y or N
    "Education": "Graduate",                // Can be either School, College or Graduate
    "Married": "Y",                         // Can be either Y or N
    "DOB": d!,                              // Date of Birth. An NSDate object
    "Age": 28,                              // Not required if DOB is set
    "Tz":"Asia/Kolkata",                    //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                            //or a custom ID such as "GMT-8:00"
    "Photo": "www.foobar.com/image.jpeg",   // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
    "MSG-email": false,                     // Disable email notifications
    "MSG-push": true,                       // Enable push notifications
    "MSG-sms": false                        // Disable SMS notifications
    "MSG-dndPhone": true,                  // Opt out phone number from SMS                                                    notifications
    "MSG-dndEmail": @YES,                  // Opt out email from email                                                    notifications
]

CleverTap.sharedInstance()?.profilePush(profile)
```
```swift Arbitrary Single Value
let profile: Dictionary<String, AnyObject> = [
    "Customer Type": "Silver",
    "Prefered Language": "English"
]

CleverTap.sharedInstance()?.profilePush(profile)

/**
 * Data types:
 * The value of a property can be of type NSDate, a Number, a String, or a Bool.
 */
```
```swift Arbitrary Multi Value
// To set a multi-value property
CleverTap.sharedInstance()?.profileSetMultiValues(["bag", "shoes"], forKey: "myStuff")

// To add an additional value(s) to a multi-value property
CleverTap.sharedInstance()?.profileAddMultiValue("coat", forKey: "myStuff")
// or
CleverTap.sharedInstance()?.profileAddMultiValues(["socks", "scarf"], forKey: "myStuff")

//To remove a value(s) from a multi-value property
CleverTap.sharedInstance()?.profileRemoveMultiValue("bag", forKey: "myStuff")
CleverTap.sharedInstance()?.profileRemoveMultiValues(["shoes", "coat"], forKey: "myStuff")

//To remove the value of a property (scalar or multi-value)
CleverTap.sharedInstance()?.profileRemoveValueForKey("myStuff")
```

CleverTap provides easy ways to enrich the user profile with data from sources, such as Facebook. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

## DND

### Phone

You can opt-out a user or a phone number. 

* To opt-out a specific user when multiple users share the same phone number,  you can disable SMS for a specific user. Set the MSG-sms flag to false and the specified user stops receiving SMS. However, all other users sharing the number continue to receive messages if they have not opted out.  

* To opt-out a phone number and all the users associated with it, set the MSG-dndPhone flag to true.

### Email

You can opt-out a user or an email address.  

* To opt-out a specific user when multiple users share the same email address,  you can disable email for a specific user. Set the MSG-email flag to false and the specified user stops receiving email. However, all other users sharing the email address continue to receive messages if they have not opted out.  

* To opt-out an email and all the users associated with it, set the MSG-dndEmail flag to true.

# User Events

A User Event is an event that a user takes in your mobile application. CleverTap records the event on the User Profile, using an Event Name and optional associated key:value-based Event Properties. You can then segment users, target and personalize messaging based on both the Event Name and specific Event Properties.

An example of recording a User Event called Product Viewed.

```objectivec
// event without properties
[[CleverTap sharedInstance] recordEvent:@"Product viewed"];
```
```swift
// event without properties
CleverTap.sharedInstance()?.recordEvent("Product viewed")
```

An example of recording a User Event called Product Viewed with Properties.

```objectivec
// event with properties
NSDictionary *props = @{
    @"Product name": @"Casio Chronograph Watch",
    @"Category": @"Mens Accessories",
    @"Price": @59.99,
    @"Date": [NSDate date]
};

[[CleverTap sharedInstance] recordEvent:@"Product viewed" withProps:props];

/**
 * Data types:
 * The value of a property can be of type NSDate, a NSNumber, a NSString, or a BOOL.
 *
 * NSDate object:
 * When a property value is of type NSDate, the date and time are both recorded to the second.
 * This can be later used for targeting scenarios.
 * For e.g. if you are recording the time of the flight as an event property,
 * you can send a message to the user just before their flight takes off.
 */
```
```swift
// event with properties
let props = [
    "Product name": "Casio Chronograph Watch",
    "Category": "Mens Accessories",
    "Price": 59.99,
    "Date": NSDate()
]

CleverTap.sharedInstance()?.recordEvent("Product viewed", withProps: props)

/**
 * Data types:
 * The value of a property can be of type NSDate, a Number, a String, or a Bool.
 *
 * NSDate object:
 * When a property value is of type NSDate, the date and time are both recorded to the second.
 * This can be later used for targeting scenarios.
 * For e.g. if you are recording the time of the flight as an event property,
 * you can send a message to the user just before their flight takes off.
 */
```

For an example of how to record a custom event from a watchOS app, see [here](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftStarter/SwiftWatchOS%20Extension/InterfaceController.swift).

For an example of recording an event from an App Extension, see [here](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftStarter/NotificationService/NotificationService.swift).

Events help you understand how your users interact with your app. CleverTap tracks certain common User Events automatically, while giving you the flexibility to record business specific events.

# Push Notifications

If you automatically integrated the SDK in the quick start guide, you have already enabled push notification support. If you have not yet, follow the steps below. 

CleverTap using the SDK is capable of sending push notifications to your users using the dashboard. To enable this support, when your app delegate receives the application:didRegisterForRemoteNotificationsWithDeviceToken: message, include a call to CleverTap setPushToken: as follows.

```objectivec
- (void) application:(UIApplication *)application
    didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
      [[CleverTap sharedInstance] setPushToken:deviceToken];
}
```
```swift
func application(application: UIApplication,
                 didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData) {
    CleverTap.sharedInstance()?.setPushToken(deviceToken)
}
```

The above will save the user’s APNS token with CleverTap. This token is used to send push notifications.

Next, include a call to handleNotificationWithData: when your app delegate is sent the following messages:

* application:didReceiveRemoteNotification:
* application:didReceiveLocalNotification:
* application:didReceiveRemoteNotification:fetchCompletionHandler:
* application:handleActionWithIdentifier:forRemoteNotification:completionHandler:
* application:handleActionWithIdentifier:forLocalNotification:completionHandler: 

```objectivec
// iOS 8 and below
- (void) application:(UIApplication *)application
    didReceiveRemoteNotification:(NSDictionary *)userInfo {
      [[CleverTap sharedInstance] handleNotificationWithData:userInfo];
    }
- (void) application:(UIApplication *)application
    didReceiveLocalNotification:(UILocalNotification *)notification {
      [[CleverTap sharedInstance] handleNotificationWithData:notification];
    }
    
// For iOS 8 and iOS 9
- (void) application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier
    forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
        [[CleverTap sharedInstance] handleNotificationWithData:notification];
        if (completionHandler) completionHandler();
    }
    
- (void) application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier
    forRemoteNotification:(NSDictionary *)userInfo completionHandler:(void (^)())completionHandler {
        [[CleverTap sharedInstance] handleNotificationWithData:userInfo];
        if (completionHandler) completionHandler();
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo 
fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {
	[[CleverTap sharedInstance] handleNotificationWithData:userInfo];
    completionHandler(UIBackgroundFetchResultNoData);
}
```
```swift
func application(application: UIApplication, didReceiveRemoteNotification
    userInfo: [NSObject : AnyObject]) {
    CleverTap.sharedInstance()?.handleNotificationWithData(userInfo)
}

func application(application: UIApplication, didReceiveLocalNotification
    notification: UILocalNotification) {
    CleverTap.sharedInstance()?.handleNotificationWithData(notification)
}

// As of iOS 8 and above
func application(application: UIApplication, handleActionWithIdentifier identifier: String?,
                 forLocalNotification notification: UILocalNotification, completionHandler: () -> Void) {
    CleverTap.sharedInstance()?.handleNotificationWithData(notification)
    completionHandler()
}

func application(application: UIApplication, handleActionWithIdentifier identifier: String?,
                 forRemoteNotification userInfo: [NSObject : AnyObject], completionHandler: () -> Void) {
    CleverTap.sharedInstance()?.handleNotificationWithData(userInfo)
    completionHandler()
}

func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], 
fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
	CleverTap.sharedInstance()?.handleNotification(withData: userInfo)
    completionHandler(.noData)
}
```

**Required for CleverTap iOS SDK v.3.1.7 and below and Manual Integration:**

iOS 10 has introduced the UNUserNotificationCenterDelegate for handling notifications. Implementation is optional, but if you do implement the Delegate, please note that, whether or not you have used automatic integration, if you wish CleverTap to track notification opens and fire attached deep links, you must manually call the SDK as follows.

```objectivec
/** For iOS 10 and above - Background **/
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response
         withCompletionHandler:(void (^)())completionHandler {
    
    /** For iOS 10 and above.
     Use this method to perform the tasks associated with your app’s custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.
     
     You specify your app’s notification types and custom actions using UNNotificationCategory and UNNotificationAction objects. You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.
     
     If you do not implement this method, your app never responds to custom actions.
     
     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter?language=objc
     
    **/
    
    // Skip this line if you have opted for auto-integrate. If you wish CleverTap to record the notification open and fire any deep links contained in the payload.  
    [[CleverTap sharedInstance] handleNotificationWithData:response.notification.request.content.userInfo];
    
    completionHandler();
}
```
```swift
/** For iOS 10 and above - Background **/
func userNotificationCenter(_ center: UNUserNotificationCenter,
                                didReceive response: UNNotificationResponse,
                                withCompletionHandler completionHandler: @escaping () -> Void) {
        
	/**
	 Use this method to perform the tasks associated with your app’s custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.
	 
	 You specify your app’s notification types and custom actions using UNNotificationCategory and UNNotificationAction objects. You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.
	 
	 If you do not implement this method, your app never responds to custom actions.
	 
	 see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter
	 
	 **/
	
	// if you wish CleverTap to record the notification open and fire any deep links contained in the payload. Skip this line if you have opted for auto-integrate. 
	CleverTap.sharedInstance().handleNotification(withData: response.notification.request.content.userInfo)
	
	completionHandler()
        
}
```

When your app is not running or is in the background, the above method will track notification opens and fire attached deep links. But if you want CleverTap to continue to fire attached deep links when in the foreground, you must manually call the SDK as follows.

```objectivec
/** For iOS 10 and above - Foreground **/
- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler{
 
    
   /**
     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.
     
     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.
     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.
     
     If you do not implement this method, your app never responds to custom actions.
     
     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter?language=objc
     **/

    // if you wish CleverTap to record the notification open and fire any deep links contained in the payload
     [[CleverTap sharedInstance]handleNotificationWithData:notification.request.content.userInfo openDeepLinksInForeground: YES];
     completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}
```
```swift
/** For iOS 10 and above - Foreground**/

func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: (UNNotificationPresentationOptions) -> Void) {

       /**
     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.
     
     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.
     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.
     
     If you do not implement this method, your app never responds to custom actions.
     
     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter
     **/

    print("APPDELEGATE: didReceiveResponseWithCompletionHandler \(response.notification.request.content.userInfo)")

    // If you wish CleverTap to record the notification click and fire any deep links contained in the payload. 
  
     CleverTap.sharedInstance()?.handleNotification(withData: notification.request.content.userInfo, openDeepLinksInForeground: true)
     completionHandler([.badge, .sound, .alert])
}
```

## Push Notification Callback

To get the callback on the click of the Push Notifications, check  that your class implements the `CleverTapPushNotificationDelegate` delegate and then use the following method to handle the user-selected actions from Push notifications:

```objectivec
- (void)pushNotificationTappedWithCustomExtras:(NSDictionary *)customExtras {
    
      NSLog(@"Push Notification Tapped with Custom Extras: %@", customExtras);
      ...
 }
```
```swift
func pushNotificationTapped(withCustomExtras customExtras: [AnyHashable : Any]!) {
      print("Push Notification Tapped with Custom Extras: \(customExtras)")
      ...
}
```

## FAQs - Push Notification Errors in iOS

 \###1. APNS Device Token Does Not Match The Specified Topic 

Apple Push Notification service (APNs) returns this error when the device token doesn't match the specified topic.

In the context of CleverTap, you will get this error when you try to send the notification with a wrong certificate. Check that you use a production certificate for the production environment. This error is caused because of incorrect configurations. 

Check the provisioning profile used to deploy the app and send the device token to the CleverTap dashboard. To avoid this error, the "App bundle ID" of the provisioning profile and the CleverTap push certificate (p12 certificate) must match.

### 2. APNS Unregistered

The Push notification delivery to the target fails because the device token is inactive for the specified topic. If the device token is not valid/inactive you may have to wait till the user installs the app again. You can then start sending notifications. 

### 3. APNS Topic Disallowed

The topic is currently the bundle identifier of the target application on an iOS device. The "APNS Topic Disallowed" error appears when you specify the incorrect "App Bundle ID" in the Account Settings. For more information, see [Apple documentation](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CommunicatingwithAPNs.html).

To resolve this error, check the value of the "App Bundle ID" and "APNs push mode" that you have configured in the CleverTap Dashboard under Push Settings.

### 4. APNS Temporarily Blacklisted

You are temporarily blacklisted from sending push notifications to the device, which can be due to sending bad tokens. The push notification will not be delivered for some duration.

## 5. APNS Failed Delivery 

The notification to the device sent from CleverTap failed to deliver due to either of the following reasons

* The app was uninstalled and the token is no longer valid.
* You have sent too many notifications in short duration to the same device.

If the user has uninstalled the app then you may have to wait until the user installs the app again before the user can receive your notifications.\
For more info about APNs, see [Handling Notification Responses from APNs](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/handling_notification_responses_from_apns).

### 6. APNs Bad Device Token

The push notification delivery to the target device has failed due to an invalid token. The APNs bad device token error occurs when there is a mismatch between the APNs push mode on your app and the CleverTap dashboard.\
For example, you have set the APNs push mode in your app to the "development" stage and in the "CleverTap" dashboard you have set up to "production" stage. To check your CleverTap dashboard APNs push mode: Navigate to Settings > Engage > Mobile Push> iOS.

<Image title="Push_Notification_iOS_API_Key.png" alt={1166} className="border" border={true} src="https://files.readme.io/20ff216-Push_Notification_iOS_API_Key.png" />

# Push Impressions

You can now raise and record push notifications delivered onto your users’ iOS devices.\
CleverTap SDK v3.5.0 and above supports raising Push Impressions for push campaigns.\
To raise Push Impressions for iOS, the [Notification Service Extension](https://developer.apple.com/reference/usernotifications/unnotificationserviceextension) must be included.\
Notification Service Extension is a separate and distinct binary embedded in your app bundle. Prior to displaying a new push notification, the system will call your Notification Service Extension to allow you to call the Notification Viewed method for your Application.

> 🚧 Note
>
> Push Impressions are not raised if the Notification Service Extension is not included in your app bundle.

```c Objective C
- (void)didReceiveNotificationRequest:(UNNotificationRequest *)request withContentHandler:(void (^)(UNNotificationContent * _Nonnull))contentHandler {
    
    self.contentHandler = contentHandler;
    self.bestAttemptContent = [request.content mutableCopy];

// While running the Application add CleverTap Account ID and Account token in your .plist file    

// call to record the Notification viewed 
    [[CleverTap sharedInstance] recordNotificationViewedEventWithData:request.content.userInfo];
    [super didReceiveNotificationRequest:request withContentHandler:contentHandler];
    
}
```
```swift Swift
override func didReceive(_ request: UNNotificationRequest, withContentHandler contentHandler: @escaping (UNNotificationContent) -> Void) {
        // While running the Application add CleverTap Account ID and Account token in your .plist file
        
        // call to record the Notification viewed
        CleverTap.sharedInstance()?.recordNotificationViewedEvent(withData: request.content.userInfo)
        super.didReceive(request, withContentHandler: contentHandler)
    }
```

> ❗️ Dummy Profile Creation
>
> If the `Notification viewed` method is not called correctly, it will create a new profile in CleverTap with only this event. This will create a dummy profile with no other event and will not be mapped to the profile from which the Push Impressions event was raised.
>
> To avoid the creation of a new user profile while calling the Notification Viewed method from your Application, you must pass the Identity/ Email to CleverTap in the Notification Service Class or you can call recordNotificationViewedEvent API from your Main Application.

Sending notifications via APNS

* Include the mutable-content flag in your apns payload (this key must be present in the apns payload or the system will not call your app extension)

# In-App Notifications

The CleverTap SDK allows you to show In-App notifications to your users. You can design in-app notifications right from the dashboard, without writing a single line of code. There is no code required for this feature

Note: not applicable inside App Extensions and watchOS apps.

**Javascript support in In-App Notifications**\
CleverTap iOS SDK v3.5.0 and above supports embedding Javascript code inside Custom In-Apps. To make sure your Javascript code works on the app, while creating the InApp campaign, select the checkbox for Enabling javascript during the In-App campaign creation

All methods explained with examples below

```javascript
//Recording a User Event called Product Viewed in JS enabled custom in-apps.
var props = {foo: 'xyz', lang: 'French'};
var message = { action:'recordEventWithProps', event:'Product Viewed', props: props};

if(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){
 // Call iOS interface                  
   window.webkit.messageHandlers.clevertap.postMessage(message);         
} 

//Updating profile properties of the User in JS enabled custom in-apps.
var props = {foo: 'xyz', lang: 'French'};
var message = { action:'profilePush', properties: props};

if(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){
 // Call iOS interface
  window.webkit.messageHandlers.clevertap.postMessage(message);        
}

//Setting a user profile
var message = {
                action: 'profileSetMultiValues',
                values: ['bag', 'kitkat'],
                key: 'myStuff'
            };
            
if(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){
    window.webkit.messageHandlers.clevertap.postMessage(message);
}


//Add multi value
var message = {action: 'profileAddMultiValue',
             value: 'coat', 
             key: 'myStuff'};

if(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){
    window.webkit.messageHandlers.clevertap.postMessage(message);
}


//Add multi values
 var message = {action: 'profileAddMultiValues',
                values: ['bag', 'kitkat', 'Wine'],
                key: 'myStuff'};

if(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){
    window.webkit.messageHandlers.clevertap.postMessage(message);
}


//Remove value for key

var message = {action: 'profileRemoveValueForKey',
                    key: 'myStuff' };

if(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){
    window.webkit.messageHandlers.clevertap.postMessage(message);
}


//Remove multi values for key
 var message = {action: 'profileRemoveMultiValues',
                values: ['scarf', 'knife'],
                key: 'myStuff'};

if(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){
    window.webkit.messageHandlers.clevertap.postMessage(message);
}


//Remove multi Value
var message = {action: 'profileRemoveMultiValue',
                value: 'bag',
                key: 'myStuff'};

if(window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap){
    window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Record Charged Event
const chargeDetails = {
                "Amount" : 300,
                "Payment mode": "Credit Card",
                "Charged ID": 24052013
            };
const items = [{
                "Category": "books",
                "Book name": "The Millionaire next door",
                "Quantity": 1
            },{
                "Category": "books",
                "Book name": "The Millionaire previous door",
                "Quantity": 5
            }];
const message = {
                action:'recordChargedEvent',
                chargeDetails: chargeDetails,
                items: items
            };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
	window.webkit.messageHandlers.clevertap.postMessage(message);
}
```

> 📘 Inline Media
>
> To add inline media support in custom HTML for in-app notifications, add the key playsinline with value as 1 or true as a query parameter in your media URL For example, [https://www.youtube.com/watch?v=RUvrypIAPbg?playsinline=1](https://www.youtube.com/watch?v=RUvrypIAPbg?playsinline=1). This functionality is available for CleverTap iOS SDK 3.7.2 and above.

### **InApp Notification Button onClick Callbacks**

iOS SDK v3.7.1 and above supports callback on the click of InApp Notification Buttons by returning a Map of Key-Value pairs. To use this, make sure your class implements the CleverTapInAppNotificationDelegate and use the following method to handle user-selected actions from InApp notifications:

```objectivec
- (void)inAppNotificationButtonTappedWithCustomExtras:(NSDictionary *)customExtras {
     NSLog(@"inAppNotificationButtonTappedWithCustomExtras:%@", customExtras);
  ...
}
```
```swift
func inAppNotificationButtonTapped(withCustomExtras customExtras: [AnyHashable : Any]!) {
      print("In-App Button Tapped with custom extras:", customExtras ?? "");
  }
```

# **Manually Enable Support for Universal (Deep) Link Tracking**

Deep links are a way of launching a native app and providing additional information telling it do some specific event or show specific content. CleverTap automatically tracks universal links that open your application. If you have universal (deep) links coming to your app, you can capture the incoming UTM parameters easily. Call handleOpenURL:sourceApplication: when the application:openURL:sourceApplication:annotation: message is sent to your app delegate.

```objectivec
- (BOOL) application:(UIApplication *)application
    openURL:(NSURL *)url
    sourceApplication:(NSString *)sourceApplication
    annotation:(id)annotation {
      [[CleverTap sharedInstance] handleOpenURL:url sourceApplication:sourceApplication];
      return YES;
}

- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary *)options {
    [[CleverTap sharedInstance] handleOpenURL:url sourceApplication:nil];
    return YES;
}

- (void)openURL:(NSURL*)url options:(NSDictionary<NSString *, id> *)options
completionHandler:(void (^ __nullable)(BOOL success))completion {
    [[CleverTap sharedInstance] handleOpenURL:url sourceApplication:nil];
    if (completion) {
        completion(YES);
    }
}
```
```swift
func application(application: UIApplication, openURL url: NSURL,
                 sourceApplication: String?, annotation: AnyObject) -> Bool {
    CleverTap.sharedInstance()?.handleOpenURL(url, sourceApplication: sourceApplication)
    return true
}

// Swift 3
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
   CleverTap.sharedInstance()?.handleOpen(url, sourceApplication: nil)
   return true
}

func open(_ url: URL, options: [String : Any] = [:],
                   completionHandler completion: ((Bool) -> Swift.Void)? = nil) {
	CleverTap.sharedInstance()?.handleOpen(url, sourceApplication: nil)
	completion?(false)
}
```

# Send Location Information to CleverTap

```objectivec
- (void)locationManager:(CLLocationManager *)manager 
    didUpdateToLocation:(CLLocation *)newLocation 
           fromLocation:(CLLocation *)oldLocation {
    [CleverTap setLocation: newLocation.coordinate];
}
```
```swift
func locationManager(manager: CLLocationManager, didUpdateToLocation newLocation: CLLocation,
                     fromLocation oldLocation: CLLocation) {
    CleverTap.setLocation(newLocation.coordinate)
}
```

# App Inbox

* The CleverTap SDK 3.4.0 and above, allows you to create App Inbox notifications for your users. 
* You can use the App Inbox provided by CleverTap or create your own
* You can design App Inbox notifications right from the dashboard

Note: not applicable for tvOS and watchOS apps.

## Step 1: Integrating the CleverTap App Inbox

* You have to import the CleverTap Inbox Header
* Initialize the CleverTap App Inbox Method

```objectivec Objective C
#import <CleverTapSDK/CleverTap+Inbox.h> 

//Initialize the CleverTap App Inbox
    [[CleverTap sharedInstance] initializeInboxWithCallback:^(BOOL success) {
        int messageCount = (int)[[CleverTap sharedInstance] getInboxMessageCount];
        int unreadCount = (int)[[CleverTap sharedInstance] getInboxMessageUnreadCount];
        NSLog(@"Inbox Message: %d/%d", messageCount, unreadCount);
    }];
```
```swift Swift
import CleverTapSDK

//Initialize the CleverTap App Inbox
CleverTap.sharedInstance()?.initializeInbox(callback: ({ (success) in
        let messageCount = CleverTap.sharedInstance()?.getInboxMessageCount()
        let unreadCount = CleverTap.sharedInstance()?.getInboxMessageUnreadCount()
        print("Inbox Message:\(String(describing: messageCount))/\(String(describing: unreadCount)) unread")
 }))
```

## Step 2: Configure App Inbox Styling

* By default, App Inbox styling will be CleverTap's default colors and font
* You can choose to change the styling as shown below

```objectivec
// config the style of App Inbox Controller
CleverTapInboxStyleConfig *style = [[CleverTapInboxStyleConfig alloc] init];
style.backgroundColor = [UIColor blueColor];
style.messageTags = @[@"tag1", @"tag2"];
style.navigationBarTintColor = [UIColor blueColor];
style.navigationTintColor = [UIColor blueColor];
style.tabUnSelectedTextColor = [UIColor blueColor];
style.tabSelectedTextColor = [UIColor blueColor];
style.tabSelectedBgColor = [UIColor blueColor];
style.firstTabTitle = @"My First Tab";

CleverTapInboxViewController *inboxController = [[CleverTap sharedInstance] 
newInboxViewControllerWithConfig:style andDelegate:self];
    if (inboxController) {
        UINavigationController *navigationController = [[UINavigationController alloc] initWithRootViewController:inboxController];
        [self presentViewController:navigationController animated:YES completion:nil];
    }
```
```swift
// config the style of App Inbox Controller
    let style = CleverTapInboxStyleConfig.init()
    style.title = "App Inbox"
    style.backgroundColor = UIColor.blue()
    style.messageTags = ["tag1", "tag2"]
    style.navigationBarTintColor = UIColor.blue()
    style.navigationTintColor = UIColor.blue()
    style.tabUnSelectedTextColor = UIColor.blue()
    style.tabSelectedTextColor = UIColor.blue()
    style.tabSelectedBgColor = UIColor.blue()
    style.firstTabTitle = "My First Tab"
    
    if let inboxController = CleverTap.sharedInstance()?.newInboxViewController(with: style, andDelegate: self) {
        let navigationController = UINavigationController.init(rootViewController: inboxController)
        self.present(navigationController, animated: true, completion: nil)
  }
```

## Step 3: Creating your own App Inbox

* You can choose to create your own App Inbox by calling the following APIs

```objectivec Public APIs
//Initialize App Inbox
- (void)initializeInboxWithCallback:(CleverTapInboxSuccessBlock _Nonnull)callback;

//Get Inbox Message Count
- (NSUInteger)getInboxMessageCount;

//Get Inbox Unread Count
- (NSUInteger)getInboxMessageUnreadCount;

//Get All messages 
- (NSArray<CleverTapInboxMessage *> * _Nonnull )getAllInboxMessages;

//Get only Unread messages
- (NSArray<CleverTapInboxMessage *> * _Nonnull )getUnreadInboxMessages;

//Get message object belonging to the given message id only. Message id must be a String
- (CleverTapInboxMessage * _Nullable )getInboxMessageForId:(NSString *)messageId;

//Delete message from the Inbox. Message id must be a String
- (void)deleteInboxMessage:(CleverTapInboxMessage * _Nonnull )message;

//Mark Message as Read
- (void)markReadInboxMessage:(CleverTapInboxMessage * _Nonnull) message;

//Callback on Inbox Message update/delete/read (any activity)
- (void)registerInboxUpdatedBlock:(CleverTapInboxUpdatedBlock _Nonnull)block;

//Record Notification Viewed for App Inbox.
- (void)recordInboxNotificationViewedEventForID:(NSString * _Nonnull)messageId;

// Record Notification Clicked for App Inbox.
- (void)recordInboxNotificationClickedEventForID:(NSString * _Nonnull)messageId;

//Mark Message as Read. Message id must be a String
- (void)markReadInboxMessageForID:(NSString * _Nonnull)messageId;

//Delete message from the Inbox. Message id must be a String
- (void)deleteInboxMessageForID:(NSString * _Nonnull)messageId;
```

### App Inbox Button onClick Callbacks

To get the callback on the click of the App Inbox Notification, check that your class implements the CleverTapInboxViewControllerDelegate and use the following method to handle user-selected actions from InApp notifications:

```objectivec
-(void)messageDidSelect:(CleverTapInboxMessage *)message atIndex:(int)index withButtonIndex:(int)buttonIndex {
      // This is called when an inbox message is clicked(tapped or call to action)
    CleverTapInboxMessageContent *content = (CleverTapInboxMessageContent*)message.content[index];
    ...
}
```
```swift
func messageDidSelect(_ message: CleverTapInboxMessage, at index: Int32, withButtonIndex buttonIndex: Int32) {
    // This is called when an inbox message is clicked(tapped or call to action)
  ...
  }
```

iOS SDK v3.7.1 and above supports callback on the click of App Inbox Notification Buttons by returning a map of Key-Value pairs. Use the following method:

```objectivec
- (void)messageButtonTappedWithCustomExtras:(NSDictionary *)customExtras {
          NSLog(@"App Inbox Button Tapped with custom extras::%@", customExtras);
  
}
```
```swift
func messageButtonTapped(withCustomExtras customExtras: [AnyHashable : Any]?) {
        print("App Inbox Button Tapped with custom extras: ", customExtras ?? "");
    }
```

# Debugging

By default, CleverTap logs are set to CleverTapLogLevel.info. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level to CleverTapLogLevel.debug. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTapLogLevel.off.

```objectivec
#ifdef DEBUG
   [CleverTap setDebugLevel:CleverTapLogDebug];
#else
   [CleverTap setDebugLevel:CleverTapLogOff];
#endif
```
```swift
#if DEBUG
    CleverTap.setDebugLevel(CleverTapLogLevel.debug.rawValue)
#else
    CleverTap.setDebugLevel(CleverTapLogLevel.off.rawValue)
#endif
```

## Advanced iOS Push Notifications

Apart from Title and Message, you have the below-mentioned options to add to your iOS push notification. Please note that each of these is optional.

**Rich Media**\
Rich media requires iOS 10.

Single Media

* You will have to install our <a href="https://github.com/CleverTap/CTNotificationService">single media</a> library to enable sending media iOS notifications.
* If a media link is specified, the media will be displayed as a static thumbnail in the default view, and as the actual media (image/video/gif/audio) in the expanded view. You will have to install our single media library to enable sending media iOS notifications.
* Supported file formats are .png, .jpeg, .jpg, .gif, .mp3, .mp4.
* Aspect ratio: 16:9

Image Slideshow

* You will have to install our <a href="https://github.com/CleverTap/CTNotificationContent">image carousel</a> and <a href="https://github.com/CleverTap/CTNotificationService">single media</a> libraries to enable sending slideshow media iOS notifications.
* You can add up to 5 slides. The first image will be shown as a thumbnail in the default view. A slideshow of all the images will be shown in the expanded view. You will have to install our image carousel and single media libraries to enable sending slideshow media iOS notifications.

![760](https://files.readme.io/4a4fe52-ios1.png "ios1.png")

You have 2 options to set the behavior of the notification after the user clicks on the notification. The notification can either be dismissed, or the notification will stay in the notification tray until explicitly swiped/dismissed.

**Sound File**\
You will have to specify the name of the sound file which is included in your app bundle. Apple supports .aiff, .caf and .wav extensions. For more information on how you can generate such sound files, please read [this article](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/SupportingNotificationsinYourApp.html).

**Badge Count**\
This updates the badge count for your app to the one specified while creating the campaign. Please note that this doesn’t increment the badge count or set the value to 0 (zero) to hide the badge number. That should be handled manually at the application level.

**Category**\
You can input the name of the category while creating the campaign. Each category has to be registered with the app. Each such category is associated with actions that the user can perform when a notification of rich media type is delivered. Each category can have up to four actions associated with it, although the number of actions actually displayed depends on the type of notification delivered. This enables users to take multiple actions for the notification. For example, a single media push notification can have two buttons – ‘Buy Now’ and ‘Save for Later’.

**Deep Link/External URL**\
Deep links allow you to land the user to a particular part of your app. Your app’s OpenURL method is called with the deep link specified here. If you wish to use external URLs, then you will have to whitelist the IPs or provide http/https before the URL so that they can be handled properly by the SDK.

**Mutable Content**\
Please check this box if you would like to send the mutable-content flag along with your payload. This will invoke your app’s notification service extension. For more information click [here](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ModifyingNotifications.html).

**Content Available**\
If you include the content-available key with a value of 1 to send out a silent notification to your users. It will not alert the user in any way (update badge count/play a sound/show a notification), but it will wake your app up in the background, allowing you to fetch new content and prepare it for the next time the user opens your app.

* Key: content-available
* Value: 1

## Push Notification Clicked

You can now raise and record push notifications clicked on your users’ iOS devices.\
CleverTap SDK v3.8.1 and above supports raising Push Notification Clicked for push campaigns.

```objectivec
[CleverTap sharedInstance] recordNotificationClickedEventWithData:response.notification.request.content.userInfo
```
```swift
CleverTap.sharedInstance()?.recordNotificationClickedEvent(withData: response.notification.request.content.userInfo)
```

> 🚧 Using the Notification Clicked public API
>
> Use this API only if you wish to raise a Notification Clicked event if the carousel action button is tapped.

> ❗️ Dummy Profile Creation
>
> If the `Notification Clicked` method is not called correctly, it will create a new profile in CleverTap with only this event. This will create a dummy profile with no other event and will not be mapped to the profile from which the Notification Clicked event was raised.
>
> To avoid the creation of a new user profile while calling the Notification Clicked method from your Application, you must pass the Identity/ Email to CleverTap in the Notification Content Class.

# Setting Native Display in iOS

We support Native Display for CleverTap iOS SDK version 3.7.2 and above. Follow the steps to integrate Native Display with your App. 

## Step 1: Register the CleverTap Display Unit

Import the CleverTap Display Unit Header.

```objectivec
#import <CleverTapSDK/CleverTap+DisplayUnit.h>
```
```swift
import CleverTapSDK;
```

To use this, check that your class implements the CleverTapAdUnitDelegate\
and set the setAdUnitDelegate to the class.

```objectivec
#import "ViewController.h"
#import <CleverTapSDK/CleverTap.h>
#import <CleverTapSDK/CleverTap+DisplayUnit.h>

@interface ViewController () <CleverTapDisplayUnitDelegate>

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    ...
    [[CleverTap sharedInstance] setDisplayUnitDelegate:self];
}

@end
```
```swift
import UIKit
import CleverTapSDK

class ViewController: UIViewController, CleverTapDisplayUnitDelegate  {

    override func viewDidLoad() {
        super.viewDidLoad()
        ...
        CleverTap.sharedInstance()?.setDisplayUnitDelegate(self)
    }
}
```

## Step 2: Handle Native Display Data

Use the following method:

```objectivec
- (void)displayUnitsUpdated:(NSArray<CleverTapDisplayUnit *>*_Nonnull)displayUnits {
       // you will get display units here
    NSArray <CleverTapDisplayUnit *> *units = displayUnits;
}
```
```swift
func displayUnitsUpdated(_ displayUnits: [CleverTapDisplayUnit]) {   
       // you will get display units here
    var units:[CleverTapDisplayUnit] = displayUnits;
}
```

### Public APIs

Definition of classes for CleverTapDisplayUnit and CleverTapDisplayUnitContent.

```objectivec CleverTapDisplayUnit
@interface CleverTapDisplayUnit : NSObject
/*!
* json defines the display unit data in the form of NSDictionary.
*/
@property (nullable, nonatomic, copy, readonly) NSDictionary *json;
/*!
* unitID defines the display unit identifier.
*/
@property (nullable, nonatomic, copy, readonly) NSString *unitID;
/*!
* type defines the display unit type.
*/
@property (nullable, nonatomic, copy, readonly) NSString *type;
/*!
* bgColor defines the backgroundColor of the display unit.
*/
@property (nullable, nonatomic, copy, readonly) NSString *bgColor;
/*!
* customExtras defines the extra data in the form of an NSDictionary. The extra key/value pairs set in the CleverTap dashboard.
*/
@property (nullable, nonatomic, copy, readonly) NSDictionary *customExtras;
/*!
* content defines the content of the display unit.
*/
@property (nullable, nonatomic, copy, readonly) NSArray<CleverTapDisplayUnitContent *> *contents;
@end
  
  /*!
@method
@abstract
This method returns all the display units.
*/
- (NSArray<CleverTapDisplayUnit *>*_Nonnull)getAllDisplayUnits;
 
 /*!
 @method
 @abstract
 This method return display unit for the provided unitID
 */
- (CleverTapDisplayUnit *_Nullable)getDisplayUnitForID:(NSString *_Nonnull)unitID;
```
```objectivec CleverTapDisplayUnitContent
@interface CleverTapDisplayUnitContent : NSObject
/*!
* title defines the title section of the display unit content.
*/
@property (nullable, nonatomic, copy, readonly) NSString *title;
/*!
* titleColor defines hex-code value of the title color as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *titleColor;
/*!
* message defines the message section of the display unit content.
*/
@property (nullable, nonatomic, copy, readonly) NSString *message;
/*!
* messageColor defines hex-code value of the message color as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *messageColor;
/*!
* videoPosterUrl defines video URL of the display unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *videoPosterUrl;
/*!
* actionUrl defines action URL of the display unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *actionUrl;
/*!
* mediaUrl defines media URL of the display unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *mediaUrl;
/*!
* iconUrl defines icon URL of the display unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *iconUrl;
/*!
* mediaIsAudio check whether mediaUrl is an audio.
*/
@property (nonatomic, readonly, assign) BOOL mediaIsAudio;
/*!
* mediaIsVideo check whether mediaUrl is a video.
*/
@property (nonatomic, readonly, assign) BOOL mediaIsVideo;
/*!
* mediaIsImage check whether mediaUrl is an image.
*/
@property (nonatomic, readonly, assign) BOOL mediaIsImage;
/*!
* mediaIsGif check whether mediaUrl is a gif.
*/
@property (nonatomic, readonly, assign) BOOL mediaIsGif;
@end

/*!
@method
@abstract
This method returns all the display units.
*/
- (NSArray<CleverTapDisplayUnit *>*_Nonnull)getAllDisplayUnits;
 
 /*!
 @method
 @abstract
 This method return display unit for the provided unitID
 */
- (CleverTapDisplayUnit *_Nullable)getDisplayUnitForID:(NSString *_Nonnull)unitID;
```

## Step 3:  Raise Notification Viewed and Notification Clicked event 

Raise the events for the Native Display using the following methods:

```objectivec
CleverTapDisplayUnit *displayUnit;
[[CleverTap sharedInstance]recordDisplayUnitViewedEventForID:displayUnit.unitID];




CleverTapDisplayUnit *displayUnit;
[[CleverTap sharedInstance]recordDisplayUnitClickedEventForID:displayUnit.unitID];
```
```swift
CleverTap.sharedInstance()?.recordAdUnitViewedEvent(forID: displayUnit.unitID)


CleverTap.sharedInstance()?.recordAdUnitViewedEvent(forID: displayUnit.unitID)
```

# Setting Account Credentials

If you do not wish to insert your account credentials in your app’s Info.plist, or you want to set your account ID programmatically, you can do so in your app delegate, in application:didFinishLaunchingWithOptions:

```objectivec Objective-C
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [CleverTap setCredentialsWithAccountID:@"Your account ID here" andToken:@"Your account token here"];
    [CleverTap autoIntegrate];
    ...
    return YES;
}
```
```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject:AnyObject]?) -> Bool {
    CleverTap.setCredentialsWithAccountID("Your account ID here", andToken: "Your account token here")
    CleverTap.autoIntegrate()
    ...
    return true
}
```

If you have set used this method to set your CleverTap Account ID and Token, please ensure that you do not make an entry for these values in your info.plist files.

# Checking Push Notifications from CleverTap

If you wish to determine whether a notification originated from CleverTap, call this method:

* (BOOL)isCleverTapNotification:(NSDictionary \*)payload;

You must manually call the SDK as follows/ Add following CleverTap code to your AppDelegate.

```objectivec
- (void) userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)(void))completionHandler {

    /**
     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.

     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.
     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.

     If you do not implement this method, your app never responds to custom actions.

     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter?language=objc
     **/

    if ([[CleverTap sharedInstance] isCleverTapNotification:response.notification.request.content.userInfo]) {

       ...
    }
    completionHandler();
}
```
```swift Swift
func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
        
     /**
     Use this method to perform the tasks associated with your app's custom actions. When the user responds to a notification, the system calls this method with the results. You use this method to perform the task associated with that action, if at all. At the end of your implementation, you must call the completionHandler block to let the system know that you are done processing the notification.
     
     You specify your app's notification types and custom actions using UNNotificationCategory and UNNotificationAction objects.
     You create these objects at initialization time and register them with the user notification center. Even if you register custom actions, the action in the response parameter might indicate that the user dismissed the notification without performing any of your actions.
     
     If you do not implement this method, your app never responds to custom actions.
     
     see https://developer.apple.com/reference/usernotifications/unusernotificationcenterdelegate/1649501-usernotificationcenter
     **/

      print("APPDELEGATE: didReceiveResponseWithCompletionHandler \(response.notification.request.content.userInfo)")

      // If you wish to determine whether a notification originated from CleverTap, you must manually call the SDK as follows:
      if CleverTap.sharedInstance().isCleverTapNotification(response.notification.request.content.userInfo) {

          ...
      }
 }
```
