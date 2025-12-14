---
title: Cordova Advance Features
excerpt: Learn more about Cordova advanced features.
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

This section outlines the advanced features of the CleverTap Cordova SDK. These capabilities provide greater control over debugging, remote configuration, personalization, attribution, and data privacy.

## Debugging

During integration, we recommend that you set the SDK to DEBUG mode, to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level.

### Set Debug Level

Debug level can be one of the following:

- `-1`: Disables all debugging. You can set the `debugLevel` to `-1` if you want to disable CleverTap logs for the production environment.
- `0`: Default, shows minimal SDK integration-related logging.
- `2`: Shows debug output.
- `3`: Shows verbose output.

```javascript JavaScript
CleverTap.setDebugLevel(debugLevel);
```

## Custom Push Notifications (Android Only)

CleverTap's Cordova SDK provides a built-in mechanism for receiving push notifications via FCM. However, if you have your own custom Service for managing push notifications, you can inform CleverTap about the service’s token ID whenever available. The following are the APIs for supported Platforms:

> 📘 Note
> 
> FCM Token, HPS Token, and BPS Token should be fetched from respective push providers after user permissions are granted.

### Registering FCM Token

```javascript JavaScript
CleverTap.setPushToken("<Your FCM Token>")
```

### Registering Huawei Token

```javascript
CleverTap.registerPushToken("<Your HMS Token>", {
  type: 'hps',
  prefKey: 'hps_token',
  className: 'com.clevertap.android.hms.HmsPushProvider',
  messagingSDKClassName: 'com.huawei.hms.push.HmsMessageService'
});
```

### Registering Baidu Token

```javascript
CleverTap.registerPushToken("<Your BPS Token>", {
  type: 'bps',
  prefKey: 'bps_token',
  className: 'com.clevertap.android.bps.BaiduPushProvider',
  messagingSDKClassName: 'com.baidu.android.pushservice.PushMessageReceiver'
});
```

## Native Display

Show personalized banners or visuals using Native Display.

### On Display Units Loaded

```javascript JavaScript
document.addEventListener('onCleverTapDisplayUnitsLoaded', e => {
            log("onCleverTapDisplayUnitsLoaded")
           }
    )
```

### Get All Display Units

```javascript JavaScript
CleverTap.getAllDisplayUnits(val => log("Native Display units are " + JSON.stringify(val)))
```

### Display unit viewed event for ID

```javascript JavaScript
CleverTap.pushDisplayUnitViewedEventForID("unitID");
```

### Display unit clicked event for ID

```javascript JavaScript
CleverTap.pushDisplayUnitClickedEventForID("unitID");
```

## Product Config

> 📘 Feature Availability
> 
> A new and enhanced version of Product Experiences is coming soon. New customers (CleverTap for Enterprise or CleverTap for Startups) who have not enabled the current functionalities can use this feature only when the new version is released. However, the existing users can continue to use it. The [methods](https://developer.clevertap.com/docs/august-2023#cordova-270) for the Product Experiences feature have been deprecated and will be removed from the code by September 2024.

### Set Product Configuration to Default

```javascript JavaScript
document.addEventListener('onCleverTapProductConfigDidInitialize', () => log("onCleverTapProductConfigDidInitialize"))

CleverTap.setDefaultsMap({"test": "val1", "test1": "val2"})
```

### Fetching Product Configs

```javascript JavaScript
CleverTap.fetch()
document.addEventListener('onCleverTapProductConfigDidFetch', 
                          () => log("onCleverTapProductConfigDidFetch"))
```

### Activate the most recently fetched product config

```javascript JavaScript
CleverTap.activate()
document.addEventListener('onCleverTapProductConfigDidActivate', 
                          () => log("onCleverTapProductConfigDidActivate"))
```

### Fetch And Activate product config

```javascript JavaScript
CleverTap.fetchAndActivate();
```

### Fetch Minimum Time Interval

```javascript JavaScript
CleverTap.setMinimumFetchIntervalInSeconds(100)
```

### Get Boolean key

```javascript JavaScript
CleverTap.getBoolean("test", val => log("Value is " + val))CleverTap.getBoolean("test", val => log("Value is " + val))
```

### Get String Key

```javascript
CleverTap.getString("test", val => log("Value is " + val))
```

### Get Number key

```javascript
CleverTap.getDouble("test", val => log("Value is " + val))
```

### Get last fetched timestamp in millis

```javascript JavaScript
CleverTap.getLastFetchTimeStampInMillis(val => log("Value is " + val))
```

## Feature Flag

Control feature rollouts using feature flags.

### Get Feature Flag

```javascript JavaScript
CleverTap.getFeatureFlag("test", true, val => log("Value is " + val))
```

## App Personalization

Enable or disable personalized experiences based on user profiles.

### Enable Personalization

```javascript JavaScript
CleverTap.enablePersonalization()
```

### Disable Personalization

```javascript JavaScript
CleverTap.disablePersonalization()
```

## Attributions

Track install sources for marketing attribution.

### Push Install Referrer

```javascript JavaScript
CleverTap.pushInstallReferrer("source", "medium", "campaign")
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

To turn this data collection on or off in your Cordova, use the following method:

```javascript
// Turn ON device network info collection
CleverTap.enableDeviceNetworkInfoReporting(true);

// Turn OFF device network info collection
CleverTap.enableDeviceNetworkInfoReporting(false);
```

### Set Offline

CleverTap Cordova SDK provides a method to set the user as offline. Setting the user offline means that no events/profile information will be sent to CleverTap. You can set the user as offline/online using the following method:

```javascript JavaScript
// Set user online (enable real-time data transmission)
CleverTap.setOffline(false);

// Set user offline (pause data transmission)
CleverTap.setOffline(true);
```

## Encryption for PII data

PII data is stored across the SDK and could be sensitive information. From `Cordova SDK v2.7.1`. onwards, you can enable encryption for PII data such as Email, Identity, Name, and Phone.

Currently, two levels of encryption are supported, i.e., None(0) and Medium(1). The encryption level is **None** by default.

- **None**: All stored data is in plaintext
- **Medium**: PII data is encrypted completely

### Android

Add encryption level in the `AndroidManifest.xml` as following:

```xml
<meta-data
    android:name="CLEVERTAP_ENCRYPTION_LEVEL"
    android:value="1" />
```

### iOS

The only way to set the encryption level is from the `info.plist`. Add the `CleverTapEncryptionLevel` String key to `info.plist` file, where value 1 means Medium and 0 means None. The encryption level is set to **None** if any other value is provided.

### Encryption in Transit

Encrypting data in transit ensures that sensitive personal data, such as Email, Identity, Name, and Phone, is encrypted before transmitting it over the network. This protects user data during transmission and adds an extra layer of security beyond the at-rest encryption.

From CleverTap Android SDK v7.5.0 and iOS SDK v7.3.1 onwards, this feature is supported natively in Cordova SDK v4.2.0 and above. The underlying native SDKs handle encryption and decryption automatically.

To learn more, refer to:

- [Encryption in Transit for Android](doc:android-advanced-features#encryption-in-transit)
- [Encryption in Transit for iOS](doc:advanced-options-ios#encryption-in-transit)