---
title: Android User Events
excerpt: Learn how to record events.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: link
      title: Android Push
      url: https://developer.clevertap.com/docs/android-push
    - type: link
      title: Huawei Push Integration
      url: https://developer.clevertap.com/docs/clevertap-huawei-push-integration
    - type: link
      title: Baidu Push Integration
      url: https://developer.clevertap.com/docs/baidu-push-notifications
    - type: link
      title: Xiaomi Push Integration
      url: https://developer.clevertap.com/docs/xiaomi-push-notifications
    - type: basic
      slug: push-templates-android
      title: Android Push Templates
    - type: link
      title: Android In-App
      url: https://developer.clevertap.com/docs/android-in-app-notifications
    - type: link
      title: Android App Inbox
      url: https://developer.clevertap.com/docs/android-app-inbox
    - type: link
      title: Android Native Display
      url: https://developer.clevertap.com/docs/android-native-display
    - type: link
      title: Android Product Experiences
      url: https://developer.clevertap.com/docs/product-experiences-android
    - type: link
      title: Multi-Instance Support Android
      url: https://developer.clevertap.com/docs/multi-instance-support-android
    - type: link
      title: Android Advanced Settings
      url: https://developer.clevertap.com/docs/android-advanced-settings
    - type: link
      title: Android Resources
      url: https://developer.clevertap.com/docs/android-resources
---
# User Events

A user event is an action a user takes in your mobile application. CleverTap records the event on the user profile using an event name and optional associated key:value-based event properties. You can then segment users, target, and personalize messaging based on the event name and specific event properties.

Below is an example of recording a user event called *Product Viewed*: 

```java JAVA
// event without properties
cleverTap.pushEvent("Product viewed");
```
```kotlin Kotlin
// event without properties
clevertapDefaultInstance?.pushEvent("Product viewed")
```

To record a list of items viewed, use the Items collection. Along with the product name, you can also add properties like size, color, category etc.  An example of recording a *Product Viewed* user event with properties is shown as follows:

```java JAVA
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
 *
 * Date object
 * When a property value is of type Date, the date and time are both recorded to the second.
 * This can be later used for targeting scenarios.
 * For e.g. if you are recording the time of the flight as an event property,
 * you can send a message to the user just before their flight takes off.
 */
```
```kotlin Kotlin
// event with properties
val prodViewedAction = mapOf(
"Product Name" to "Casio Chronograph Watch",
"Category" to "Mens Accessories",
"Price" to 59.99,
"Date" to Date())
clevertapDefaultInstance?.pushEvent("Product viewed", prodViewedAction)
/**
* Data types
* The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,
* a Float, a Character, a String, or a Boolean.
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

Charged is a special event as it lets you record key details, including items sold, categories, transaction amount, transaction ID, and user information. 

Recording a purchase identifies a user as a customer in CleverTap, letting you compare funnel reports between customers and non-customers.

### Recording Items Sold

You should use the items collection to record a list of items sold. In addition to the product name, you can add properties like size, color, category, and so on. 

### Recording the Transaction Amount

The transaction total or subscription charge should be recorded in an event property called Amount.

```java
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
```kotlin
fun recordPurchase(cleverTapAPI: CleverTapAPI){
        val charges = hashMapOf<String,Any>("Total Number Of Items" to 3, "Total Amount" to 400)         
        val items = arrayListOf(
                hashMapOf<String,Any>("Item name" to "Shoes", "Number of Items" to 1, "Amount" to 200),
                hashMapOf<String,Any>("Item name" to "Watch", "Number of Items" to 1, "Amount" to 100),
                hashMapOf<String,Any>("Item name" to "Biscuit", "Number of Items" to 1, "Amount" to 100),
        )
        cleverTapAPI.pushChargedEvent(charges,items)
}
```

For a complete guide to recording events, refer to [Events](https://developer.clevertap.com/docs/events).

## Record Screen

Record screen changes on the device. For example, whenever the user switches from the home screen to the payment screen.

```java
cleverTapAPI.recordScreen("Home")
```
```kotlin
cleverTapAPI.recordScreen("Home")
```

## Record Errors

 Internally records an *Error Occurred* event, which can be viewed in the dashboard.

```java
cleverTapAPI.pushError("Payments Api Failed",501)
```
```kotlin
cleverTapAPI.pushError("Payments Api Failed",501)
```
