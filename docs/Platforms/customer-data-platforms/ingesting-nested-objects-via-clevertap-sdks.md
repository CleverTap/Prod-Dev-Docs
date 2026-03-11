---
title: Ingesting Nested Objects via CleverTap SDKs
excerpt: >-
  Learn how to structure and send nested objects in user profiles and event
  properties using the CleverTap Android and iOS SDKs.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

[Nested objects](https://docs.clevertap.com/docs/nested-objects-for-user-properties-and-custom-event-properties) allows you to group related attributes under a single parent key. This helps structure complex user data, such as preferences, purchase history, and subscription details, into a more organized, readable format.

For example, instead of storing multiple flat properties such as `favoriteCategory`, `wishlistItems`, and `loyaltyPoints` separately at the profile level, you can group them under a logical parent object such as `ShoppingProfile`. This keeps related attributes organized and makes the data structure easier to read and maintain.

Nested object support is available in the CleverTap Android and iOS SDKs, and can be used when sending:

* User profile updates
* Event properties

The following sections demonstrate how to ingest nested objects using the Android and iOS SDKs.

* [Nested Objects For Android SDK](doc:ingesting-nested-objects-via-clevertap-sdks#use-nested-objects-with-android-sdk)
* [Nested Objects For iOS SDK](doc:ingesting-nested-objects-via-clevertap-sdks#use-nested-objects-with-ios-sdk)

# Supported SDK Versions

Nested objects are supported in the following SDK versions:

* [CleverTap Android SDK version 7.8.0 and above](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-780-january-22-2026)
* [CleverTap iOS SDK version 7.5.0 and above](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-750-february-9-2026)

# Use Nested Objects with Android SDK

This section explains how to use nested objects with the CleverTap Android SDK.

## Create User Profile with Nested Objects

You can send nested objects when creating or updating a user profile.

#### Example: Send Nested Objects Using `onUserLogin`

The following example shows how to send a nested `ShoppingProfile` object using `onUserLogin()`.

```kotlin
val shoppingProfile = hashMapOf(
    "favoriteCategories" to arrayListOf("Electronics", "Fashion", "Home"),
    "recentPurchases" to arrayListOf(
        hashMapOf(
            "item" to "Laptop",
            "date" to Calendar.getInstance().apply {
                set(2024, Calendar.NOVEMBER, 20)
 }.time,
            "amount" to 1299.99
 ),
        hashMapOf(
            "item" to "Headphones",
            "date" to Calendar.getInstance().apply {
                set(2024, Calendar.NOVEMBER, 18)
 }.time,
            "amount" to 199.99
 )
 ),
    "wishlist" to arrayListOf("Smart Watch", "Camera", "Desk Chair"),
    "loyaltyPoints" to 2500
)

val profileUpdate = hashMapOf<String, Any>(
    "Identity" to "USER_12345",
    "ShoppingProfile" to shoppingProfile
)

cleverTapAPI?.onUserLogin(profileUpdate)
```

## Update Nested Objects in User Profile

After a user logs in, you can update nested profile properties using `pushProfile()`.

The following example updates values inside the nested `ShoppingProfile` object in the user profile:

```kotlin
val updatedShoppingProfile = hashMapOf<String, Any>(
    "preferences" to hashMapOf(
        "wishlist" to arrayListOf(
            "Camera",
            "Desk Chair",
            "Gaming Mouse"
        )
    ),
    "account" to hashMapOf(
        "rewards" to hashMapOf(
            "loyaltyPoints" to 3000
        )
    )
)

val profileUpdate = hashMapOf<String, Any>(
    "ShoppingProfile" to updatedShoppingProfile
)

cleverTapAPI?.pushProfile(profileUpdate)
```

## Send Nested Objects with Events

You can pass nested objects as event properties using `pushEvent()`.

The following example sends a nested `ShoppingProfile` object as part of an event.

```kotlin
val eventProps = hashMapOf<String, Any>(
    "ShoppingProfile" to hashMapOf(
        "preferences" to hashMapOf(
            "wishlist" to arrayListOf(
                "Camera",
                "Desk Chair",
                "Gaming Mouse"
            )
        ),
        "account" to hashMapOf(
            "rewards" to hashMapOf(
                "loyaltyPoints" to 3000
            )
        )
    ),
    "source" to "app"
)

cleverTapAPI?.pushEvent("ProfileUpdated", eventProps)
```

## Increment or Decrement Nested Object Values

You can increment or decrement numeric values inside nested objects using dot notation keys.

The following example increments the `amount` value of the second item in the `recentPurchases` array.

```kotlin
cleverTapAPI?.incrementValue(
    "ShoppingProfile.recentPurchases[1].amount",
    50
)
```

Only numeric fields of type `Integer`, `Float`, or `Double` are supported.

The following code decrements the `amount` value of the second item in the `recentPurchases` array.

```kotlin
cleverTapAPI?.decrementValue(
  "ShoppingProfile.recentPurchases[1].amount",
  50
)
```

# Use Nested Objects with iOS SDK

This section describes the use of nested objects in the CleverTap iOS SDK.

## Create User Profile with Nested Objects

Use onUserLogin() to set profile properties, including nested objects, when a user logs in.

```swift
let shoppingProfile: [String: Any] = [
    "favoriteCategories": ["Electronics", "Fashion", "Home"],
    "recentPurchases": [
        ["item": "Laptop", "date": "2024-11-20", "amount": 1299.99],
        ["item": "Headphones", "date": "2024-11-18", "amount": 199.99]
    ],
    "wishlist": ["Smart Watch", "Camera", "Desk Chair"],
    "loyaltyPoints": 2500
]

let profileUpdate: [String: Any] = [
    "Identity": "USER_12345",
    "ShoppingProfile": shoppingProfile
]

CleverTap.sharedInstance()?.onUserLogin(profileUpdate as [String: AnyObject])
```

## Send Nested Objects with Events (iOS)

You can include nested objects as event properties using `recordEvent()`.

```swift
let eventProps: [String: Any] = [
    "ShoppingProfile": shoppingProfile,
    "source": "app"
]

CleverTap.sharedInstance()?.recordEvent("ProfileUpdated", withProps: eventProps)
```

## Update Nested Objects in User Profile

After a user logs in, you can update nested profile properties using `profilePush()`.

```swift
let updatedShoppingProfile: [String: Any] = [
    "loyaltyPoints": 3000,
    "wishlist": ["Camera", "Desk Chair", "Gaming Mouse"]
]

let profileUpdate: [String: Any] = [
    "ShoppingProfile": updatedShoppingProfile
]

CleverTap.sharedInstance()?.profilePush(profileUpdate)
```

## Increment or Decrement Nested Object Values

You can increment or decrement numeric values inside nested objects using dot-notation keys.

### Dot Notation Rules

* Use dot notation (`.`) to reference nested object keys.
* Use array index notation `[index]` to access array elements.
* Only numeric fields of type `Integer`, `Float`, or `Double` are supported.

#### Example: Increment Nested Value

The following code increases the `amount` value of the item at index `[1]` (the second item) in the `recentPurchases` array by 500:

```swift
CleverTap.sharedInstance()?.profileIncrementValue(by: NSNumber(value: 500), forKey: "ShoppingProfile.recentPurchases[1].amount")
```

#### Example: Decrement Nested Value

The following code decreases the `amount` value of the item at index `[1]` (the second item) in the `recentPurchases` array by `50`:

```swift
CleverTap.sharedInstance()?.profileDecrementValue(by: NSNumber(value: 50), forKey: "ShoppingProfile.recentPurchases[1].amount")
```

# Validation Rules for Nested Objects

The CleverTap SDK enforces validation rules to ensure reliable ingestion and maintain data consistency.

When sending nested objects in profile or event properties, ensure that your payload complies with the following constraints.

## Size and Structure Limits

The following limits apply when ingesting nested objects in profile and event properties.

| Parameter                           | Limit          |
| ----------------------------------- | -------------- |
| Maximum key length                  | 120 characters |
| Maximum value size                  | 512 characters |
| Maximum nesting depth               | 3 levels       |
| Maximum keys in an object           | 5              |
| Maximum array length                | 100            |
| Maximum key-value pairs per payload | 100            |

## Restricted Characters

Certain characters are restricted in nested object keys and values. Ensure that your payload does not include these characters, as they may cause ingestion failures or parsing issues.

#### Restricted Key Characters

Keys must not contain the following characters:

```
: $ ' " \
```

These characters are restricted because they may interfere with ingestion parsing or internal processing.

#### Restricted Value Characters

Values must not contain the following characters:

```
' " \
```

Avoid including these characters in string values to ensure successful ingestion and processing.

## Event Name Restrictions

When sending nested objects within event properties, the event name must also comply with CleverTap's event naming rules.

* Maximum event name length: 512 characters.
* Event names cannot contain the following characters:

```
. : $ ' " \
```

### Restricted Fields and Events

The SDK enforces default restrictions on certain event names and profile fields.

#### Restricted Event Names

The following event names are reserved and cannot be used:

* Stayed
* Notification Clicked
* Notification Viewed
* UTM Visited
* Notification Sent
* App Launched
* wzrk_d
* App Uninstalled
* Notification Bounced
* Geofence Entered
* Geofence Exited
* SC Outgoing
* SC Incoming
* SC End
* SC Campaign Opt Out

#### Restricted Multi-Value Profile Fields

For the following profile fields, `onUserLogin()` and `pushProfile()` (Android) or `profilePush()` (iOS) must not send an array or object at the top level:

* name
* email
* education
* married
* dob
* gender
* phone
* age
* fbid
* gpid
* birthday
* identity

# Best Practices

Follow these best practices when working with nested objects:

* Keep nesting shallow and avoid unnecessary depth. Nested objects support a maximum nesting depth of three levels.
* Use clear and consistent key names to ensure your data structure remains easy to understand and maintain.
* Avoid storing highly volatile data in user profiles, such as frequently changing activity or session data.
* Use events to capture transactional or high-volume data instead of storing it in user profiles.
* Validate payload size and structure before sending data to ensure it complies with CleverTap ingestion limits.

# Troubleshooting

**Issue:** Nested object data does not appear in profiles or events.

**Causes**

This issue can occur for the following reasons:

* The SDK version used does not support nested objects.
* The payload exceeds the allowed size, nesting depth, or key limits for nested objects.
* Restricted characters are used in nested object keys or values.

**Resolution**

To resolve this issue, do the following:

* Ensure that your application uses a supported SDK version for nested objects.
* Verify that the payload structure complies with the limits for nested object validation.
* Check nested object keys and values for restricted characters.
* Review SDK logs to identify ingestion or validation errors.
