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
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8faf4aa-9.png",
        "9.png",
        1924,
        1758,
        "#fcfcfd"
      ]
    }
  ]
}
[/block]
**Event Categories**

There are two categories of events in CleverTap: System Events and Custom Events. 

System Events are events recorded automatically after you integrate our SDK. Custom Events are events you define and track with our SDK or API.

**Event Properties**

Events have details that describe the action taking place called properties. 

For example, while recording the “Product viewed” event, you could also store event properties like product name, category, and price. Recording event properties will help you answer questions like which category of products are more popular, and help you segment users based on which categories or price points they’ve viewed.

# System Events 

System events are events recorded automatically after you integrate our SDK. 
[block:parameters]
{
  "data": {
    "h-0": "Event Type",
    "h-1": "Description",
    "0-1": "This event is recorded when a user installs your application and launches it for the first time.",
    "0-0": "App Installed",
    "0-2": "There are three cases when this event will be recorded multiple times for a single user. The first case is when a user installs your app, uninstalls it, then reinstalls it. The second case is when a user clears your app's memory. The third case is when a user installs your app on multiple devices.",
    "1-0": "App Launched",
    "2-0": "App Uninstalled",
    "3-0": "Notification Sent",
    "4-0": "Notification Viewed",
    "6-0": "UTM Visited",
    "1-1": "This event is recorded every time a user launches your application.",
    "h-2": "How Event is Tracked",
    "2-1": "This event is recorded when a user uninstalls your application.",
    "2-2": "This event is tracked by sending silent push notifications, which are type of notification that is not rendered on a user’s device. We send silent push notifications to all the users who have not launched the app the same day  to track uninstalls. For more information, please visit [this page](https://clevertap.com/blog/track-app-uninstalls-effectively/).",
    "3-1": "This event is tracked when a campaign message is sent to a user. This event is always recorded, even if the user does not open or click on the message. This event is recorded for email, mobile push, SMS, web push, and Facebook Audience campaigns.",
    "3-2": "The event is tracked when the notification is successfully sent from CleverTap to the communication channel you select for your campaign.",
    "6-1": "This event is tracked when user clicks on a link from a marketing campaign that has a UTM parameter defined on it. This event is also tracked when a CleverTap-integrated attribution platform, such as Branch or Apsalar, sends this information to CleverTap.",
    "4-1": "This event is tracked when a user views an email, in-app notification, or a web notification sent from CleverTap.",
    "6-2": "The UTM Visited event is recorded for your marketing campaigns from external sources, such as Google Adwords or AdRoll.",
    "4-2": "The CleverTap SDK recognizes when a notification sent via CleverTap is viewed by a user.",
    "1-2": "There are two cases when this event will recorded. The first case is a fresh app launch, which is when an app is launched from a killed state. The second case is when a app is brought to the foreground after 20 minutes of inactivity in the background.",
    "5-0": "Notification Clicked",
    "5-1": "This event is tracked only when a user clicks on a marketing campaign sent via CleverTap. The related UTM Visited event must raised via CleverTap.  The event storage duration is derived from the UTM Visited event.",
    "5-2": "Recorded when a user clicks on a mobile push, in-app, email, web-popup or web push message sent via the CleverTap dashboard, or through the campaign API. \n\nPlease note Android Push notifications containing deep links to third-party apps are not tracked",
    "8-0": "Identity Set",
    "8-1": "This DEBUG event is raised when a new user is identified on a customer’s app or an identified user pushes another identity.",
    "8-2": "This event monitors the status and data points that are important for identification and engagement of users. This event is used for monitoring and debugging only.\n\n   * Actual Identity\n   * Type of user: New User/ Merged/ Appended\n   * Dropped history: true/false (blacklisted flag)\n   * source: API/SDK",
    "9-0": "Identity Reset",
    "9-1": "This DEBUG event is raised when a profile is de-merged (after de-merge a new profile for every device is created and identities are dropped) either from dashboard (“reset identities” click on profile page) or through the D-emerge Profile API.",
    "10-0": "Identity Error",
    "10-1": "This DEBUG event is raised when an existing identity is associated incorrectly with another identity.  The former identity is now declared as invalid for the latter's identity.",
    "10-2": "This event is used for monitoring and debugging only.\n\n   * Actual Identity\n   * Source: API or SDK",
    "9-2": "It monitors the reset of identities and handles unnecessary merges. This event is used for monitoring and debugging only.\n\n   * Actual Identity\n   * Source: API or dashboard",
    "7-0": "Channel Unsubscribed",
    "7-1": "This event is raised when an email is not acknowledged.",
    "7-2": "Properties:\n\n1. Campaign Id - This is the ID of\nthe campaign from which user are updating subscriptions.\nCampaign Type - Currently only Email.\n2. Group - Group name from which the user unsubscribed/resubscribed.\n3. Identity - The user Identity/Email address.\n4. Variant\n5. Type - Valid values are bounced, dropped, and spam. Email IDs which bounce, drop or marked as spam are opted out from future emails\n6. Subscription Type - Account Level and Profile Level. Profile Level signifies that the user who qualified for the communication is opted out. Account Level signifies that all users with the email address are opted out from future communications.\n7. reason - Reason which was given by the email provider for the type of the error. For example : \"smtp;550 5.1.1 The email account that you tried to reach does not exist. Please try double-checking the recipient's email address for typos or unnecessary spaces.\""
  },
  "cols": 3,
  "rows": 11
}
[/block]

