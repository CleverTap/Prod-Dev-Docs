---
title: Events
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

Events track what individual actions users perform in your app or website. Some examples of events include a user launching an app, viewing a product, listening to a song, sharing a photo, making a purchase, or favoriting an item.

By tracking events in your app, you can better understand what users are doing. In CleverTap, you can analyze these events in many different ways, such as getting aggregating metrics of a specific event or measuring how a specific event type trends over time. You can also engage with your users based on these events by creating campaigns in CleverTap that are triggered by them.

<Image align="center" alt={1924} border={true} caption="Track User Activities" title="Track User Activities on the CleverTap Dashboard" src="https://files.readme.io/8faf4aa-9.png" />

**Event Categories**

There are two categories of events in CleverTap: System Events and Custom Events.

System Events are events recorded automatically after you integrate our SDK. Custom Events are events you define and track with our SDK or API.

**Event Properties**

Events have details that describe the action taking place, called properties.

For example, while recording the “Product viewed” event, you could also store event properties like product name, category, and price. Recording event properties will help you answer questions like which category of products are more popular and help you segment users based on which categories or price points they’ve viewed.

# System Events

System events are events recorded automatically after you integrate our SDK.

| <p>Event Type</p>             | <p>Description</p>                                                                                                                                                                                                                                                                                                                                                                                    | <p>How Event is Tracked</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| :---------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>App Installed</p>          | <p>This event is recorded when a user installs your application.</p>                                                                                                                                                                                                                                                                                                                                  | <p>The event is raised when the user launches the app for the first time. </p><p>There are three cases when this event will be recorded multiple times for a single user. The first case is when a user installs your app, uninstalls it, and then reinstalls it. The second case is when a user clears your app's memory. The third case is when a user installs your app on multiple devices.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| <p>App Launched</p>           | <p>This event is recorded every time a user launches your application.</p>                                                                                                                                                                                                                                                                                                                            | <p>There are two cases when this event will recorded. The first case is a fresh app launch, which is when an app is launched from a killed state. The second case is when an app is brought to the foreground after 20 minutes of inactivity in the background.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| <p>App Uninstalled</p>        | <p>This event is recorded when a user uninstalls your application.</p>                                                                                                                                                                                                                                                                                                                                | <p>This event is tracked by sending silent push notifications which is a type of notifications that is not rendered on a user’s device. We send silent push notifications to your entire install base once every 24 hours to track uninstalls. For more information, refer to <a href="https://clevertap.com/blog/track-app-uninstalls-effectively/">App Uninstall</a>.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| <p>Web Session Started</p>    | <p>This is a web entry event similar to the App launched event. This event refers to the event triggered when a user starts a session or engages with your website.</p>                                                                                                                                                                                                                               | <p>This event will be recorded in two cases: when a user opens a webpage and when a user reloads the browser after 20 minutes of inactivity.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| <p>UTM Visited</p>            | <p>This event is tracked when a user clicks on a link from a marketing campaign that has a UTM parameter defined on it. This event is also tracked when a CleverTap-integrated attribution platform, such as Branch or Singular, sends this information to CleverTap.</p>                                                                                                                             | <p>The <em>UTM Visited</em> event is recorded for your marketing campaigns from external sources, such as Google Adwords or AdRoll.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| <p>Notification Sent</p>      | <p>This event is tracked when a campaign message is sent to a user. This event is always recorded, even if the user does not open or click on the message. This event is recorded for email, mobile push, SMS and web push, campaigns. The <em>Notification Sent</em> event is available on the <em>Event</em> dashboard but it is not displayed on the <em>User Profile</em>.</p>                    | <p>The event is tracked when the notification is successfully sent from CleverTap to the communication channel you select for your campaign.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| <p>Notification Viewed</p>    | <p>This event is tracked when a user views an email, in-app notification, or a web notification sent from CleverTap.</p>                                                                                                                                                                                                                                                                              | <p>The CleverTap SDK recognizes when a user views a notification sent via CleverTap.</p><p><em>Notification Viewed</em> is available for email, web push, in-app, web popup, and app inbox.</p><p>Important Properties:</p><ul><li>wzrk_pivot: String value containing an A/B testing campaign's variant name.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| <p>Notification Clicked</p>   | <p>This event is tracked only when a user clicks on a campaign sent via CleverTap. You can track or create a <em>Notification Clicked</em> event for every <em>UTM Visited</em> event that is tracked by CleverTap and not any other provider. There is no separate event storage required for the <em>Notification Clicked</em> event because it is derived from the <em>UTM Visited</em> event.</p> | <p>Recorded when a user clicks on a mobile push, in-app, email, web-popup, or web push message sent via the CleverTap dashboard or through the campaign API. </p><p>The Android push notifications containing deep links to third-party apps are not tracked.</p><p>Important Properties:</p><ul><li>wzrk_pivot: String value containing an A/B testing campaign's variant name.</li><li>wzrk_c2a: String value containing the action button name of notification clicked events.</li> <li>button_id: This property will be visible within the event only when tracking the Notification Clicked event for button clicks associated with the new Image Interstitial template </li></ul>.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| <p>Push Impressions</p>       | <p>This event is tracked when a push notification sent from CleverTap is delivered on a user’s device. The funnels on the <em>Push</em> campaign statistics page reflects the count for this event.</p>                                                                                                                                                                                               | <p>After the toggle for <em>Push Impressions</em> is turned on/setup from settings, the CleverTap SDK starts recording an event whenever a push notification sent via CleverTap is delivered to the user’s device.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| <p>App Version Changed</p>    | <p>This event is raised when a user’s current app version is different from the user’s previous app version.</p>                                                                                                                                                                                                                                                                                      | <p>This event is tracked when the <em>CT App version</em> system property differs from one <em>App Launched</em> event to another.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| <p>Notification Replied</p>   | <p>This event is recorded when a user replies to a WhatsApp message.</p>                                                                                                                                                                                                                                                                                                                              | <p>This event is raised when the brand receives a reply from the user.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| <p>Notification Delivered</p> | <p>This is an event raised only for WhatsApp. It checks if the WhatsApp notification has reached the end-user.</p>                                                                                                                                                                                                                                                                                    | <p>It is raised when the WhatsApp provider confirms that the notification has reached the end user (double-tick of WhatsApp).</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| <p>Reply Sent</p>             | <p>This event is recorded when an agent (CleverTap user) replies to a message from the end user.</p>                                                                                                                                                                                                                                                                                                  | <p>This event is raised against the user profile of the end user.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| <p>State Transitioned</p>     | <p>This event is recorded for lifecycle optimizer when a user transitions from one stage to another.</p>                                                                                                                                                                                                                                                                                              | <p>This event is raised whenever a user transitions from one state to another or from unmapped to one of the states in the lifecycle optimization framework. It is meant for internal usage at CleverTap and therefore, unavailable for querying.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| <p>Session Concluded</p>      | <p>This event is recorded to mark the end of a session. Session tracking must be enabled for the event to be tracked.</p>                                                                                                                                                                                                                                                                             | <p>This event is raised when there is 20 minutes of inactivity after an event is raised for a user.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| <p>Geocluster Entered</p>     | <p>This event is recorded to mark when a device enters a geofence. </p><p>The event will only be applicable for customers who have the geofence feature enabled for them.</p>                                                                                                                                                                                                                         | <p>Properties:</p><ol><li>Cluster ID</li><li>Cluster name</li><li>Geofence ID</li></ol>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| <p>Geocluster Exited</p>      | <p>This event is recorded to mark when a device exits a geofence.<br />The event will only be applicable for customers who have the geofence feature enabled for them.</p>                                                                                                                                                                                                                            | <p>Properties:</p><ol><li>Cluster ID</li><li>Cluster name</li><li>Geofence ID</li></ol>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| <p>Channel Unsubscribed</p>   | <p>This event is raised when an email is not acknowledged.</p>                                                                                                                                                                                                                                                                                                                                        | <p>Properties:</p><ol><li>Campaign ID: This is the ID of the campaign from which users are updating subscriptions.</li><li>Campaign Type: Currently only present for email and push campaigns.</li><li>Group: Group name from which the user unsubscribed/resubscribed.</li><li>Identity: The user identity/email address.</li><li>Variant</li><li>Type: Valid values are bounced, dropped, and spam. Email IDs that bounce, drop, or are marked as spam are opted out of future emails.</li><li>Subscription Type: Account and Profile levels.<ul><li>Profile Level: The specific profile is opted out for all email campaigns in the future until they opt-in again.</li><li>Account Level: The specific profile and email address are opted out of all email campaigns. Even if other profiles have that email address, they will not be sent emails in the future because that email address has been unsubscribed at the account level.</li></ul></li><li> Resubscribed: The value is true if the user has resubscribed, or else the value will be false.</li><li>Reason: Currently, present for email campaigns only. It is the same reason the email provider gave for the type of error. For example: "smtp;550 5.1.1 The email account you tried to reach does not exist. Please try double-checking the recipient's email address for typos or unnecessary spaces."</li></ol> |
| Channel Subscribed            | This event is raised when the user subscribes to a push notification channel for devices with Android OS version 13 and above. For Android 13 and above devices, the event is raised when the user gives permission. In the case of devices on Android 12, the event is raised when the user resubscribes for the push campaigns.                                                                     | <p>Properties:</p> <ul><li>Source: Indicates if the event is raised through a device, application, or API. </li> <li> Channel: Indicates the channel the user has subscribed to, i.e., Push Notification. </li> <li>App Version: Indicates the app version from which the event is raised. </li> <li> SDK Version: Indicates the SDK version installed on the device. </li> </ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

