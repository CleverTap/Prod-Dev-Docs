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
        "https://files.readme.io/152856f-events1.png",
        "events1.png",
        898,
        704,
        "#fcfcfc"
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

System Events are events recorded automatically after you integrate our SDK. 
[block:parameters]
{
  "data": {
    "h-0": "Event Type",
    "h-1": "Description",
    "0-1": "This event is recorded when a user installs your application.",
    "0-0": "App Installed",
    "0-2": "The event is raised when the user launches the app for the first time. \n\nThere are three cases when this event will be recorded multiple times for a single user. The first case is when a user installs your app, uninstalls it, and then reinstalls it. The second case is when clear your app's memory. The third case is when a user installs your app on multiple devices.",
    "1-0": "App Launched",
    "2-0": "App Uninstalled",
    "3-0": "Notification Sent",
    "4-0": "Notification Viewed",
    "5-0": "UTM Visited",
    "6-0": "App Screen Viewed (optional)",
    "7-0": "Web Page Viewed (optional)",
    "7-2": "The CleverTap JS SDK tracks this event when a URL is changed. Please note that if the URL does not change, the page view will not be tracked.",
    "7-1": "This event tracks the name of every web page a user views. \n\nThis event is not tracked by default. To activate tracking for this event, you have to set this event to active from the Events section on your Settings page.",
    "6-2": "The CleverTap JS SDK tracks this event every time a user navigates to a different screen in your app.",
    "6-1": "This event tracks the name of the screen a user views in an iOS or Android app. \n\nThis event is not tracked by default. To activate tracking for this event, you have to set this event to active from the Events section on your Settings page.",
    "1-1": "This event is recorded every time a user launches your application.",
    "h-2": "How Event is Tracked",
    "2-1": "This event is recorded when a user uninstalls your application.",
    "2-2": "This event is tracked by sending silent push notifications, which are type of notification that is not rendered on a user’s device. We send silent push notifications to your entire install base once every 24 hours to track uninstalls. For more information, please visit [this page](https://clevertap.com/blog/track-app-uninstalls-effectively/).",
    "3-1": "This event is tracked when a campaign message is sent to a user. This event is always recorded, even if the user does not open or click on the message. This event is recorded for email, mobile push, SMS, web push, and Facebook Audience campaigns.",
    "3-2": "The event is tracked when the notification is successfully sent from CleverTap to the communication channel you select for your campaign.",
    "5-1": "This event is tracked when user clicks on a link from a marketing campaign that has a UTM parameter defined on it. This event is also tracked when a CleverTap-integrated attribution platform, such as Branch or Apsalar, sends this information to CleverTap.",
    "4-1": "This event is tracked when a user views an email, in-app notification, or a web notification sent from CleverTap.",
    "5-2": "For mobile push, in-app notification, web push, and email campaigns, we will record a UTM Visited event when a user clicks on a message. For web pop-ups campaigns, we will record a UTM Visited event when a user views the web pop-up. For SMS campaigns, we do not record a UTM Visited event. \n\nThe UTM Visited event is also recorded for your marketing campaigns from external sources, such as Google Adwords or AdRoll.\n\nPlease note Android Push notifications containing deep links to third-party apps are not tracked.",
    "4-2": "The CleverTap SDK recognizes when a notification sent via CleverTap is viewed by a user.",
    "1-2": "There are two cases when this event will recorded. The first case is a fresh app launch, which is when an app is launched from a killed state. The second case is when a app is brought to the foreground after 20 minutes of inactivity in the background."
  },
  "cols": 3,
  "rows": 8
}
[/block]
# Custom Events

Custom Events are events you define and track with our SDK or API.

