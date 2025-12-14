---
title: Fire TV Integration with Android SDK
excerpt: Learn how to integrate CleverTap SDK  for apps on Fire TV.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Introduction to Fire TV and FireOS

Fire TV stick is a device that can transform any TV into a smart TV. It consists of a remote control and a USB device inserted into a TV or hub. The USB essentially contains FireOS, a fork of AOSP. The latest FireTV is a fork of Android Open Source Project (AOSP) Android Pie. As a result, it can transform a conventional TV into a FireOS-based TV with a remote control.

# Develop Apps For FireOS

Since Fire OS is a platform from Amazon based on Android OS, the applications are created for Fire OS in a similar manner. However, Fire OS does not support certain Google APIs and hence does not allow the working of certain libraries, such as FCM. You can integrate the CleverTap SDK with your Fire OS App to engage with customers on Fire TV.

For more information on how Fire TV development differs from Android TV development, refer to the [document. ](https://developer.amazon.com/docs/fire-tv/differences-from-android-tv-development.html). 

# Integrate CleverTap with FireTV Apps

FireOS does not support certain Google APIs and Libraries. Therefore, we cannot use CleverTap-based push notifications with a Fire TV app. For more information, refer to the [Smart TV Feature Matrix](https://developer.clevertap.com/docs/feature-matrix-for-smart-tv-sdks).

1. [Integrate CleverTap SDK in Gradle and Manifest](https://developer.clevertap.com/docs/fire-tv-integration-with-android-sdk#integrate-clevertap-sdk-in-gradle-and-manifest)
2. [Initialize CleverTap SDK in the Application Class](https://developer.clevertap.com/docs/fire-tv-integration-with-android-sdk#initialize-clevertap-sdk-in-the-application-class)
3. [Use the CleverTapAPI Instance to Run various Functions](https://developer.clevertap.com/docs/fire-tv-integration-with-android-sdk#use-the-clevertapapi-instance-to-run-various-functions)

## Integrate CleverTap SDK in Gradle and Manifest

This section describes how to integrate CleverTap SDK in Gradle and Manifest files. 

- [Gradle Files](https://developer.clevertap.com/docs/fire-tv-integration-with-android-sdk#gradle-files)
- [Manifest Files](https://developer.clevertap.com/docs/fire-tv-integration-with-android-sdk#manifest-files)

### Gradle Files

To integrate CleverTap SDK in the Gradle file, add the dependencies below in your application's `build.gradle` file:

```groovy
/* ============ src/build.gradle ========== */

//...
dependencies {
    implementation "androidx.core:core-ktx:1.7.0"
    implementation "androidx.leanback:leanback:1.0.0"
    implementation "androidx.appcompat:appcompat:1.4.0"
    implementation "androidx.constraintlayout:constraintlayout:2.1.2"

    // google libs
    implementation "com.google.android.gms:play-services-location:18.0.0" 
    implementation "com.google.android.exoplayer:exoplayer:2.15.1"
    implementation "com.google.android.material:material:1.4.0"

    // com.android libs
    implementation "com.android.installreferrer:installreferrer:2.2"
    

    // 3rd party libs
    implementation "com.github.bumptech.glide:glide:4.11.0"
    
    //CLEVERTAP Libs
    implementation "com.clevertap.android:clevertap-android-sdk:7.7.1"
    
}
```

### Manifest Files

To integrate CleverTap SDK in the Manifest file, use the following code:

```xml
<!-- ============ src/main/AndroidManifest.xml ============ -->

<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    
    <!--  required by clevertap sdk  -->
    <uses-permission android:name="android.permission.INTERNET" />
    
    
    <!-- ... -->
    
    <application android:name=".MainApp">
        <meta-data android:name="CLEVERTAP_ACCOUNT_ID" android:value="YOUR_ACCOUNT_ID" />
        <meta-data android:name="CLEVERTAP_TOKEN" android:value="YOUR_TOKEN" />
        <meta-data android:name="CLEVERTAP_REGION" android:value="YOUR_REGION"/>
    
        <!-- ... -->
        
    </application>
</manifest>
```

## Initialize CleverTap SDK in the Application class

To initialize CleverTap SDK in the Application class, use the following code: 

```kotlin
class MainApp : Application() {

    var cleverTapAPI: CleverTapAPI? = null

    override fun onCreate() {
        CleverTapAPI.setDebugLevel(com.clevertap.android.sdk.CleverTapAPI.LogLevel.VERBOSE)
        ActivityLifecycleCallback.register(this)

        super.onCreate()
        cleverTapAPI = CleverTapAPI.getDefaultInstance(applicationContext)

        val importance = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) NotificationManager.IMPORTANCE_MAX else 5
        CleverTapAPI.createNotificationChannel(applicationContext, "id", "name", "description", importance, true)

    }
    /*...*/
}
```

## Use the CleverTapAPI Instance to Run Various Functions

You can call the `CleverTapAPI` instance to run the following functions:

### Send User Profile

To send a user profile to CleverTap, use the following code: 

```kotlin
val map = hashMapOf<String,Any>("Name" to "new_tv_user", "Email" to "new_tv_user@gmail.com")
cleverTapAPI.onUserLogin(map)
```

### Push Event

For example, let's say you want to record an event called "BUTTON_PRESSED" when a user clicks on a button in the Custom Push. To record this event with the CleverTap SDK, you can use the following code snippet:

```kotlin
cleverTapAPI.pushEvent("BUTTON_PRESSED")
```

### Push Event with Parameters

To push events with properties to CleverTap, use the following code:

```kotlin
val date = Date().toString()
cleverTapAPI.pushEvent("REMOTE_BUTTON_PRESSED", mapOf("time" to date))
```

### Push Charged Event

To push a _Charged_ event to CleverTap, use the following code:

```kotlin
val charges = hashMapOf<String,Any>("Total Number Of Items" to "4", "Total Amount" to "400")
val items = arrayListOf(hashMapOf<String,Any>("Item name" to "jeans", "Number of Items" to "4", "Item category" to "clothing", "Amount" to "400"))
cleverTapAPI.pushChargedEvent(charges,items)
```

### Add Profile Properties

To add profile properties, use the following code:

```kotlin
cleverTapAPI.addMultiValueForKey("userTVCount","1")
```

### Remove Profile Properties

To remove profile properties, use the following code:

```kotlin
cleverTapAPI.removeValueForKey("userTVCount")
```