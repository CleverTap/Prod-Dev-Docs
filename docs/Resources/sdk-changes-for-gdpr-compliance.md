---
title: SDK Changes for GDPR Compliance
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

From CleverTap Android SDK 3.1.10 onwards, in compliance with GDPR, you can restrict the use of Google Ad ID to generate CleverTap IDs by adding the following meta-data tag in the AndroidManifest.xml.

```xml
<meta-data
   android:name="CLEVERTAP_USE_GOOGLE_AD_ID"
   android:value="0"/>
```

By default, CleverTap Android SDK will not use the Google Ad Id to generate CleverTap IDs.

## Right to Suppress - Opt Out

From CleverTap Android SDK 3.1.10 onwards, in compliance with GDPR, we provide the following method using which the app can stop sending events to CleverTap.

```java
cleverTap.setOptOut(true);
```

If the user wants to opt in again to share their data, you call the same method like this.

```java
cleverTap.setOptOut(false);
```

By default, the SDK will send data to CleverTap i.e opt out will be set to false. It would be a good practice to capture the opt out flag of the user and set it whenever the app launches.

## Device Network Information Reporting

From CleverTap Android SDK 3.1.10 onwards, in compliance with GDPR, the CleverTap Android SDK will not capture any kind of personal information like WiFi, Bluetooth, Network Information and user IP information. To enable the capturing of all this information, you can call the following method,

```java
cleverTap.enableDeviceNetworkInfoReporting(true);
```

To disable Device Network information to be sent to CleverTap you can use the same method as follows.

```java
cleverTap.enableDeviceNetworkInfoReporting(false);
```

By default, Device Network Information will not be collected by CleverTap.

## Marketing Opt Out

From CleverTap Android SDK 3.1.10 onwards, in compliance with GDPR, to ensure opt out of the user from all marketing channels, you can set a profile property against that user as shown below.

```java
profileUpdate.put("MSG-email", false); // Disable email notifications
profileUpdate.put("MSG-push", true); // Enable push notifications
profileUpdate.put("MSG-sms", false); // Disable SMS notifications
profileUpdate.pit(“MSG-push-all”, false); //Disable push notifications for all devices
cleverTap.profile.push(profileUpdate);
```

By default, all marketing channels will be available for communication with the user.

# iOS

## Restrict Tracking with the Identifier for Advertiser (IDFA)

From CleverTap iOS SDK 3.1.7 onwards, in compliance with GDPR, you can restrict the use of IDFA to generate CleverTap IDs by adding CleverTapUseIFA with the type Boolean NO in the Info.plist

Navigate to the Info.plist file in your project navigator, and then create a key called *CleverTapUseIFA* with type Boolean.

<Image title="image1.png" alt={1872} className="border" border={true} src="https://files.readme.io/a99a535-image1.png" />

By default, CleverTap iOS SDK will not use the IDFA (IFA) to generate CleverTap IDs.

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

## Device Network Information Reporting

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

## Javascript SDK Changes

**Data Suppress**\
This will ensure that the data from the said device will not reach CleverTap servers.\
By default, the optOut is set to False\
If a device needs to be opted out, at the JS SDK end, the flag needs to be set

**IP Collection**\
This will ensure that the CleverTap does not auto collect IP information of the said device\
By default, the useIP is set to False\
If Customers want to process to IP of the said device, they need to explicitely set it to true

For more information, visit our [web quickstart guide](https://developer.clevertap.com/docs/web-quickstart-guide) 

```text Javascript
clevertap.privacy.push({optOut: false}); //call the flag to false, if the user of the device opts out of sharing their data
clevertap.privacy.push({useIP: false}); //call the flag to true, if the user agrees to share their IP data
```
