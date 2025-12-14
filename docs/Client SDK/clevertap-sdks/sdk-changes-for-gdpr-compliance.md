---
title: GDPR Compliance SDK Updates
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

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.setOptOut(true)
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.setOptOut(true);
```

From CleverTap Android SDK 7.5.0 onwards, the `setOptOut()` method supports an enhanced signature that allows apps to stop tracking events while sending generic, non-personalized communications, in compliance with GDPR guidelines.

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
// Only stop tracking, allow generic comms
cleverTap.setOptOut(true, true)
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
// Only stop tracking, allow generic comms
cleverTap.setOptOut(true, true);
```

If the user wants to opt in again to share their data, you call the same method like this.

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.setOptOut(false)
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.setOptOut(false);
```

By default, the SDK will send data to CleverTap, and the opt-out will be set to false. It would be a good practice to capture the opt-out flag of the user and set it whenever the app launches.

## Device Network Information Reporting in Android

From CleverTap Android SDK 3.1.9 onwards, in compliance with GDPR, the CleverTap Android SDK will not capture any kind of personal information like WiFi, Bluetooth, Network Information and user IP information. To enable the capturing of all this information, you can call the following method:

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.enableDeviceNetworkInfoReporting(true)
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.enableDeviceNetworkInfoReporting(true);
```

To disable Device Network information to be sent to CleverTap you can use the same method as follows.

```kotlin
val cleverTap = CleverTapAPI.getDefaultInstance(this)
cleverTap.enableDeviceNetworkInfoReporting(false)
```
```java
CleverTapAPI cleverTap = CleverTapAPI.getDefaultInstance(this);
cleverTap.enableDeviceNetworkInfoReporting(false);
```

By default, CleverTap does not collect device network information.

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

```swift
CleverTap.sharedInstance()?.setOptOut(true)
```
```objectivec
[[CleverTap sharedInstance] setOptOut:YES];
```

From CleverTap iOS SDK 7.3.0 onwards, the `setOptOut()` method supports an enhanced signature that allows apps to stop tracking events while sending generic, non-personalized communications, in compliance with GDPR guidelines.

```swift
// Only stop event tracking, allow generic communications
CleverTap.sharedInstance()?.setOptOut(true, allowSystemEvents: true)
```
```objectivec
// Only stop tracking, allow generic communications
[[CleverTap sharedInstance]setOptOut:YES allowSystemEvents:YES];
```

If the user wants to opt in again to share their data, you call the same method like this.

```swift
CleverTap.sharedInstance()?setOptOut(false);
```
```objectivec
[[CleverTap sharedInstance] setOptOut:NO];
```

By default, the SDK will send data to CleverTap, and the opt-out will be set to false. It would be a good practice to capture the opt-out flag of the user and set it whenever the app launches.

## Device Network Information Reporting in iOS

From CleverTap iOS SDK 3.1.7 onwards, in compliance with GDPR, the CleverTap iOS SDK will not capture any kind of personal information like WiFi, Network Information, and user IP information. To enable the capturing of all this information, you can call the following method:

```swift
CleverTap.sharedInstance().enableDeviceNetworkInfoReporting(true)
```
```objectivec
[[CleverTap sharedInstance] enableDeviceNetworkInfoReporting:YES];
```

To disable Device Network information to be sent to CleverTap you can use the same method as follows.

```swift
CleverTap.sharedInstance().enableDeviceNetworkInfoReporting(false)
```
```objectivec
[[CleverTap sharedInstance] enableDeviceNetworkInfoReporting:NO];
```

By default, CleverTap does not collect device network information.

## Marketing Opt Out

From CleverTap iOS SDK 3.1.7 onwards, in compliance with GDPR, to ensure opt out of the user from all marketing channels, you can set a profile property against that user in the following way.

```swift
let profile: Dictionary<String, AnyObject> = [
    "MSG-email": false, // Disable email notifications
    "MSG-push": false, // Disable push notifications
    "MSG-sms": false, // Disable SMS notifications
    "MSG-push-all": false // Disable push notifications for all devices
]

CleverTap.sharedInstance()?.profilePush(profile)
```
```objectivec
NSDictionary *profile = @{
    @"MSG-email": @NO, // Disable email notifications
    @"MSG-push": @NO, // Enable push notifications
    @"MSG-sms": @NO, // Disable SMS notifications
    @"MSG-push-all": @NO //Disable push notifications for all devices
};

[[CleverTap sharedInstance] profilePush:profile];
```

By default, all marketing channels will be available for communication with the user.

# Web/Javascript SDK

## Data Suppress

- This will ensure that the data from the device will not reach CleverTap's servers. 
- By default, the optOut is set to False.
- If a device needs to be opted out, at the JS SDK end, the flag needs to be set.

## IP Collection

- This will ensure that the CleverTap does not auto collect the device IP.
- By default, the useIP is set to False.
- If a customer wants to collect to the device IP, they need to explicitly set it to true.

For more information, visit the [Web Quick Start Guide](https://developer.clevertap.com/docs/web-quickstart-guide).

```javascript Javascript
clevertap.privacy.push({optOut: true}); //call the flag to true, if the user of the device opts out of sharing their data

clevertap.privacy.push({useIP: false}); //call the flag to true, if the user agrees to share their IP data
```

## CleverTap Cookie Details

CleverTap sets first-party cookies and they are used to anonymously identify the website visitors/users. Customers explicitly choose to record data against the user's profile & activity. There's no implicit PII data recorded by the cookie. Refer to the table below to learn about the different cookies.

| Cookie Name | Description                                                                                 | Expiry                                                                                                |
| :---------- | :------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------- |
| WZRK_G      | This cookie stores the guid/anonymous identifier of the user                                | Expires 10 years from the time it is set.                                                             |
| WZRK_S      | This cookie stores the session details like last session timestamp, number of sessions etc. | Expires 20 mins from the time it is set.                                                              |
| WZRK_P      | This cookie tracks the web page url.                                                        | Expires as soon as it is set. This is not used and will be deprecated in the next version of the SDK. |

### Parameters for Local Storage

[block:parameters]
{
  "data": {
    "h-0": "Parameters",
    "h-1": "Description",
    "0-0": "WZRK_FPU",
    "0-1": "FPU stands for Fire Push Unregistered. It is a boolean flag indicating when to fire Push Unregistered event as soon as a new user login is tracked via the Onuserlogin method.",
    "1-0": "WZRK_K",
    "1-1": "Stores the unique identifier for the current user (may contain email ID).",
    "2-0": "WZRK_X",
    "2-1": "Stores the GUID of all the users who have loggedIn till date against their unique identifiers. It is  \nused when we want to use the same GUID for the user who had previously loggedIn > logs in again.  \nUnique identifier could be an email.",
    "3-0": "WZRK_META",
    "3-1": "This cookie stores session related details (timestamp of previousSession, currentSession, sessionCount) and few other properties that help in determining if we have to show the popup on notification.push.",
    "4-0": "WZRK_PSD",
    "4-1": "PSD stands for Push Subscription Data. It stores the push subscription object. It is used to register the token for the newly loggedIn user.",
    "5-0": "WZRK_L",
    "5-1": "This cookie contains details regarding the events (event name and properties) that are sent to CleverTap Platform."
  },
  "cols": 2,
  "rows": 6,
  "align": [
    "left",
    "left"
  ]
}
[/block]


### FAQ

1. Can this cookie data collection be blocked through configuration?  
   No. The cookie can not be blocked without blocking all cookies of the customer's domain.