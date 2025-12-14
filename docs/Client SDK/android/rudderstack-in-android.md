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


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/643cee7-Rudderstack_source.png",
        "Rudderstack_source.png",
        2080,
        228,
        "#f5f6f4"
      ]
    }
  ]
}
[/block]
## CleverTap Configuration settings in RudderStack:
To successfully configure CleverTap as a destination, you will need to configure the following settings:

  * Account ID: Your account ID is a unique ID generated for your account. It can be found in your account in the Settings as your Project ID. 
  * Passcode: Your account passcode is a unique code generated for your account. It can be found in the Settings as Passcode.
  * Token: Your account token is a unique code generated for your account. It can be found in the Settings as Token. 


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0261022-Rudderstack_clevertap_account.png",
        "Rudderstack_clevertap_account.png",
        1836,
        1114,
        "#e7e8ea"
      ]
    }
  ]
}
[/block]
Enable tracking for anonymous users: Enable this option to track anonymous users in CleverTap.

  * Region: Server Only: This is your dedicated CleverTap region. For example, if your CleverTap URL is [CleverTap Dashboard](https://us1.dashboard.clevertap.com/login.html#/) then select *Singapore* as the region. 
  * Use Native SDK to send Events: Enable this option. 


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e40c338-Rudderstack_native_sdk.png",
        "Rudderstack_native_sdk.png",
        1158,
        1456,
        "#f4f7fa"
      ]
    }
  ]
}
[/block]
For more information, see [RudderStack Documentation](https://docs.rudderstack.com/destinations/marketing/clevertap#getting-started).

# Configuring CleverTap within the app

Add your CleverTap Account ID and Token to your `AndroidManifest.xml` within the `<application></application> ` tags.
[block:code]
{
  "codes": [
    {
      "code": "<meta-data\n    android:name=\"CLEVERTAP_ACCOUNT_ID\"\n    android:value=\"Your CleverTap Account ID\"/>\n<meta-data\n    android:name=\"CLEVERTAP_TOKEN\"\n    android:value=\"Your CleverTap Account Token\"/>",
      "language": "java"
    }
  ]
}
[/block]
Add the region tag depending on where your account is hosted. For example, if your CleverTap URL is [CleverTap Dashboard](https://us1.dashboard.clevertap.com/login.html#/) then add the following tag in the AndroidManifest.xml file:
[block:code]
{
  "codes": [
    {
      "code": "<meta-data \nandroid:name=\"CLEVERTAP_REGION\" \nandroid:value=\"sg1\"/>",
      "language": "java"
    }
  ]
}
[/block]
For more information, see [Configuration Guide for CleverTap Regions](https://developer.clevertap.com/docs/idc#section-android)


Register CleverTap’s ActivityLifecycleCallback methods. To do so, you can either set the `android:name` in your `AndroidManifest.xml` tag to `com.clevertap.android.sdk.Application`. Or, if you have a custom Application class, call `ActivityLifecycleCallback.register(this);` before `super.onCreate() `in your Application class.

This is essential to track lifecycle events on CleverTap like App Installed & App Launched, as well as for appropriate functioning of push notifications from CleverTap.

For more information, see [enable tracking by adding permissions](https://developer.clevertap.com/docs/android-quickstart-guide#section-enable-tracking-by-adding-permissions).

# Install SDK in Native application

To add the SDK as a dependency, perform the following steps:

Add these lines to your `app/build.gradle`:


[block:code]
{
  "codes": [
    {
      "code": "repositories {\n  mavenCentral()\n}\n",
      "language": "java"
    }
  ]
}
[/block]
Add the following dependencies under dependencies within app/build.gradle:
[block:code]
{
  "codes": [
    {
      "code": "implementation 'com.rudderstack.android.sdk:core:1+'\nimplementation 'com.rudderstack.android.integration:clevertap:1+'\n\nimplementation 'com.clevertap.android:clevertap-android-sdk:4.0.0'\nimplementation 'com.google.android.gms:play-services-base:17.4.0'\n\nimplementation 'com.google.android.exoplayer:exoplayer:2.11.5' //Optional for Audio/Video\nimplementation 'com.google.android.exoplayer:exoplayer-hls:2.11.5' //Optional for Audio/Video\nimplementation 'com.google.android.exoplayer:exoplayer-ui:2.11.5' //Optional for Audio/Video\nimplementation 'com.github.bumptech.glide:glide:4.11.0' //Mandatory for App Inbox\nimplementation 'androidx.recyclerview:recyclerview:1.1.0' //Mandatory for App Inbox\nimplementation 'androidx.viewpager:viewpager:1.0.0' //Mandatory for App Inbox\nimplementation 'com.google.android.material:material:1.2.1' //Mandatory for App Inbox\nimplementation 'androidx.appcompat:appcompat:1.2.0' //Mandatory for App Inbox\nimplementation 'androidx.core:core:1.3.0'\nimplementation 'androidx.fragment:fragment:1.1.0' // InApp\n//Mandatory for React Native SDK v0.3.9 and above add the following -\nimplementation 'com.android.installreferrer:installreferrer:2.1'",
      "language": "java"
    }
  ]
}
[/block]
# Initialize RudderStack Client

Import the library on the classes you desire to use `RudderClient` library.
[block:code]
{
  "codes": [
    {
      "code": "import com.rudderstack.android.sdk.core.*;\n",
      "language": "java"
    }
  ]
}
[/block]
Initialize the CleverTap-RudderStack SDK as follows:
[block:code]
{
  "codes": [
    {
      "code": "crudderClient = RudderClient.getInstance(\n                    this,\n                    <YOUR_WRITE_KEY>,\n                    new RudderConfig.Builder()\n                            //.withDataPlaneUrl(<YOUR_DATA_PLANE_URL>)\n                            .withTrackLifecycleEvents(true)\n                            .withLogLevel(RudderLogger.RudderLogLevel.DEBUG) \n                            .withFactory(CleverTapIntegrationFactory.FACTORY)\n                            .build()\n            );\n            ",
      "language": "java"
    }
  ]
}
[/block]
  * `YOUR_DATA_PLANE_URL`: You get this Data plane URL from the RudderStack console > Connections.

  * `YOUR_WRITE_KEY` :You get this from the RudderStack console > Source > WRITE KEY.

  * `trackAppLifecycleEvents`: This allows you to track Rudderstack SDK to capture their Lifecycle events.
 
  * `logLevel`: This allows you to go through the application log while developing the application.
  



[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c226ca9-Rudderstack_connection_key.png",
        "Rudderstack_connection_key.png",
        1854,
        456,
        "#b7bbc7"
      ]
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/699ad16-Rudderstack_write_key.png",
        "Rudderstack_write_key.png",
        1570,
        386,
        "#fcfcfb"
      ]
    }
  ]
}
[/block]
# Push User Information

