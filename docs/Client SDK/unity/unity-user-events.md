---
title: Unity User Events
excerpt: Learn how to record user events with Unity and Unity Native SDK.
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

CleverTap records a user event whenever a user performs an action in your app. Events are tagged with a name and can optionally include key-value properties. These properties allow for segmentation, targeting, and personalized engagement.

## Record Event

Use this method to track basic In-App actions, such as button clicks, screen views, or session starts, without any additional data.

```csharp
// Record a basic event with no associated properties
CleverTap.RecordEvent("ButtonClicked");
// Note: For SDK v2.4.2 or below, use CleverTapBinding instead.
```

## Record Event with Properties

Use this method to log additional context with the event, such as product details, user preferences, or campaign source.

```csharp
// Record an event with contextual event properties
Dictionary<string, object> props = new Dictionary<string, object>();
props.Add("ProductName", "Wireless Mouse");
props.Add("Category", "Electronics");
props.Add("Price", 29.99);

CleverTap.RecordEvent("ProductViewed", props);
```

If the same event is recorded multiple times with different values, CleverTap stores each event instance separately. This allows you to analyze trends or variations over time.

## Record a Purchase Event (Charged Event)

Use this method to track successful transactions. Include payment metadata (such as amount and currency) and a list of purchased items (including name, price, and quantity).

```c C#
// Record a charged event for a completed transaction
Dictionary<string, object> chargeDetails = new Dictionary<string, object>();
chargeDetails.Add("amount", 150.00);
chargeDetails.Add("currency", "USD");
chargeDetails.Add("payment_mode", "Credit Card");

Dictionary<string, object> item1 = new Dictionary<string, object>();
item1.Add("product_name", "Book");
item1.Add("price", 50.00);
item1.Add("quantity", 1);

Dictionary<string, object> item2 = new Dictionary<string, object>();
item2.Add("product_name", "Plant");
item2.Add("price", 100.00);
item2.Add("quantity", 2);

List<Dictionary<string, object>> items = new List<Dictionary<string, object>>();
items.Add(item1);
items.Add(item2);

CleverTap.RecordChargedEventWithDetailsAndItems(chargeDetails, items);
```

You can view recorded events in the CleverTap dashboard. Navigate to *Analytics* > *Events* to inspect event names, properties, and user counts.

For a complete implementation guide, refer to [Events](doc:events).
