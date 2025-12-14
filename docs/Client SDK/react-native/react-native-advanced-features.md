---
title: React Native Advanced Features
excerpt: Learn more about React Native advanced features.
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
      title: React Native Resources
      url: https://developer.clevertap.com/docs/react-native-resources
---
# Overview

This section outlines the advanced features of the CleverTap React Native SDK. These capabilities provide greater control over debugging, remote configuration, personalization, attribution, and data privacy.

## Debugging

Set the SDK to DEBUG mode during development to view warnings and other important logs via the platform's logging system. This helps in troubleshooting SDK-related integration issues.

### Set Debug Level

Debug level can be one of the following:

* `-1`: Disables all debugging. You can set the `debugLevel` to -1 if you want to disable CleverTap logs for the production environment.
* `0`: Default, shows minimal SDK integration related logging.
* `2`: Shows debug output.
* `3`: Shows verbose output.

```javascript JavaScript
CleverTap.setDebugLevel(debugLevel);
```

> 🚧 Note
>
> To get the SDK logs in the killed state, add platform-specific debugging. 
>
> * For Android platform, refer to [Android Debugging](doc:android-advanced-features#debugging).
> * For iOS platform, refer to [iOS Debugging](doc:advanced-options-ios#debugging).

## Custom Push Notifications (Android Only)

CleverTap's React Native SDK provides a built-in mechanism for receiving [push notifications](https://developer.clevertap.com/docs/react-native-push-notification) via FCM. However, if you have your own custom Service for managing push notifications, you can inform CleverTap about the service’s token ID whenever it is available. Following are the APIs for supported Platforms:

> 📘 Note
>
> FCM Token, HPS Token, and BPS Token should be fetched from respective push providers after user permissions are granted.

#### Registering FCM Token

```javascript JavaScript
CleverTap.setFCMPushToken("<Your FCM Token>");
```

#### Registering Huawei Token

```javascript
CleverTap.pushRegistrationToken("<Your HMS Token>", {
  type: 'hps',
  prefKey: 'hps_token',
  className: 'com.clevertap.android.hms.HmsPushProvider',
  messagingSDKClassName: 'com.huawei.hms.push.HmsMessageService'
});
```

#### Registering Baidu Token

```javascript
CleverTap.pushRegistrationToken("<Your BPS Token>", {
  type: 'bps',
  prefKey: 'bps_token',
  className: 'com.clevertap.android.bps.BaiduPushProvider',
  messagingSDKClassName: 'com.baidu.android.pushservice.PushMessageReceiver'
});
```

#### Create Notification from Payload

```javascript JavaScript
// Create a notification from payload (when using custom push handler)
CleverTap.createNotification(data);
```

## Native Display

Show personalized banners or visuals using Native Display.

#### On Display Units Loaded

```javascript JavaScript
// Listener for display unit load events
CleverTap.addListener(CleverTap.CleverTapDisplayUnitsLoaded, (data) => {
    // Use the 'data' to render UI or store display units
});
```

#### Get All Display Units

```javascript JavaScript
// Fetch all available display units
CleverTap.getAllDisplayUnits((err, res) => {
    console.log('All Display Units: ', res, err);
});
```

#### Display unit viewed event for ID

```javascript JavaScript
// Track display unit impression
CleverTap.pushDisplayUnitViewedEventForID('Display Unit Id');// Replace "display_unit_id" with the actual ID configured in the CleverTap dashboard.
```

#### Display unit clicked event for ID

```javascript JavaScript
// Track display unit click
CleverTap.pushDisplayUnitClickedEventForID('Display Unit Id');// Replace "display_unit_id" with the actual ID configured in the CleverTap dashboard.
```

## Product Config

> 📘 Feature Availability
>
> A new and enhanced version of Product Experiences is coming soon. New customers (CleverTap for Enterprise or CleverTap for Startups) who have not enabled the current functionalities can use this feature only when the new version is released. However, the existing users can continue to use it. The [methods](https://developer.clevertap.com/docs/may-2023#react-native-110) for the Product Experiences feature have been deprecated and will be removed from the code by September 2024.

#### Set Product Configuration to Default

```javascript JavaScript
// Initialize product config and listen for event
CleverTap.addListener(CleverTap.CleverTapProductConfigDidInitialize, (data) => {
    // Called when Product Config is initialized
});

// Set default values for product config keys
CleverTap.setDefaultsMap({
    'text_color': 'red',
    'msg_count': 100,
    'price': 100.50,
    'is_shown': true,
    'json': '{"key":"val"}'  // JSON string example
});
```

#### Fetching product configs

```javascript JavaScript
//Add the Product Config Fetched listener
CleverTap.addListener(CleverTap.CleverTapProductConfigDidFetch, (data) => {
          /* consume the event data */
    });

//Fetch the Product Config
CleverTap.fetch();
```

#### Activate the most recently fetched product config

```javascript JavaScript
//Add the Product Config Activated listener
CleverTap.addListener(CleverTap.CleverTapProductConfigDidActivate, (data) => {
          /* consume the event data */
    });

//Activate the Product Config
CleverTap.activate();
```

#### Fetch And Activate product config

```javascript JavaScript
// Fetch and activate in a single call
CleverTap.fetchAndActivate();
```

#### Fetch Minimum Time Interval

```javascript JavaScript
// Set minimum interval between fetches (in seconds)
CleverTap.setMinimumFetchIntervalInSeconds(interval);
```

#### Get Boolean key

```javascript JavaScript
// Get boolean value for a config key
CleverTap.getProductConfigBoolean(“key”);
```

#### Get String Key

```javascript
// Get string value for a config key
CleverTap.getProductConfigString(“key”);
```

#### Get Number key

```javascript
// Get number value for a config key
CleverTap.getNumber(“key”);
```

#### Get last fetched timestamp in millis

```javascript JavaScript
// Get last fetch time in milliseconds
CleverTap.getLastFetchTimeStampInMillis();
```

## Feature Flag

Control feature rollouts using feature flags.

#### Get Feature Flag

```javascript JavaScript
// Listen for updates to feature flags
CleverTap.addListener(CleverTap.CleverTapFeatureFlagsDidUpdate, (data) => {
    // Use the updated flag values
    const isEnabled = CleverTap.getFeatureFlag("BoolKey", false); // default fallback = false
});
```

## App Personalization

Enable or disable personalized experiences based on user profiles.

#### Enable Personalization

```javascript JavaScript
// Enable personalization features (based on user profile data)
CleverTap.enablePersonalization();
```

#### Disable Personalization

```javascript JavaScript
// Disable personalization (e.g., after user opt-out)
CleverTap.disablePersonalization();
```

## Attributions

Track install sources for marketing attribution.

### Push Install Referrer

```javascript JavaScript
// Track install referrer info for attribution
CleverTap.pushInstallReferrer("source", "medium", "campaign");

// Example: source = "google", medium = "cpc", campaign = "summer_sale"
```

## GDPR

Manage user opt-in and opt-out preferences and control device-level tracking to ensure compliance with GDPR privacy regulations.

### Set Opt Out

To help you comply with GDPR and user privacy requirements, CleverTap offers a method to control whether a user’s data is tracked.

#### Opt Out from All Tracking and Communication

To completely disable event tracking and stop sending any communication (personalized or generic) to the user.

```javascript JavaScript
CleverTap.setOptOut(true); // Will opt out the user to send data to CleverTap
```

#### Opt Out from Tracking Only, but Allow Generic Communication

To disable event tracking, but still allow you to send non-personalized messages (such as broadcast announcements) to the user.

```javascript
CleverTap.setOptOut(true, true); // Only stop tracking, allow generic communication
```

#### Opt In to Tracking and Communication

To re-enable event tracking and allow sending personalized messages to the user, assuming prior consent was collected.

```javascript
CleverTap.setOptOut(false); // Will opt in the user to send data to CleverTap
```

### Enable Device Networking Info Reporting

By default, in compliance with GDPR, the CleverTap SDK does not collect device-level information such as Wi-Fi, Bluetooth, network details, or IP address.

To turn this data collection on or off in your React Native app, use the following method:

```javascript
// Turn ON device network info collection
CleverTap.enableDeviceNetworkInfoReporting(true);

// Turn OFF device network info collection
CleverTap.enableDeviceNetworkInfoReporting(false);
```

### Set Offline

CleverTap React Native SDK provides a method to set the user as offline. Setting the user offline means that no events/profile information will be sent to CleverTap. You can set the user as offline/online using the following method:

```javascript JavaScript
// Set user online (enable real-time data transmission)
CleverTap.setOffline(false);

// Set user offline (pause data transmission)
CleverTap.setOffline(true);
```

## Encryption for PII data

PII data is stored across the SDK and could be sensitive information. From CleverTap React Native SDK v1.2.0 onwards, you can enable encryption for PII data such as Email, Identity, Name, and Phone.

Currently, two levels of encryption are supported, i.e., None(0) and Medium(1). The encryption level is None by default.

* **None**: All stored data is in plaintext
* **Medium**: PII data is encrypted completely

### Android

The only way to set the encryption level in Android is from the `manifest` file. Add the encryption level in the `manifest` as following:

```xml XML
<!-- Add this to AndroidManifest.xml to set encryption level -->
<meta-data
    android:name="CLEVERTAP_ENCRYPTION_LEVEL"
    android:value="1" /> <!-- 1 = Medium, 0 = None -->
```

### iOS

The only way to set the encryption level in iOS is from the `info.plist` file. Add the `CleverTapEncryptionLevel` String key to `info.plist` file, where value 1 means Medium and 0 means None. The encryption level is set to **None** if any other value is provided.

```xml XML
<!-- Add this to Info.plist -->
<key>CleverTapEncryptionLevel</key>
<string>1</string> <!-- 1 = Medium, 0 = None -->
```

### Encryption in Transit

Encrypting data in transit ensures that sensitive personal data, such as Email, Identity, Name, and Phone, is encrypted before transmitting it over the network. This protects user data during transmission and adds an extra layer of security beyond the at-rest encryption.

From CleverTap Android SDK v7.5.0 and iOS SDK v7.3.1 onwards, this feature is supported natively in React Native SDK v3.6.0 and above. The underlying native SDKs handle encryption and decryption automatically.

To learn more, refer to:

* [Encryption in Transit for Android](doc:android-advanced-features#encryption-in-transit)
* [Encryption in Transit for iOS](doc:advanced-options-ios#encryption-in-transit)