# System Properties

CleverTap tracks the following system properties automatically from the mobile SDK. All the system properties are prefixed by ‘CT’, indicating that they are provided by CleverTap.

The following properties are tracked automatically on all events (except Uninstalled and Notification Sent):

| <p>System Property</p> | <p>Description</p>                                                                                                                                                                                                                                                                                                                        |
| :--------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>CT App version</p>  | <p>This is the current version of your application installed on the user device.</p>                                                                                                                                                                                                                                                      |
| <p>CT Latitude</p>     | <p>The user location identified by the latitude.</p>                                                                                                                                                                                                                                                                                      |
| <p>CT Longitude</p>    | <p>The user location identified by the longitude.</p>                                                                                                                                                                                                                                                                                     |
| <p>CT Source</p>       | <p>The source of the event.<br />For example, the event may originate from a Mobile SDK or an API.</p><p>All possible values:</p><ul><li>Mobile (Mobile SDK)</li><li>Web (Web SDK)</li><li>API</li><li>segment</li><li>appsflyer</li><li>apsalar</li><li>branch</li><li>tune</li><li>System (For events generated by CleverTap)</li></ul> |

The following properties are available on the App Launched event:

| <p>System Property</p>      | <p>Description</p>                                                                                                                                                                                                                                                                                                                        |
| :-------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>CT App version</p>       | <p>This is the current version of your application installed on the user device.</p>                                                                                                                                                                                                                                                      |
| <p>CT Latitude</p>          | <p>The user location identified by the latitude.</p>                                                                                                                                                                                                                                                                                      |
| <p>CT Longitude</p>         | <p>The user location identified by the longitude.</p>                                                                                                                                                                                                                                                                                     |
| <p>CT OS Version</p>        | <p>The Operating system of the device.<br />For example,  1.0.0</p>                                                                                                                                                                                                                                                                       |
| <p>CT SDK Version</p>       | <p>The CleverTap SDK version. For example, 30501</p>                                                                                                                                                                                                                                                                                      |
| <p>CT Network Carrier</p>   | <p>The network carrier of the device.<br />For example, AT&T, Vodafone.</p>                                                                                                                                                                                                                                                               |
| <p>CT Network Type</p>      | <p>The network type of the device<br />For example, 4g</p>                                                                                                                                                                                                                                                                                |
| <p>CT Connected To WiFi</p> | <p>Indicates if the device is connected to the Wi-FI.</p>                                                                                                                                                                                                                                                                                 |
| <p>CT Bluetooth Version</p> | <p>The Bluetooth version of the device</p>                                                                                                                                                                                                                                                                                                |
| <p>CT Bluetooth Enabled</p> | <p>Indicates if  Bluetooth is enabled on the device.</p>                                                                                                                                                                                                                                                                                  |
| <p>CT Source</p>            | <p>The source of the event.<br />For example, the event may originate from a Mobile SDK or an API.</p><p>All possible values:</p><ul><li>Mobile (Mobile SDK)</li><li>Web (Web SDK)</li><li>API</li><li>segment</li><li>appsflyer</li><li>apsalar</li><li>branch</li><li>tune</li><li>System (For events generated by CleverTap)</li></ul> |