#System Properties

CleverTap tracks the following system properties automatically from the mobile SDK. All the system properties are prefixed by ‘CT’, indicating that they are provided by CleverTap. 

The following properties are tracked automatically on all events (except Uninstalled and Notification Sent):

[block:parameters]
{
  "data": {
    "1-0": "CT Latitude",
    "h-0": "System Property",
    "0-0": "CT App version",
    "h-1": "Description",
    "0-1": "This is the current version of your application installed on the user device.",
    "1-1": "The user location identified by the latitude.",
    "2-0": "CT Longitude",
    "2-1": "The user location identified by the longitude.",
    "3-0": "CT Source",
    "3-1": "The source of the event.\nFor example, the event may originate from a Mobile SDK or an API.\n\nAll possible values:\n  * Mobile (Mobile SDK)\n  * Web (Web SDK)\n  * API\n  * segment\n  * appsflyer\n  * apsalar\n  * branch\n  * tune\n  * System (For events generated by CleverTap)"
  },
  "cols": 2,
  "rows": 4
}
[/block]
The following properties are available on the App Launched event:
[block:parameters]
{
  "data": {
    "h-0": "System Property",
    "h-1": "Description",
    "0-0": "CT App version",
    "0-1": "This is the current version of your application installed on the user device.",
    "1-0": "CT Latitude",
    "1-1": "The user location identified by the latitude.",
    "2-0": "CT Longitude",
    "2-1": "The user location identified by the longitude.",
    "3-0": "CT OS Version",
    "3-1": "The Operating system of the device. \nFor example,  1.0.0",
    "4-0": "CT SDK Version",
    "4-1": "The CleverTap SDK version. For example, 30501",
    "5-0": "CT Network Carrier",
    "5-1": "The network carrier of the device.\nFor example, AT&T, Vodafone.",
    "6-0": "CT Network Type",
    "6-1": "The network type of the device\nFor example, 4g",
    "7-0": "CT Connected To WiFi",
    "7-1": "Indicates if the device is connected to the Wi-FI.",
    "8-0": "CT Bluetooth Version",
    "8-1": "The Bluetooth version of the device",
    "9-0": "CT Bluetooth Enabled",
    "9-1": "Indicates if  Bluetooth is enabled on the device.",
    "10-0": "CT Source",
    "10-1": "The source of the event.\nFor example, the event may originate from a Mobile SDK or an API.\n\nAll possible values:\n  * Mobile (Mobile SDK)\n  * Web (Web SDK)\n  * API\n  * segment\n  * appsflyer\n  * apsalar\n  * branch\n  * tune\n  * System (For events generated by CleverTap)"
  },
  "cols": 2,
  "rows": 11
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Latitude and Longitude",
  "body": "The system properties 'latitude' and 'longitude' are captured and sent from the SDK only if the user gives consent on your app."
}
[/block]