For example, let's say you want to record an event called "Product viewed" when a user views a product. To record this event with the CleverTap SDK, you can use the following code snippet.
[block:code]
{
  "codes": [
    {
      "code": "// event without properties\ncleverTap.event.push(\"Product viewed\");",
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
    },
    {
      "code": "// event without properties\nCleverTapInstance.Event.Push(\"Product viewed\");",
      "language": "csharp",
      "name": "WinRT"
    },
    {
      "code": "// event without properties\nCleverTapInstance.Event.Push(\"Product viewed\");",
      "language": "csharp",
      "name": "Silverlight"
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
      "code": "// event with properties\nHashMap<String, Object> prodViewedAction = new HashMap<String, Object>();\nprodViewedAction.put(\"Product Name\", \"Casio Chronograph Watch\");\nprodViewedAction.put(\"Category\", \"Mens Accessories\");\nprodViewedAction.put(\"Price\", 59.99);\nprodViewedAction.put(\"Date\", new java.util.Date());\n\ncleverTap.event.push(\"Product viewed\", prodViewedAction);\n\n/**\n * Data types\n * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n *\n * Date object\n * When a property value is of type Date, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
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
    },
    {
      "code": "// event with properties\nDictionary<string, object> evtProps = new Dictionary<string, object>();\nevtProps.Add(\"Product name\", \"Casio Chronograph Watch\");\nevtProps.Add(\"Category\", \"Mens Accessories\");\nevtProps.Add(\"Price\", 59.99);\n\nCleverTapInstance.Event.Push(\"Product viewed\", evtProps);\n\n/**\n * Data types\n * The value of a property can be either a DateTime, an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n *\n * Date object\n * When you pass the value of the property as DateTime, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "csharp",
      "name": "WinRT"
    },
    {
      "code": "// event with properties\nDictionary<string, object> evtProps = new Dictionary<string, object>();\nevtProps.Add(\"Product name\", \"Casio Chronograph Watch\");\nevtProps.Add(\"Category\", \"Mens Accessories\");\nevtProps.Add(\"Price\", 59.99);\n\nCleverTapInstance.Event.Push(\"Product viewed\", evtProps);\n\n/**\n * Data types\n * The value of a property can be either a DateTime, an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n *\n * Date object\n * When you pass the value of the property as DateTime, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "csharp",
      "name": "Silverlight"
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
      "code": "HashMap<String, Object> chargeDetails = new HashMap<String, Object>();\nchargeDetails.put(\"Amount\", 300);\nchargeDetails.put(\"Payment Mode\", \"Credit card\");\nchargeDetails.put(\"Charged ID\", 24052013);\n\nHashMap<String, Object> item1 = new HashMap<String, Object>();\nitem1.put(\"Product category\", \"books\");\nitem1.put(\"Book name\", \"The Millionaire next door\");\nitem1.put(\"Quantity\", 1);\n\nHashMap<String, Object> item2 = new HashMap<String, Object>();\nitem2.put(\"Product category\", \"books\");\nitem2.put(\"Book name\", \"Achieving inner zen\");\nitem2.put(\"Quantity\", 1);\n\nHashMap<String, Object> item3 = new HashMap<String, Object>();\nitem3.put(\"Product category\", \"books\");\nitem3.put(\"Book name\", \"Chuck it, let's do it\");\nitem3.put(\"Quantity\", 5);\n\nArrayList<HashMap<String, Object>> items = new ArrayList<HashMap<String, Object>>();\nitems.add(item1);\nitems.add(item2);\nitems.add(item3);\n\ntry {\n    cleverTap.event.push(CleverTapAPI.CHARGED_EVENT, chargeDetails, items);\n} catch (InvalidEventNameException e) {\n    // You have to specify the first parameter to push()\n    // as CleverTapAPI.CHARGED_EVENT\n}",
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
    },
    {
      "code": "Dictionary<string, object> chargeDetails = new Dictionary<string, object>();\nchargeDetails.Add(\"Amount\", 300);\nchargeDetails.Add(\"Currency\", \"USD\");\nchargeDetails.Add(\"Payment mode\", \"Credit card\");\nchargeDetails.Add(\"Charged ID\", 24052013);\n\nDictionary<string, object> item1 = new Dictionary<string, object>();\nitem1.Add(\"Category\", \"books\");\nitem1.Add(\"Book name\", \"The Millionaire next door\");\nitem1.Add(\"Quantity\", 1);\n\nDictionary<string, object> item2 = new Dictionary<string, object>();\nitem2.Add(\"Category\", \"books\");\nitem2.Add(\"Book name\", \"Achieving inner zen\");\nitem2.Add(\"Quantity\", 1);\n\nDictionary<string, object> item3 = new Dictionary<string, object>();\nitem3.Add(\"Category\", \"books\");\nitem3.Add(\"Book name\", \"Chuck it, let's do it\");\nitem3.Add(\"Quantity\", 5);\n\nList<Dictionary<string, object>> items = new List<Dictionary<string, object>>();\nitems.Add(item1);\nitems.Add(item2);\nitems.Add(item3);\n\nCleverTapInstance.Event.PushCharged(chargeDetails, items);",
      "language": "csharp",
      "name": "WinRT"
    },
    {
      "code": "Dictionary<string, object> chargeDetails = new Dictionary<string, object>();\nchargeDetails.Add(\"Amount\", 300);\nchargeDetails.Add(\"Currency\", \"USD\");\nchargeDetails.Add(\"Payment mode\", \"Credit card\");\nchargeDetails.Add(\"Charged ID\", 24052013);\n\nDictionary<string, object> item1 = new Dictionary<string, object>();\nitem1.Add(\"Category\", \"books\");\nitem1.Add(\"Book name\", \"The Millionaire next door\");\nitem1.Add(\"Quantity\", 1);\n\nDictionary<string, object> item2 = new Dictionary<string, object>();\nitem2.Add(\"Category\", \"books\");\nitem2.Add(\"Book name\", \"Achieving inner zen\");\nitem2.Add(\"Quantity\", 1);\n\nDictionary<string, object> item3 = new Dictionary<string, object>();\nitem3.Add(\"Category\", \"books\");\nitem3.Add(\"Book name\", \"Chuck it, let's do it\");\nitem3.Add(\"Quantity\", 5);\n\nList<Dictionary<string, object>> items = new List<Dictionary<string, object>>();\nitems.Add(item1);\nitems.Add(item2);\nitems.Add(item3);\n\nCleverTapInstance.Event.PushCharged(chargeDetails, items);",
      "language": "csharp",
      "name": "Silverlight"
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

# Platform Considerations
  * The maximum number of User Event types per app is 500. While the number might seem limiting, if used along side properties can help you record a lot more User Event data than it seems. The volume of events submitted per account across those event types is practically unlimited.
  * For each User Event recorded, the maximum number of Event Properties is limited to 256.
  * ‘Charged’ Event supports up to 256 Items values
  * Event property keys must be of type String and property values must be scalar values, i.e. String, Boolean, Integer, Float or a Date object.
  * Prohibited characters: &, $, “, \, %, >, <, !
  * User Event keys are limited to 120 characters in length.
  * User Event property values are limited to 512 characters in length.