## When to call this method?

Call this method whenever you identify your user in your Application i.e. When a user tries to log in or Register to your application or when you want to update any user information.

## Why do you need to call this method?

When a user launches the app for the first time after installation, an “anonymous” profile gets created to track the activities performed by the user anonymously. The first time you identify a user on the device, the “anonymous” history on the device will be associated with the newly identified user.

## How to call this method?

You can call this method as follows:

[block:code]
{
  "codes": [
    {
      "code": "RudderTraits traits = new RudderTraits();\n                traits.putBirthday(new Date());\n                traits.putEmail(\"abc@123.com\");\n                traits.putFirstName(\"First\");\n                traits.putLastName(\"Last\");\n                traits.putGender(\"m\");\n                traits.putPhone(\"5555555555\");\n                traits.put(\"boolean\", Boolean.TRUE);\n                traits.put(\"integer\", 50);\n                traits.put(\"float\", 120.4f);\n                traits.put(\"long\", 1234L);\n                traits.put(\"string\", \"hello\");\n                traits.put(\"date\", new Date(System.currentTimeMillis()));\n\n                rudderClient.identify(\"test_user_id1\", traits, null);\n\n\n",
      "language": "java"
    }
  ]
}
[/block]
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



[block:code]
{
  "codes": [
    {
      "code": "rudderClient.track(\n                    \"Product Added\",\n                    new RudderProperty()\n                    .putValue(\"product_id\", \"product_001\")\n                );\n",
      "language": "java"
    }
  ]
}
[/block]
For more information, see [Rudderstack Track](https://docs.rudderstack.com/destinations/marketing/clevertap#track).

## Pushing Order Completed (“Charged” Event in CleverTap)

Sample Code:
[block:code]
{
  "codes": [
    {
      "code": "rudderClient.track(\"Order Completed\", {\n      checkout_id: \"12345\",\n      order_id: \"1234\",\n      affiliation: \"Apple Store\",\n      'Payment mode': \"Credit Card\",\n      total: 20,\n      revenue: 15.0,\n      shipping: 22,\n      tax: 1,\n      discount: 1.5,\n      coupon: \"Games\",\n      currency: \"USD\",\n      products: [\n        {\n          product_id: \"123\",\n          sku: \"G-32\",\n          name: \"Monopoly\",\n          price: 14,\n          quantity: 1,\n          category: \"Games\",\n          url: \"https://www.website.com/product/path\",\n          image_url: \"https://www.website.com/product/path.jpg\",\n        },\n        {\n          product_id: \"345\",\n          sku: \"F-32\",\n          name: \"UNO\",\n          price: 3.45,\n          quantity: 2,\n          category: \"Games\",\n        },\n        {\n          product_id: \"125\",\n          sku: \"S-32\",\n          name: \"Ludo\",\n          price: 14,\n          quantity: 7,\n          category: \"Games\",\n          brand: \"Ludo King\"\n        },\n      ],\n    });\n    \n    ",
      "language": "java"
    }
  ]
}
[/block]
For more information, see [RudderStack Order Completed](https://docs.rudderstack.com/destinations/marketing/clevertap#order-completed).

# Integrate Push Notification in Android

## Adding Dependency
  * Get the *google-services.json* file from the Firebase Cloud Messaging console and place it into your project under the *android/app* folder. To get the google-service.json file, follow the instructions from [Setup Firebase Console.](https://firebase.google.com/docs/android/setup#console).
 
  * In your root-level (project-level) Gradle file (`build.gradle`), add rules to include the Google Services Gradle plugin. Check that you have Google's Maven repository, as well.

[block:code]
{
  "codes": [
    {
      "code": "buildscript {\n\n  repositories {\n    // Check that you have the following line (if not, add it):\n    google()  // Google's Maven repository\n  }\n\n  dependencies {\n    // ...\n\n    // Add the following line:\n    classpath 'com.google.gms:google-services:4.3.5'  // Google Services plugin\n  }\n}\n\nallprojects {\n  // ...\n\n  repositories {\n    // Check that you have the following line (if not, add it):\n    google()  // Google's Maven repository\n    // ...\n  }\n}\n",
      "language": "java"
    }
  ]
}
[/block]
Navigate to your `Android/App` folder and add the following dependency to your `app/build.gradle` file:

[block:code]
{
  "codes": [
    {
      "code": "dependencies {\nimplementation 'com.google.firebase:firebase-messaging:20.2.4'\n}\n\napply plugin: \"com.google.gms.google-services\"\n\n  ",
      "language": "java"
    }
  ]
}
[/block]
Add the following service in your AndroidManifest.xml file :
[block:code]
{
  "codes": [
    {
      "code": "<service\nandroid:name=\"com.clevertap.android.sdk.pushnotification.fcm.FcmMessageListenerService\">\n<intent-filter>\n<action android:name=\"com.google.firebase.MESSAGING_EVENT\"/>\n</intent-filter>\n</service>\n\n",
      "language": "java"
    }
  ]
}
[/block]
Login to your CleverTap Dashboard. Go to *Settings > Channels > Mobile Push > Android*. 
Set-up FCM credentials.

## Setting up Notification Channel Id

Android has modified the way push notifications can be sent to an app in its Android version Oreo and above. Starting with CleverTap SDK 3.1.7, we support Android Oreo’s latest features like Notification Channels and Notification Badges. To create a Notification Channel Id, use the following Code.

[block:code]
{
  "codes": [
    {
      "code": " CleverTap.createNotificationChannel(\"CleverTapChannelId\",\"CleverTap\",\"This is a test channel\",5,true);\n",
      "language": "java"
    }
  ]
}
[/block]
Here,

  * `CleverTapChannelId` - Replace this with your Notification Channel Id. Marketers will have to use this Channel Id while creating the Push Notification campaigns in CleverTap
 
  * `CleverTap` - Replace this with Notification Channel Name, which you want Clients to see on their Device under the *Application Settings > Notifications*.
  
  * `This is a test channel` - Replace this with your Notification Channel Description
 
  * `5` - It is the Priority for rendering notification on the user device. Note: 5 is the highest priority.
 
  * `true` - This is the flag for showing a badge on the user device.

For more information, see [Push Notifications for Android O](https://developer.clevertap.com/docs/android#section-push-notifications-for-android-o)

## custom handling android notification

In case you choose to use your custom FCMMessageListenerService (for eg: MyFcmMessageListenerService) and add the following code within the `MyFcmMessageListenerService.Java` file:
[block:code]
{
  "codes": [
    {
      "code": "package com.clevertaprudderstack; //Replace this with your Package Name\n\nimport com.google.firebase.messaging.RemoteMessage;\nimport com.clevertap.android.sdk.CleverTapAPI;\nimport android.os.Bundle; \nimport android.util.Log;\nimport java.util.Map;\nimport com.clevertap.android.sdk.pushnotification.NotificationInfo;\n\npublic class MyFcmMessageListenerService extends FirebaseMessagingService {\n\n    @Override\n    public void onMessageReceived(RemoteMessage message){\n        try {\n            if (message.getData().size() > 0) {\n                Bundle extras = new Bundle();\n                for (Map.Entry<String, String> entry : message.getData().entrySet()) {\n                    extras.putString(entry.getKey(), entry.getValue());\n                }\n\n                NotificationInfo info = CleverTapAPI.getNotificationInfo(extras);\n\n                if (info.fromCleverTap) {\n                    CleverTapAPI.createNotification(getApplicationContext(), extras);\n                } else {\n                    // not from CleverTap handle yourself or pass to another provider\n                }\n            }\n        } catch (Throwable t) {\n           Log.d(\"MYFCMLIST\", \"Error parsing FCM message\", t);\n        }\n    }\n}\n\n",
      "language": "java"
    }
  ]
}
[/block]
Add the following code in your Android Manifest file:

[block:code]
{
  "codes": [
    {
      "code": "<service android:name=\".MyFirebaseMessagingService\">\n  <intent-filter>\n     <action android:name=\"com.google.firebase.MESSAGING_EVENT\"/>\n  </intent-filter>\n</service>\n       ",
      "language": "java"
    }
  ]
}
[/block]
Additionally, add the code to push the FCM token to CleverTap:
[block:code]
{
  "codes": [
    {
      "code": "String fcmRegId = FirebaseInstanceId.getInstance().getToken();\nclevertapDefaultInstance.pushFcmRegistrationId(fcmRegId,true);\n",
      "language": "java"
    }
  ]
}
[/block]
For more information, see [Handling custom push notifications in Android](https://developer.clevertap.com/docs/android#section-custom-android-push-notifications-handling).

# Debugging
  * To view CleverTap debug logs, you will require Android Studio.
  * Enable debug logs by adding the following line of code in your app's build: 

`CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG);`

For more information, see [Debugging in Android](https://developer.clevertap.com/docs/android#section-debugging)

* Once you run the application > go to Android Studio > Logcat > Search by CleverTap and you will be able to see all the logs printed over there. You can use this log to identify your CleverTap Id, check which events are getting a push to CleverTap, which user information is pushed to CleverTap, and so on.

For reference, sharing a sample screenshot of the Logcat which you will able to see in your Android Studio


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/928dfed-Rudderstack_debugging.png",
        "Rudderstack_debugging.png",
        2808,
        1360,
        "#e6e5e5"
      ]
    }
  ]
}
[/block]
# Troubleshooting

Getting the following error

`Note: Recompile with -Xlint:deprecation for details.
D8: Cannot fit requested classes in a single dex file (# methods: 79994 > 65536 ; # fields: 68161 > 65536)
com.android.builder.dexing.DexArchiveMergerException: Error while merging dex archives: 
`

**Explanation**: Android can build only files up to 65K, if it exceeds more than that then you will get this error.

**Solution**: Add the following code in your `android/app/build.gradle` file:
[block:code]
{
  "codes": [
    {
      "code": "defaultConfig {\n        multiDexEnabled true\n    }\n    ",
      "language": "java"
    }
  ]
}
[/block]