# Custom Events

Custom Events are events you define and track with our SDK or API.

For example, let's say you want to record an event called "Product viewed" when a user views a product. To record this event with the CleverTap SDK, you can use the following code snippet.
[block:code]
{
  "codes": [
    {
      "code": "// event without properties\ncleverTap.pushEvent(\"Product viewed\");",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// event without properties\n[[CleverTap sharedInstance] recordEvent:@\"Product viewed\"];",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "// event without properties\nCleverTap.sharedInstance()?.recordEvent(\"Product viewed\")",
      "language": "swift",
      "name": "iOS (Swift)"
    },
    {
      "code": "// event without properties\nclevertap.event.push(\"Product viewed\");",
      "language": "javascript",
      "name": "Web"
    }
  ]
}
[/block]
## Adding Properties to a Custom Event

Use the following code snippet to record properties along with the Event.
[block:code]
{
  "codes": [
    {
      "code": "// event with properties\nHashMap<String, Object> prodViewedAction = new HashMap<String, Object>();\nprodViewedAction.put(\"Product Name\", \"Casio Chronograph Watch\");\nprodViewedAction.put(\"Category\", \"Mens Accessories\");\nprodViewedAction.put(\"Price\", 59.99);\nprodViewedAction.put(\"Date\", new java.util.Date());\n\ncleverTap.pushEvent(\"Product viewed\", prodViewedAction);\n\n/**\n * Data types\n * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n *\n * Date object\n * When a property value is of type Date, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// event with properties\nNSDictionary *props = @{\n    @\"Product name\": @\"Casio Chronograph Watch\",\n    @\"Category\": @\"Mens Accessories\",\n    @\"Price\": @59.99,\n    @\"Date\": [NSDate date]\n};\n\n[[CleverTap sharedInstance] recordEvent:@\"Product viewed\" withProps:props];\n\n/**\n * Data types:\n * The value of a property can be of type NSDate, a NSNumber, a NSString, or a BOOL.\n *\n * NSDate object:\n * When a property value is of type NSDate, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "// event with properties\nlet props = [\n    \"Product name\": \"Casio Chronograph Watch\",\n    \"Category\": \"Mens Accessories\",\n    \"Price\": 59.99,\n    \"Date\": NSDate()\n]\n\nCleverTap.sharedInstance()?.recordEvent(\"Product viewed\", withProps: props)\n\n/**\n * Data types:\n * The value of a property can be of type NSDate, a Number, a String, or a Bool.\n *\n * NSDate object:\n * When a property value is of type NSDate, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "swift"
    },
    {
      "code": "// event with properties\nclevertap.event.push(\"Product viewed\", {\n    \"Product name\": \"Casio Chronograph Watch\",\n    \"Category\": \"Mens Accessories\",\n    \"Price\": 59.99,\n    \"Date\": new Date()\n});\n\n/**\n * Data types\n * Event property keys must be Strings and property values must, with certain specific exceptions,\n * be scalar values, i.e. String, Boolean, Integer, or Float, or a Date object.\n *\n * Date object\n * When a property value is of type Date, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "javascript",
      "name": "Web"
    }
  ]
}
[/block]
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
[block:code]
{
  "codes": [
    {
      "code": "HashMap<String, Object> chargeDetails = new HashMap<String, Object>();\nchargeDetails.put(\"Amount\", 300);\nchargeDetails.put(\"Payment Mode\", \"Credit card\");\nchargeDetails.put(\"Charged ID\", 24052013);\n\nHashMap<String, Object> item1 = new HashMap<String, Object>();\nitem1.put(\"Product category\", \"books\");\nitem1.put(\"Book name\", \"The Millionaire next door\");\nitem1.put(\"Quantity\", 1);\n\nHashMap<String, Object> item2 = new HashMap<String, Object>();\nitem2.put(\"Product category\", \"books\");\nitem2.put(\"Book name\", \"Achieving inner zen\");\nitem2.put(\"Quantity\", 1);\n\nHashMap<String, Object> item3 = new HashMap<String, Object>();\nitem3.put(\"Product category\", \"books\");\nitem3.put(\"Book name\", \"Chuck it, let's do it\");\nitem3.put(\"Quantity\", 5);\n\nArrayList<HashMap<String, Object>> items = new ArrayList<HashMap<String, Object>>();\nitems.add(item1);\nitems.add(item2);\nitems.add(item3);\n\ntry {\n    cleverTap.pushChargedEvent(chargeDetails, items);\n} catch (InvalidEventNameException e) {\n    // You have to specify the first parameter to push()\n    // as CleverTapAPI.CHARGED_EVENT\n}",
      "language": "java"
    },
    {
      "code": "NSDictionary *chargeDetails = @{\n   @\"Amount\" : @300,\n   @\"Payment mode\": @\"Credit Card\",\n   @\"Charged ID\": @24052013 \n};\n\nNSDictionary *item1 = @{\n   @\"Category\": @\"books\",\n   @\"Book name\": @\"The Millionaire next door\",\n   @\"Quantity\": @1\n};\n\nNSDictionary *item2 = @{\n   @\"Category\": @\"books\",\n   @\"Book name\": @\"Achieving inner zen\",\n   @\"Quantity\": @1\n};\n\nNSDictionary *item3 = @{\n   @\"Category\": @\"books\",\n   @\"Book name\": @\"Chuck it, let's do it\",\n   @\"Quantity\": @5\n};\n\nNSArray *items = @[item1, item2, item3];\n[[CleverTap sharedInstance] recordChargedEventWithDetails:chargeDetails\n                                                 andItems:items];",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "let chargeDetails = [\n    \"Amount\": 300,\n    \"Payment mode\": \"Credit Card\",\n    \"Charged ID\": 24052013\n]\n\nlet item1 = [\n    \"Category\": \"books\",\n    \"Book name\": \"The Millionaire next door\",\n    \"Quantity\": 1\n]\n\nlet item2 = [\n    \"Category\": \"books\",\n    \"Book name\": \"Achieving inner zen\",\n    \"Quantity\": 1\n]\n\nlet item3 = [\n    \"Category\": \"books\",\n    \"Book name\": \"Chuck it, let's do it\",\n    \"Quantity\": 5\n]\n\nCleverTap.sharedInstance()?.recordChargedEventWithDetails(chargeDetails, andItems: [item1, item2, item3])",
      "language": "swift"
    },
    {
      "code": "clevertap.event.push(\"Charged\", {\n    \"Amount\": 300,\n    \"Payment mode\": \"Credit Card\",\n    \"Charged ID\": 24052013,\n    \"Items\": [\n        {\n            \"Category\": \"Books\",\n            \"Book name\": \"The Millionaire next door\",\n            \"Quantity\": 1\n        },\n        {\n            \"Category\": \"Books\",\n            \"Book name\": \"Achieving inner zen\",\n            \"Quantity\": 1\n        },\n        {\n            \"Category\": \"Books\",\n            \"Book name\": \"Chuck it, let's do it\",\n            \"Quantity\": 5\n        }\n    ]\n});",
      "language": "javascript",
      "name": "Web"
    }
  ]
}
[/block]
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
[block:callout]
{
  "type": "warning",
  "body": "Please note discarding events cannot be undone."
}
[/block]
To discard events, go to Dashboard → Settings → My Account → Events & Properties → Discard Event for that particular event. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/dfe59c1-discard1.png",
        "discard1.png",
        1261,
        619,
        "#dee0e5"
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
        "https://files.readme.io/862fb45-discard2.png",
        "discard2.png",
        1002,
        169,
        "#f7f7f8"
      ],
      "border": true
    }
  ]
}
[/block]
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