---
title: CleverTap Xiaomi Push Integration
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
Chinese OEMs command a huge market share in the smartphone market. To improve battery life, many of these Chinese OEMs terminate processes running in the background and limit an app’s ability to send push notifications. Due to this limitation, users are unable to receive important push notifications. 

CleverTap can send push notifications powered by Xiaomi Cloud Push, an Android push notification delivery service for Chinese devices.  For a greater chance of delivery success, we send a push notification through both Xiaomi Cloud Push and Firebase Cloud Messaging (FCM) push services. If a message is delivered through one of these push services, the notification from the other cloud service is suppressed. There is no priority defined and there are no changes to the current flow of FCM. This ensures that the user will only receive the push notification once. 


#Register as a Xiaomi Developer

The first step to access the Xiaomi cloud push is registered as a Xiaomi developer on the [Xiaomi Website](https://dev.mi.com/console/).

# Enter console and create an application
Once you login to the console, click on Create App and enter the required details.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/27d3874-Xiaomi_push_Developer_console.png",
        "Xiaomi_push_Developer_console.png",
        1999,
        1075,
        "#e9e9e8"
      ],
      "border": true
    }
  ]
}
[/block]
# Get the app's Package name/App ID / AppKey / App Secret
Once the App is created on your console, click on the App name to get your Package Name/App ID/ App Key/ App Secret. Among these, the AppID and AppKey are the client’s identity, used when the client SDK initializes; the AppSecret is authenticated for sending a message at the server-side.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ee0d481-Xiaomi_Push_API_Key.png",
        "Xiaomi_Push_API_Key.png",
        1999,
        813,
        "#fbf7f3"
      ],
      "border": true
    }
  ]
}
[/block]
Click on [Mi Push Console](http://admin.xmpush.global.xiaomi.com/) and click on Enable Push to enable push services for your app.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/14ff6c4-Xiaomi_Push_MI_Console.png",
        "Xiaomi_Push_MI_Console.png",
        1999,
        84,
        "#f8f6f6"
      ],
      "border": true
    }
  ]
}
[/block]
# Integrate Xiaomi Push SDK
Download the SDK from the [Xiaomi Website](http://mipush.global.xiaomi.com/download.html). The Android SDK is provided in a JAR library, so the third party app only needs to add a small number of codes to adapt to the Mi Push service. 

##Configure AndroidManifest.xml file

###Configure with CleverTap Xiaomi plugin

It is easier to configure the Xiaomi push with our CleverTap plugin.

####Step 1

Configure the AndroidManifest.xml file
[block:code]
{
  "codes": [
    {
      "code": "<meta-data\n    android:name=\"CLEVERTAP_XIAOMI_APP_KEY\"\n    android:value=\"@string/xiaomi_app_key\" />\n\n<meta-data\n    android:name=\"CLEVERTAP_XIAOMI_APP_ID\"\n    android:value=\"@string/xiaomi_app_id\" />",
      "language": "xml"
    }
  ]
}
[/block]
####Step 2
Declare the Xiaomi App Key and your Xiaomi App ID in the res/strings.xml file.
[block:code]
{
  "codes": [
    {
      "code": "<string name=\"xiaomi_app_key\">Your Xiaomi App Key</string>\n<string name=\"xiaomi_app_id\">Your Xiaomi App ID</string>  ",
      "language": "xml"
    }
  ]
}
[/block]
####Step 3
Add the following dependency in your app-level build.gradle file. 
[block:code]
{
  "codes": [
    {
      "code": "implementation 'com.clevertap.android:clevertap-xiaomi-sdk:1.0.0'\n",
      "language": "groovy"
    }
  ]
}
[/block]
###Configure Manually

Use the following steps if you do not wish to use the CleverTap plugin to configure the Xiaomi push.
[block:code]
{
  "codes": [
    {
      "code": "<uses-permission android:name=\"android.permission.INTERNET\" />\n<uses-permission android:name=\"android.permission.ACCESS_NETWORK_STATE\" />\n<uses-permission android:name=\"android.permission.ACCESS_WIFI_STATE\" />\n<uses-permission android:name=\"android.permission.READ_PHONE_STATE\" />\n<uses-permission android:name=\"android.permission.VIBRATE\"/>\n\n<!-- the following 2 yourpackage should be changed to your package name -->\n    <permission\n        android:name=\"yourpackage.permission.MIPUSH_RECEIVE\"\n        android:protectionLevel=\"signature\" />\n    <uses-permission android:name=\"yourpackage.permission.MIPUSH_RECEIVE\" />\n    <uses-permission android:name=\"android.permission.VIBRATE\" />\n<service\n  android:enabled=\"true\"\n  android:process=\":pushservice\"\n  android:name=\"com.xiaomi.push.service.XMPushService\"/>\n<service\n  android:name=\"com.xiaomi.push.service.XMJobService\"\n  android:enabled=\"true\"\n  android:exported=\"false\"\n  android:permission=\"android.permission.BIND_JOB_SERVICE\"\n  android:process=\":pushservice\" />\n<!--Note：This service must be added to the version 3.0.1 or later（including version 3.0.1）-->\n<service\n  android:enabled=\"true\"\n  android:exported=\"true\"\n  android:name=\"com.xiaomi.mipush.sdk.PushMessageHandler\" />\n<service android:enabled=\"true\"\n  android:name=\"com.xiaomi.mipush.sdk.MessageHandleService\" />\n<!--Note：this service must be added to version 2.2.5 or later （includes version 2.2.5）-->\n<receiver\n  android:exported=\"true\"\n  android:name=\"com.xiaomi.push.service.receivers.NetworkStatusReceiver\" >\n  <intent-filter>\n    <action android:name=\"android.net.conn.CONNECTIVITY_CHANGE\" />\n <category android:name=\"android.intent.category.DEFAULT\" />\n</intent-filter>\n</receiver>\n<receiver\n  android:exported=\"false\"\n  android:process=\":pushservice\"\n  android:name=\"com.xiaomi.push.service.receivers.PingReceiver\" >\n  <intent-filter>\n<action android:name=\"com.xiaomi.push.PING_TIMER\" />\n  </intent-filter>\n</receiver>\n\n\n",
      "language": "xml"
    }
  ]
}
[/block]

##Implement a BroadcastReceiver class
In order to receive messages, you must implement a BroadcastReceiver inherited from PushMessageReceiver and realize all required methods in it: `onReceivePassThroughMessage, onNotificationMessageClicked, onNotificationMessageArrived, onCommandResult`, and `onReceiveRegisterResult`, and then register your receiver in the `AndroidManifest.xml` file. Method `onReceivePassThroughMessage` is used to receive transparent messages sent by the server.

##Pass token to CleverTap
In your MainActivity’s `onCreate` method, pass the token to CleverTap using the following code:

[block:code]
{
  "codes": [
    {
      "code": "MiPushClient.registerPush(this, APP_ID, APP_KEY);\nString xiaomiToken = MiPushClient.getRegId(this);\nif(cleverTapAPI != null){\n    cleverTapAPI.pushXiaomiRegistrationId(xiaomiToken,true);\n}else{\n    Log.e(TAG,\"CleverTap is NULL\");\n}",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "MiPushClient.registerPush(this, APP_ID, APP_KEY)\nval xiaomiToken = MiPushClient.getRegId(this)\nif (cleverTapAPI != null)\n{\n  cleverTapAPI.pushXiaomiRegistrationId(xiaomiToken, true)\n}\nelse\n{\n  Log.e(TAG, \"CleverTap is NULL\")\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
##Receive Push Notifications from CleverTap
In the `onReceivePassThroughMessage` method, write the following code for CleverTap to render Push Notification and raise Notification Viewed event.


[block:code]
{
  "codes": [
    {
      "code": "@Override\n    public void onReceivePassThroughMessage(Context context, MiPushMessage message) {\n    try {\n            String ctData = message.getContent();\n            Bundle extras = Utils.stringToBundle(ctData);\n            CleverTapAPI.createNotification(context,extras);\n\n        } catch (JSONException e) {\n            e.printStackTrace();\n        }\n    }\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "fun onReceivePassThroughMessage(context:Context, message:MiPushMessage) {\n  try\n  {\n    val ctData = message.getContent()\n    val extras = Utils.stringToBundle(ctData)\n    CleverTapAPI.createNotification(context, extras)\n  }\n  catch (e:JSONException) {\n    e.printStackTrace()\n  }\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
# Update AppSecret / Package Name in Settings Dashboard
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6a83025-17.png",
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