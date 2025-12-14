---
title: Multi-Instance Support Android
excerpt: Learn how to use multiple CleverTap accounts in a single app.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: android-advanced-settings
      title: Android Advanced Settings
    - type: basic
      slug: android-resources
      title: Android Resources
---
# Overview

In version 3.2.0 of our [Android](https://github.com/CleverTap/clevertap-android-sdk) SDK, we released a significant update that enables you to use multiple CleverTap accounts in a single app. 

For example, let's say you have an app composed of multiple smaller apps (chat, map, shopping). With this new SDK update, you can track each smaller app with a different CleverTap account. 

Another use case for multiple CleverTap accounts in a single app is using a different CleverTap account depending on the app store or country from which the app was downloaded.

# Version 3.2.0 Changes

In [version 3.2.0 of the Android SDK](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/3.2.0), we released the following new features: 

* Added support for creating additional CleverTap instances to send data to multiple CleverTap accounts from your app.
* Added APIs for setting the SDK to offline and SSL Pinning.

We also deprecated the following SDK methods: 

* Deprecated CleverTapException, CleverTapMetaDataNotFoundException, CleverTapPermissionsNotSatisfied, and InvalidEventNameException.
* Deprecated CleverTapAPI.CHARGED\_EVENT.
* Deprecated CleverTapAPI.getInstance() method. 

# Create Additional Instances

The first step is to create a CleverTapInstanceConfig object and specify the CleverTap account needed with its account id and account token.

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
> All configuration to the CleverTapInstanceConfig object must be done prior to calling CleverTapAPI.instanceWithConfig. Subsequent changes to the CleverTapInstanceConfig object will not affect the additional CleverTap instance created.

The third step is to instantiate the additional CleverTap instance by calling the `CleverTapAPI.instanceWithConfig` method with the `CleverTapInstanceConfig`  object you created.

```java
CleverTapAPI clevertapAdditionalInstance = CleverTapAPI.instanceWithConfig(clevertapAdditionalInstanceConfig);
```

# Track Events with Additional Instances

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

# Update Profiles with Additional Instances

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

# Set Offline

You can set the CleverTap SDK to offline by using the method below. By default, it is set to false.

Once offline, events will be recorded and queued locally but will not be sent to the server until offline is disabled. Calling this method again with offline set to NO will allow events to be sent to the server, and the SDK instance will immediately attempt to send events that have been queued while offline.

```java
cleverTapAPI.setOffline(true);
```

# Encrypt PII Data

PII data is stored across the SDK and could be sensitive information. From CleverTap SDK v5.2.0 onwards, you can enable encryption for PII data such as Email, Identity, Name, and Phone.

Currently, two levels of encryption are supported, i.e., None(0) and Medium(1). The encryption level is **None** by default.

* **None**: All stored data is in plaintext
* **Medium**: PII data is encrypted completely

To set an encryption level for an additional instance:

```java
CleverTapInstanceConfig clevertapAdditionalInstanceConfig = CleverTapInstanceConfig.createInstance(
    applicationContext,
    "ADDITIONAL_CLEVERTAP_ACCOUNT_ID",
    "ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN"
)

clevertapAdditionalInstanceConfig.setEncryptionLevel(CryptHandler.EncryptionLevel.MEDIUM)
CleverTapAPI clevertapAdditionalInstance = CleverTapAPI.instanceWithConfig(applicationContext ,clevertapAdditionalInstanceConfig)
```
