---
title: GDPR Compliance SDK Updates
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
> 📘 Note
>
> Please review the [User Docs](https://docs.clevertap.com/docs/gdpr) before implementation. This SDK changes the existing implementation.

# Overview

This page describes the changes you need to make for GDPR compliance to the installation of the CleverTap Android and iOS SDKs in your application.

# Android

## Restrict Tracking with Google Ad ID

**In compliance with GDPR, by default the CleverTap Android SDK will not use the Google Ad Id to generate CleverTap IDs.**

From CleverTap Android SDK 3.1.9 onwards, you can choose to enable the use of Google Ad ID to generate CleverTap IDs by adding the following meta-data tag in the AndroidManifest.xml.

```xml
<meta-data
   android:name="CLEVERTAP_USE_GOOGLE_AD_ID"
   android:value="1"/>
```

## Right to Suppress - Opt Out

From CleverTap Android SDK 3.1.9 onwards, in compliance with GDPR, we provide the following method using which the app can stop sending events to CleverTap.

```java
cleverTap.setOptOut(true);
```

If the user wants to opt in again to share their data, you call the same method like this.

```java
cleverTap.setOptOut(false);
```

By default, the SDK will send data to CleverTap i.e opt out will be set to false. It would be a good practice to capture the opt out flag of the user and set it whenever the app launches.

## Device Network Information Reporting in Android

From CleverTap Android SDK 3.1.9 onwards, in compliance with GDPR, the CleverTap Android SDK will not capture any kind of personal information like WiFi, Bluetooth, Network Information and user IP information. To enable the capturing of all this information, you can call the following method,

```java
cleverTap.enableDeviceNetworkInfoReporting(true);
```

To disable Device Network information to be sent to CleverTap you can use the same method as follows.

```java
cleverTap.enableDeviceNetworkInfoReporting(false);
```

By default, Device Network Information will not be collected by CleverTap.

## Marketing Opt Out

From CleverTap Android SDK 3.1.9 onwards, in compliance with GDPR, to ensure opt out of the user from all marketing channels, you can set a profile property against that user as shown below.

```java
profileUpdate.put("MSG-email", false); // Disable email notifications
profileUpdate.put("MSG-push", true); // Enable push notifications
profileUpdate.put("MSG-sms", false); // Disable SMS notifications
profileUpdate.put(“MSG-push-all”, false); //Disable push notifications for all devices
cleverTap.profile.push(profileUpdate);
```

By default, all marketing channels will be available for communication with the user.

# iOS

## Right to Suppress - Opt Out

From CleverTap iOS SDK 3.1.7 onwards, in compliance with GDPR, we provide the following method that your app can use to stop sending events to CleverTap.

```objectivec
[[CleverTap sharedInstance] setOptOut:YES];
```
```swift
CleverTap.sharedInstance().setOptOut(true)
```

If the user wants to opt-in again to share their data, you call the same method like this.

```objectivec
[[CleverTap sharedInstance] setOptOut:NO];
```
```swift
CleverTap.sharedInstance().setOptOut(false)
```

By default, the SDK will send data to CleverTap i.e opt-out will be set to false. It would be a good practice to capture the opt-out flag of the user and set it whenever the app launches.

## Device Network Information Reporting in iOS

From CleverTap iOS SDK 3.1.7 onwards, in compliance with GDPR, the CleverTap iOS SDK will not capture any kind of personal information like WiFi, Network Information, and user IP information. To enable the capturing of all this information, you can call the following method.

```objectivec
[[CleverTap sharedInstance] enableDeviceNetworkInfoReporting:YES];
```
```swift
CleverTap.sharedInstance().enableDeviceNetworkInfoReporting(true)
```

To disable Device Network information to be sent to CleverTap you can use the same method as follows.

```objectivec
[[CleverTap sharedInstance] enableDeviceNetworkInfoReporting:NO];
```
```swift
CleverTap.sharedInstance().enableDeviceNetworkInfoReporting(false)
```

By default, Device Network Information will not be collected by CleverTap.

## Marketing Opt Out

From CleverTap iOS SDK 3.1.7 onwards, in compliance with GDPR, to ensure opt out of the user from all marketing channels, you can set a profile property against that user in the following way.

```objectivec
NSDictionary *profile = @{
    @"MSG-email": @NO, // Disable email notifications
    @"MSG-push": @NO, // Enable push notifications
    @"MSG-sms": @NO, // Disable SMS notifications
    @"MSG-push-all": @NO //Disable push notifications for all devices
};

[[CleverTap sharedInstance] profilePush:profile];
```
```swift
let profile: Dictionary<String, AnyObject> = [
    "MSG-email": false, // Disable email notifications
    "MSG-push": false, // Disable push notifications
    "MSG-sms": false, // Disable SMS notifications
    "MSG-push-all": false // Disable push notifications for all devices
]

CleverTap.sharedInstance()?.profilePush(profile)
```

By default, all marketing channels will be available for communication with the user.

# Web/Javascript SDK

## Data Suppress

* This will ensure that the data from the device will not reach CleverTap's servers. 
* By default, the optOut is set to False.
* If a device needs to be opted out, at the JS SDK end, the flag needs to be set.

## IP Collection

* This will ensure that the CleverTap does not auto collect the device IP.
* By default, the useIP is set to False.
* If a customer wants to collect to the device IP, they need to explicitly set it to true.

For more information, visit the [Web Quick Start Guide](https://developer.clevertap.com/docs/web-quickstart-guide).

```javascript Javascript
clevertap.privacy.push({optOut: true}); //call the flag to true, if the user of the device opts out of sharing their data

clevertap.privacy.push({useIP: false}); //call the flag to true, if the user agrees to share their IP data
```