> 📘 Latitude and Longitude
>
> The system properties 'latitude' and 'longitude' are captured and sent from the SDK only if the user gives consent on your app.

# Debug Events

Debug events are events recorded automatically after you integrate our SDK. These events are raised at certain lifecycle stages in your integration and help you track and manage your integration. These events are available on any profile page and _Find People_ page by adding a parameter to the end of the URL `?showDebugEvents=true`.

| <p>Event Name</p>        | <p>Description</p>                                                                                                                                                                                                                                                                  | <p>When is it raised</p>                                                                                                                                                                                                                                                                                                                                                                                                                 |
| :----------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Identity Set</p>      | <p>This debug event is raised when a new user is identified on a customer’s app or an identified user pushes another identity.</p>                                                                                                                                                  | <p>This event monitors the status and data points that are important for the identification and engagement of users. This event is for monitoring and debugging only.</p>                                                                                                                                                                                                                                                                |
| <p>Identity Reset</p>    | <p>This debug event is raised when a profile is demerged (after demerged, a new profile for every device is created and identities are dropped) either from the dashboard (click on the <em>Profile</em> page to reset identities) or through the <em>Demerge Profile API</em>.</p> | <p>It monitors the reset of identities and handles unnecessary merges. This event is used for monitoring and debugging only.</p>                                                                                                                                                                                                                                                                                                         |
| <p>Identity Error</p>    | <p>This debug event is raised when an existing identity is associated incorrectly with another identity. The former identity is now declared as invalid for the latter's profile.</p>                                                                                               | <p>This event is for monitoring and debugging when identity merges are invalid.</p>                                                                                                                                                                                                                                                                                                                                                      |
| <p>Reachable By</p>      | <p>This debug event is raised when a user becomes reachable by a communication channel such as SMS, email, mobile push, or when there are changes to the existing communication channel.</p>                                                                                        | <p>Tracked for a profile when:</p><ul><li>Push token is added/changed.</li><li>Email ID is added/changed.</li><li>Phone number is added/changed. </li></ul>                                                                                                                                                                                                                                                                              |
| <p>Push Unregistered</p> | <p>This event tracks the removal of a mobile push token from the profile.</p>                                                                                                                                                                                                       | <p>This debug event is raised when an existing mobile push token is removed for a profile.</p><p>Tracked for a profile when:</p><ul><li><p>A user logs out of the device and another user logs in. Applicable only if the <code>onUserLogin()</code> method is implemented. </p></li><li><p>When a push token is removed using the <code>pushFcmRegisterationId("token",false)</code> method. Applicable only for Android.</p></li></ul> |

