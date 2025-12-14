---
title: RudderStack in Android
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
# Available Features:

1. Pushing User Profile.
2. Pushing Events.
3. Push Notifications

# Set Source and Destination at RudderStack

Select source as *Android* and destination as *CleverTap*. 

![2080](https://files.readme.io/643cee7-Rudderstack_source.png "Rudderstack_source.png")

## CleverTap Configuration settings in RudderStack:

To successfully configure CleverTap as a destination, you will need to configure the following settings:

* Account ID: Your account ID is a unique ID generated for your account. It can be found in your account in the Settings as your Project ID. 
* Passcode: Your account passcode is a unique code generated for your account. It can be found in the Settings as Passcode.
* Token: Your account token is a unique code generated for your account. It can be found in the Settings as Token. 

![1836](https://files.readme.io/0261022-Rudderstack_clevertap_account.png "Rudderstack_clevertap_account.png")

Enable tracking for anonymous users: Enable this option to track anonymous users in CleverTap.

* Region: Server Only: This is your dedicated CleverTap region. For example, if your CleverTap URL is [CleverTap Dashboard](https://us1.dashboard.clevertap.com/login.html#/) then select *Singapore* as the region. 
* Use Native SDK to send Events: Enable this option. 

![1158](https://files.readme.io/e40c338-Rudderstack_native_sdk.png "Rudderstack_native_sdk.png")

For more information, see [RudderStack Documentation](https://docs.rudderstack.com/destinations/marketing/clevertap#getting-started).

# Configuring CleverTap within the app

Add your CleverTap Account ID and Token to your `AndroidManifest.xml` within the `<application></application> ` tags.

```java
<meta-data
    android:name="CLEVERTAP_ACCOUNT_ID"
    android:value="Your CleverTap Account ID"/>
<meta-data
    android:name="CLEVERTAP_TOKEN"
    android:value="Your CleverTap Account Token"/>
```

Add the region tag depending on where your account is hosted. For example, if your CleverTap URL is [CleverTap Dashboard](https://us1.dashboard.clevertap.com/login.html#/) then add the following tag in the AndroidManifest.xml file:

```java
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="sg1"/>
```

For more information, see [Configuration Guide for CleverTap Regions](https://developer.clevertap.com/docs/idc#section-android)

Register CleverTap’s ActivityLifecycleCallback methods. To do so, you can either set the `android:name` in your `AndroidManifest.xml` tag to `com.clevertap.android.sdk.Application`. Or, if you have a custom Application class, call `ActivityLifecycleCallback.register(this);` before `super.onCreate() `in your Application class.

This is essential to track lifecycle events on CleverTap like App Installed & App Launched, as well as for appropriate functioning of push notifications from CleverTap.

For more information, see [enable tracking by adding permissions](https://developer.clevertap.com/docs/android-quickstart-guide#section-enable-tracking-by-adding-permissions).

# Install SDK in Native application

To add the SDK as a dependency, perform the following steps:

Add these lines to your `app/build.gradle`:

```java
repositories {
  mavenCentral()
}
```

Add the following dependencies under dependencies within app/build.gradle:

```java
implementation 'com.rudderstack.android.sdk:core:1+'
implementation 'com.rudderstack.android.integration:clevertap:1+'

implementation 'com.clevertap.android:clevertap-android-sdk:4.0.0'
implementation 'com.google.android.gms:play-services-base:17.4.0'

implementation 'com.google.android.exoplayer:exoplayer:2.11.5' //Optional for Audio/Video
implementation 'com.google.android.exoplayer:exoplayer-hls:2.11.5' //Optional for Audio/Video
implementation 'com.google.android.exoplayer:exoplayer-ui:2.11.5' //Optional for Audio/Video
implementation 'com.github.bumptech.glide:glide:4.11.0' //Mandatory for App Inbox
implementation 'androidx.recyclerview:recyclerview:1.1.0' //Mandatory for App Inbox
implementation 'androidx.viewpager:viewpager:1.0.0' //Mandatory for App Inbox
implementation 'com.google.android.material:material:1.2.1' //Mandatory for App Inbox
implementation 'androidx.appcompat:appcompat:1.2.0' //Mandatory for App Inbox
implementation 'androidx.core:core:1.3.0'
implementation 'androidx.fragment:fragment:1.1.0' // InApp
//Mandatory for React Native SDK v0.3.9 and above add the following -
implementation 'com.android.installreferrer:installreferrer:2.1'
```

# Initialize RudderStack Client

Import the library on the classes you desire to use `RudderClient` library.

```java
import com.rudderstack.android.sdk.core.*;
```

Initialize the CleverTap-RudderStack SDK as follows:

```java
crudderClient = RudderClient.getInstance(
                    this,
                    <YOUR_WRITE_KEY>,
                    new RudderConfig.Builder()
                            //.withDataPlaneUrl(<YOUR_DATA_PLANE_URL>)
                            .withTrackLifecycleEvents(true)
                            .withLogLevel(RudderLogger.RudderLogLevel.DEBUG) 
                            .withFactory(CleverTapIntegrationFactory.FACTORY)
                            .build()
            );
```

* `YOUR_DATA_PLANE_URL`: You get this Data plane URL from the RudderStack console > Connections.

* `YOUR_WRITE_KEY` :You get this from the RudderStack console > Source > WRITE KEY.

* `trackAppLifecycleEvents`: This allows you to track Rudderstack SDK to capture their Lifecycle events.

* `logLevel`: This allows you to go through the application log while developing the application.

![1854](https://files.readme.io/c226ca9-Rudderstack_connection_key.png "Rudderstack_connection_key.png")

![1570](https://files.readme.io/699ad16-Rudderstack_write_key.png "Rudderstack_write_key.png")

# Push User Information

## When to call this method?

Call this method whenever you identify your user in your Application i.e. When a user tries to log in or Register to your application or when you want to update any user information.

## Why do you need to call this method?

When a user launches the app for the first time after installation, an “anonymous” profile gets created to track the activities performed by the user anonymously. The first time you identify a user on the device, the “anonymous” history on the device will be associated with the newly identified user.

## How to call this method?

You can call this method as follows:

```java
RudderTraits traits = new RudderTraits();
                traits.putBirthday(new Date());
                traits.putEmail("abc@123.com");
                traits.putFirstName("First");
                traits.putLastName("Last");
                traits.putGender("m");
                traits.putPhone("5555555555");
                traits.put("boolean", Boolean.TRUE);
                traits.put("integer", 50);
                traits.put("float", 120.4f);
                traits.put("long", 1234L);
                traits.put("string", "hello");
                traits.put("date", new Date(System.currentTimeMillis()));

                rudderClient.identify("test_user_id1", traits, null);
```

Replace userid with your database User Id (Unique Identifier of the user)

For more information, see [RudderStack Identify](https://docs.rudderstack.com/destinations/marketing/clevertap#identify).

# Push Events

## When to call this method?

Whenever you want to capture any user action in your application, call this method. For example, when a user is a login into your application and you want to capture this event then call the above method and instead of ‘testEvent’ pass ‘Login’ or instead of ‘Product Viewed’ pass ‘Login' and replace the key product viewed, category and Amount with the required event property.

## Points to remember

* Data type - The value of a property can be of type an Integer, a Long, a Double, a Float, a Character, a String, or a Boolean.

* For passing Date as a value, pass it as Date() object value only

## How to call this method?

You can call this method as follows:

```java
rudderClient.track(
                    "Product Added",
                    new RudderProperty()
                    .putValue("product_id", "product_001")
                );
```

For more information, see [Rudderstack Track](https://docs.rudderstack.com/destinations/marketing/clevertap#track).

## Pushing Order Completed (“Charged” Event in CleverTap)

Sample Code:

```java
rudderClient.track("Order Completed", {
      checkout_id: "12345",
      order_id: "1234",
      affiliation: "Apple Store",
      'Payment mode': "Credit Card",
      total: 20,
      revenue: 15.0,
      shipping: 22,
      tax: 1,
      discount: 1.5,
      coupon: "Games",
      currency: "USD",
      products: [
        {
          product_id: "123",
          sku: "G-32",
          name: "Monopoly",
          price: 14,
          quantity: 1,
          category: "Games",
          url: "https://www.website.com/product/path",
          image_url: "https://www.website.com/product/path.jpg",
        },
        {
          product_id: "345",
          sku: "F-32",
          name: "UNO",
          price: 3.45,
          quantity: 2,
          category: "Games",
        },
        {
          product_id: "125",
          sku: "S-32",
          name: "Ludo",
          price: 14,
          quantity: 7,
          category: "Games",
          brand: "Ludo King"
        },
      ],
    });
```

For more information, see [RudderStack Order Completed](https://docs.rudderstack.com/destinations/marketing/clevertap#order-completed).

# Integrate Push Notification in Android

## Adding Dependency

* Get the *google-services.json* file from the Firebase Cloud Messaging console and place it into your project under the *android/app* folder. To get the google-service.json file, follow the instructions from [Setup Firebase Console.](https://firebase.google.com/docs/android/setup#console).

* In your root-level (project-level) Gradle file (`build.gradle`), add rules to include the Google Services Gradle plugin. Check that you have Google's Maven repository, as well.

```java
buildscript {

  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository
  }

  dependencies {
    // ...

    // Add the following line:
    classpath 'com.google.gms:google-services:4.3.5'  // Google Services plugin
  }
}

allprojects {
  // ...

  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository
    // ...
  }
}
```

Navigate to your `Android/App` folder and add the following dependency to your `app/build.gradle` file:

```java
dependencies {
implementation 'com.google.firebase:firebase-messaging:20.2.4'
}

apply plugin: "com.google.gms.google-services"
```

Add the following service in your AndroidManifest.xml file :

```java
<service
android:name="com.clevertap.android.sdk.pushnotification.fcm.FcmMessageListenerService">
<intent-filter>
<action android:name="com.google.firebase.MESSAGING_EVENT"/>
</intent-filter>
</service>
```

Login to your CleverTap Dashboard. Go to *Settings > Channels > Mobile Push > Android*.\
Set-up FCM credentials.

## Setting up Notification Channel Id

Android has modified the way push notifications can be sent to an app in its Android version Oreo and above. Starting with CleverTap SDK 3.1.7, we support Android Oreo’s latest features like Notification Channels and Notification Badges. To create a Notification Channel Id, use the following Code.

```java
CleverTap.createNotificationChannel("CleverTapChannelId","CleverTap","This is a test channel",5,true);
```

Here,

* `CleverTapChannelId` - Replace this with your Notification Channel Id. Marketers will have to use this Channel Id while creating the Push Notification campaigns in CleverTap

* `CleverTap` - Replace this with Notification Channel Name, which you want Clients to see on their Device under the *Application Settings > Notifications*.

* `This is a test channel` - Replace this with your Notification Channel Description

* `5` - It is the Priority for rendering notification on the user device. Note: 5 is the highest priority.

* `true` - This is the flag for showing a badge on the user device.

For more information, see [Push Notifications for Android O](https://developer.clevertap.com/docs/android#section-push-notifications-for-android-o)

## custom handling android notification

In case you choose to use your custom FCMMessageListenerService (for eg: MyFcmMessageListenerService) and add the following code within the `MyFcmMessageListenerService.Java` file:

```java
package com.clevertaprudderstack; //Replace this with your Package Name

import com.google.firebase.messaging.RemoteMessage;
import com.clevertap.android.sdk.CleverTapAPI;
import android.os.Bundle; 
import android.util.Log;
import java.util.Map;
import com.clevertap.android.sdk.pushnotification.NotificationInfo;

public class MyFcmMessageListenerService extends FirebaseMessagingService {

    @Override
    public void onMessageReceived(RemoteMessage message){
        try {
            if (message.getData().size() > 0) {
                Bundle extras = new Bundle();
                for (Map.Entry<String, String> entry : message.getData().entrySet()) {
                    extras.putString(entry.getKey(), entry.getValue());
                }

                NotificationInfo info = CleverTapAPI.getNotificationInfo(extras);

                if (info.fromCleverTap) {
                    CleverTapAPI.createNotification(getApplicationContext(), extras);
                } else {
                    // not from CleverTap handle yourself or pass to another provider
                }
            }
        } catch (Throwable t) {
           Log.d("MYFCMLIST", "Error parsing FCM message", t);
        }
    }
}
```

Add the following code in your Android Manifest file:

```java
<service android:name=".MyFirebaseMessagingService">
  <intent-filter>
     <action android:name="com.google.firebase.MESSAGING_EVENT"/>
  </intent-filter>
</service>
```

Additionally, add the code to push the FCM token to CleverTap:

```java
String fcmRegId = FirebaseInstanceId.getInstance().getToken();
clevertapDefaultInstance.pushFcmRegistrationId(fcmRegId,true);
```

For more information, see [Handling custom push notifications in Android](https://developer.clevertap.com/docs/android#section-custom-android-push-notifications-handling).

# Debugging

* To view CleverTap debug logs, you will require Android Studio.
* Enable debug logs by adding the following line of code in your app's build: 

`CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG);`

For more information, see [Debugging in Android](https://developer.clevertap.com/docs/android#section-debugging)

* Once you run the application > go to Android Studio > Logcat > Search by CleverTap and you will be able to see all the logs printed over there. You can use this log to identify your CleverTap Id, check which events are getting a push to CleverTap, which user information is pushed to CleverTap, and so on.

For reference, sharing a sample screenshot of the Logcat which you will able to see in your Android Studio

![2808](https://files.readme.io/928dfed-Rudderstack_debugging.png "Rudderstack_debugging.png")

# Troubleshooting

Getting the following error

`Note: Recompile with -Xlint:deprecation for details.
D8: Cannot fit requested classes in a single dex file (# methods: 79994 > 65536 ; # fields: 68161 > 65536)
com.android.builder.dexing.DexArchiveMergerException: Error while merging dex archives: 
`

**Explanation**: Android can build only files up to 65K, if it exceeds more than that then you will get this error.

**Solution**: Add the following code in your `android/app/build.gradle` file:

```java
defaultConfig {
        multiDexEnabled true
    }
```
