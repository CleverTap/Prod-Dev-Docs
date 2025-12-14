---
title: Custom Push Amplification Handling
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
1. In your custom FCM Receiver class, add the following code snippet when you receive the RemoteMessage object from Firebase. This method allows you to ensure that duplicate Push notification is not rendered on your device via Push Amplification.
[block:code]
{
  "codes": [
    {
      "code": "if (message.getData().size() > 0) {\n                Bundle extras = new Bundle();\n                Iterator var = message.getData().entrySet().iterator();\n\n                while(var.hasNext()) {\n                    Map.Entry entry = (Map.Entry)var.next();\n                    extras.putString((String)entry.getKey(), (String)entry.getValue());\n                }\n                    CleverTapAPI.processPushNotification(getApplicationContext(),extras);\n                }\n            }\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "if (message.getData().size() > 0)\n{\n  val extras = Bundle()\n  val `var` = message.getData().entrySet().iterator()\n  while (`var`.hasNext())\n  {\n    val entry = `var`.next() as Map.Entry\n    extras.putString(entry.getKey() as String, entry.getValue() as String)\n  }\n  CleverTapAPI.processPushNotification(getApplicationContext(), extras)\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
2. In your Application class, implement the CTPushAmpListener interface.
[block:code]
{
  "codes": [
    {
      "code": "public class MyApplication extends Application implements CTPushAmpListener {\n\n// your application class code\n}\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "class MyApplication:Application(), CTPushAmpListener// your application class code",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
3. In the onCreate() method of your Application class, create an instance of CleverTapAPI and set the `CTPushAmpListener` as following:
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());\ncleverTapAPI.setCTPushAmpListener(this);\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "val cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext())\ncleverTapAPI.setCTPushAmpListener(this)",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
4. After the listener is set, whenever a Push Notification is sent to the device via Push Amplification, the app will receive the payload in the onPushPayloadReceived method. Implement this method as follows and ensure that your rendering logic of the Push Notification is written here:
[block:code]
{
  "codes": [
    {
      "code": "@Override\npublic void onPushPayloadReceived(Bundle bundle) {\n    //write push notification rendering logic here\n}\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "fun onPushPayloadReceived(bundle:Bundle) {\n  //write push notification rendering logic here\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]