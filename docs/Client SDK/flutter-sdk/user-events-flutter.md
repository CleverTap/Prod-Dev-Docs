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
[block:code]
{
  "codes": [
    {
      "code": "CleverTapPlugin.recordEvent(\"Product Viewed\",{});",
      "language": "javascript",
      "name": "Dart"
    }
  ]
}
[/block]
An example of recording an event with name "Flutter Event" user event along with event data is shown as follows:
[block:code]
{
  "codes": [
    {
      "code": "var eventData = {\n    // Key:    Value\n   'first': 'partridge',\n   'second': 'turtledoves'\n};\nCleverTapPlugin.recordEvent(\"Flutter Event\", eventData);",
      "language": "javascript",
      "name": "Dart"
    }
  ]
}
[/block]
## Record Charged event
A sample example to log the events involving transactions.
[block:code]
{
  "codes": [
    {
      "code": "var item1 = {\n    // Key:    Value\n    'name': 'thing1',\n    'amount': '100'\n};\nvar item2 = {\n   // Key:    Value\n   'name': 'thing2',\n   'amount': '100'\n};\nvar items = [item1, item2];\nvar chargeDetails = {\n    // Key:    Value\n    'total': '200',\n    'payment': 'cash'\n};\nCleverTapPlugin.recordChargedEvent(chargeDetails, items);\n\n",
      "language": "javascript",
      "name": "Dart"
    }
  ]
}
[/block]
Events help you understand how your users interact with your app. CleverTap tracks certain common user events automatically while giving you the flexibility to record business-specific events.

For a complete guide to recording events, refer to [Events](https://developer.clevertap.com/docs/events).