# Custom Events

Custom Events are events you define and track with our SDK or API.

For example, let's say you want to record an event called "Product viewed" when a user views a product. To record this event with the CleverTap SDK, you can use the following code snippet.

```java Android
// event without properties
cleverTap.pushEvent("Product viewed");
```
```objectivec iOS (Objective-C)
// event without properties
[[CleverTap sharedInstance] recordEvent:@"Product viewed"];
```
```swift iOS (Swift)
// event without properties
CleverTap.sharedInstance()?.recordEvent("Product viewed")
```
```javascript Web
// event without properties
clevertap.event.push("Product viewed");
```
```javascript React Native
// event without properties
CleverTap.recordEvent('Product Viewed')
```
```javascript Flutter/Dart
CleverTapPlugin.recordEvent("Product Viewed");
```

> 📘 Note
>
> Event property values must be a date object or scalar value—such as string, boolean, integer, or float. The Charged event is an exception, where property values can also include _arrays_.

## Adding Properties to a Custom Event

Use the following code snippet to record properties along with the Event.

```java Android
// event with properties
HashMap<String, Object> prodViewedAction = new HashMap<String, Object>();
prodViewedAction.put("Product Name", "Casio Chronograph Watch");
prodViewedAction.put("Category", "Mens Accessories");
prodViewedAction.put("Price", 59.99);
prodViewedAction.put("Date", new java.util.Date());

cleverTap.pushEvent("Product viewed", prodViewedAction);

/**
 * Data types
 * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 *
 * Date object
 * When a property value is of type Date, the date and time are both recorded to the second.
 * This can be later used for targeting scenarios.
 * For e.g. if you are recording the time of the flight as an event property,
 * you can send a message to the user just before their flight takes off.
 */
```
```objectivec iOS (Objective-C)
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
```javascript Web
// event with properties
clevertap.event.push("Product viewed", {
    "Product name": "Casio Chronograph Watch",
    "Category": "Mens Accessories",
    "Price": 59.99,
    "Date": new Date()
});

