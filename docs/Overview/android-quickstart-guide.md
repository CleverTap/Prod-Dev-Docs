---
title: Android Quick Start Guide
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
CleverTap provides an Android SDK that enables app developers to track, segment, and engage their users. 

This guide will show you how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard in less than ten minutes. 

# Step 1: Install SDK

To use the CleverTap Android SDK, you have two options. You can install it automatically using Gradle in Android Studio or manually install it by including the SDK source code in your Android Studio project. 

## Option A - Install using Android Studio and Gradle (Recommended) 

CleverTap supports Firebase Cloud Messaging (FCM). 

For using FCM, add the dependencies below in your application’s build.gradle file. You will also have to add the FCM generated google-services.json file to your project. **ASDK**

```groovy
dependencies {
    implementation 'com.clevertap.android:clevertap-android-sdk:4.0.1'
    implementation 'com.google.firebase:firebase-messaging:20.2.4'
		implementation 'androidx.core:core:1.3.0'
  //Mandatory for CleverTap Android SDK v3.6.4 and above add the following -
		implementation 'com.android.installreferrer:installreferrer:2.1'
}
// at the end of the build.gradle file
apply plugin: 'com.google.gms.google-services'
```

Add the following configuration to your gradle properties file. 

```groovy
android.useAndroidX=true
```

Once you have updated your build.gradle file, sync your project by clicking on Sync Project With Gradle Files button. You could find this button by navigating to the Tools menu and then the Android submenu.

