---
title: Flutter Advanced Features
excerpt: >-
  Learn more about advanced features such as Product Config, Feature Flags,
  Native Display, and more.
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
      title: Flutter Resources
      url: https://developer.clevertap.com/docs/flutter-resources
---
# Overview

This section outlines the advanced features of the CleverTap Flutter SDK. These capabilities provide greater control over debugging, remote configuration, personalization, attribution, and data privacy.

## Debugging

During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level. 

#### Set Debug Level

Debug level can be one of the following:

* `-1`: Disables all debugging. You can set the `debugLevel` to -1 if you want to disable CleverTap logs for the production environment.
* `0` : Default, shows minimal SDK integration related logging.
* `2` : Shows debug output.
* `3` : Shows verbose output.

```javascript Dart
CleverTapPlugin.setDebugLevel(debugLevel);
```

> 🚧 Note
>
> To get the SDK logs in the killed state, add platform-specific debugging. 
>
> * For Android platform, refer to [Android Debugging](doc:android-advanced-features#debugging).
> * For iOS platform, refer to [iOS Debugging](doc:advanced-options-ios#debugging).

## Custom Handling for Pull Notifications

#### Process Push Notification

```javascript Dart
CleverTapPlugin.processPushNotification(data);
```

## Native Display

Native Display helps to display content natively within your app without interrupting the user. It also provides the ability to change the content of your app dynamically and deliver relevant and contextual content to your users.

#### On Display Units Loaded Callback

```javascript Dart
_clevertapPlugin.setCleverTapDisplayUnitsLoadedHandler(onDisplayUnitsLoaded);

void onDisplayUnitsLoaded(List<dynamic>? displayUnits) {
    this.setState(() {
      print("Display Units = " + displayUnits.toString());
   });
}
```

#### Get All Display Units

```javascript Dart
void getAdUnits() {
    this.setState(() async {
      List? displayUnits = await CleverTapPlugin.getAllDisplayUnits();
      print("Display Units Payload = " + displayUnits.toString());

      displayUnits?.forEach((element) {
        var customExtras = element["custom_kv"];
        if (customExtras != null) {
           print("Display Units CustomExtras: " +  customExtras.toString());
         }
      });
     });
}
```

#### Display Unit Viewed Event for ID

```javascript Dart
CleverTapPlugin.pushDisplayUnitViewedEvent(“unitId”);
```

#### Display Unit Clicked Event for ID

```javascript Dart
CleverTapPlugin.pushDisplayUnitClickedEvent(“unitId”);
```

## Product Config

> 📘 Feature Availability
>
> A new and enhanced version of Product Experiences is coming soon. New customers (CleverTap for Enterprise or CleverTap for Startups) who have not enabled the current functionalities can use this feature only when the new version is released. However, the existing users can continue to use it. The [methods](https://developer.clevertap.com/docs/june-2023#flutter-170) for the Product Experiences feature have been deprecated and will be removed from the code by September 2024.

With Product Experiences, you can change the behavior and appearance of your app remotely without an update. This helps you to deliver an in-app personalization experience to your app users and test their response. You can use product config to modify app behavior and feature flags to add or remove features from your app without performing an app store deployment. 

#### Set Product Configuration to Default

You can set in-app default parameter values in the Product Config object so that your app behaves as intended before values are fetched from CleverTap, and so that default values are available if none are set on the dashboard.

```javascript Dart
void productConfigInitialized() {
    print("Product Config Initialized");
    this.setState(() async {
      await CleverTapPlugin.fetch();
    });
}
```

### Fetch and Activate Values

To fetch parameter values from CleverTap, call the fetch() method. Any values you set on the dashboard are fetched and stored in the Product Config object.\
To make fetched parameter values available to your app, call the activate() method.

For cases where you want to fetch and activate values in one call, you can use a fetchAndActivate()request to fetch values from CleverTap and make them available to the app:

#### Fetching Product Configs

By default, the fetch calls are throttled, which is controlled from the CleverTap servers as well as SDK. To know more, see the Throttling section. The default value for minimum fetch interval is set at 60\*10 by default from CleverTap.

```javascript Dart
void fetch() {
    CleverTapPlugin.fetch();
    // CleverTapPlugin.fetchWithMinimumIntervalInSeconds(60*10);
}
```

#### Activate the Most Recently Fetched Product Config

```javascript Dart
void activate() {
    CleverTapPlugin.activate();
}
```

#### Fetch And Activate Product Config

```javascript Dart
void fetchAndActivate() {
    CleverTapPlugin.fetchAndActivate();
 }
```

#### Fetch Minimum Time Interval

```javascript Dart
CleverTapPlugin.setMinimumFetchIntervalInSeconds(interval);
```

#### Get Boolean Key

```javascript Dart
CleverTapPlugin.getProductConfigBoolean(“key”);
```

#### Get String Key

```javascript Dart
CleverTapPlugin.getProductConfigString("StringKey");
```

#### Get Long Key

```javascript Dart
CleverTapPlugin.getProductConfigLong("IntKey");
```

#### Get Double Key

```javascript Dart
CleverTapPlugin.getProductConfigDouble("DoubleKey");
```

#### Get the Last Fetched Timestamp in Milliseconds

```javascript Dart
CleverTapPlugin.getLastFetchTimeStampInMillis();
```

## Feature Flag

Feature flags let you toggle a feature on and off controlled via CleverTap Backend.

#### Get Feature Flag

Feature flags are automatically fetched every time a new app session is created. Once the flags are fetched you can get it via the getters.

```javascript Dart
void featureFlagsUpdated() {
    this.setState(() async {
      bool booleanVar = await CleverTapPlugin.getFeatureFlag("BoolKey", false);
   });
}
```

## App Personalization

#### Enable Personalization

```javascript Dart
CleverTapPlugin.enablePersonalization();
```

#### Disable Personalization

```javascript Dart
CleverTapPlugin.disablePersonalization();
```

## Attributions

#### Push Install Referrer

```javascript Dart
CleverTapPlugin.pushInstallReferrer("source", "medium", "campaign");
```

## GDPR

Manage user opt-in and opt-out preferences and control device-level tracking to ensure compliance with GDPR privacy regulations.

### Set Opt Out

To help you comply with GDPR and user privacy requirements, CleverTap offers a method to control whether a user’s data is tracked.

#### Opt Out from All Tracking and Communication

To completely disable event tracking and stop sending any communication (personalized or generic) to the user.

```javascript Dart
CleverTapPlugin.setOptOut(true); // Will opt out the user to send data to CleverTap
```

#### Opt Out from Tracking Only, but Allow Generic Communication

To disable event tracking, but still allow you to send non-personalized messages (such as broadcast announcements) to the user.

```javascript Dart
CleverTapPlugin.setOptOut(true, true); // Only stop tracking, allow generic communication
```

#### Opt In to Tracking and Communication

To re-enable event tracking and allow sending personalized messages to the user, assuming prior consent was collected.

```javascript Dart
CleverTapPlugin.setOptOut(false); // Will opt in the user to send data to CleverTap
```

### Enable Device Networking Info Reporting

By default, in compliance with GDPR, the CleverTap SDK does not collect device-level information such as Wi-Fi, Bluetooth, network details, or IP address.

To turn this data collection on or off in your Flutter app, use the following method:

```javascript Dart
// Will opt out the user to send Device Network data to CleverTap
CleverTapPlugin.enableDeviceNetworkInfoReporting(false);

// Will opt in the user to send Device Network data to CleverTap
CleverTapPlugin.enableDeviceNetworkInfoReporting(true);
```

### Set Offline

CleverTap Flutter SDK provides a method to set the user as offline. Setting the user offline means that no events/profile information will be sent to CleverTap. You can set the user as offline using the following method:

```javascript Dart
// Set user online (enable real-time data transmission)
CleverTapPlugin.setOffline(false);

// Set user offline (pause data transmission)
CleverTapPlugin.setOffline(true);
```

## Encryption for PII data

PII data is stored across the SDK and could be sensitive information. From `CleverTap Flutter SDK v1.9.0` onwards, you can enable encryption for PII data such as Email, Identity, Name, and Phone.

Currently, two levels of encryption are supported, i.e., None(0) and Medium(1). The encryption level is None by default.

* **None**: All stored data is in plaintext
* **Medium**: PII data is encrypted completely

### Android

The only way to set the encryption level in Android is from the `manifest` file. Add the encryption level in the `manifest` as following:

```xml XML
<meta-data
    android:name="CLEVERTAP_ENCRYPTION_LEVEL"
    android:value="1" />
```

### iOS

The only way to set the encryption level in iOS is from the `info.plist` file. Add the `CleverTapEncryptionLevel` String key to `info.plist` file, where value 1 means Medium and 0 means None. The encryption level is set to **None** if any other value is provided.

### Encryption in Transit

Encrypting data in transit ensures that sensitive personal data, such as Email, Identity, Name, and Phone, is encrypted before transmitting it over the network. This protects user data during transmission and adds an extra layer of security beyond the at-rest encryption.

From CleverTap Android SDK v7.5.0 and iOS SDK v7.3.1 onwards, this feature is supported natively in Flutter SDK v3.5.0 and above. The underlying native SDKs handle encryption and decryption automatically.

To learn more, refer to:

* [Encryption in Transit for Android](doc:android-advanced-features#encryption-in-transit)
* [Encryption in Transit for iOS](doc:advanced-options-ios#encryption-in-transit)
