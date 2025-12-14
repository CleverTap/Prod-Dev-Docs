---
title: Android User Profiles
excerpt: Learn more about updating user profile properties and handling DND.
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
      title: User Events
      url: https://developer.clevertap.com/docs/user-events-android
    - type: link
      title: Android Push
      url: https://developer.clevertap.com/docs/android-push
    - type: link
      title: Huawei Push Integration
      url: https://developer.clevertap.com/docs/clevertap-huawei-push-integration
    - type: link
      title: Baidu Push Integration
      url: https://developer.clevertap.com/docs/baidu-push-notifications
    - type: link
      title: Xiaomi Push Integration
      url: https://developer.clevertap.com/docs/xiaomi-push-notifications
    - type: basic
      slug: push-templates-android
      title: Android Push Templates
    - type: link
      title: Android In-App
      url: https://developer.clevertap.com/docs/android-in-app-notifications
    - type: link
      title: Android App Inbox
      url: https://developer.clevertap.com/docs/android-app-inbox
    - type: link
      title: Android Native Display
      url: https://developer.clevertap.com/docs/android-native-display
    - type: link
      title: Android Product Experiences
      url: https://developer.clevertap.com/docs/product-experiences-android
    - type: link
      title: Multi-Instance Support Android
      url: https://developer.clevertap.com/docs/multi-instance-support-android
    - type: link
      title: Android Advanced Settings
      url: https://developer.clevertap.com/docs/android-advanced-settings
    - type: link
      title: Android Resources
      url: https://developer.clevertap.com/docs/android-resources
---
# User Profiles

CleverTap stores the user's demographic data (gender, age, location), app and website interactions, campaign visits, and transaction history to give you a complete picture of every user.

## Create a User profile when user logs in (On User Login)

A user profile is automatically created for every user launching your mobile application, whether logged in or not.

At first, the user profile starts as *Anonymous* which means that the profile does not contain any identifiable information about the user yet. You can choose to enrich the profile with attributes, such as name, age, and customer ID. You can also add custom attributes that you define to extend the CleverTap data model.

Sending user profile information to CleverTap using our Android SDK requires two steps:

1. You have to build a HashMap with the profile properties.
2. You have to call the SDK's `OnUserLogin` method and pass the HashMap you created as a parameter.

```java
// each of the below mentioned fields are optional
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Name", "Jack Montana");    // String
profileUpdate.put("Identity", 61026032);      // String or number
profileUpdate.put("Email", "jack@gmail.com"); // Email address of the user
profileUpdate.put("Phone", "+14155551234");   // Phone (with the country code, starting with +)
profileUpdate.put("Gender", "M");             // Can be either M or F
profileUpdate.put("DOB", new Date());         // Date of Birth. Set the Date object to the appropriate value first
// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.put("MSG-email", false);        // Disable email notifications
profileUpdate.put("MSG-push", true);          // Enable push notifications
profileUpdate.put("MSG-sms", false);          // Disable SMS notifications
profileUpdate.put("MSG-whatsapp", true);      // Enable WhatsApp notifications
ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
profileUpdate.put("MyStuff", stuff);                        //ArrayList of Strings
String[] otherStuff = {"Jeans","Perfume"};
profileUpdate.put("MyStuff", otherStuff);                   //String Array

clevertapDefaultInstance.onUserLogin(profileUpdate);
```

