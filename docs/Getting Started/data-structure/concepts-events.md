---
title: Events
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
# Overview

Events track what individual actions users perform in your app or website. Some examples of events include a user launching an app, viewing a product, listening to a song, sharing a photo, making a purchase, or favoriting an item. 

By tracking events in your app, you can better understand what users are doing. In CleverTap, you can analyze these events in many different ways, such as getting aggregating metrics of a specific event or measuring how a specific event type trends over time. You can also engage with your users based on these events by creating campaigns in CleverTap that are triggered by them. 

![1924](https://files.readme.io/8faf4aa-9.png "9.png")

**Event Categories**

There are two categories of events in CleverTap: System Events and Custom Events. 

System Events are events recorded automatically after you integrate our SDK. Custom Events are events you define and track with our SDK or API.

**Event Properties**

Events have details that describe the action taking place called properties. 

For example, while recording the “Product viewed” event, you could also store event properties like product name, category, and price. Recording event properties will help you answer questions like which category of products are more popular, and help you segment users based on which categories or price points they’ve viewed.

# System Events

System events are events recorded automatically after you integrate our SDK. 

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Event Type
      </th>

      <th>
        Description
      </th>

      <th>
        How Event is Tracked
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        App Installed
      </td>

      <td>
        This event is recorded when a user installs your application and launches it for the first time.
      </td>

      <td>
        There are three cases when this event will be recorded multiple times for a single user. The first case is when a user installs your app, uninstalls it, then reinstalls it. The second case is when a user clears your app's memory. The third case is when a user installs your app on multiple devices.
      </td>
    </tr>

    <tr>
      <td>
        App Launched
      </td>

      <td>
        This event is recorded every time a user launches your application.
      </td>

      <td>
        There are two cases when this event will recorded. The first case is a fresh app launch, which is when an app is launched from a killed state. The second case is when a app is brought to the foreground after 20 minutes of inactivity in the background.
      </td>
    </tr>

    <tr>
      <td>
        App Uninstalled
      </td>

      <td>
        This event is recorded when a user uninstalls your application.
      </td>

      <td>
        This event is tracked by sending silent push notifications, which are type of notification that is not rendered on a user’s device. We send silent push notifications to all the users who have not launched the app the same day  to track uninstalls. For more information, please visit [this page](https://clevertap.com/blog/track-app-uninstalls-effectively/).
      </td>
    </tr>

    <tr>
      <td>
        Notification Sent
      </td>

      <td>
        This event is tracked when a campaign message is sent to a user. This event is always recorded, even if the user does not open or click on the message. This event is recorded for email, mobile push, SMS and web push campaigns.
      </td>

      <td>
        The event is tracked when the notification is successfully sent from CleverTap to the communication channel you select for your campaign.
      </td>
    </tr>

    <tr>
      <td>
        Notification Viewed
      </td>

      <td>
        This event is tracked when a user views an email, in-app notification, or a web notification sent from CleverTap.
      </td>

      <td>
        The CleverTap SDK recognizes when a notification sent via CleverTap is viewed by a user.
      </td>
    </tr>

    <tr>
      <td>
        Notification Clicked
      </td>

      <td>
        This event is tracked only when a user clicks on a marketing campaign sent via CleverTap. The related UTM Visited event must raised via CleverTap.  The event storage duration is derived from the UTM Visited event.
      </td>

      <td>
        Recorded when a user clicks on a mobile push, in-app, email, web-popup or web push message sent via the CleverTap dashboard, or through the campaign API. 

        Please note Android Push notifications containing deep links to third-party apps are not tracked
      </td>
    </tr>

    <tr>
      <td>
        UTM Visited
      </td>

      <td>
        This event is tracked when user clicks on a link from a marketing campaign that has a UTM parameter defined on it. This event is also tracked when a CleverTap-integrated attribution platform, such as Branch or Apsalar, sends this information to CleverTap.
      </td>

      <td>
        The UTM Visited event is recorded for your marketing campaigns from external sources, such as Google Adwords or AdRoll.
      </td>
    </tr>

    <tr>
      <td>
        Channel Unsubscribed
      </td>

      <td>
        This event is raised when an email is not acknowledged.
      </td>

      <td>
        Properties:

        1. Campaign Id - This is the ID of\
           the campaign from which user are updating subscriptions.\
           Campaign Type - Currently only Email.
        2. Group - Group name from which the user unsubscribed/resubscribed.
        3. Identity - The user Identity/Email address.
        4. Variant
        5. Type - Valid values are bounced, dropped, and spam. Email IDs which bounce, drop or marked as spam are opted out from future emails
        6. Subscription Type - Account Level and Profile Level. Profile Level signifies that the user who qualified for the communication is opted out. Account Level signifies that all users with the email address are opted out from future communications.
        7. reason - Reason which was given by the email provider for the type of the error. For example : "smtp;550 5.1.1 The email account that you tried to reach does not exist. Please try double-checking the recipient's email address for typos or unnecessary spaces."
      </td>
    </tr>

    <tr>
      <td>
        Identity Set
      </td>

      <td>
        This DEBUG event is raised when a new user is identified on a customer’s app or an identified user pushes another identity.
      </td>

      <td>
        This event monitors the status and data points that are important for identification and engagement of users. This event is used for monitoring and debugging only.

        * Actual Identity
        * Type of user: New User/ Merged/ Appended
        * Dropped history: true/false (blacklisted flag)
        * source: API/SDK
      </td>
    </tr>

    <tr>
      <td>
        Identity Reset
      </td>

      <td>
        This DEBUG event is raised when a profile is de-merged (after de-merge a new profile for every device is created and identities are dropped) either from dashboard (“reset identities” click on profile page) or through the D-emerge Profile API.
      </td>

      <td>
        It monitors the reset of identities and handles unnecessary merges. This event is used for monitoring and debugging only.

        * Actual Identity
        * Source: API or dashboard
      </td>
    </tr>

    <tr>
      <td>
        Identity Error
      </td>

      <td>
        This DEBUG event is raised when an existing identity is associated incorrectly with another identity.  The former identity is now declared as invalid for the latter's identity.
      </td>

      <td>
        This event is used for monitoring and debugging only.

        * Actual Identity
        * Source: API or SDK
      </td>
    </tr>
  </tbody>
</Table>

# System Properties

CleverTap tracks the following system properties automatically from the mobile SDK. All the system properties are prefixed by ‘CT’, indicating that they are provided by CleverTap. 

The following properties are tracked automatically on all events (except Uninstalled and Notification Sent):

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        System Property
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        CT App version
      </td>

      <td>
        This is the current version of your application installed on the user device.
      </td>
    </tr>

    <tr>
      <td>
        CT Latitude
      </td>

      <td>
        The user location identified by the latitude.
      </td>
    </tr>

    <tr>
      <td>
        CT Longitude
      </td>

      <td>
        The user location identified by the longitude.
      </td>
    </tr>

    <tr>
      <td>
        CT Source
      </td>

      <td>
        The source of the event.\
        For example, the event may originate from a Mobile SDK or an API.

        All possible values:

        * Mobile (Mobile SDK)
        * Web (Web SDK)
        * API
        * segment
        * appsflyer
        * apsalar
        * branch
        * tune
        * System (For events generated by CleverTap)
      </td>
    </tr>
  </tbody>
</Table>

The following properties are available on the App Launched event:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        System Property
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        CT App version
      </td>

      <td>
        This is the current version of your application installed on the user device.
      </td>
    </tr>

    <tr>
      <td>
        CT Latitude
      </td>

      <td>
        The user location identified by the latitude.
      </td>
    </tr>

    <tr>
      <td>
        CT Longitude
      </td>

      <td>
        The user location identified by the longitude.
      </td>
    </tr>

    <tr>
      <td>
        CT OS Version
      </td>

      <td>
        The Operating system of the device.\
        For example,  1.0.0
      </td>
    </tr>

    <tr>
      <td>
        CT SDK Version
      </td>

      <td>
        The CleverTap SDK version. For example, 30501
      </td>
    </tr>

    <tr>
      <td>
        CT Network Carrier
      </td>

      <td>
        The network carrier of the device.\
        For example, AT\&T, Vodafone.
      </td>
    </tr>

    <tr>
      <td>
        CT Network Type
      </td>

      <td>
        The network type of the device\
        For example, 4g
      </td>
    </tr>

    <tr>
      <td>
        CT Connected To WiFi
      </td>

      <td>
        Indicates if the device is connected to the Wi-FI.
      </td>
    </tr>

    <tr>
      <td>
        CT Bluetooth Version
      </td>

      <td>
        The Bluetooth version of the device
      </td>
    </tr>

    <tr>
      <td>
        CT Bluetooth Enabled
      </td>

      <td>
        Indicates if  Bluetooth is enabled on the device.
      </td>
    </tr>

    <tr>
      <td>
        CT Source
      </td>

      <td>
        The source of the event.\
        For example, the event may originate from a Mobile SDK or an API.

        All possible values:

        * Mobile (Mobile SDK)
        * Web (Web SDK)
        * API
        * segment
        * appsflyer
        * apsalar
        * branch
        * tune
        * System (For events generated by CleverTap)
      </td>
    </tr>
  </tbody>
</Table>

> 📘 Latitude and Longitude
>
> The system properties 'latitude' and 'longitude' are captured and sent from the SDK only if the user gives consent on your app.

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

# Event Metadata Recorded Automatically

For every event that’s recorded, CleverTap records the following standard metadata:

* Information about the user who performed the event.
* Date and time when the event was recorded.
* The number of screens viewed by the user before performing the action.
* The referring site and the source of the user visit if it was from an external source.

Additionally, CleverTap keeps the user profiles updated with the latest:

* Geographic information like their city, region, country, and latitude/longitude (if available).
* Browser or device make, model, etc. used to access the website or app.

# Recording Customer Purchases

You should record transactions or purchases in CleverTap using a special event called Charged.

### What makes Charged a special event?

Charged is a special event because it provides a way for you to specify the items sold, their categories, transaction amount, the transaction id, and the information about your users.

Recording a purchase against a user marks them as a customer in CleverTap. This enables you to compare your funnel reports between customers and non-customers.

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

# Discard Events

If you reach the max number of event types that can be recorded for your CleverTap account, you can discard existing event types that you no longer need to record. This will free up space to record new event types. 

> 🚧 Please note discarding events cannot be undone.

To discard events, go to Dashboard → Settings → My Account → Events & Properties → Discard Event for that particular event. 

<Image title="discard1.png" alt={1261} className="border" border={true} src="https://files.readme.io/dfe59c1-discard1.png" />

<Image title="discard2.png" alt={1002} className="border" border={true} src="https://files.readme.io/862fb45-discard2.png" />

# Best Practices

Listed are some recommended best practices that you should keep in mind during event design

## Naming Events

* We recommend starting your integration with a max of 5 events that are critical to the success of your business
* Use short event and property names if possible – you’ll thank us when analyzing events in the dashboard
* Event names should match the action a user performs on your site. Example event names – “Viewed product”, “Added To cart”, “Video watched”, etc.
* Whenever possible, use the same names across your mobile apps and websites
* Group similar events with a common prefix – e.g. “Booking initiated”, “Booking dates selected”, “Booking room selected”, “Booking completed”

## Do Not Do

* Record broader events instead of granular ones. E.g. record a “Video watched” event with “Duration” as a property value, instead of three separate events like – “Video start”, “Video pause” and “Video end”
* Recording every user action results in too many events, which will make it harder to find meaningful answers from the dashboard
* Do not record screen loads or unloads, button clicks or form submissions. Instead, record user actions that are in-line with your business objectives.
* Do not capture page views as events, or page URLs as event properties. You can get better insights on how people use your product than by the pages they visit

## Things to Note

* It’s okay to have some events recorded specifically for your application or website as the flows might be different
* If your purchase process is broken into multiple steps, record each step as a separate event for better funnel analysis
* If you accept payments in multiple currencies, you can record it via event properties. However, in the amount field convert the incoming amount to a single currency value. This will simplify revenue and LTV analysis
* Avoid storing user profile information like age, gender etc. as event properties. Store them as User Attributes instead