/**
 * Data types
 * Event property keys must be Strings and property values must, with certain specific exceptions,
 * be scalar values, i.e. String, Boolean, Integer, or Float, or a Date object.
 *
 * Date object
 * When a property value is of type Date, the date and time are both recorded to the second.
 * This can be later used for targeting scenarios.
 * For e.g. if you are recording the time of the flight as an event property,
 * you can send a message to the user just before their flight takes off.
 */
```
```javascript React Native
// event with properties
var props = { 'Name': 'XYZ', 'Price': 123 }

CleverTap.recordEvent('Product Viewed', props)

/**
 * Data types
 * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 *
 * Date object
 * When a property value is of type Date, the date and time are both recorded to the second.
 * This can be later used for targeting scenarios.
 * For e.g. if you are recording the time of the flight as an event property,
 * you can send a message to the user just before their flight takes off.
 */
```
```javascript Flutter/Dart
var eventProps = {
  // Key:    Value
  'Name': 'XYZ',
  'Price': 123,
  'date': CleverTapPlugin.getCleverTapDate(new DateTime.now()),
  'number': 1
};
CleverTapPlugin.recordEvent("Product Viewed", eventProps);
```

# Event Metadata Recorded Automatically

For every event that’s recorded, CleverTap records the following standard metadata:

* Information about the user who performed the event.
* Date and time when the event was recorded.
* The number of screens viewed by the user before performing the action.
* The referring site and the source of the user visit if it was from an external source.

Additionally, CleverTap keeps the user profiles updated with the latest:

* Geographic information like their city, region, country, and latitude/longitude (if available).
* Browser or device make, model, and so on, used to access the website or app.

# Adding Multi-Value Arrays to Custom Event Properties

Multi-value arrays allow you to send multiple related values within a single event property. Instead of sending separate events for each item, you can include an array of values in the `eventData` payload.

CleverTap supports arrays of strings or integers for custom event properties. However, all array values are stored internally as strings.

The following sample code shows how to use a Multi-value array in custom event properties:

```java Android
HashMap<String, Object> eventProps = new HashMap<>();

eventProps.put("Cart value", 1054.5);
eventProps.put("Currency", "INR");

ArrayList<String> multiValProp = new ArrayList<>();
multiValProp.add("Yogurt");
multiValProp.add("Ghee");
multiValProp.add("Cucumber");
multiValProp.add("Onion");
multiValProp.add("Green Chilli");

eventProps.put("Cart items", multiValProp);

cleverTapDefaultInstance.pushEvent("Order Confirmation", eventProps);
```

<Callout icon="📘" theme="info">
  **`Charged -> Items` is Reserved**

  Multi-value arrays do not apply to the `Charged` event’s `Items` key. The `Charged -> Items` field is a reserved implementation and supports an array of objects by design.

  For more information, refer to [Charged as a Special Event](https://developer.clevertap.com/docs/events#what-makes-charged-a-special-event).
</Callout>

The following table summarizes the limits and ingestion behavior for Multi-value arrays in event properties:

| Constraint                            | Limit          | Behavior if Limit is Exceeded                                 |
| ------------------------------------- | -------------- | ------------------------------------------------------------- |
| Maximum values per event property key | 100 items      | Additional values beyond 100 are dropped using FIFO logic.    |
| Maximum length per array value        | 512 characters | Values exceeding 512 characters are dropped during ingestion. |
| Empty array after processing          | Not allowed    | The event property is removed during ingestion.               |

# Recording Customer Purchases

You should record transactions or purchases in CleverTap using a special event called Charged.

### What makes Charged a special event?

Charged is a special event because it provides a way for you to specify the items sold, their categories, transaction amount, the transaction id, and the information about your users.

Recording a purchase against a user marks them as a customer in CleverTap. This enables you to compare your funnel reports between customers and other users.

### Recording Items Sold

To record a list of items sold, you should use the Items collection. See the code sample below. Along with the product name, you can also add properties like size, color, category etc.

### Recording the Transaction Amount

The transaction total or subscription charge should be recorded in an event property called Amount.

```java
HashMap<String, Object> chargeDetails = new HashMap<String, Object>();
chargeDetails.put("Amount", 300);
chargeDetails.put("Payment Mode", "Credit card");
chargeDetails.put("Charged ID", 24052013);

