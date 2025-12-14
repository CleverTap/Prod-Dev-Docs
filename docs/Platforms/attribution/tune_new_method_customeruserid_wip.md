---
title: Tune_new_method_CustomerUserId_WIP
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
CleverTap helps you to track your app installs via Tune. This integration requires changes to both the Tune Dashboard and your Android/iOS app for successful integration.

# Step 1: Add CleverTap Credentials to Tune Dashboard

Go to the Tune dashboard and click on Manage Partners.

![624](https://files.readme.io/8844c7d-tune.png "tune.png")

Search and enable CleverTap as an integrated partner. Click on the CleverTap Logo. Enter your credentials including your CleverTap Account ID and Token. 

Enable View-through Attribution in Attribution Setting. 

Enable Install Postback which allows tune to report data to CleverTap API. This is under Postbacks (Postbacks Templates) > Templates > Generic.

# Step 2: Integration

**Android App**

Follow the [Android Quick Start Guide](doc:android-quickstart-guide) to understand how to instantiate a CleverTap instance.

If you haven’t done the Tune integration for Android apps, follow [this guide](https://developers.tune.com/sdk/android-quick-start/).

In your Android app, add the following code in your Application Class within the `onCreate()` method.

### SDK version 4.2.0 and above

The `getCleverTapAttributionIdentifier` method is deprecated in Clevertap Android SDKs version 4.2.0 and above. Use the new `getCleverTapID` method to get the CleverTap ID on the `OnInitCleverTapIDListener` callback to  set `setUserId` in Tune.

```java
cleverTapInstance.getCleverTapID(new OnInitCleverTapIDListener() {
   @Override
   public void onInitCleverTapID(final String cleverTapID) {
   // Callback on main thread
   Tune.init(this, "your_advertiser_ID", "your_conversion_key"); 
   Tune.getInstance().setUserId(cleverTapID);
   }
});
```
```kotlin
cleverTapInstance?.getCleverTapID {
   // Callback on main thread
   Tune.init(this, "your_advertiser_ID", "your_conversion_key")
   Tune.getInstance().setUserId(it)

}
```

### SDK version 4.1.1 and below

```java
String attributionId = cleverTapInstance.getCleverTapAttributionIdentifier();
Tune.init(this, "your_advertiser_ID", "your_conversion_key"); 
Tune.getInstance().setUserId(attributionId);
```
```kotlin
Tune.init(this, "your_advertiser_ID", "your_conversion_key"); 
Tune.getInstance().setUserId(cleverTapInstance?.cleverTapAttributionIdentifier)
```

**iOS App**

In your AppDelegate.m file, add the following code.

```objectivec
[Tune initializeWithTuneAdvertiserId:@"your_advertiser_ID" tuneConversionKey:@"your_conversion_key"];
[Tune  setUserID:[[CleverTap sharedInstance] profileGetCleverTapAttributionIdentifier]];
```

If you haven’t done the Tune integration for iOS app, follow [this guide](https://developers.tune.com/sdk/ios-quick-start/).

# Step 3: View Data in the CleverTap Dashboard

After you integrate, Tune will now push data in your CleverTap dashboard. You can view it under event UTM Visited filtered by event property, UTM\_source, UTM\_medium or UTM\_campaign.
