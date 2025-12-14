---
title: CleverTap Huawei Push Integration_AndroidX
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
Huawei uses their own push service to deliver notifications to their devices.

CleverTap can send push notifications powered by Huawei Cloud Push, an Android push notification delivery service.  For a greater chance of delivery success, we send a push notification through both Huawei Cloud Push and Firebase Cloud Messaging push services. If a message is delivered through one of these push services, the notification from the other cloud service is suppressed. This ensures that the user will only receive the push notification once. 


#Register as a Huawei Developer

The first step to access the Huawei cloud push is registered as a Huawei developer on the [Huawei Website](https://id5.cloud.huawei.com/CAS/portal/loginAuth.html).

# Enable the Huawei Push Kit
Once you login to the console, enable the Push Kit

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b51d8cc-Screenshot_2020-04-22_at_12.03.30_PM.png",
        "Screenshot 2020-04-22 at 12.03.30 PM.png",
        2712,
        1478,
        "#f6f8f9"
      ],
      "border": true
    }
  ]
}
[/block]
# Enable the Push Service
In the Huawei console, enable the push kit. Enable other settings as mentioned by Huawei [here](https://developer.huawei.com/consumer/en/codelab/HMSPushKit/index.html#3)

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/08d71a6-Huawei_push_kit.png",
        "Huawei push kit.png",
        1614,
        579,
        "#fafafb"
      ]
    }
  ]
}
[/block]
# Get the app's App ID/ App Secret 
Once the App is created on your console, click on the App name to get your App ID/ App Secret. Among these, the AppID and AppKey are the client’s identity, used when the client SDK initializes; the AppSecret is authenticated for sending a message at the server-side.
[block:image]
{
  "images": [
    {
      "image": [],
      "border": true
    }
  ]
}
[/block]
# Integrate Huawei HMS SDK
Download the `agconnect-services.json` file from the [Huawei Console](https://developer.huawei.com/consumer/en/codelab/HMSPushKit/index.html#4). Move the downloaded `agconnect-services.json` file to the app directory of your Android Studio project.

##Configure Manifest and Gradle files with CleverTap plugin
It is easier to configure Huawei push with CleverTap plugin. 

###Step 1

Configure the root level build.gradle file.
[block:code]
{
  "codes": [
    {
      "code": "buildscript {\n    repositories {\n        // HUAWEI ADD THIS\n        maven {url 'http://developer.huawei.com/repo/'}\n        \n    }\n    dependencies {\n        classpath \"com.android.tools.build:gradle:4.0.1\"\n        \n        // HUAWEI ADD THIS\n        classpath 'com.huawei.agconnect:agcp:1.3.1.300'\n    }\n}\n\nallprojects {\n    repositories {\n        // HUAWEI ADD THIS\n        maven {url 'http://developer.huawei.com/repo/'}\n        \n    }\n}",
      "language": "groovy"
    }
  ]
}
[/block]
###Step 2

Configure the build.gradle at the app-level.
[block:code]
{
  "codes": [
    {
      "code": "implementation 'com.huawei.hms:push:5.0.0.300'//Add the dependency\napply plugin: 'com.huawei.agconnect'//Add This at the bottom of the file",
      "language": "groovy"
    }
  ]
}
[/block]
##Configure Manifest and Gradle files manually
Set up your app per the configuration provided by [Huawei](https://developer.huawei.com/consumer/en/codelab/HMSPushKit/index.html#4)

##Implement a Service class that extends `HmsMessageService` class
In order to receive messages, you must implement a Service inherited from HmsMessageService and override all required methods in it: `onNewToken`, `onMessageReceived`, `onMessageSent` and `onSendError`  and then register your Service in the `AndroidManifest.xml` file. Method `onMessageReceived` is used to receive messages sent by CleverTap.

##Pass token to CleverTap
In your MainActivity’s `onCreate` method, pass the token to CleverTap using the following code:
[block:code]
{
  "codes": [
    {
      "code": " String appId = AGConnectServicesConfig.fromContext(MainActivity.this).getString(\"client/app_id\");\nString token = HmsInstanceId.getInstance(MainActivity.this).getToken(appId, \"HCM\");\nif(cleverTapAPI != null){\n\t\tcleverTapAPI.pushHuaweiRegistrationId(token,true);\n}else{\n\t\tLog.e(TAG,\"CleverTap is NULL\");\n}",
      "language": "java"
    }
  ]
}
[/block]
##Receive Push Notifications from CleverTap
In the `onMessageReceived` method, write the following code for CleverTap to render Push Notification and raise Notification Viewed event.


[block:code]
{
  "codes": [
    {
      "code": "@Override\npublic void onMessageReceived(RemoteMessage message) {\n try {\n \t\t\tString ctData = message.getData();\n   \t\tBundle extras = Utils.stringToBundle(ctData);\n   \t\tCleverTapAPI.createNotification(getApplicationContext(),extras);\n\n\t} catch (JSONException e) {\n   \t\te.printStackTrace();\n\t}\n}\n",
      "language": "java"
    }
  ]
}
[/block]
# Update App ID/App Secret in Settings Dashboard
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/57e4389-17.png",
        "17.png",
        1462,
        670,
        "#f9fafc"
      ],
      "border": true
    }
  ]
}
[/block]