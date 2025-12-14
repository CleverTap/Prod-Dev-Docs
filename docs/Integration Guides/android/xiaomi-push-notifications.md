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

# Register as a Xiaomi Developer

The first step to access the Xiaomi cloud push is registered as a Xiaomi developer on the [Xiaomi Website](https://dev.mi.com/console/).

# Enter console and create an application

Once you login to the console, click on Create App and enter the required details.

<Image title="Xiaomi_push_Developer_console.png" alt={1999} className="border" border={true} src="https://files.readme.io/27d3874-Xiaomi_push_Developer_console.png" />

# Get the app's Package name/App ID / AppKey / App Secret

Once the App is created on your console, click on the App name to get your Package Name/App ID/ App Key/ App Secret. Among these, the AppID and AppKey are the client’s identity, used when the client SDK initializes; the AppSecret is authenticated for sending a message at the server-side.

<Image title="Xiaomi_Push_API_Key.png" alt={1999} className="border" border={true} src="https://files.readme.io/ee0d481-Xiaomi_Push_API_Key.png" />

Click on [Mi Push Console](http://admin.xmpush.global.xiaomi.com/) and click on Enable Push to enable push services for your app.

<Image title="Xiaomi_Push_MI_Console.png" alt={1999} className="border" border={true} src="https://files.readme.io/14ff6c4-Xiaomi_Push_MI_Console.png" />

# Integrate Xiaomi Push SDK

Download the SDK from the [Xiaomi Website](http://mipush.global.xiaomi.com/download.html). The Android SDK is provided in a JAR library, so the third party app only needs to add a small number of codes to adapt to the Mi Push service. 

## Configure AndroidManifest.xml file

### Configure with CleverTap Xiaomi plugin

It is easier to configure the Xiaomi push with our CleverTap plugin.

#### Step 1

Configure the AndroidManifest.xml file

```xml
<meta-data
    android:name="CLEVERTAP_XIAOMI_APP_KEY"
    android:value="@string/xiaomi_app_key" />

<meta-data
    android:name="CLEVERTAP_XIAOMI_APP_ID"
    android:value="@string/xiaomi_app_id" />
```

#### Step 2

Declare the Xiaomi App Key and your Xiaomi App ID in the res/strings.xml file.

```xml
<string name="xiaomi_app_key">Your Xiaomi App Key</string>
<string name="xiaomi_app_id">Your Xiaomi App ID</string>
```

#### Step 3

Add the following dependency in your app-level build.gradle file. 

```groovy
implementation 'com.clevertap.android:clevertap-xiaomi-sdk:1.0.0'
```

### Configure Manually

Use the following steps if you do not wish to use the CleverTap plugin to configure the Xiaomi push.

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.VIBRATE"/>

<!-- the following 2 yourpackage should be changed to your package name -->
    <permission
        android:name="yourpackage.permission.MIPUSH_RECEIVE"
        android:protectionLevel="signature" />
    <uses-permission android:name="yourpackage.permission.MIPUSH_RECEIVE" />
    <uses-permission android:name="android.permission.VIBRATE" />
<service
  android:enabled="true"
  android:process=":pushservice"
  android:name="com.xiaomi.push.service.XMPushService"/>
<service
  android:name="com.xiaomi.push.service.XMJobService"
  android:enabled="true"
  android:exported="false"
  android:permission="android.permission.BIND_JOB_SERVICE"
  android:process=":pushservice" />
<!--Note：This service must be added to the version 3.0.1 or later（including version 3.0.1）-->
<service
  android:enabled="true"
  android:exported="true"
  android:name="com.xiaomi.mipush.sdk.PushMessageHandler" />
<service android:enabled="true"
  android:name="com.xiaomi.mipush.sdk.MessageHandleService" />
<!--Note：this service must be added to version 2.2.5 or later （includes version 2.2.5）-->
<receiver
  android:exported="true"
  android:name="com.xiaomi.push.service.receivers.NetworkStatusReceiver" >
  <intent-filter>
    <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
 <category android:name="android.intent.category.DEFAULT" />
</intent-filter>
</receiver>
<receiver
  android:exported="false"
  android:process=":pushservice"
  android:name="com.xiaomi.push.service.receivers.PingReceiver" >
  <intent-filter>
<action android:name="com.xiaomi.push.PING_TIMER" />
  </intent-filter>
</receiver>
```

## Implement a BroadcastReceiver class

In order to receive messages, you must implement a BroadcastReceiver inherited from PushMessageReceiver and realize all required methods in it: `onReceivePassThroughMessage, onNotificationMessageClicked, onNotificationMessageArrived, onCommandResult`, and `onReceiveRegisterResult`, and then register your receiver in the `AndroidManifest.xml` file. Method `onReceivePassThroughMessage` is used to receive transparent messages sent by the server.

## Pass token to CleverTap

In your MainActivity’s `onCreate` method, pass the token to CleverTap using the following code:

```java JAVA
MiPushClient.registerPush(this, APP_ID, APP_KEY);
String xiaomiToken = MiPushClient.getRegId(this);
if(cleverTapAPI != null){
    cleverTapAPI.pushXiaomiRegistrationId(xiaomiToken,true);
}else{
    Log.e(TAG,"CleverTap is NULL");
}
```
```kotlin Kotlin
MiPushClient.registerPush(this, APP_ID, APP_KEY)
val xiaomiToken = MiPushClient.getRegId(this)
if (cleverTapAPI != null)
{
  cleverTapAPI.pushXiaomiRegistrationId(xiaomiToken, true)
}
else
{
  Log.e(TAG, "CleverTap is NULL")
}
```

## Receive Push Notifications from CleverTap

In the `onReceivePassThroughMessage` method, write the following code for CleverTap to render Push Notification and raise Notification Viewed event.

```java JAVA
@Override
    public void onReceivePassThroughMessage(Context context, MiPushMessage message) {
    try {
            String ctData = message.getContent();
            Bundle extras = Utils.stringToBundle(ctData);
            CleverTapAPI.createNotification(context,extras);

        } catch (JSONException e) {
            e.printStackTrace();
        }
    }
```
```kotlin Kotlin
fun onReceivePassThroughMessage(context:Context, message:MiPushMessage) {
  try
  {
    val ctData = message.getContent()
    val extras = Utils.stringToBundle(ctData)
    CleverTapAPI.createNotification(context, extras)
  }
  catch (e:JSONException) {
    e.printStackTrace()
  }
}
```

# Update AppSecret / Package Name in Settings Dashboard

<Image title="17.png" alt={1462} className="border" border={true} src="https://files.readme.io/6a83025-17.png" />
