---
title: Flutter User Events
excerpt: Understand how to record events with Flutter SDK.
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

A User Event is an action that a user takes in your mobile application. CleverTap records the event on the User Profile, using an event name and optional associated key:value-based event properties. You can then segment users, and target and personalize messaging based on both the event name and specific event properties.

An example of recording a Product Viewed user event is shown as follows:

```javascript Dart
CleverTapPlugin.recordEvent("Product Viewed",{});
```

An example of recording an event with name "Flutter Event" user event along with event data is shown as follows:

```javascript Dart
var eventData = {
    // Key:    Value
   'first': 'partridge',
   'second': 'turtledoves'
};
CleverTapPlugin.recordEvent("Flutter Event", eventData);
```

> 📘 Nested Objects Support
>
> To update profile properties using nested objects, refer to [Ingesting Nested Objects via CleverTap SDK](https://staging.docs.dev.clevertap.net/docs/ingesting-nested-objects-via-clevertap-sdk).

## Record Charged event

A sample example to log the events involving transactions.

```javascript Dart
var item1 = {
    // Key:    Value
    'name': 'thing1',
    'amount': '100'
};
var item2 = {
   // Key:    Value
   'name': 'thing2',
   'amount': '100'
};
var items = [item1, item2];
var chargeDetails = {
    // Key:    Value
    'total': '200',
    'payment': 'cash'
};
CleverTapPlugin.recordChargedEvent(chargeDetails, items);
```

Events help you understand how your users interact with your app. CleverTap tracks certain common user events automatically while giving you the flexibility to record business-specific events.

For a complete guide to recording events, refer to [Events](https://developer.clevertap.com/docs/events).
