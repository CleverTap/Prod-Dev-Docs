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
[block:callout]
{
  "type": "info",
  "body": "Please review the [User Docs](https://docs.clevertap.com/docs/gdpr) before implementation. This SDK changes the existing implementation.",
  "title": "Note"
}
[/block]
# Overview
This page describes the changes you need to make for GDPR compliance to the installation of the CleverTap Android and iOS SDKs in your application.

# Android 

## Restrict Tracking with Google Ad ID

**In compliance with GDPR, by default the CleverTap Android SDK will not use the Google Ad Id to generate CleverTap IDs.**

From CleverTap Android SDK 3.1.9 onwards, you can choose to enable the use of Google Ad ID to generate CleverTap IDs by adding the following meta-data tag in the AndroidManifest.xml.
[block:code]
{
  "codes": [
    {
      "code": "<meta-data\n   android:name=\"CLEVERTAP_USE_GOOGLE_AD_ID\"\n   android:value=\"1\"/>",
      "language": "xml"
    }
  ]
}
[/block]
## Right to Suppress - Opt Out

From CleverTap Android SDK 3.1.9 onwards, in compliance with GDPR, we provide the following method using which the app can stop sending events to CleverTap.
[block:code]
{
  "codes": [
    {
      "code": "cleverTap.setOptOut(true);",
      "language": "java"
    }
  ]
}
[/block]
If the user wants to opt in again to share their data, you call the same method like this.
[block:code]
{
  "codes": [
    {
      "code": "cleverTap.setOptOut(false);",
      "language": "java"
    }
  ]
}
[/block]
By default, the SDK will send data to CleverTap i.e opt out will be set to false. It would be a good practice to capture the opt out flag of the user and set it whenever the app launches.

## Device Network Information Reporting in Android

From CleverTap Android SDK 3.1.9 onwards, in compliance with GDPR, the CleverTap Android SDK will not capture any kind of personal information like WiFi, Bluetooth, Network Information and user IP information. To enable the capturing of all this information, you can call the following method,
[block:code]
{
  "codes": [
    {
      "code": "cleverTap.enableDeviceNetworkInfoReporting(true);",
      "language": "java"
    }
  ]
}
[/block]
To disable Device Network information to be sent to CleverTap you can use the same method as follows.
[block:code]
{
  "codes": [
    {
      "code": "cleverTap.enableDeviceNetworkInfoReporting(false);",
      "language": "java"
    }
  ]
}
[/block]
By default, Device Network Information will not be collected by CleverTap.

## Marketing Opt Out

From CleverTap Android SDK 3.1.9 onwards, in compliance with GDPR, to ensure opt out of the user from all marketing channels, you can set a profile property against that user as shown below.
[block:code]
{
  "codes": [
    {
      "code": "profileUpdate.put(\"MSG-email\", false); // Disable email notifications\nprofileUpdate.put(\"MSG-push\", true); // Enable push notifications\nprofileUpdate.put(\"MSG-sms\", false); // Disable SMS notifications\nprofileUpdate.put(“MSG-push-all”, false); //Disable push notifications for all devices\ncleverTap.profile.push(profileUpdate);",
      "language": "java"
    }
  ]
}
[/block]
By default, all marketing channels will be available for communication with the user.

# iOS 

## Right to Suppress - Opt Out

From CleverTap iOS SDK 3.1.7 onwards, in compliance with GDPR, we provide the following method that your app can use to stop sending events to CleverTap.
[block:code]
{
  "codes": [
    {
      "code": "[[CleverTap sharedInstance] setOptOut:YES];",
      "language": "objectivec"
    },
    {
      "code": "CleverTap.sharedInstance().setOptOut(true)",
      "language": "swift"
    }
  ]
}
[/block]
If the user wants to opt-in again to share their data, you call the same method like this.
[block:code]
{
  "codes": [
    {
      "code": "[[CleverTap sharedInstance] setOptOut:NO];",
      "language": "objectivec"
    },
    {
      "code": "CleverTap.sharedInstance().setOptOut(false)\n",
      "language": "swift"
    }
  ]
}
[/block]
By default, the SDK will send data to CleverTap i.e opt-out will be set to false. It would be a good practice to capture the opt-out flag of the user and set it whenever the app launches.

## Device Network Information Reporting in iOS

From CleverTap iOS SDK 3.1.7 onwards, in compliance with GDPR, the CleverTap iOS SDK will not capture any kind of personal information like WiFi, Network Information, and user IP information. To enable the capturing of all this information, you can call the following method.
[block:code]
{
  "codes": [
    {
      "code": "[[CleverTap sharedInstance] enableDeviceNetworkInfoReporting:YES];",
      "language": "objectivec"
    },
    {
      "code": "CleverTap.sharedInstance().enableDeviceNetworkInfoReporting(true)",
      "language": "swift"
    }
  ]
}
[/block]
To disable Device Network information to be sent to CleverTap you can use the same method as follows.
[block:code]
{
  "codes": [
    {
      "code": "[[CleverTap sharedInstance] enableDeviceNetworkInfoReporting:NO];",
      "language": "objectivec"
    },
    {
      "code": " CleverTap.sharedInstance().enableDeviceNetworkInfoReporting(false)",
      "language": "swift"
    }
  ]
}
[/block]
By default, Device Network Information will not be collected by CleverTap.

## Marketing Opt Out

From CleverTap iOS SDK 3.1.7 onwards, in compliance with GDPR, to ensure opt out of the user from all marketing channels, you can set a profile property against that user in the following way.
[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *profile = @{\n    @\"MSG-email\": @NO, // Disable email notifications\n    @\"MSG-push\": @NO, // Enable push notifications\n    @\"MSG-sms\": @NO, // Disable SMS notifications\n    @\"MSG-push-all\": @NO //Disable push notifications for all devices\n};\n\n[[CleverTap sharedInstance] profilePush:profile];",
      "language": "objectivec"
    },
    {
      "code": "let profile: Dictionary<String, AnyObject> = [\n    \"MSG-email\": false, // Disable email notifications\n    \"MSG-push\": false, // Disable push notifications\n    \"MSG-sms\": false, // Disable SMS notifications\n    \"MSG-push-all\": false // Disable push notifications for all devices\n]\n\nCleverTap.sharedInstance()?.profilePush(profile)",
      "language": "swift"
    }
  ]
}
[/block]
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
[block:code]
{
  "codes": [
    {
      "code": "clevertap.privacy.push({optOut: true}); //call the flag to true, if the user of the device opts out of sharing their data\n\nclevertap.privacy.push({useIP: false}); //call the flag to true, if the user agrees to share their IP data",
      "language": "javascript",
      "name": "Javascript"
    }
  ]
}
[/block]