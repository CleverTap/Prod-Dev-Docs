---
title: Android User Events
description: >-
  A user event is an action a user takes in your mobile application. CleverTap
  records the event on the user profile using an event name and optional
  associated key:value-based event properties. You can then segment users,
  target, and personalise messaging based on the event name and specific event
  properties.
hidden: true
recipe:
  color: '#f43201'
  icon: 🦉
---
```java Java
// event with properties
HashMap<String, Object> prodViewedAction = new HashMap<String, Object>();
prodViewedAction.put("Product Name", "Casio Chronograph Watch");
prodViewedAction.put("Category", "Mens Accessories");
prodViewedAction.put("Price", 59.99);
prodViewedAction.put("Date", new java.util.Date());

clevertapDefaultInstance.pushEvent("Product viewed", prodViewedAction);

/**
 * Data types
 * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 * Date object
 * When a property value is of type Date, the date and time are both recorded to the second.
 * This can be later used for targeting scenarios.
 * For e.g. if you are recording the time of the flight as an event property,
 * you can send a message to the user just before their flight takes off.
 */
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

```json Response Example
{"success":true}
```

# Record User Events with Properties

<!-- java@2-8 -->

In this step, we explain how to record user events with Properties. 
CleverTap tracks certain common user events automatically while giving you the flexibility to record business-specific events.

# Send Transactions/Purchases Events

<!-- java@20-50 -->

*Charged* event type is used to record transactions or purchases in CleverTap. 
The Items collection can be used to record a list of items sold. 
In this step, we explain how to record a list of items sold using Item Collection.