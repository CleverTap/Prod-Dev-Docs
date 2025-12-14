---
title: iOS In App Notification
excerpt: >-
  Learn how to implement, control, and customize In-App Notifications in
  CleverTap iOS SDK.
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

The CleverTap iOS SDK allows you to show In-App notifications to your users, and it provides the capability to send In-App messages with images, GIFs, video, and audio. You can design In-App notifications directly from the dashboard, without writing a single line of code. You can also run A/B tests on your In-Apps.

> 📘 Platform Limitation
>
> In-App Notifications are not supported inside App Extensions, tvOS and watchOS apps.

# In-App Notification Callbacks

You can track user interactions with In-App Notifications using delegate methods provided by the CleverTap iOS SDK.\
These callbacks notify your app when a user **views**, **dismisses**, or **taps** an In-App Notification.

To use callbacks, implement the `CleverTapInAppNotificationDelegate` protocol in your class and define the following methods:

* [`shouldShowInAppNotificationWithExtras`](doc:in-app-notification-ios#shouldshowinappnotificationwithextras-callback)
* [`inAppNotificationDidShow`](doc:in-app-notification-ios#inappnotificationdidshow-callback)
* [`inAppNotificationDismissedWithExtras`](doc:in-app-notification-ios#inappnotificationdismissedwithextras-callback)
* [`inAppNotificationButtonTappedWithCustomExtras`](doc:in-app-notification-ios#inappnotificationbuttontappedwithcustomextras-callback)

Implementing these callbacks allows you to control when notifications appear and capture user interactions for analytics or personalization.

### `shouldShowInAppNotificationWithExtras` Callback

This callback is triggered *before* an In-App Notification is displayed. Use it to decide whether a notification should be shown.

Return `false` to suppress a notification if specific app conditions are not met (for example, if the user is already on the target screen).

```swift
// Called before an In-App Notification is displayed.
// Return false to prevent rendering the notification.
func shouldShowInAppNotification(withExtras extras: [AnyHashable : Any]!) -> Bool {
    if let screen = extras["screen"] as? String, screen == "home" {
        return false
    }
    return true
}
```
```objectivec
// Called before an In-App Notification is displayed.
// Return NO to prevent rendering the notification.
- (BOOL)shouldShowInAppNotificationWithExtras:(NSDictionary *)extras {
    NSString *screen = extras[@"screen"];
    if ([screen isEqualToString:@"home"]) {
        return NO;
    }
    return YES;
}
```

### `inAppNotificationDidShow` Callback

This callback is triggered *after* an In-App Notification is displayed to the user. Use this callback to log impression data or trigger custom analytics events.

```swift
// Called when an In-App Notification is displayed.
func inAppNotificationDidShow(_ notification: [AnyHashable : Any]!) {
    print("In-App Notification displayed:", notification ?? [:])
}
```
```objectivec
// Called when an In-App Notification is displayed.
- (void)inAppNotificationDidShow:(NSDictionary *)notification {
    NSLog(@"In-App Notification displayed: %@", notification);
}
```

### `inAppNotificationDismissedWithExtras` Callback

This callback is triggered *after* a notification is dismissed by the user or automatically after a timeout. Use this callback to perform cleanup tasks or record dismissal events.

```swift
// Called when an In-App Notification is dismissed.
func inAppNotificationDismissed(
    withExtras extras: [AnyHashable : Any]!,
    andActionExtras actionExtras: [AnyHashable : Any]!
) {
    print("Notification dismissed with extras:", extras ?? [:])
    print("Action extras:", actionExtras ?? [:])
}
```
```objectivec
// Called when an In-App Notification is dismissed.
- (void)inAppNotificationDismissedWithExtras:(NSDictionary *)extras
                               andActionExtras:(NSDictionary *)actionExtras {
    NSLog(@"Notification dismissed with extras: %@", extras);
    NSLog(@"Action extras: %@", actionExtras);
}
```

### `inAppNotificationButtonTappedWithCustomExtras` Callback

Available from iOS SDK v3.7.1 and above, this callback is triggered when a user taps a button within an In-App Notification. It provides a dictionary of key-value pairs that you can use to handle button actions or custom payloads.

```swift
// Called when a user taps a button in an In-App Notification.
func inAppNotificationButtonTapped(withCustomExtras customExtras: [AnyHashable : Any]!) {
    print("Button tapped with custom extras:", customExtras ?? [:])
}
```
```objectivec
// Called when a user taps a button in an In-App Notification.
- (void)inAppNotificationButtonTappedWithCustomExtras:(NSDictionary *)customExtras {
    NSLog(@"Button tapped with custom extras: %@", customExtras);
}
```

### Example: Implementing the Delegate

The following examples show how to set and implement the CleverTap In-App Notification delegate in your app.

```swift
import UIKit
import CleverTapSDK

class MyViewController: UIViewController, CleverTapInAppNotificationDelegate {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Register the delegate
        CleverTap.sharedInstance()?.setInAppNotificationDelegate(self)
    }

    func shouldShowInAppNotification(withExtras extras: [AnyHashable : Any]!) -> Bool {
        return true
    }

    func inAppNotificationDidShow(_ notification: [AnyHashable : Any]!) {
        print("Notification displayed:", notification ?? [:])
    }

    func inAppNotificationButtonTapped(withCustomExtras customExtras: [AnyHashable : Any]!) {
        print("Button tapped with extras:", customExtras ?? [:])
    }
}
```
```objectivec
#import "MyViewController.h"
#import <CleverTapSDK/CleverTap.h>

@interface MyViewController () <CleverTapInAppNotificationDelegate>
@end

@implementation MyViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Register the delegate
    [[CleverTap sharedInstance] setInAppNotificationDelegate:self];
}

- (BOOL)shouldShowInAppNotificationWithExtras:(NSDictionary *)extras {
    return YES;
}

- (void)inAppNotificationDidShow:(NSDictionary *)notification {
    NSLog(@"Notification displayed: %@", notification);
}

- (void)inAppNotificationButtonTappedWithCustomExtras:(NSDictionary *)customExtras {
    NSLog(@"Button tapped with extras: %@", customExtras);
}

@end
```

> ❗️ Warning
>
> Always register the delegate **before** an In-App Notification is triggered. Otherwise, callbacks may not be received.

### Troubleshooting

| Issue                   | Possible Cause                | Suggested Fix                                                                        |
| ----------------------- | ----------------------------- | ------------------------------------------------------------------------------------ |
| Callback not triggered  | Delegate not registered       | Ensure `setInAppNotificationDelegate(self)` is called in `viewDidLoad()` or earlier. |
| Empty extras dictionary | No key–value pairs configured | Add custom key–value pairs when creating the campaign in the dashboard.              |
| Button tap not detected | Using an older SDK version    | Upgrade to iOS SDK v3.7.1 or higher.                                                 |

# Control In-App Notifications

For CleverTap iOS SDK 3.10.0 and above, you can suspend, discard, or resume in-app notifications. 

> 📘 Note
>
> The CleverTap SDK automatically resumes displaying the in-app notification after every new session.

You can control the in-app notifications in the following ways:

### Suspend

The `suspendInAppNotifications` method suspends and saves in-app notifications until the `resumeInAppNotifications`  method is called for the current session.

```swift
CleverTap.sharedInstance()?.suspendInAppNotifications()
```
```objectivec
#import <CleverTapSDK/CleverTap+InAppNotifications.h>
[[CleverTap sharedInstance] suspendInAppNotifications];
```

### Discard

The `discardInAppNotifications` method discards in-app notifications until the `resumeInAppNotifications` method is called for the current session.

```swift
CleverTap.sharedInstance()?.discardInAppNotifications()
```
```objectivec
#import <CleverTapSDK/CleverTap+InAppNotifications.h>
[[CleverTap sharedInstance] discardInAppNotifications];
```

From CleverTap iOS SDK version 7.4.0 and above, you can optionally pass a parameter to control whether the currently visible In-App notification should be dismissed:

#### Parameters

* `inAppNotifications`
  * If `true`: Immediately dismisses the currently visible In-App notification (if any).
  * If `false`: Allows the currently visible In-App notification to remain on the screen.

```swift
CleverTap.sharedInstance()?.discard(inAppNotifications: true)
```
```objectivec
[[CleverTap sharedInstance]discardInAppNotifications:YES];
```

### Resume

The `resumeInAppNotifications` method resumes displaying in-app notifications.\
If you call this method after the `discardInAppNotifications()` method, it resumes the in-app notifications for events raised after the call is performed.

However, if you call the resumeInAppNotifications method after the suspendInAppNotifications() method, then it displays all queued in-app notifications and also resumes in-app notifications for events raised after the call is performed.

```java
CleverTap.sharedInstance()?.resumeInAppNotifications()
```
```objectivec
#import <CleverTapSDK/CleverTap+InAppNotifications.h>
[[CleverTap sharedInstance] resumeInAppNotifications];
```

> 📘 Note
>
> The `showInAppNotificationIfAny` method will be deprecated soon.  Use the `resumeInAppNotifications` method to manually display the pending in-app notifications.

# Custom Handling Deeplink

For CleverTap iOS SDK 3.10.0 and above, you can implement custom handling for URLs of in-app notification CTAs, push notifications, and app Inbox messages. 

Check that your class conforms to the `CleverTapURLDelegate` protocol by first calling `setURLDelegate`. Use the following protocol method  `shouldHandleCleverTap(_ : forChannel:)`  to handle URLs received from channels such as in-app notification CTAs, push notifications, and app Inbox messages:

```swift
// Set the URL Delegate
CleverTap.sharedInstance()?.setUrlDelegate(self)


 // CleverTapURLDelegate method
public func shouldHandleCleverTap(_ url: URL?, for channel: CleverTapChannel) -> Bool {  
    print("Handling URL: \(url!) for channel: \(channel)") 
    return true 
}
```
```objectivec
#import <CleverTapSDK/CleverTapURLDelegate.h>

// Set the URL Delegate
[[CleverTap sharedInstance]setUrlDelegate:self];

// CleverTapURLDelegate method 
- (BOOL)shouldHandleCleverTapURL:(NSURL *)url forChannel:(CleverTapChannel)channel {
    NSLog(@"Handling URL: \(%@) for channel: \(%d)", url, channel);
    return YES;
}
```

# Javascript support in In-App Notifications

CleverTap iOS SDK v3.5.0 and above supports embedding Javascript code inside Custom In-Apps. To make sure your Javascript code works on the app, while creating the InApp campaign, select the checkbox for Enabling javascript during the In-App campaign creation

<Image title="Custom HTML Templates in In-App Notification" alt={2870} align="center" border={true} src="https://files.readme.io/d45b488-185fa3f-image_1.png">
  Custom HTML Templates in In-App Notification
</Image>

<Image alt="Include JavaScript in In-App Notifications" align="center" border={true} src="https://files.readme.io/61ffc95-small-iOS_CustomHTML_inApp.png">
  Include JavaScript in In-App Notifications
</Image>

All methods are explained with examples below:

```javascript
//Recording a User Event called Product Viewed with properties, in JS enabled custom In-Apps
var props = {foo: 'xyz', lang: 'French'};
var message = { action:'recordEventWithProps', event:'Product Viewed', properties: props};
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Updating profile properties of the User in JS enabled custom In-Apps
const props = { name: 'John', email: 'john@mymail.com' , score: 0};
const message = { action:'profilePush', properties: props };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Adding user property for the User in JS enabled custom In-Apps
const message = { action: 'profileAddMultiValue', value: 'coat', key: 'myStuff' };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Adding multiple user properties for the User in JS enabled custom In-Apps
const message = { action: 'profileAddMultiValues', values: ['bag', 'kitkat'], key: 'myStuff' };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Removing a unique value from a multi-value profile in JS enabled custom In-Apps
const message = { action: 'profileRemoveMultiValue', value:'scarf', key: 'myStuff' };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Removing multiple unique values from a multi-value profile in JS enabled custom In-Apps
const message = { action: 'profileRemoveMultiValues', values:['scarf', 'knife'], key: 'myStuff' };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Removing a user property by specifying a key in JS enabled custom In-Apps
const message = { action: 'profileRemoveValueForKey', key: 'myStuff' };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Setting a user property by specifying the key in JS enabled custom In-Apps
const message = { action: 'profileSetMultiValues', values:['scarf', 'knife'], key: 'myStuff' };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

// Increment user property value by specifying the key in JS enabled custom In-Apps
const message = { action: 'profileIncrementValueBy', value: 1, key: 'score' };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

// Decrement user property value by specifying the key in JS enabled custom In-Apps
const message = { action: 'profileDecrementValueBy', value: 1, key: 'score' };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Calling onUserLogin 
const props = { name: 'JohnWeb', email: 'johnWeb@mymail.com' };
const message = { action:'onUserLogin', properties: props };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Recording a Charged Event in JS enabled custom In-Apps
const chargeDetails = { "Amount" : 300, "Payment mode": "Credit Card", "Charged ID": 24052013 };
const items = [{"Category": "books","Book name": "The Millionaire next door","Quantity": 1}, 
               {"Category": "books","Book name": "The Millionaire previous door","Quantity": 5}];
const message = { action:'recordChargedEvent', chargeDetails: chargeDetails, items: items };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}

//Prompt for push permission
const message = { action:'promptForPushPermission', showFallbackSettings:true };
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {
  window.webkit.messageHandlers.clevertap.postMessage(message);
}
```

> 📘 Inline Media
>
> To add inline media support in custom HTML for in-app notifications, add the key playsinline with value as 1 or true as a query parameter in your media URL. For example, watch this [YouTube video](https://www.youtube.com/watch?v=RUvrypIAPbg?playsinline=1). This functionality is available for CleverTap iOS SDK 3.7.2 and above.

# Dismissing JS-Enabled Custom In-App Notifications

If your In-App Notification supports JavaScript, you can dismiss it by calling the JS method when you set the action `dismissInAppNotification` in the CleverTap dashboard. Use the following code:

```javascript
// Dismissing JS-enabled custom In-Apps
if (window.webkit && window.webkit.messageHandlers && window.webkit.messageHandlers.clevertap) {  
    var message = { action: 'dismissInAppNotification' };  
    window.webkit.messageHandlers.clevertap.postMessage(message);  
}
```

# System In-App Functions

The CleverTap iOS SDK (v7.2.0 and above) supports a set of built-in system in-app functions. These can be triggered either via in-app notification button actions or launched as stand-alone campaigns.  For more information about the implementation flows, refer to [CleverTap Github Repository](https://github.com/CleverTap/clevertap-ios-sdk/blob/0da0b384c7ae97a73f8dfe6dcd4792512e34fd4c/docs/SystemInAppFunctions.md). For more information about using the App Functions from the dashboard, refer to [App Functions](https://docs.user.clevertap.net/docs/app-functions). 

## Open URL

The Open URL function opens a specific URL or deep link on iOS, configured via the CleverTap Dashboard when using a stand-alone in-app template. It is typically used to redirect users to destinations such as product pages, help centers, or feedback forms, seamlessly within the app experience. When a valid URL is launched, the system automatically raises the *Notification Viewed* event.

## Push Permission

The [Push Permission function ](https://github.com/CleverTap/clevertap-ios-sdk/blob/0da0b384c7ae97a73f8dfe6dcd4792512e34fd4c/docs/SystemInAppFunctions.md#push-permission)triggers the iOS system prompt to request notification permissions. It can be launched as a stand-alone campaign or invoked via an in-app notification button. This function is ideal for prompting users at relevant moments, such as after a key engagement, rather than during app launch. 

## App Rating

The App Rating function invokes the native iOS app rating prompt. It helps gather user feedback within the app experience by prompting users at contextually relevant moments, such as after a successful transaction or a positive interaction. When displayed, it automatically records the *Notification Viewed* event to measure user engagement with the prompt.

> 📘 Apple Rating Prompt Limits
>
> Apple limits rating prompts to three per user per year. Avoid prompting immediately on app launch.\
> For more information, refer to [Apple’s App Review documentation](https://developer.apple.com/documentation/storekit/requesting-app-store-reviews#Overview).