![509](https://files.readme.io/001ca2d-image3.png "image3.png")

## Option B - Manual Install

[Download](https://github.com/CleverTap/clevertap-android-sdk/releases/download/3.7.2/clevertap-android-sdk-3.7.2.aar) and copy the AAR file to the `/libs` directory of your project and then add it as a dependency to your project.

# Step 2: Add Your CleverTap Credentials in AndroidManifest.xml

To associate your Android app with your CleverTap account, you will need to add your CleverTap credentials in the AndroidManifest.xml file in your application.

Navigate to the AndroidManifest.xml file in your project navigator. 

<Image title="android-2018-1.png" alt={1027} className="border" border={true} src="https://files.readme.io/be3caa0-android-2018-1.png" />

Add your CleverTap Account ID and Token to your AndroidManifest.xml, within the <application></application> tags.

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

> 🚧 Using Google AD ID to uniquely identify users
>
> GDPR mandates that if you are using Google AD ID to uniquely identify users, there should be prominent disclosure in your application that explains this to your end customer.  You can read more about  GDPR [here](https://clevertap.com/blog/in-preparation-of-gdpr-compliance/). 
>
> Note: if you still want to use Google AD ID to generate a users identity, so that you can identify users across re-installs, add the meta tag that force uses the Google AD ID to generate an identity in CleverTap.

Insert the Account ID and Account Token values from your CleverTap account. These values are available on the Settings page. To navigate to the Settings page, log into [your CleverTap account](https://eu1.dashboard.clevertap.com/login.html), click on the gear icon on the bottom left navigation, and select Settings dashboard.

<Image title="1.png" alt={1088} width="smart" src="https://files.readme.io/55e91dc-1.png" />

<Image title="2.png" alt={1449} className="border" width="80%" border={true} src="https://files.readme.io/d1467d9-2.png" />

# Step 3: Enable Tracking by Adding Permissions

In your AndroidManifest.xml file, add the following snippet within the <application></application> tags.

```xml
<application
    android:label="@string/app_name"
    android:icon="@drawable/ic_launcher"
    android:name="com.clevertap.android.sdk.Application">
```

![796](https://files.readme.io/319c1ad-image6.png "image6.png")

Next add the snippet below in the same file, so the CleverTap SDK can access the internet.

```xml
<!-- Required to allow the app to send events and user profile information -->
<uses-permission android:name="android.permission.INTERNET"/>
<!-- Recommended so that CleverTap knows when to attempt a network call -->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

<Image title="image2.png" alt={935} className="border" border={true} src="https://files.readme.io/da53966-image2.png" />

> 🚧 Custom Application Class
>
> If you have a custom application class, call ActivityLifecycleCallback.register(this); before super.onCreate() in your class.

# Step 4: Initialize the CleverTap SDK

In the onCreate() method of your application’s main activity include the code below. This code will initialize the CleverTap SDK.

```java JAVA
CleverTapAPI clevertapDefaultInstance = CleverTapAPI.getDefaultInstance(getApplicationContext());
```
```kotlin Kotlin
var cleverTapDefaultInstance: CleverTapAPI? = null
cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(applicationContext)
```

# Step 5: Run Your Application

Build and run your application. 

Navigate to your CleverTap dashboard. If you successfully integrated the CleverTap SDK, you will see a new active user.

![1320](https://files.readme.io/3a402b2-image5.png "image5.png")

# Step 6: Add Information to a User Profile

A User Profile is automatically created in CleverTap for each user launching your application.

Initially, the User Profile starts out as anonymous, which means the profile does not contain any identifiable information about the user. You can enrich the profile with pre-defined attributes from the CleverTap data model, such as name and email. You can also add custom attributes that you define to extend the CleverTap data model.

Sending user profile information to CleverTap using our Android SDK requires two steps. First, you have to build a HashMap with the profile properties. Second, you have to call the SDK's profile.push method and pass the HashMap you created as a parameter. 

```java JAVA
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();

//Update pre-defined profile properties
profileUpdate.put("Name", "Jack Montana");
profileUpdate.put("Email", "jack@gmail.com");      
//Update custom profile properties
profileUpdate.put("Plan Type", "Silver");
profileUpdate.put("Favorite Food", "Pizza");

clevertapDefaultInstance.pushProfile(profileUpdate);
```
```kotlin Kotlin
val profileUpdate = HashMap<String, Any>()
profileUpdate["Name"] = "Jack Montana"
profileUpdate["Email"] = "jack@gmail.com"
profileUpdate["Plan Type"] = "Silver"
profileUpdate["Favorite Food"] = "Pizza"
clevertapDefaultInstance?.pushProfile(profileUpdate)
```

When the profile.push method is called, the user profile information will be sent to CleverTap.

Let's see how this information appears within the CleverTap dashboard. Log into the dashboard, and then click on the Find People button under the Segment tab. In the By Identity box, enter the email you set on the user profile record, and then click the Find button.

<Image title="4.png" alt={2880} className="border" border={true} src="https://files.readme.io/701c748-4.png" />

If CleverTap finds a user profile with this email, you will be taken to that user record. On that page, we'll see name and email as pre-defined fields, and Customer type and Age as custom fields.

<Image title="5.png" alt={2752} className="border" border={true} src="https://files.readme.io/5b1bb26-5.png" />

# Step 7: Track Custom Events

Once you integrate the CleverTap SDK, we automatically start tracking events, such as App Launch and Notification Viewed. In addition to the [default events tracked by CleverTap](https://developer.clevertap.com/docs/concepts-events), you can also track custom events.

To send custom events to CleverTap using our Android SDK, you will have to call the pushEvent method with the name of the custom event you want to track. 

```java JAVA
clevertapDefaultInstance.pushEvent("Product viewed");
```
```kotlin Kotlin
clevertapDefaultInstance?.pushEvent("Product viewed")
```

Let's look at the same user profile in CleverTap from step 6. Click on the Activity button within the user profile record. This will show a list of activities associated with the user profile. If the custom event was successfully sent from your application to CleverTap, you will see it in this list.

<Image title="6.png" alt={2756} className="border" border={true} src="https://files.readme.io/5340063-6.png" />

# Next Steps 

Congrats on successfully integrating CleverTap into your Android application! 

You are now automatically tracking user events like app launches, and associating that information with profiles for each of your users. You also learned how to add information to a user profile and how to track custom events.

In the [next Android guide](doc:android), you will learn more advanced options for tracking custom events and enriching user profiles.
