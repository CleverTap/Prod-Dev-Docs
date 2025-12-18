---
title: RudderStack Native Android Integration
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

The RudderStack Android SDK allows the following from your Android applications and sends it to CleverTap via RudderStack:

* Push User Profiles
* Push Events
* Integrate Push Notifications

# Prerequisites

The following are the prerequisites for performing this integration:

* A CleverTap account
* A RudderStack account
* A functional React Native app

# Integration

For CleverTap RudderStack Android integration, the following are the major steps:

1. [Set Up Source and Destination at RudderStack](doc:rudderstack-in-android#set-up-source-and-destination-at-rudderstack).
2. [Configure CleverTap within the application](doc:rudderstack-in-android#configure-clevertap-within-the-application).
3. [Install SDK in the Native application](doc:rudderstack-in-android#install-sdk-in-native-application).
4. [Initialize the RudderStack Client](doc:rudderstack-in-android#initialize-rudderstack-client).

## Set Up Source and Destination at RudderStack

To set up the source and destination at RudderStack:

1. Navigate to _Connect_ > _Sources_ from the RudderStack dashboard.
2. Click **New source** to add a new source and then select _React native_ from the list of sources.

<Image align="center" alt={2876} border={true} caption="Add New Android Source" title="Add New Android Source" src="https://files.readme.io/6c8ba2f-Add_New_Source_Android.png" />

3. Enter the source name under _Name this source_ field to identify it within your workspace and click **Continue**. The _Write Key_ for this source displays, as shown in the following figure:

<Image align="center" alt={2334} border={true} caption="Obtain Write Key Android" title="Obtain Write Key" src="https://files.readme.io/2d733fd-Write_Key.png" />

4. Click **Add Destination** and select the following options from the dropdown as per your requirement:
   * _Use Existing Destination_: Select from the list of available destinations.
   * _Create new Destination_: Create a new destination for your source.
5. Select _CleverTap_ from the destination list and click **Continue**.
6. Enter the destination name under _Name the destination_ field to identify it in your workspace and click **Continue**. The _Connection Settings_ page displays.
7. Enter the following CleverTap details:
   * **Account ID**: A unique ID generated for your account. It is available under _Settings_ > _Project_ as the _Project ID_.
   * **Passcode**: A unique code generated for your account. It is available under _Settings_ > _Project_ as the _Passcode_.
   * **Token**: A unique code generated for your account. It is available under _Settings_ > _Project_ as _Token_.
   * **Region**: Server Only. This is the region assigned to your CleverTap account. Refer to the following table to identify it:

| CleverTap Dashboard URL                                                                                | Region            |
| :----------------------------------------------------------------------------------------------------- | :---------------- |
| [https://in1.dashboard.clevertap.com/login.html#/](https://in1.dashboard.clevertap.com/login.html#/)   | India             |
| [https://sg1.dashboard.clevertap.com/login.html#/](https://sg1.dashboard.clevertap.com/login.html#/)   | Singapore         |
| [https://us1.dashboard.clevertap.com/login.html#/](https://us1.dashboard.clevertap.com/login.html#/)   | US                |
| [https://sk1.dashboard.clevertap.com/login.html#/](https://sk1.dashboard.clevertap.com/login.html#/)   | South Korea       |
| [https://eu1.dashboard.clevertap.com/login.html#/](https://eu1.dashboard.clevertap.com/login.html#/)   | None              |
| [https://aps3.dashboard.clevertap.com/login.html#/](https://aps3.dashboard.clevertap.com/login.html#/) | Indonesia         |
| [https://mec1.dashboard.clevertap.com/login.html#/](https://mec1.dashboard.clevertap.com/login.html#/) | Middle East (UAE) |

<Image align="center" alt={512} border={true} caption="Enter CleverTap Details" title="Enter CleverTap Details" src="https://files.readme.io/ca666fe-Connection_Settings_-_1.png" width="smart" />

8. Select the **Enable tracking for anonymous user** option to track anonymous users in CleverTap.
9. Select the **Use Clevertap ObjectId for Mapping** option under _React Native SDK settings_ section.  
   This option enables both CleverTap `objectId` and `identity` to map events from RudderStack to CleverTap.
10. Click **Continue**. The _Transformation_ settings page displays.
11. Select _No transformation needed_ if you do not want to customize JavaScript code functions to implement specific use cases on your event data.

<Image align="center" alt={1788} border={true} caption="Select Transformations" title="Select Transformations" src="https://files.readme.io/aeb7595-No_transformation_needed.png" />

12. Click **Continue**. The connection is now established between source and destination. You can check if the connection is enabled under _Connect_ > _Connections_.

<Image align="center" alt={2346} border={true} caption="View the Connection" title="View the Connection" src="https://files.readme.io/cf807be-Connection_Enabled.png" />

## Install SDK in React Native Application

We recommend installing the React Native SDK through `npm`. To add the SDK as a dependency, perform the following steps:

1. Navigate to the root of your application and add `@rudderstack/rudder-sdk-react-native` to your application as a dependency with:

```java
<meta-data
    android:name="CLEVERTAP_ACCOUNT_ID"
    android:value="Your CleverTap Account ID"/>
<meta-data
    android:name="CLEVERTAP_TOKEN"
    android:value="Your CleverTap Account Token"/>
```

2. Add the region tag based on the region of your account. Refer to the above table listed under [Set Up Source and Destination at RudderStack](doc:rudderstack-in-android#set-up-source-and-destination-at-rudderstack) to identify the region of your account. Based on the region of your account, add the tag as follows:

```xml India Region
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="in1"/>
```
```xml Singapore Region
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="sg1"/>
```
```xml U.S. Region
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="us1"/>
```

3. Register CleverTap’s ActivityLifecycleCallback methods. To do so, you can either set the `android:name` in your `AndroidManifest.xml` tag to `com.clevertap.android.sdk.Application`. Or, if you have a custom Application class, call `ActivityLifecycleCallback.register(this)`; before super.onCreate()` in your application class.

This is important for tracking lifecycle events on CleverTap, such as App Installed and App Launched, and for the appropriate functioning of push notifications from CleverTap.

## Install SDK in Native application

To install Android SDK within the application:

1. Add the following lines of code to your `app/build.gradle` file:

```groovy
repositories {
  mavenCentral()
}
```

2. Add the following dependencies under `dependencies` within `app/build.gradle` file:

```groovy
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

## Initialize RudderStack Client

To initialize the RudderStack client:

1. Import the library for the classes you desire to use in the `RudderClient` library.

```java
import com.rudderstack.android.sdk.core.*;
```

2. Initialize the CleverTap-RudderStack SDK as follows:

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

| Parameter                 | Description                                                                                                             |
| :------------------------ | :---------------------------------------------------------------------------------------------------------------------- |
| `YOUR_DATA_PLANE_URL`     | Navigate to _Connect_ > _Connections._ page from the RudderStack dashboard to get this data plane URL.                  |
| `YOUR_WRITE_KEY`          | Navigate to the _Connect_ > _Source_ page from the RudderStack dashboard and click **Details** to get this information. |
| `trackAppLifecycleEvents` | Allows you to track Rudderstack SDK to capture their Lifecycle events.                                                  |
| `logLevel`                | Allows you to check the application log when developing the application.                                                |

<Image align="center" alt={2880} border={true} caption="Obtain Data Plane URL" title="Data Plane URL" src="https://files.readme.io/3c8b00f-data_plane_URL.png" />

<Image align="center" alt={1570} border={true} caption="Obtain RudderStack Write Key" title="RudderStack Write Key.png" src="https://files.readme.io/699ad16-Rudderstack_write_key.png" />

# Push User Profiles

You can call the `rudderClient.identify()` method for pushing user profiles whenever you want to identify the user in your application, that is, when a user logs in or registers to your application or when you want to update any user information. After installing an app on a device, your user is assigned an "anonymous" profile. The first time when the application identifies the user on the device, the "anonymous" history on the device is associated with the newly identified user. You can call this method as follows:

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

Replace `userID` with the unique identifier for your application. This identifier must match the identifier selected on the CleverTap dashboard. For more information, refer to [RudderStack Identify](https://docs.rudderstack.com/destinations/marketing/clevertap#identify).

# Push Events

You can call rudderClient.track() for pushing events whenever you want to capture any user action in your application. For example, when a user adds any product, and you want to capture this event, then call this method and pass the following parameters:

* Event: For example, Product Added
* Event Properties: _product id_ or any other property that you want to pass

For more information, refer to [Rudderstack Track](https://docs.rudderstack.com/destinations/marketing/clevertap#track).

> 🚧 Event Property Data Types
>
> * The value of a property can be of type Integer, Long, Double, Float, Character, String, a Boolean.
> * The Date value must always be passed as $D_EPOCH value only.

Calling a push method may vary based on the type of event as follows:

* All events except the Order Completed event:

```java
rudderClient.track(
                    "Product Added",
                    new RudderProperty()
                    .putValue("product_id", "product_001")
                );
```

* Order Completed (equivalent to the Charged event in CleverTap)

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

The following are the three major steps to integrating push notifications into Android:

1. [Add Dependency](doc:rudderstack-in-android#add-dependency).
2. [Set Up Notification Channel Id](doc:rudderstack-in-android#set-up-notification-channel-id).
3. [Custom handle Android Notification](doc:rudderstack-in-android#custom-handle-android-notification).

## Add Dependency

To add a dependency:

1. Get the `google-services.json` file from the Firebase Cloud Messaging (FCM) console and place it into your project under the `android/app` folder. To get the `google-service.json` file, follow the steps listed under [Setup Firebase Console.](https://firebase.google.com/docs/android/setup#console).
2. In your root-level (project-level) Gradle file (`build.gradle`), add rules to include the _Google Services Gradle_ plugin.
3. Ensure that you have Google's Maven repository, as well.

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

4. Navigate to your `Android/App` folder and add the following dependency to your `app/build.gradle` file:

```java
dependencies {
implementation 'com.google.firebase:firebase-messaging:20.2.4'
}
apply plugin: "com.google.gms.google-services"
```

5. Add the following service in your `AndroidManifest.xml` file :

```xml
<service
    android:name="com.clevertap.android.sdk.pushnotification.fcm.FcmMessageListenerService"
android:exported="true">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter>
</service>
```

6. Navigate to _Settings > Channels > Mobile Push > Android_ from the CleverTap dashboard.
7. Set up the FCM credentials.

## Set Up Notification Channel Id

Android has modified the way push notifications can be sent to an app in its Oreo version and above. Starting with CleverTap SDK 3.1.7, we support Android Oreo's latest features, such as _Notification Channels_ and _Notification Badges_. To create a _Notification Channel Id_, use the following code:

```java
CleverTap.createNotificationChannel("CleverTapChannelId","CleverTap","This is a test channel",5,true);
```

The following table provides an explanation for the parameter values used in the above code:

| Parameter/Parameter Value | Description                                                                                                                                       |
| :------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------ |
| `CleverTapChannelId`      | Replace this with your _Notification Channel Id_. Marketers must use this Channel Id while creating the Push Notification campaigns in CleverTap. |
| `CleverTap`               | Replace this with the display name of the Notification Channel under the _Application Settings_ > _Notifications_.                                |
| `This is a test channel`  | Replace this with the description of your Notification Channel.                                                                                   |
| `5`                       | Denotes the priority for rendering notifications on the user's device. Priority 5 is the highest priority.                                        |
| `true`                    | A flag for showing a badge on the user's device.                                                                                                  |

For more information, refer to [Push Notifications for Android O](doc:android-push#push-notifications-for-android-o).

## Custom Handle Android Notification

The following are steps for custom handling your Android notifications:

1. Add the following code within the `MyFcmMessageListenerService.Java` file if you choose to use your custom `FCMMessageListenerService` as `MyFcmMessageListenerService`:

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

2. Add the following code to your Android manifest file:

```xml
<service android:name=".MyFirebaseMessagingService">
  <intent-filter>
     <action android:name="com.google.firebase.MESSAGING_EVENT"/>
  </intent-filter>
</service>
```

3. Additionally, add the code to push the FCM token to CleverTap:

```java
String fcmRegId = FirebaseInstanceId.getInstance().getToken();
clevertapDefaultInstance.pushFcmRegistrationId(fcmRegId,true);
```

For more information, refer to [Handling custom push notifications in Android](doc:android-push#custom-android-push-notification-handling).

# Advanced Features

## Debugging

1. To view CleverTap debug logs, you need Android Studio.
2. Enable debug logs by adding the following line of code in your app's build:  
   `CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG);`
3. After running the application, navigate to _Android Studio > Logcat and search by _CleverTap*. The printed logs display all information associated with CleverTap. You can use this log to identify your CleverTap ID, check the events and user information that are getting pushed to CleverTap, etc.

The following is a sample screenshot of the Logcat in your Android Studio:

<Image align="center" alt={2808} border={false} caption="Logcat in Android Studio" title="Logcat in Android Studio" src="https://files.readme.io/928dfed-Rudderstack_debugging.png" />

For more information, refer to [Debugging in Android](https://developer.clevertap.com/docs/android#section-debugging).
