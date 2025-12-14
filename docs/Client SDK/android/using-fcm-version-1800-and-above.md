---
title: Use FCM Version 18.0.0 and Above
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
For Firebase Messaging version 18 and above, you can use your own service to check for tokens and send it to CleverTap. Use the following steps to create a service:

#Step 1: Create a MyFirebaseMessagingService Java file in your project
Following is an example:
 
[block:code]
{
  "codes": [
    {
      "code": "import android.os.Bundle;\nimport android.util.Log;\nimport com.clevertap.android.sdk.CleverTapAPI;\nimport com.clevertap.android.sdk.NotificationInfo;\nimport com.google.firebase.messaging.FirebaseMessagingService;\nimport com.google.firebase.messaging.RemoteMessage;\nimport java.util.Map;\npublic class MyFirebaseMessagingService extends FirebaseMessagingService {\n    @Override\n    public void onMessageReceived(RemoteMessage message){\n        try {\n            if (message.getData().size() > 0) {\n                Bundle extras = new Bundle();\n                for (Map.Entry<String, String> entry : message.getData().entrySet()) {\n                    extras.putString(entry.getKey(), entry.getValue());\n                }\n                Log.e(\"TAG\",\"onReceived Mesaage Called\");\n                NotificationInfo info = CleverTapAPI.getNotificationInfo(extras);\n                if (info.fromCleverTap) {\n                    CleverTapAPI.createNotification(getApplicationContext(), extras);\n                }\n            }\n        } catch (Throwable t) {\n            Log.d(\"MYFCMLIST\", \"Error parsing FCM message\", t);\n        }\n    }\n    @Override\n    public void onNewToken(String token) {\n        CleverTapAPI.getDefaultInstance(this).pushFcmRegistrationId(token,true);\n        // If you want to send messages to this application instance or\n        // manage this apps subscriptions on the server side, send the\n        // Instance ID token to your app server.\n        //sendRegistrationToServer(token);\n    }\n}\n\n\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "import android.os.Bundle\nimport android.util.Log\nimport com.clevertap.android.sdk.CleverTapAPI\nimport com.clevertap.android.sdk.NotificationInfo\nimport com.google.firebase.messaging.FirebaseMessagingService\nimport com.google.firebase.messaging.RemoteMessage\n\nclass MyFirebaseMessagingService:FirebaseMessagingService() {\n  fun onMessageReceived(message:RemoteMessage) {\n    try\n    {\n      if (message.getData().size() > 0)\n      {\n        val extras = Bundle()\n        for (entry in message.getData().entrySet())\n        {\n          extras.putString(entry.key, entry.value)\n        }\n        Log.e(\"TAG\", \"onReceived Mesaage Called\")\n        val info = CleverTapAPI.getNotificationInfo(extras)\n        if (info.fromCleverTap)\n        {\n          CleverTapAPI.createNotification(getApplicationContext(), extras)\n        }\n      }\n    }\n    catch (t:Throwable) {\n      Log.d(\"MYFCMLIST\", \"Error parsing FCM message\", t)\n    }\n  }\n  fun onNewToken(token:String) {\n    CleverTapAPI.getDefaultInstance(this).pushFcmRegistrationId(token, true)\n    // If you want to send messages to this application instance or\n    // manage this apps subscriptions on the server side, send the\n    // Instance ID token to your app server.\n    //sendRegistrationToServer(token);\n  }\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
#Step 2: Append your Android manifest file

Add the following code:
[block:code]
{
  "codes": [
    {
      "code": "<service android:name=\".MyFirebaseMessagingService\">\n    <intent-filter>\n        <action android:name=\"com.google.firebase.MESSAGING_EVENT\"/>\n    </intent-filter>\n</service>",
      "language": "xml"
    }
  ]
}
[/block]
#Step 3 : Remove CleverTap's FCMTokenListenerService from your AndroidManifest.xml file

Since you have upgraded to FCM v18.0.0 and above, the `FCMTokenListenerService` will now cause a crash when you run your app. So please, remove the following code from your `AndroidManifest.xml` file.
[block:code]
{
  "codes": [
    {
      "code": "<service\n    android:name=\"com.clevertap.android.sdk.FcmTokenListenerService\">\n    <intent-filter>\n        <action android:name=\"com.google.firebase.INSTANCE_ID_EVENT\"/>\n    </intent-filter>\n</service>",
      "language": "xml"
    }
  ]
}
[/block]