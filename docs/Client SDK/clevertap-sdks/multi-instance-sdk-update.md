---
title: Multi-Instance SDK Update
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
# Overview

In version 3.2.0 of our [Android](https://github.com/CleverTap/clevertap-android-sdk) and [iOS](https://github.com/CleverTap/clevertap-android-sdk) SDKs, we released a major update that enables you to use multiple CleverTap accounts in a single app. 

For example, let's say you have an app that is composed of multiple smaller apps (chat, map, shopping). With this new SDK update, you can track each of the smaller apps with a different CleverTap account. 

Another use case for multiple CleverTap accounts in a single app is using a different CleverTap account depending on the app store or country that the app was downloaded from.

# Android

## Version 3.2.0 Changes

In [version 3.2.0 of the Android SDK](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/3.2.0), we released the following new features: 

* Added support for creating additional CleverTap instances to send data to multiple CleverTap accounts from your app.
* Added APIs for setting the SDK to offline and SSL Pinning.

We also deprecated the following SDK methods: 

* Deprecated CleverTapException, CleverTapMetaDataNotFoundException, CleverTapPermissionsNotSatisfied, and InvalidEventNameException.
* Deprecated CleverTapAPI.CHARGED\_EVENT.
* Deprecated CleverTapAPI.getInstance() method. 

## Create Additional Instances

The first step is to create a CleverTapInstanceConfig object, and specify the CleverTap account needed with its account id and account token.

```java
CleverTapInstanceConfig clevertapAdditionalInstanceConfig =  CleverTapInstanceConfig.createInstance(context, "ADDITIONAL_CLEVERTAP_ACCOUNT_ID", "ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN");
```

The second step is to configure the CleverTapInstanceConfig object with the options needed for the debug level, analytics, Google Ad Id, and personalization.

```java
clevertapAdditionalInstanceConfig.setDebugLevel(CleverTapAPI.LogLevel.DEBUG); // default is CleverTapAPI.LogLevel.INFO

clevertapAdditionalInstanceConfig.setAnalyticsOnly(true); // disables the user engagement features of the instance, default is false

clevertapAdditionalInstanceConfig.useGoogleAdId(true); // enables the collection of the Google ADID by the instance, default is false

clevertapAdditionalInstanceConfig.enablePersonalization(false); //enables personalization, default is true.
```

> 🚧
>
> All configuration to the CleverTapInstanceConfig object must be done prior to calling CleverTapAPI.instanceWithConfig. Subsequent changes to the CleverTapInstanceConfig object will have no effect on the additional CleverTap instance created.

The third step is to instantiate the additional CleverTap instance by calling CleverTapAPI.instanceWithConfig method with the CleverTapInstanceConfig object you created.

```java
CleverTapAPI clevertapAdditionalInstance = CleverTapAPI.instanceWithConfig(clevertapAdditionalInstanceConfig);
```

## Track Events with Additional Instances

```java
//Create a new instance
CleverTapInstanceConfig config =  CleverTapInstanceConfig.createInstance(this,"YOUR_ACCOUNT_ID","YOUR_ACCOUNT_TOKEN");
CleverTapAPI cleverTapAPI = CleverTapAPI.instanceWithConfig(this,config);

//Push an event
cleverTapAPI.pushEvent(“Event Name”);


// Push an event with properties
HashMap<String, Object> prodViewedAction = new HashMap<String, Object>();
prodViewedAction.put("Product Name", "Casio Chronograph Watch");
prodViewedAction.put("Category", "Mens Accessories");
prodViewedAction.put("Price", 59.99);
prodViewedAction.put("Date", new java.util.Date());

cleverTapAPI.pushEvent("Product viewed", prodViewedAction);

//Push a charged event
HashMap<String, Object> chargeDetails = new HashMap<String, Object>();
chargeDetails.put("Amount", 300);
chargeDetails.put("Payment Mode", "Credit card");
chargeDetails.put("Charged ID", 24052013);

HashMap<String, Object> item1 = new HashMap<String, Object>();
item1.put("Product category", "books");
item1.put("Book name", "The Millionaire next door");
item1.put("Quantity", 1);

HashMap<String, Object> item2 = new HashMap<String, Object>();
item2.put("Product category", "books");
item2.put("Book name", "Achieving inner zen");
item2.put("Quantity", 1);

HashMap<String, Object> item3 = new HashMap<String, Object>();
item3.put("Product category", "books");
item3.put("Book name", "Chuck it, let's do it");
item3.put("Quantity", 5);

ArrayList<HashMap<String, Object>> items = new ArrayList<HashMap<String, Object>>();
items.add(item1);
items.add(item2);
items.add(item3);

cleverTapAPI.pushChargedEvent(chargeDetails, items);
```

## Update Profiles with Additional Instances

```java
//Create a new instance
CleverTapInstanceConfig config =  CleverTapInstanceConfig.createInstance(this,"YOUR_ACCOUNT_ID","YOUR_ACCOUNT_TOKEN");
CleverTapAPI cleverTapAPI = CleverTapAPI.instanceWithConfig(this,config);

//Push a simple profile update
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Customer Type", "Silver");
profileUpdate.put("Prefered Language", "English");

cleverTapAPI.profile.push(profileUpdate);


//Push a complex profile update
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Name", "Jack Montana");                  // String
profileUpdate.put("Identity", 61026032);                    // String or number
profileUpdate.put("Email", "jack@gmail.com");               // Email address of the user
profileUpdate.put("Phone", "+14155551234");                 // Phone (with the country code, starting with +)
profileUpdate.put("Gender", "M");                           // Can be either M or F
profileUpdate.put("Employed", "Y");                         // Can be either Y or N
profileUpdate.put("Education", "Graduate");                 // Can be either Graduate, College or School
profileUpdate.put("Married", "Y");                          // Can be either Y or N
profileUpdate.put("DOB", new Date());                       // Date of Birth. Set the Date object to the appropriate value first
profileUpdate.put("Age", 28);                               // Not required if DOB is set
profileUpdate.put("Tz", "Asia/Kolkata");                    //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                                            //or a custom ID such as "GMT-8:00"
profileUpdate.put("Photo", "www.foobar.com/image.jpeg");    // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.put("MSG-email", false);                      // Disable email notifications
profileUpdate.put("MSG-push", true);                        // Enable push notifications
profileUpdate.put("MSG-sms", false);                        // Disable SMS notifications

ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
profileUpdate.put("MyStuff", stuff);                        //ArrayList of Strings

String[] otherStuff = {"Jeans","Perfume"};
profileUpdate.put("MyStuff", otherStuff);                   //String Array

cleverTapAPI.pushProfile(profileUpdate);
```

## Set Offline

You can set the CleverTap SDK to offline by using the method below. By default, it is set to false.

Once offline, events will be recorded and queued locally, but will not be sent to the server until offline is disabled. Calling this method again with offline set to NO will allow events to be sent to server, and the SDK instance will immediately attempt to send events that have been queued while offline.

```java
cleverTapAPI.setOffline(true);
```

## SSL Pinning

You can perform SSL pinning for the following CleverTap SDKs:

* Android Core SDK v4.5.1 and above.
* iOS Core SDK v4.1.1 and above.

To enable SSL Pinning, please contact [customer support](https://help.clevertap.com/hc/en-us/requests/new).

> ❗️ SSL Pinning Considerations
>
> Implementing SSL pinning may lead to unexpected outcomes. The pinned certificates are shipped with the SDK. If the pinned certificates expire or change, you must update the SDK and roll out an application update to your users. After the certificate expires, your application will be unable to send any data to CleverTap servers until your users upgrade their applications to the latest SDK version and certificate. For more information, refer to [SSL Pinning](https://www.digicert.com/blog/certificate-pinning-what-is-certificate-pinning).

## Deprecated Methods

We deprecated the SDK methods listed in the table below. Note that some of the functionality supported by the deprecated methods is now supported through new methods.

| Deprecated Method                  | New Method                          |
| :--------------------------------- | :---------------------------------- |
| CleverTapException                 | *Not supported in new SDK versions* |
| CleverTapMetaDataNotFoundException | *Not supported in new SDK versions* |
| CleverTapPermissionsNotSatisfied   | *Not supported in new SDK versions* |
| InvalidEventNameException          | *Not supported in new SDK versions* |
| CleverTapAPI.CHARGED\_EVENT        | cleverTapAPI.pushChargedEvent()     |
| CleverTapAPI.getInstance()         | CleverTapAPI.getDefaultInstance()   |

# iOS

## Version 3.2.0 Changes

In [version 3.2.0 of the iOS SDK](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/3.2.0), we released the following new features: 

* Added support for creating additional CleverTap instances to send data to multiple CleverTap accounts from your app.
* Added APIs for setting the SDK to offline and SSL Pinning.
* Added support for integration via Carthage.
* Moved from a static to dynamic framework.

## Create Additional Instances

First, import the header in your class.

```objectivec Objective-C
#import <CleverTapSDK/CleverTapInstanceConfig.h>
```
```text Swift
#import "CleverTapSDK/CleverTapInstanceConfig.h"
```

Second, configure the instance with the CleverTap account id and account token. 

You can also configure the CleverTapInstanceConfig object with the options needed for the debug level, analytics, IDFA, and personalization.

```objectivec
// Config an additional instance
CleverTapInstanceConfig *ctConfig = [[CleverTapInstanceConfig alloc] initWithAccountId:@"YOUR_ACCOUNT_ID" accountToken:@"YOUR_ACCOUNT_TOKEN"];

//By default, CleverTap logs are set to CleverTapLogLevel.info. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level to CleverTapLogLevel.debug. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTapLogLevel.off
[ctConfig setLogLevel:CleverTapLogDebug];

//By default,  the CleverTap iOS SDK will not use the IDFA (IFA) to generate CleverTap IDs. You can choose to enable the use of IDFA to generate CleverTap IDs by call below method.
[ctConfig setUseIDFA:YES];

//Setting this boolean value to true will disable all engagement.
[ctConfig setAnalyticsOnly:YES];

//Call this method to enable the Profile/Events Read and Synchronization API.
[ctConfig setEnablePersonalization:NO];
```
```swift
// Config an additional instance
let ctConfig = CleverTapInstanceConfig.init(accountId: "YOUR_ACCOUNT_ID", accountToken:  "YOUR_ACCOUNT_TOKEN")

//By default, CleverTap logs are set to CleverTapLogLevel.info. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level to CleverTapLogLevel.debug. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTapLogLevel.off
ctConfig.logLevel = CleverTapLogLevel.debug

//By default,  the CleverTap iOS SDK will not use the IDFA (IFA) to generate CleverTap IDs. You can choose to enable the use of IDFA to generate CleverTap IDs by call below method.
ctConfig.useIDFA = true

//Setting this boolean value to true will disable all engagement.
ctConfig.analyticsOnly = true

//Call this method to enable the Profile/Events Read and Synchronization API.
ctConfig.enablePersonalization = false
```

> 🚧
>
> All configuration to the CleverTapInstanceConfig object must be done prior to calling instanceWithConfig:ctConfig. Subsequent changes to the CleverTapInstanceConfig object will have no effect on the additional CleverTap instance created.

The third step is to instantiate the additional CleverTap instance by calling the instanceWithConfig:ctConfig method with the CleverTapInstanceConfig object you created.

```objectivec
// Init an additional instance 
CleverTap *additionalCleverTapInstance = [CleverTap instanceWithConfig:ctConfig];
```
```swift
// Init an additional instance
let cleverTapAdditionalInstance = CleverTap.instance(with: ctConfig)
```

## Track Events with Additional Instances

```objectivec
CleverTapInstanceConfig *ctConfig = [[CleverTapInstanceConfig alloc] initWithAccountId:@"YOUR_ACCOUNT_ID" accountToken:@"YOUR_ACCOUNT_TOKEN"];

CleverTap *cleverTapAdditionalInstance = [CleverTap instanceWithConfig:ctConfig];

[cleverTapAdditionalInstance recordEvent:@"TestCT1WProps" withProps:@{@"one": @1}];
```
```swift Swift
let ctConfig = CleverTapInstanceConfig.init(accountId: "YOUR_ACCOUNT_ID", accountToken:  "YOUR_ACCOUNT_TOKEN")
       
let cleverTapAdditionalInstance = CleverTap.instance(with: ctConfig)

cleverTapAdditionalInstance.recordEvent("TestCT1WProps", withProps: ["one": NSNumber.init(integerLiteral: 1)])
```

## Update Profiles with Additional Instances

```objectivec
CleverTapInstanceConfig *ctConfig = [[CleverTapInstanceConfig alloc] initWithAccountId:@"YOUR_ACCOUNT_ID" accountToken:@"YOUR_ACCOUNT_TOKEN"];
   
CleverTap *cleverTapAdditionalInstance = [CleverTap instanceWithConfig:ctConfig];

[cleverTapAdditionalInstance profileSetMultiValues:@[@"bag", @"shoes"] forKey:@"myCart"];
```
```swift
let ctConfig = CleverTapInstanceConfig.init(accountId: "YOUR_ACCOUNT_ID", accountToken:  "YOUR_ACCOUNT_TOKEN")
       
let cleverTapAdditionalInstance = CleverTap.instance(with: ctConfig)

cleverTapAdditionalInstance.profileSetMultiValues(["a"], forKey: "letters")
```

## Install CleverTap with Carthage

CleverTap now supports Carthage to package your dependencies as a framework. 

To integrate CleverTap into your Xcode project using Carthage, first specify it in your Cartfile. Second, run the 'carthage update' command to build the framework. Third, drag the built CleverTapSDK.framework into your Xcode project.

```text
github "CleverTap/clevertap-ios-sdk"
```

## SSL Pinning

To enable SSL Pinning, please contact [customer support](https://help.clevertap.com/hc/en-us/requests/new).

## Set Offline

You can set the CleverTap SDK to offline by using the method below. By default, it is set to false.

Once offline, events will be recorded and queued locally, but will not be sent to the server until offline is disabled. Calling this method again with offline set to NO will allow events to be sent to server, and the SDK instance will immediately attempt to send events that have been queued while offline.

```objectivec
[cleverTapAdditionalInstance setOffline:YES];
```
```swift
cleverTapAdditionalInstance.setOffline = YES
```