HashMap<String, Object> item1 = new HashMap<String, Object>();
item1.put("Product category", "books");
item1.put("Book name", "The Millionaire next door");
item1.put("Quantity", 1);

HashMap<String, Object> item2 = new HashMap<String, Object>();
item2.put("Product category", "books");
item2.put("Book name", "Achieving inner zen");
item2.put("Quantity", 1);

HashMap<String, Object> item3 = new HashMap<String, Object>();
item3.put("Product category", "books");
item3.put("Book name", "Chuck it, let's do it");
item3.put("Quantity", 5);

ArrayList<HashMap<String, Object>> items = new ArrayList<HashMap<String, Object>>();
items.add(item1);
items.add(item2);
items.add(item3);

try {
    cleverTap.pushChargedEvent(chargeDetails, items);
} catch (InvalidEventNameException e) {
    // You have to specify the first parameter to push()
    // as CleverTapAPI.CHARGED_EVENT
}
```
```objectivec iOS (Objective-C)
NSDictionary *chargeDetails = @{
   @"Amount" : @300,
   @"Payment mode": @"Credit Card",
   @"Charged ID": @24052013 
};

NSDictionary *item1 = @{
   @"Category": @"books",
   @"Book name": @"The Millionaire next door",
   @"Quantity": @1
};

NSDictionary *item2 = @{
   @"Category": @"books",
   @"Book name": @"Achieving inner zen",
   @"Quantity": @1
};

NSDictionary *item3 = @{
   @"Category": @"books",
   @"Book name": @"Chuck it, let's do it",
   @"Quantity": @5
};

NSArray *items = @[item1, item2, item3];
[[CleverTap sharedInstance] recordChargedEventWithDetails:chargeDetails
                                                 andItems:items];
```
```swift
let chargeDetails = [
    "Amount": 300,
    "Payment mode": "Credit Card",
    "Charged ID": 24052013
]

let item1 = [
    "Category": "books",
    "Book name": "The Millionaire next door",
    "Quantity": 1
]

let item2 = [
    "Category": "books",
    "Book name": "Achieving inner zen",
    "Quantity": 1
]

let item3 = [
    "Category": "books",
    "Book name": "Chuck it, let's do it",
    "Quantity": 5
]

CleverTap.sharedInstance()?.recordChargedEventWithDetails(chargeDetails, andItems: [item1, item2, item3])
```
```javascript Web
clevertap.event.push("Charged", {
    "Amount": 300,
    "Payment mode": "Credit Card",
    "Charged ID": 24052013,
    "Items": [
        {
            "Category": "Books",
            "Book name": "The Millionaire next door",
            "Quantity": 1
        },
        {
            "Category": "Books",
            "Book name": "Achieving inner zen",
            "Quantity": 1
        },
        {
            "Category": "Books",
            "Book name": "Chuck it, let's do it",
            "Quantity": 5
        }
    ]
});
```
```javascript React Native
var chargeDetails = { 'totalValue': 20, 
                      'category': 'books', 
                      'purchase_date': new Date()
                    }
var items = [
{ 'title': 'book1', 'published_date': new Date('2010-12-12T06:35:31'), 'author': 'ABC' },
{ 'title': 'book2', 'published_date': new Date('2020-12-12T06:35:31'), 'author': 'DEF') },
{ 'title': 'book3', 'published_date': new Date('2000-12-12T06:35:31'), 'author': 'XYZ' }]

