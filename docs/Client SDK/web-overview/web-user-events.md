---
title: Web User Events
excerpt: Learn how to record web user events.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# User Events

A user event is an action a user takes on your website. CleverTap records the event on the user profile using an event name and optional associated key:value-based event properties. You can then segment users, target, and personalize messaging based on the event name and specific event properties.

An example of recording a _Product Viewed_ user event is shown as follows:

```Text web
// event without properties
clevertap.event.push("Product viewed");
```

To record a list of items viewed, use the Items collection. Along with the product name, you can also add properties like size, color, category etc.  An example of recording a _Product Viewed_ user event with properties is shown as follows:

```Text web
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

Events help you understand how your users interact with your app. CleverTap tracks certain common user events automatically while giving you the flexibility to record business-specific events.

## Recording Customer Purchases

You should record transactions or purchases in CleverTap using a special event called Charged.

### Charged Special Event

Charged is a special event as it allows you to capture key details, including items sold, categories, transaction amount, transaction ID, and user information.

Recording a purchase identifies a user as a customer in CleverTap, letting you compare funnel reports between customers and non-customers.

### Recording Items Sold

To record a list of items sold, you should use the Items collection. Along with the product name, you can also add properties like size, color, category etc. See the code sample below.

### Recording the Transaction Amount

The transaction total or subscription charge should be recorded in an event property called Amount.

```Text web
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

You can personalize the message for customers who purchase multiple items in a single transaction by using [Liquid Tags](https://docs.clevertap.com/docs/events#charged-event-personalization) from the CleverTap dashboard.

### Advantages of the Charged Event

* Helps you identify your customers and how they are using your app or website
* Run campaigns to reward loyal users or get lost customers back
* Measure customer loyalty via running a cohort analysis on repeat purchases
* Analyze paid campaign performance by total revenue earned
* Get revenue insights like – total revenue, number of transactions, count of paying users and much more

For a complete guide to recording events, refer to [Events](https://developer.clevertap.com/docs/events).

# Discarded Events

CleverTap dashboard administrators can mark specific event names as **discarded** via the Schema page. A discarded event name is permanently excluded from recording, all past data for that event is purged, and no future instances are stored.

No action is required in your Web SDK integration to respect this setting. Discarding is an admin-only, dashboard-driven operation.
