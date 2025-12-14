---
title: iOS User Events
excerpt: ''
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

An example of recording a _Product Viewed_ user event is shown as follows:

```swift
// event without properties
CleverTap.sharedInstance()?.recordEvent("Product viewed")
```
```objectivec
// event without properties
[[CleverTap sharedInstance] recordEvent:@"Product viewed"];
```

To record a list of items viewed, use the Items collection. Along with the product name, you can also add properties like size, color, category etc.  An example of recording a _Product Viewed_ user event with properties is shown as follows:

```swift
// event with properties
let props = [
    "Product name": "Casio Chronograph Watch",
    "Category": "Mens Accessories",
    "Price": 59.99,
    "Date": NSDate()
] as [String : Any]

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
```objectivec
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

Events help you understand how your users interact with your app. CleverTap tracks certain common user events automatically while giving you the flexibility to record business-specific events.

## Recording Customer Purchases

You should record transactions or purchases in CleverTap using a special event called Charged.

### Charged Special Event

Charged is a special event as it allows you to capture key details, including items sold, categories, transaction amount, transaction ID, and user information. 

Recording a purchase identifies a user as a customer in CleverTap, letting you compare funnel reports between customers and non-customers.

### Recording Items Sold

To record a list of items sold, you should use the items collection. Along with the product name, you can also add properties like size, color, category etc. 

### Recording the Transaction Amount

The transaction total or subscription charge should be recorded in an event property called Amount.

```swift
func recordUserChargedEvent() {
        //charged event
        let chargeDetails = [
            "Amount": 300,
            "Payment mode": "Credit Card",
            "Charged ID": 24052013
            ] as [String : Any]
        
        let item1 = [
            "Category": "books",
            "Book name": "The Millionaire next door",
            "Quantity": 1
            ] as [String : Any]
        
        let item2 = [
            "Category": "books",
            "Book name": "Achieving inner zen",
            "Quantity": 1
            ] as [String : Any]
        
        let item3 = [
            "Category": "books",
            "Book name": "Chuck it, let's do it",
            "Quantity": 5
            ] as [String : Any]
        
        CleverTap.sharedInstance()?.recordChargedEvent(withDetails: chargeDetails, andItems: [item1, item2, item3])
    }
```
```objectivec
- (void)recordUserChargedEvent {
    // charged event
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
}
```

For an example of recording a custom event from a watchOS app, refer to [SwiftWatchOS Extension](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftStarter/SwiftWatchOS%20Extension/InterfaceController.swift). For an example of recording an event from an App Extension, refer to [Notification Service](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/SwiftStarter/NotificationService/NotificationService.swift).

For a complete guide to recording events, refer to [Events](https://developer.clevertap.com/docs/events).