CleverTap.recordChargedEvent(chargeDetails, items);
```
```javascript Flutter/Dart
var item1 = {
  // Key:    Value
  'Name': 'Thing1',
  'Amount': '100'
};
var item2 = {
  // Key:    Value
  'name': 'Thing2',
  'amount': '100'
};
var items = [item1, item2];
var chargeDetails = {
  // Key:    Value
  'Total': '200',
  'Payment': 'cash'
};
CleverTapPlugin.recordChargedEvent(chargeDetails, items);
```

You can personalize the message for customers who purchase multiple items in a single transaction. You can do so by using [Liquid Tags](https://docs.clevertap.com/docs/events#charged-event-personalization) from the CleverTap dashboard.

## Advantages of the Charged Event

* Helps you identify your customers, and how are they using your app or website
* Run campaigns to reward loyal users or get lost customers back
* Measure customer loyalty via running a cohort analysis on repeat purchases
* Analyze paid campaign performance by total revenue earned
* Get revenue insights like – total revenue, number of transactions, count of paying users and much more

# Understand Event Property Query Operators

* Contains: String contains query text
* Equals: String equals query text
* Not equals: Expected values should not equal query text (includes values where property value is not set)
* Greater than: Integer value is greater than query value
* Less than: Integer value is less than query value
* Exists: Some value exists for the selected property
* Does not exist: No value exists for the selected property

# Discard Events and Event Properties

You can discard existing events or event properties that you no longer need to record.

> 📘 Note
>
> You can discard only custom events, and it cannot be undone.

To discard events,

1. Navigate to Dashboard → Settings → Schema → Events.
2. Select the Custom Events tab.
3. Click the ellipsis for the event you wish to discard.

<Image align="center" alt={1396} border={true} caption="Navigation to Discard Custom Events" title="Navigation to Discard Custom Events" src="https://files.readme.io/136432e-Discard_events.png" />

4. Click **Discard** on the message to confirm the Discard.

<Image align="center" alt={1440} border={true} caption="Discard Custom Events" title="Discard Custom Events Pop Up" src="https://files.readme.io/238dd7d-Discard_events2.png" />

To discard event properties,

1. Navigate to Dashboard → Settings → Schema → Events.
2. Select the Custom Events tab.
3. Select the properties for an event.
4. Click the ellipsis for the event property you wish to discard.

<Image align="center" alt={1437} border={true} caption="Discard a Custom Event Property" title="Discard Custom Event Properties" src="https://files.readme.io/921c1a4-Discard_event_property.png" />

5. Click **Discard** on the message to confirm the Discard.  
   This discards the event property, and it will no longer be recorded.

# Event Consideration

The following are the important CleverTap User Event considerations:

* The maximum number of User Event types per application is 512. While the number may seem limited, if used alongside properties can help you record a lot more User Event data. The volume of events submitted per account across those user event types is practically unlimited.

* For each User Event recorded, the maximum number of Event Properties is limited to 256.

* ‘Charged’ Event supports up to 256 item values.

* Event property keys must be of type _String_ and property values must be scalar values, that is, String, Boolean, Integer, Float, or a Date object.

* Prohibited characters: &, $, “, , %, >, \<, !

* User Event keys are limited to 120 characters in length.

* User Event property values are limited to 512 characters in length.

# Best Practices

Listed are some recommended best practices that you should keep in mind during event design.

## Naming Events

* We recommend starting your integration with a max of 5 events that are critical to the success of your business
* Use short event and property names if possible – this will help you when analyzing events in the dashboard
* Event names should match the action a user performs on your site. Example event names – “Viewed product”, “Added To cart”, “Video watched”, etc.
* Whenever possible, use the same names across your mobile apps and websites
* Group similar events with a common prefix – e.g. “Booking initiated”, “Booking dates selected”, “Booking room selected”, “Booking completed”

## Recommendations

* Record broader events instead of granular ones. E.g. record a “Video watched” event with “Duration” as a property value, instead of three separate events like – “Video start”, “Video pause” and “Video end”
* Recording every user action results in too many events, which will make it harder to find meaningful answers from the dashboard
* Do not record screen loads or unloads, button clicks, or form submissions. Instead, record user actions that are in line with your business objectives.
* Do not capture page views as events, or page URLs as event properties. You can get better insights on how people use your product than by the pages they visit

## Things to Note

* It is okay to have some events recorded specifically for your application or website as the flows might be different
* If your purchase process is broken into multiple steps, record each step as a separate event for better funnel analysis
* If you accept payments in multiple currencies, you can record it via event properties. However, in the amount field convert the incoming amount to a single currency value. This will simplify revenue and LTV analysis
* Avoid storing user profile information like age, gender, etc. as event properties. Store them as User Attributes instead
* Even if every custom property is discarded, the profile update still carries meaningful data, such as timezone, device information, and so on.

<br />
