---
title: CleverTap Huawei Push Integration
excerpt: Learn how to integrate and send push notifications from Huawei.
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
      slug: baidu-push-notifications
      title: CleverTap Baidu Push Integration
    - type: link
      title: Xiaomi Push Integration
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
    - type: link
      title: Android Resources
      url: https://developer.clevertap.com/docs/android-resources
---
Huawei uses its own push service to deliver notifications to their devices.

CleverTap can send push notifications powered by Huawei Cloud Push, an Android push notification delivery service.  We send a push notification through both Huawei Cloud Push and Firebase Cloud Messaging push services for a greater chance of delivery success. If a message is delivered through one of these push services, the notification from the other cloud service is suppressed. This ensures that the user will only receive the push notification once.

# Register as a Huawei Developer

The first step to accessing the Huawei cloud push is to register as a Huawei developer on the [Huawei Website](https://id5.cloud.huawei.com/CAS/portal/loginAuth.html).

<Image title="Register Huawei Developer" alt={1439} align="center" border={true} src="https://files.readme.io/fff209e-Huawei_Sign-up.png">
  Register Huawei Developer
</Image>

# Enable the Push Service

In the Huawei console, enable the push kit. Enable other settings as mentioned by Huawei [here](https://developer.huawei.com/consumer/en/codelab/HMSPushKit/index.html#3)

<Image title="Enable the Push Service" alt={1899} align="center" border={true} src="https://files.readme.io/c2b2b8c-EnableHuaweiPushKit.png">
  Enable the Push Service
</Image>

# Get the app's App ID/ App Secret

Once the App is created on your console, click on the App name to get your App ID/ App Secret. Among these, the AppID and AppKey are the client’s identity, used when the client SDK initializes; the AppSecret is authenticated for sending a message at the server-side.

# Integrate Huawei HMS SDK

Integrate the Huawei Mobile Services (HMS) SDK with your Android app to enable push notifications for Huawei devices.

Download the `agconnect-services.json` file from the [Huawei Console](https://developer.huawei.com/consumer/en/codelab/HMSPushKit/index.html#4). Move the downloaded `agconnect-services.json` file to the app directory of your Android Studio project.

> 🚧 SDK Compatibility Advisory
>
> CleverTap HMS SDK 1.5.0 and above is compatible with CleverTap Android SDK 7.3.1 and above.

### Configure the CleverTap Provider in `AndroidManifest.xml` (Mandatory for HMS SDK 1.5.0 and Above)

Add the following meta-data inside your `AndroidManifest.xml` file:

```coffeescript XML
<meta-data
    android:name="CLEVERTAP_PROVIDER_1"
    android:value="@string/hps_manifest_entry" />
```

Huawei push notifications can be integrated in two ways, either use the *CleverTap HMS SDK* or integrate the Huawei HMS SDK manually.\
Refer to the Huawei checklist for [integration preparation](https://developer.huawei.com/consumer/en/codelab/HMSPushKit/index.html#2).

## Method 1 - Configure Gradle files with *CleverTap HMS SDK*

It is easier to configure the Huawei push with CleverTap HMS SDK. 

### Step 1

Configure the root-level `build.gradle` file.

```groovy
buildscript {
    repositories {
        // HUAWEI ADD THIS
        maven {url 'https://developer.huawei.com/repo/'}
        
    }
    dependencies {
        classpath "com.android.tools.build:gradle:8.6.0" 
        
        // FOR HUAWEI ADD THIS
        classpath "com.huawei.agconnect:agcp:1.9.1.300" //check the version of the latest compatible library here : https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTHUAWEIPUSH.md
    }
}

allprojects {
    repositories {
        // HUAWEI ADD THIS
        maven {url 'https://developer.huawei.com/repo/'}
        
    }
}
```

### Step 2

Configure the `build.gradle` at the app-level.

```groovy
implementation "com.clevertap.android:clevertap-hms-sdk:1.5.1"
implementation "com.huawei.hms:push:6.11.0.300"

//At the bottom of the file add this
apply plugin: 'com.huawei.agconnect'
```

## Method 2 - Configure Manifest and Gradle files manually

Set up your app per the configuration provided by [Huawei](https://developer.huawei.com/consumer/en/codelab/HMSPushKit/index.html#4)

### Implement a Service class that extends the `HmsMessageService` class

To receive messages, you must implement a Service inherited from HmsMessageService and override all required methods in it: `onNewToken`, `onMessageReceived`, `onMessageSent` and `onSendError`  and then register your Service in the `AndroidManifest.xml` file. Method `onMessageReceived` is used to receive messages sent by CleverTap.

### Pass token to CleverTap

In your custom Service class that extends `HmsMessageService` class, inside the `onNewToken` method, pass the token to CleverTap using the following code. Use the appropriate method based on the HMS SDK version integrated in your app.

#### For HMS SDK 1.5.0 and Above

```java Java
@Override
public void onNewToken(@Nullable String token, @Nullable Bundle bundle) {
    super.onNewToken(token, bundle);
    if (token != null) {
        CleverTapAPI.getDefaultInstance(getApplicationContext())
            .pushRegistrationToken(token, HmsConstants.HPS, true);
    }
}
```
```coffeescript Kotlin
override fun onNewToken(token: String?, bundle: Bundle?) {
    super.onNewToken(token, bundle)
    token?.let {
        CleverTapAPI.getDefaultInstance(applicationContext)
            ?.pushRegistrationToken(it, HmsConstants.HPS, true)
    }
}
```

#### For HMS SDK 1.4.0 and Below

```java
@Override
public void onNewToken(@Nullable String token, @Nullable Bundle bundle) {
    super.onNewToken(token, bundle);
    if (token != null && cleverTapAPIInstance != null) {
        cleverTapAPIInstance.pushHuaweiRegistrationId(token, true);
    }
}
```
```kotlin
override fun onNewToken(token: String?, bundle: Bundle?) {
    super.onNewToken(token, bundle)
    token?.let {
        cleverTapAPIInstance?.pushHuaweiRegistrationId(it, true)
    }
}
```

### Receive Push Notifications from CleverTap

In the `onMessageReceived` method, write the following code for CleverTap to render Push Notification and raise the Notification Viewed event.

```java Java
@Override
public void onMessageReceived(RemoteMessage message) {
    IHmsMessageHandler mHandler = new CTHmsMessageHandler(new HmsNotificationParser());
    mHandler.createNotification(getApplicationContext(), message);
}
```
```kotlin Kotlin
override fun onMessageReceived(message: RemoteMessage) {
    val mHandler: IHmsMessageHandler = CTHmsMessageHandler(HmsNotificationParser())
    mHandler.createNotification(applicationContext, message)
}
```

> 📘 Important Note for CleverTap SDK v5.1.0+
>
> CleverTap Android SDK 5.1.0 onwards, the following API now runs on the caller’s thread.\
> Ensure you call it on a background thread in `onMessageReceived()` of messaging service:
>
> * `CTHmsMessageHandler().createNotification(getApplicationContext(), message)`
> * `CleverTapAPI.createNotification(getApplicationContext(), extras);`

# Update App ID/App Secret in Settings Dashboard

Enter your Huawei App ID and App Secret in the CleverTap Settings dashboard to enable push notification support for Huawei devices.

<Image title="Update App ID/Secret in the Settings Dashboard" alt={730} align="center" border={true} src="https://files.readme.io/cda5508-Huawei_Credentials.png">
  Update App ID/Secret in the Settings Dashboard
</Image>
