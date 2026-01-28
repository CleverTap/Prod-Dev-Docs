---
title: Android Quick Start Guide
excerpt: Learn how to integrate and start using the CleverTap Android SDK.
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
      title: User Profiles
      url: https://developer.clevertap.com/docs/user-profiles-android
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
      title: Xiaomi Push Integration (Deprecated)
      url: https://developer.clevertap.com/docs/xiaomi-push-notifications
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
---
# Overview

This section shows how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard in less than ten minutes. CleverTap provides an Android SDK that enables app developers to track, segment, and engage their users.

# Install SDK

To use the CleverTap Android SDK, you have two options. You can install it automatically using Gradle in Android Studio or manually by including the SDK source code in your Android Studio project.

> 📘 Note
>
> 1. When minifying is enabled for Gradle wrapper 8.0+ and Android Gradle plugin 8.0.0+, R8 reports missing classes as errors (previously reported as warnings) and the build fails.
>
> * Upgrade to `com.clevertap.android:clevertap-android-sdk v6.1.1` and `com.clevertap.android:clevertap-hms-sdk v1.3.4` to fix this issue.
> * This occurs due to change in behaviour in the Android Gradle Plugin (AGP). When R8 traces the program, it tries to handle all the classes, methods and fields that it finds in the part of the program it considers live. Earlier during this tracing, it returned a warning that allowed building the APK. But these are now converted into errors.
>
> For more information, refer to the [Android Gradle Plugin 8.0.0](https://developer.android.com/build/releases/past-releases/agp-8-0-0-release-notes) document.
>
> 2. For developers that have BACKGROUND_SYNC ([Android Pull Notification](https://developer.clevertap.com/docs/android-push#pull-notification)) enabled in their older apps and are now upgrading to CleverTap Android SDK v6.1.1, please add the below code snippet to your `AndroidManifest.xml` to avoid `ClassNotFoundException` error.
>
> ```xml
> < service
> 	android: name = "com.clevertap.android.sdk.pushnotification.amp.CTBackgroundJobService"
> 	android: exported = "false"
> 	android: enabled = "false"
> 	tools: ignore = "MissingClass" / >
> ```

## Option A: Install using Android Studio and Gradle (Recommended)

1. For FCM, add the dependencies below in your application's `build.gradle` file. You will also have to add the FCM-generated`google-services.json` file to your project.

2. Add the following configuration to your gradle properties file.

```groovy
dependencies {
    implementation 'androidx.core:core:1.13.0'

    implementation 'com.clevertap.android:clevertap-android-sdk:7.8.0' // checkout the latest sdk version at https://github.com/CleverTap/clevertap-android-sdk/blob/master/README.md#-installation
    implementation 'com.google.firebase:firebase-messaging:24.0.0'
}

// at the end of the build.gradle file
apply plugin: 'com.google.gms.google-services'
```

3. Once you have updated your `build.gradle` file, sync your project by clicking on _Sync Project with Gradle Files_ button.

## Option B: Manual Install

For a manual install, follow these steps:

1. Download the latest SDK's AAR file from the [Releases](https://github.com/CleverTap/clevertap-android-sdk/releases) section
2. Copy the AAR file to your projects' `app/libs` directory.
3. Add the following line in your module's `build.gradle`:

```groovy
dependencies {
    implementation fileTree(include: ['*.jar', '*.aar'], dir: 'libs')

    implementation 'androidx.core:core:1.13.0'
}

// at the end of the build.gradle file
apply plugin: 'com.google.gms.google-services'
```

# Add Your CleverTap Credentials in the `AndroidManifest.xml` File

To associate your Android app with your CleverTap account, you must add your CleverTap credentials to your application's `AndroidManifest.xml` file.

1. Navigate to the `AndroidManifest.xml` file in your project navigator.

2. Add your CleverTap _Account ID_ and _Token_ to `AndroidManifest.xml` within the `<application></application>` tags.

> 🚧 Credentials and Access
>
> The _CleverTap Account ID_ and  _CleverTap Token_ are available as _Project ID_ and _Project Token_, respectively, on the CleverTap dashboard. You can find these details by navigating to Settings > Projects from the dashboard.  
> Member and Creator roles in the project do not have access to view the _Passcode_ and _Project Token_ of the account on the dashboard.

```xml
<meta-data
    android:name="CLEVERTAP_ACCOUNT_ID"
    android:value="Your CleverTap Account ID"/>
<meta-data
    android:name="CLEVERTAP_TOKEN"
    android:value="Your CleverTap Account Token"/>
<!-- IMPORTANT: To force use Google AD ID to uniquely identify  users, use the following meta tag. GDPR mandates that if you are using this tag, there is prominent disclousure to your end customer in their application. Read more about GDPR here - https://clevertap.com/blog/in-preparation-of-gdpr-compliance/ -->
<meta-data
    android:name="CLEVERTAP_USE_GOOGLE_AD_ID"
    android:value="1"/>
```

> 🚧 Key Points to Remember When Using Google AD ID to Uniquely Identify Users
>
> * GDPR mandates that if you use Google AD ID to identify users uniquely, there should be prominent disclosure in your application that explains this to your end customer. For more information, refer to the [GDPR article](https://clevertap.com/blog/in-preparation-of-gdpr-compliance/).
>
> * If you still want to use Google AD ID to generate a user's identity to identify users across re-installs, add the meta tag that forces the use of the Google AD ID to generate an identity in CleverTap.
>
> * You must also integrate the Google Ads SDK to ensure that the CleverTap Android SDK picks up the Google Ad ID to generate the user's identity. To integrate the Google Ads SDK, refer to [Mobile Ads SDK Integration](https://developers.google.com/ad-manager/mobile-ads-sdk/android/quick-start)
>
> * Declare the following permission when using Google Ad ID for apps targeting Android 13 and above:
>
> `<uses-permission android:name="com.google.android.gms.permission.AD_ID"/>`.

3. Insert the Account ID (Project ID) and Account Token values from your CleverTap account. You can obtain this information by navigating to the _Settings_ > _Project_ page from the CleverTap dashboard.

<Image align="center" alt={758} border={true} caption="CleverTap Account Credentials" title="CleverTap Account Credentials" src="https://files.readme.io/4a8831c-Android_Project_account_ID.png" width="80%" />

## Add Region Code

To know how to add region code in Android SDK, refer to [Region Codes.](doc:idc#android)

## Initialize the CleverTap SDK

### Option A: Custom Application Class

If you have an already registered custom application class, call `ActivityLifecycleCallback.register(this)`; before `super.onCreate()` in your class.

### Option B: No Application Class

You can add CleverTap's class as your application class to capture system events correctly.

1. In your AndroidManifest.xml file, add the following snippet within the <application /> tags.

```xml
<application
    android:label="@string/app_name"
    android:icon="@drawable/ic_launcher"
    android:name="com.clevertap.android.sdk.Application">
```

<Image align="center" alt={796} border={true} caption="Initialize the CleverTap SDK" title="Initialize the CleverTap SDK" src="https://files.readme.io/319c1ad-image6.png" />

## Enable Tracking by Adding Permissions

Add the snippet below in the `AndroidManifest.xml` file so that the CleverTap SDK can access the internet.

```xml
<!-- Required to allow the app to send events and user profile information -->
<uses-permission android:name="android.permission.INTERNET"/>
<!-- Recommended so that CleverTap knows when to attempt a network call -->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

<Image align="center" alt={2396} border={true} caption="Enable Tracking by Adding Permissions" title="Enable Tracking by Adding Permissions" src="https://files.readme.io/310c7af-Screenshot_2022-08-01_at_1.07.23_PM.png" />

# Access CleverTap Instance

Include the code below in the `onCreate()` method of your app's main activity. This code initialises the CleverTap SDK.

```java JAVA
CleverTapAPI clevertapDefaultInstance = CleverTapAPI.getDefaultInstance(getApplicationContext());
```
```kotlin Kotlin
var cleverTapDefaultInstance: CleverTapAPI? = null
cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(applicationContext)
```

# Verify Integration

To check if the integration was performed correctly, you can run the application.

After the application is launched on the emulator or Android device, navigate to your CleverTap dashboard > _Boards_ > _Mobile App_. You will see a new active user if you successfully integrated the CleverTap SDK.

<Image align="center" alt={1405} border={true} caption="View Active Users on the Dashboard" title="View Active Users on the Dashboard" src="https://files.readme.io/9e07e5a-New_mobile_users.png" />

# Identify Users

A user profile is automatically created in CleverTap for each user launching your application.

Initially, the user profile starts as anonymous, which means the profile does not contain any identifiable information about the user. You can enrich the profile with pre-defined attributes from the CleverTap data model, such as name and email. You can also add custom attributes that you define to extend the CleverTap data model.

Sending user profile information to CleverTap using our Android SDK requires two steps:

1. You have to build a HashMap with the profile properties.
2. You have to call the SDK's `OnUserLogin` method and pass the HashMap you created as a parameter.

```java JAVA
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
```kotlin Kotlin
// each of the below mentioned fields are optional
val profileUpdate = HashMap<String, Any>()
profileUpdate["Name"] = "Jack Montana" // String
profileUpdate["Identity"] = 61026032 // String or number
profileUpdate["Email"] = "jack@gmail.com" // Email address of the user
profileUpdate["Phone"] = "+14155551234" // Phone (with the country code, starting with +)
profileUpdate["Gender"] = "M" // Can be either M or F
profileUpdate["DOB"] = Date() // Date of Birth. Set the Date object to the appropriate value first
// optional fields. controls whether the user will be sent email, push etc.
// optional fields. controls whether the user will be sent email, push etc.
profileUpdate["MSG-email"] = false // Disable email notifications
profileUpdate["MSG-push"] = true // Enable push notifications
profileUpdate["MSG-sms"] = false // Disable SMS notifications
profileUpdate["MSG-whatsapp"] = true // Enable WhatsApp notifications
val stuff = ArrayList<String>()
stuff.add("bag")
stuff.add("shoes")
profileUpdate["MyStuff"] = stuff //ArrayList of Strings
val otherStuff = arrayOf("Jeans", "Perfume")
profileUpdate["MyStuff"] = otherStuff //String Array
CleverTapAPI.getDefaultInstance(applicationContext)?.onUserLogin(profileUpdate)
```

When the `OnUserLogin` method is called, the user profile information will be sent to CleverTap.

To see how this information displays within the CleverTap dashboard, perform the following:

1. Navigate to _Segments_ > _Find People_.
2. In the _By Identity_ box, enter the email you set on the user profile record, then click the **Find** button.

<Image align="center" alt={1407} border={true} caption="Find People on CleverTap Dashboard" title="Find People on CleverTap Dashboard" src="https://files.readme.io/f0467dc-1_By_identity_search_box.png" />

If CleverTap finds a user profile with this email, you will be taken to that user record. On that page, you will see the name and email as pre-defined fields and _Customer type_ and _Age_ as custom fields.

<Image align="center" alt={2912} border={true} caption="User Profile" title="User Profile" src="https://files.readme.io/c635bdd-2_User_Profile.png" />

## Update the User Profile

The `Onuserlogin` method identifies the individual users on the device. However, you may need to add additional user properties to the user profile such as gender, date of birth, and so on. You can update these properties with the `clevertap.profilePush` method. For more information, refer to [User Profiles](doc:concepts-user-profiles).

# Track Custom Events

Once you integrate the CleverTap SDK, we automatically track events, such as _App Launch_ and _Notification Viewed_. In addition to the [default (system) events tracked by CleverTap](https://developer.clevertap.com/docs/events#system-events) you can also track custom events.

To send custom events to CleverTap using our Android SDK, you have to call the `pushEvent` method with the name of the custom event you want to track.

```java JAVA
clevertapDefaultInstance.pushEvent("Product viewed");
```
```kotlin Kotlin
clevertapDefaultInstance?.pushEvent("Product viewed")
```

To view the same user profile in CleverTap from the last section, click on the **Activity** button within the user profile record. This shows a list of activities associated with the user profile.

If the custom event was successfully sent from your application to CleverTap, you would see it in this list.

<Image align="center" alt={2756} border={true} caption="User Profile Activity" title="User Profile Activity" src="https://files.readme.io/1a8aecb-3_User_Activities.png" />

## Debugging

CleverTap SDK generates a verbose number of logs that provide information about the working and integration of the SDK, which is beneficial during app development. To learn how to enable/disable these logs and use them for debugging, refer to the [Advance Features](https://developer.clevertap.com/docs/android-advanced-features#debugging) section.

## GDPR Compliance

From CleverTap Android SDK 3.1.9 onwards, CleverTap provides APIs to support GDPR compliance. For more information, refer to [Advance Features](https://developer.clevertap.com/docs/android-advanced-features#opt-out).

# Next Steps

By completing this guide, you automatically track user events like app launches and associate that information with profiles for each of your users. Also, learn how to add information to a user profile and track custom events. You now have integrated CleverTap into your Android application

In the following sections, you will learn more advanced options for tracking custom events and enriching user profiles.
