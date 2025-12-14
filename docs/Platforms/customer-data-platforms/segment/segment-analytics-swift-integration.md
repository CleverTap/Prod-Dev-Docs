---
title: Segment Analytics-Swift Integration
excerpt: >-
  Learn how to integrate Segment's Analytics-Swift SDK with CleverTap to capture
  events, user traits, and engagement data in your iOS application.
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

The Segment Analytics-Swift SDK enables you to track events and user traits across your iOS application. When you configure CleverTap as a destination, you can forward this data to CleverTap and unlock engagement features such as personalization, push notifications, and In-App messaging.

This integration allows you to:

- Forward events from Analytics-Swift into CleverTap.
- Record user traits and behaviors using the standard Segment `track`, `identify`, and `screen` APIs.
- Leverage CleverTap features such as Push Notifications, App Inbox, and In-App Messaging.

# Prerequisites for Integration

Before you begin, ensure the following:

- A Segment account
- A CleverTap account

# Integration

The integration between Segment Analytics-Swift SDK and CleverTap involves configuring CleverTap as a destination in your Segment workspace and forwarding event data to CleverTap from your iOS application.

This process includes the following steps:

1. [Add Destination in Segment](doc:segment-analytics-swift-integration#add-destination-in-segment)
2. [Install Analytics-Swift and CleverTap Plugin](doc:segment-analytics-swift-integration#install-analytics-swift-and-clevertap-plugin)
3. [Initialize Analytics-Swift with CleverTap Plugin](doc:segment-analytics-swift-integration#initialize-analytics-swift-with-clevertap-plugin)

## Add Destination in Segment

Destinations are the business tools or applications to which Segment forwards your data. Adding destinations allows you to act on your data and learn more about your customers in real time.

To configure CleverTap as a destination in Segment:

1. Go to _Destinations_ in the Segment application.
2. Click **Add Destination** and select _CleverTap_.
3. In the _Destination Catalog/CleverTap_ page, click _Configure CleverTap_.
4. Add your CleverTap _Account ID_ and _Account Token_, which you can find on the CleverTap Dashboard under the _Settings_ page.
5. Select the appropriate region from the _Region_ list. To identify the region of your CleverTap account, refer to the following table:

| CleverTap Dashboard URL                                                                              | Region            |
| :--------------------------------------------------------------------------------------------------- | :---------------- |
| <https://in1.dashboard.clevertap.com/login.html#/>                                                   | IN                |
| <https://sg1.dashboard.clevertap.com/login.html#/>                                                   | SG                |
| <https://us1.dashboard.clevertap.com/login.html#/>                                                   | US                |
| <https://eu1.dashboard.clevertap.com/login.html#/>                                                   | None              |
| [https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html#/) | Indonesia         |
| [https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html#/) | Middle East (UAE) |

6. Turn on the toggle at the top of the _Settings_ page to enable the destination.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/de3fc69-2023-03-15_17-34-07.png",
        "Enable the CleverTap Settings",
        "Enable the CleverTap Settings"
      ],
      "align": "center",
      "sizing": "75% ",
      "border": true,
      "caption": "Enable the CleverTap Settings"
    }
  ]
}
[/block]


You can integrate CleverTap from the server-side or mobile destination (iOS or Android). Select mobile destinations to use CleverTap push notifications or In-App Notifications.

## Install Analytics-Swift and CleverTap Plugin

Install the Segment Analytics-Swift SDK along with the CleverTap destination plugin to enable integration.

1. In Xcode, go to _File_ > _Swift Package Manager_ > _Add Package Dependency_.
2. Enter **<https://github.com/CleverTap/clevertap-segment-swift.git>** as the package repository and click **Next**.
3. On the next screen, select an SDK version (by default, Xcode selects the latest stable version) and click **Next**.
4. Click **Finish** and ensure that `SegmentCleverTap` has been added to the appropriate target.

## Initialize Analytics-Swift with CleverTap Plugin

Set up Analytics-Swift in your application class and attach the CleverTap destination plugin.

1. Import the modules you added in the installation step:

```swift
import Segment
import SegmentCleverTap
```

2. Declare CleverTap’s destination in your app delegate or application setup:

```swift
let analytics = Analytics(configuration: Configuration(writeKey: "YOUR_WRITE_KEY_HERE"))
analytics.add(plugin: CleverTapDestination())
```

| Parameter             | Description                                                                                                                  |
| :-------------------- | :--------------------------------------------------------------------------------------------------------------------------- |
| `YOUR_WRITE_KEY_HERE` | From the Segment dashboard, go to _Connections > Sources > Select the source > Settings > API Keys_ to get this information. |

This setup initializes Analytics-Swift and enables communication between Segment and CleverTap. Refer to the [Sample App](https://github.com/CleverTap/clevertap-segment-swift/blob/main/ExampleSPM) for more details on implementation.

# Track Events and User Data

Use Analytics-Swift APIs to record events, capture user traits, and log screen views, all of which are forwarded to CleverTap.

## Record User Attributes

Segment's `identify` API maps to CleverTap's `onUserLogin`.

```swift
let floatAttribute = 12.3
let integerAttribute: Int = 18
let shortAttribute: Int16 = 2
let birthdate: Date = Date(timeIntervalSince1970: 946684800) // 1 Jan 2000
let traits: [String: Any] = [
    "email": "support@clevertap.com",
    "bool": true,
    "double": 3.14159,
    "stringInt": "1",
    "integerAttribute": integerAttribute,
    "floatAttribute": floatAttribute,
    "shortAttribute": shortAttribute,
    "gender": "female",
    "name": "Segment CleverTap",
    "phone": "+15555555556",
    "birthday": birthdate,
    "testArr": ["1", "2", "3"],
    "address": [
        "city": "New York",
        "country": "US"
    ]
]
analytics.identify(userId: "cleverTapSegmentSwiftUseriOS", traits: traits)
```

Use Segment’s `alias` method for user identification.

```swift
analytics.alias(newId: "new_id")
```

## Track Events

Segment's `track` API maps to CleverTap's `recordEvent`.

```swift
let properties: [String: Any] = [
    "eventproperty": "eventPropertyValue",
    "testPlan": "Pro",
    "testEvArr": [1, 2, 3]
]
analytics.track(name: "testEvent", properties: properties)
```

## Track a Charged Event

Events tracked using the name `Order Completed` map to CleverTap’s **Charged Event**.

```swift
let orderProperties: [String: Any] = [
    "checkout_id": "fksdjfsdjfisjf9sdfjsd9f",
    "order_id": "50314b8e9bcf000000000000",
    "affiliation": "Google Store",
    "total": 30,
    "revenue": 25,
    "currency": "USD",
    "products": [
        [
            "product_id": "507f1f77bcf86cd799439011",
            "sku": "45790-32",
            "name": "Monopoly: 3rd Edition",
            "price": 19,
            "quantity": 1,
            "category": "Games"
        ],
        [
            "product_id": "505bd76785ebb509fc183733",
            "sku": "46493-32",
            "name": "Uno Card Game",
            "price": 3,
            "quantity": 2,
            "category": "Games"
        ]
    ]
]
analytics.track(name: "Order Completed", properties: orderProperties)
```

## Log Screen Views

Segment's `screen` API maps to CleverTap's `recordScreenView`.

```swift
analytics.screen(title: "Test Screen")
```

# CleverTap Features

Beyond event forwarding, CleverTap offers engagement tools such as App Inbox (send personalized messages), In-App Messaging (show contextual messages), Push Notifications (re-engage users outside the app), and Native Display (personalize on-screen elements dynamically). For more details, see the [CleverTap iOS SDK documentation](https://developer.clevertap.com/docs/ios).

# FAQ

Find answers to the most common integration questions.

**Q: Do I need to call CleverTap APIs directly?**  
A: No. Use the Analytics-Swift APIs (`track`, `identify`, `screen`), and the CleverTap Segment plugin will automatically map them to CleverTap.

**Q: Can I mix Segment + CleverTap APIs?**  
A: Yes. Events sent through Segment will flow into CleverTap. If needed, you can also call CleverTap APIs directly in addition to Segment.

# Advanced Features

To add advanced features such as push notifications, In-App messages, deep linking, and personalization to your iOS application, refer to [iOS Advanced Settings](https://developer.clevertap.com/docs/advanced-options-ios).