Checkout the [Quick Start guide](https://developer.clevertap.com/docs/android-quickstart-guide#identify-users) for more information.

## Update the User Profile

The `Onuserlogin` method identifies the individual users on the device. However, you may need to add additional user properties to the user profile such as gender, date of birth, and so on.

CleverTap provides pre-defined profile properties, such as name, phone, gender, and age, to represent well-known properties associated with a profile. We strongly recommend using these standard property names. A list of all pre-defined property names is available. In addition, we also support arbitrary single and multi-value profile properties. 

Below are examples of updating these types of properties:

```java Predefined - JAVA
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Name", "Jack Montana");                  // String
profileUpdate.put("Email", "jack@gmail.com");               // Email address of the user
profileUpdate.put("Phone", "+14155551234");                 // Phone (with the country code, starting with +)
profileUpdate.put("Gender", "M");                           // Can be either M or F
profileUpdate.put("DOB", new Date());                       // Date of Birth. Set the Date object to the appropriate value first
                                                            
profileUpdate.put("Photo", "www.foobar.com/image.jpeg");    // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.put("MSG-email", false);                      // Disable email notifications
profileUpdate.put("MSG-push", true);                        // Enable push notifications
profileUpdate.put("MSG-sms", false);                        // Disable SMS notifications
 profileUpdate.put("MSG-dndPhone", true);                  // Opt out phone                                                                    number from SMS                                                                  notifications
 profileUpdate.put("MSG-dndEmail", true);                  // Opt out email                                                                    number from SMS                                                                  notifications
ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
profileUpdate.put("MyStuff", stuff);                        //ArrayList of Strings

String[] otherStuff = {"Jeans","Perfume"};
profileUpdate.put("MyStuff", otherStuff);                   //String Array

clevertapDefaultInstance.pushProfile(profileUpdate);
```
```kotlin Predefined - Kotlin
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
val profileUpdate = HashMap<String, Any>()
profileUpdate["Name"] = "Jack Montana" // String
profileUpdate["Email"] = "jack@gmail.com" // Email address of the user
profileUpdate["Phone"] = "+14155551234" // Phone (with the country code, starting with +)
profileUpdate["Gender"] = "M" // Can be either M or F
profileUpdate["DOB"] = Date() // Date of Birth. Set the Date object to the appropriate value first
profileUpdate["Photo"] = "www.foobar.com/image.jpeg" // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate["MSG-email"] = false // Disable email notifications
profileUpdate["MSG-push"] = true // Enable push notifications
profileUpdate["MSG-sms"] = false // Disable SMS notifications
profileUpdate["MSG-dndPhone"] = true // Opt out phone
profileUpdate["MSG-dndEmail"] = true // Opt out email
profileUpdate["MyStuff"] = arrayListOf("bag", "shoes") //ArrayList of Strings
profileUpdate["MyStuff"] = arrayOf("Jeans", "Perfume") //String Array
clevertapDefaultInstance?.pushProfile(profileUpdate)
```
```java Arbitrary Single Value - JAVA
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Customer Type", "Silver");
profileUpdate.put("Prefered Language", "English");

clevertapDefaultInstance.pushProfile(profileUpdate);

/**
 * Data types
 * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 */
```
```kotlin Arbitrary Multi Value - Kotlin
// To set a multi-value property
clevertapDefaultInstance?.setMultiValuesForKey("mystuff", arrayListOf("bag","shoes"))
 // To add an additional value(s) to a multi-value property
clevertapDefaultInstance?.addMultiValueForKey("mystuff", "coat")
// or
clevertapDefaultInstance?.addMultiValuesForKey("mystuff", arrayListOf("socks", "scarf"))
// To remove a value(s) from a multi-value property
clevertapDefaultInstance?.removeMultiValueForKey("mystuff", "bag")
// or
clevertapDefaultInstance?.removeMultiValuesForKey("mystuff", arrayListOf("shoes", "coat"))
//To remove the value of a property (scalar or multi-value)
clevertapDefaultInstance?.removeValueForKey("mystuff")
```
```java Arbitrary Multi Value - JAVA
// To set a multi-value property
ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
clevertapDefaultInstance.setMultiValuesForKey("mystuff", stuff);

// To add an additional value(s) to a multi-value property
clevertapDefaultInstance.addMultiValueForKey("mystuff", "coat");
// or
ArrayList<String> newStuff = new ArrayList<String>();
newStuff.add("socks");
newStuff.add("scarf");
clevertapDefaultInstance.addMultiValuesForKey("mystuff", newStuff);


//To remove a value(s) from a multi-value property
clevertapDefaultInstance.removeMultiValueForKey("mystuff", "bag");
// or
ArrayList<String> oldStuff = new ArrayList<String>();
oldStuff.add("shoes");
oldStuff.add("coat");
clevertapDefaultInstance.removeMultiValuesForKey("mystuff", oldStuff);

//To remove the value of a property (scalar or multi-value)
clevertapDefaultInstance.removeValueForKey("mystuff");
```
```kotlin Arbitrary Single Value - Kotlin
val profileUpdate = HashMap<String, Any>()
profileUpdate["Customer Type"] = "Silver"
profileUpdate["Prefered Language"] = "English"
clevertapDefaultInstance?.pushProfile(profileUpdate)
```

> 📘 Note
>
> You do not require to pass *Age* as a profile property in the API because it is automatically calculated and stored.

CleverTap provides easy ways to enrich the user profile with data from sources, such as Facebook. You can also store custom attributes in a user profile which can be used to segment users later.

### Increment/Decrement Operator in Android

Increment or decrement a user profile property by using the `incrementValue` or `decrementValue` methods for Clevertap Android SDK version 4.2.0 and above.

> 📘 Note
>
> The applicable user properties are of type Integer, Float, or Double. The value can be zero or greater than zero.

```java
// int values
CleverTapSingleton.getDefaultInstance(ctx).incrementValue("score",10);
CleverTapSingleton.getDefaultInstance(ctx).decrementValue("score",10);

// double values
CleverTapSingleton.getDefaultInstance(ctx).incrementValue("score",5.5);
CleverTapSingleton.getDefaultInstance(ctx).decrementValue("score",5.5);
```
```kotlin
// int values
CleverTapAPI.getDefaultInstance(ctx)!!.incrementValue("score",10)
CleverTapAPI.getDefaultInstance(ctx)!!.decrementValue("score",10)

// double values
CleverTapAPI.getDefaultInstance(ctx)!!.incrementValue("score",5.5)
CleverTapAPI.getDefaultInstance(ctx)!!.decrementValue("score",5.5)
```

## DND

You can opt-out a user by either phone or email.

### Phone

You can opt-out a user or a phone number:

* To opt out a specific user when multiple users share the same phone number, you can disable SMS for a specific user. Set the MSG-sms flag to *false*, and the specified user stops receiving SMS; however, all other users sharing the number continue to receive messages if they have not opted out.  

* To opt-out a phone number and all the users associated with it, set the MSG-dndPhone flag to *true*.

Refer to [User Profile API Endpoints](doc:user-profile-object) for complete user profile documentation.

### Email

You can opt-out a user or an email address: 

* To opt out a specific user when multiple users share the same email address, you can disable email for a specific user. Set the MSG-email flag to *false*, and the specified user stops receiving messages; however, all other users sharing the email address continue to receive messages if they have not opted out.  

* To opt-out an email address and all the users associated with it, set the MSG-dndEmail flag to *true*.

Refer to [User Profile API Endpoints](doc:user-profile-object) for complete user profile documentation.

## Custom Identity Management

Starting from CleverTap Android SDK v4.0.1, you can enable custom identity management. The `onUserLogin()` method can identify users in advance (only for new app installs) whenever a user logs in to the app. You can use any of the following identifiers or a combination of them:

Phone\
Email\
Identity\
This process varies for default(shared) CleverTap instances and additional Instances.

### Default(shared) CleverTap instance

The CleverTap Android SDK provides the default instance to call CleverTap methods. The `getDefaultInstance()` method initializes this instance as follows:

1. Navigate to the app's `AndroidManifest.xml` file in your project navigator.
2. Create a *CleverTapIdentifiers* key with the type `Array`.
3. Add the identifier keys for the user.\
   The following is an example:

```groovy
<meta-data
  android:name="CLEVERTAP_IDENTIFIER"
  android:value="Email,Phone" />
```

### Additional Instances

Additional instances pointing to a different CleverTap account can be created and used to call CleverTap methods. These can be initialized by a `CleverTapInstanceConfig` object and calling the `setIdentityKeys` property to an array of identifiers.

```java
CleverTapInstanceConfig config = CleverTapInstanceConfig.createInstance(“YourAccountID”,”YourAccountToken”,”YourAccountRegion);
config.setIdentityKeys(new String[](“Email”,”Phone”));
```

> 📘 Additional Notes
>
> * For existing app installs/legacy users, only the current/existing identifiers(Identity/Email) are utilized.
>
> * We strongly recommend not changing the identifiers for new app installs after the identifiers are set (via an instance config). This may result in an error message, and the original identifier keys will be used.
