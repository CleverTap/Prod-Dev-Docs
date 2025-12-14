---
title: Unreal User Event
excerpt: Learn how to record user events with CleverTap Unreal SDK
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Record User Events

Record user behavior in your Unreal Engine app by logging custom and transactional events using the CleverTap Unreal SDK. This data helps you analyze user activity, segment audiences, and trigger personalized campaigns.

You can log:

* Custom user actions (for example, product views, level completions)
* Purchases and checkout transactions

## Record Custom User Events

Use `PushEvent()` to log any custom in-app activity.

```cplusplus
ICleverTapInstance& CleverTap = GEngine->GetEngineSubsystem<UCleverTapSubsystem>()->SharedInstance();

// Record an event without properties
CleverTap.PushEvent(TEXT("Event No Props"));

// Record an event with additional properties
FCleverTapProperties Actions;
Actions.Add("Product Name", "Casio Chronograph Watch");
Actions.Add("Category", "Mens Accessories");
Actions.Add("Price", 59.99);

CleverTap.PushEvent(TEXT("Product Viewed"), Actions);
```

Event values can be any type that the `FCleverTapPropertyValue` variant type supports (`int32`, `int64`, `double`, `float`, `bool`, `const ANSICHAR*`, `FString`, or `FCleverTapDate`).

## Record Charged Events

Use `PushChargedEvent()` to capture transaction details along with purchased items.

```cplusplus
// Define charge-level properties
FCleverTapProperties ChargeDetails;
ChargeDetails.Add("Amount", 300);
ChargeDetails.Add("Payment Mode", "Credit Card");
ChargeDetails.Add("Charged ID", 24052014);

// Define item details
FCleverTapProperties Item1;
Item1.Add("Product category", "Books");
Item1.Add("Book name", "The Millionaire Next Door");
Item1.Add("Quantity", 1);

FCleverTapProperties Item2;
Item2.Add("Product category", "Books");
Item2.Add("Book name", "Achieving Inner Zen");
Item2.Add("Quantity", 1);

FCleverTapProperties Item3;
Item3.Add("Product category", "Books");
Item3.Add("Book name", "Chuck It, Let's Do It");
Item3.Add("Quantity", 5);

// Add items to array
TArray<FCleverTapProperties> Items = { Item1, Item2, Item3 };

// Record charged event
CleverTap.PushChargedEvent(ChargeDetails, Items);
```

To learn more about event types, structure, and usage in CleverTap, refer to [CleverTap Events](doc:events).
