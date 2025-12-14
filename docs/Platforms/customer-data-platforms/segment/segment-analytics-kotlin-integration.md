---
title: Segment Analytics-Kotlin Integration
excerpt: >-
  Learn how to integrate Segment's Analytics-Kotlin SDK with CleverTap to
  capture events, user traits, and engagement data in your Android application.
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

The Segment Analytics-Kotlin SDK enables you to track events and user traits across your Android application. When you configure CleverTap as a destination, you can forward this data to CleverTap and unlock engagement features such as personalization, push notifications, and In-App messaging.

This integration allows you to:

* Forward events from Analytics-Kotlin into CleverTap.
* Record user traits and behaviors using the standard Segment `track`, `identify`, and `screen` APIs.
* Leverage CleverTap features such as Push Notifications, App Inbox, and In-App Messaging.

# Prerequisites for Integration

Before you begin, ensure the following:

* A Segment account
* A CleverTap account

# Integration

The integration between Segment Analytics-Kotlin SDK and CleverTap involves configuring CleverTap as a destination in your Segment workspace and forwarding event data to CleverTap from your Android application.

This process includes the following steps:

1. [Add Destination in Segment](doc:segment-analytics-kotlin-integration#add-destination-in-segment)
2. [Install Analytics-Kotlin and CleverTap Plugin](doc:segment-analytics-kotlin-integration#install-analytics-kotlin-and-clevertap-plugin)
3. [Initialize Analytics-Kotlin with CleverTap Plugin](doc:segment-analytics-kotlin-integration#initialize-analytics-kotlin-with-clevertap-plugin)

## Add Destination in Segment

Destinations are the business tools or applications to which Segment forwards your data. Adding Destinations allows you to act on your data and learn more about your customers in real-time.

To configure CleverTap as a destination in Segment:

1. Go to *Destinations* in Segment application.
2. Click on **Add Destination** and select *CleverTap*.
3. In the *Destination Catalog/CleverTap* page, click on *Configure CleverTap*.
4. Add CleverTap *Account ID* and *Account Token*, which you can find on the CleverTap Dashboard under *Settings* page.
5. Select the appropriate region from the *Region* dropdown. To identify the region of your CleverTap account, refer to the following table:

| CleverTap Dashboard URL                                                                              | Region            |
| :--------------------------------------------------------------------------------------------------- | :---------------- |
| [https://in1.dashboard.clevertap.com/login.html#/](https://in1.dashboard.clevertap.com/login.html#/) | IN                |
| [https://sg1.dashboard.clevertap.com/login.html#/](https://sg1.dashboard.clevertap.com/login.html#/) | SG                |
| [https://us1.dashboard.clevertap.com/login.html#/](https://us1.dashboard.clevertap.com/login.html#/) | US                |
| [https://eu1.dashboard.clevertap.com/login.html#/](https://eu1.dashboard.clevertap.com/login.html#/) | None              |
| [https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html#/) | Indonesia         |
| [https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html#/) | Middle East (UAE) |

6. Turn on the toggle at the top of the *Settings* page to enable the destination.

<Image title="Enable the CleverTap Settings" alt="Enable the CleverTap Settings" align="center" width="75% " border={true} src="https://files.readme.io/de3fc69-2023-03-15_17-34-07.png">
  Enable the CleverTap Settings
</Image>

You can integrate CleverTap from the server-side or mobile destination (iOS or Android). Select mobile destinations to use CleverTap push notifications or In-App notifications.

## Install Analytics-Kotlin and CleverTap Plugin

Add required dependencies to your Gradle build file so your app can use Analytics-Kotlin and the CleverTap plugin.

```kotlin
dependencies {
    // Segment Analytics-Kotlin SDK
    implementation("com.segment.analytics.kotlin:android:1.21.0")
    
    // CleverTap SDK
    implementation("com.clevertap.android:clevertap-android-sdk:7.5.1")
    
    // CleverTap Destination Plugin for Analytics-Kotlin
    implementation("com.clevertap.android:clevertap-segment-kotlin:1.0.0")
}
```

## Initialize Analytics-Kotlin with CleverTap Plugin

Set up Analytics-Kotlin in your application class and attach the CleverTap destination plugin.

```kotlin
class MyApplication : Application() {
    companion object {
        private const val WRITE_KEY = "your_segment_write_key"
    }

    override fun onCreate() {
        super.onCreate()
        CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.VERBOSE)
        Analytics.debugLogsEnabled = true
        ActivityLifecycleCallback.register(this)

        // Initialize Analytics-Kotlin
        analytics = Analytics(WRITE_KEY, applicationContext).apply {
            add(plugin = CleverTapDestination(applicationContext, ::cleverTapReady, ::cleverTapInitFailed))
        }
    }

    private fun cleverTapInitFailed(reason: String) {
        // CleverTap initialization failed, handle gracefully
    }

    private fun cleverTapReady(cleverTapInstance: CleverTapAPI) {
        // Access CleverTap instance for App Inbox, Native Display, and so on.
    }
}
```

| Parameter        | Description                                                                                                                                          |
| :--------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------- |
| `YOUR_WRITE_KEY` | From the Segment dashboard, go to *Connections > Sources*. Select the source and then go to *Settings* > *API Keys* to find the *Write Key* details. |

# Track Events and User Data

To use Analytics-Kotlin APIs to record events, capture user traits, and log screen views, all of which are forwarded to CleverTap.

## Track Events

Track user actions in your application and forward them to CleverTap.

### Custom Event

Send a simple event with properties.

```kotlin
val properties = HashMap<String, Any>()
properties["category"] = "Electronics"
properties["price"] = 299.99
properties["quantity"] = 2

analytics.track("Product Purchased", properties)
```

### Charged Event

Use the `Order Completed` event to track purchases, and map to CleverTap’s **Charged Event**.

```kotlin
val product1 = hashMapOf("id" to "product_001", "sku" to "SKU001", "price" to 100.0)
val product2 = hashMapOf("id" to "product_002", "sku" to "SKU002", "price" to 200.0)

val products = arrayListOf(product1, product2)

val properties = hashMapOf(
    "orderId" to "ORDER_12345",
    "revenue" to 300.0,
    "products" to products
)

analytics?.track("Order Completed", properties)
```

## Record User Attributes

Associate user information with profiles in CleverTap using `identify`.

```kotlin
val userId = "user_123"
val traits = HashMap<String, Any>()
traits["name"] = "John Doe"
traits["email"] = "john@example.com"
traits["phone"] = "+1234567890"
traits["gender"] = "M"
traits["age"] = 30

analytics?.identify(userId, traits)
```

Alias a user to merge or update identities.

```kotlin
analytics?.alias("new_user_id")
```

### Log Screen Views

Track which screens users visit in your application.

```kotlin
analytics?.screen("Home Screen")
```

# API Mapping

The following table shows how Segment Analytics-Kotlin APIs map to CleverTap APIs.

| Analytics-Kotlin API       | CleverTap Equivalent   | Description                   |
| -------------------------- | ---------------------- | ----------------------------- |
| `track`                    | `pushEvent`            | Records custom events         |
| `track("Order Completed")` | `Charged Event`        | Purchase/transaction event    |
| `identify`                 | `onUserLogin`          | Sets user profile information |
| `alias`                    | `onUserLogin` (new ID) | Links new user ID to profile  |
| `screen`                   | `recordScreen`         | Tracks screen views           |

# CleverTap Features

Beyond event forwarding, CleverTap offers engagement tools such as App Inbox (send personalized messages), In-App Messaging (show contextual messages), Push Notifications (re-engage users outside the app), and Native Display (personalize on-screen elements dynamically). For more details, see the [CleverTap Android SDK documentation](https://developer.clevertap.com/docs/android).

# FAQ

Find answers to the most common integration questions.

**Q: Do I need to call CleverTap APIs directly?**\
A: No. Use the Analytics-Kotlin APIs (`track`, `identify`, `screen`), and the CleverTap Segment plugin will automatically map them to CleverTap.

**Q: Can I mix Segment + CleverTap APIs?**\
A: Yes. Events sent through Segment will flow into CleverTap. If needed, you can also call CleverTap APIs directly in addition to Segment.

# Advanced Features

To add advanced features such as push notifications, In-App messages, deep linking, and personalization to your Android application, refer to [Android Advanced Settings](doc:android-advanced-features).
