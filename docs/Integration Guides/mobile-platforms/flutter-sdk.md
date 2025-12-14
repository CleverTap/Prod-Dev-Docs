---
title: Flutter
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
# CleverTap Flutter SDK

The CleverTap Flutter SDK for Mobile Customer Engagement and Analytics solutions brings together real-time user insights, an advanced segmentation engine, and easy-to-use marketing tools in one mobile marketing platform — giving your team the power to create amazing experiences that deepen customer relationships. Our intelligent mobile marketing platform provides the insights you need to keep users engaged and drive long-term retention and growth.

For more information check out our [website](https://clevertap.com "CleverTap") and [documentation](https://developer.clevertap.com/docs/ "CleverTap Technical Documentation").

## Install and Integration

To add the CleverTap Flutter SDK to your project, edit your project's `pubspec.yaml` file:


[block:code]
{
  "codes": [
    {
      "code": "\n    dependencies:\n    clevertap_plugin: 1.1.4\n\n",
      "language": "yaml"
    }
  ]
}
[/block]
Run `flutter packages get` to install the SDK

Now, in your Dart code, you can use :

```dart
    import 'package:clevertap_plugin/clevertap_plugin.dart';
```


## Android

Add the following to your `dependencies` section in `project/build.gradle`

[block:code]
{
  "codes": [
    {
      "code": "\n    dependencies {\n            classpath 'com.android.tools.build:gradle:3.5.1'\n            classpath 'com.google.gms:google-services:4.3.2' //<--- Mandatory for using Firebase Messaging, skip if not using FCM\n        }\n\n",
      "language": "groovy"
    }
  ]
}
[/block]
Add the following to your `dependencies` section in `app/build.gradle`


[block:code]
{
  "codes": [
    {
      "code": "\n    implementation 'com.clevertap.android:clevertap-android-sdk:3.8.2'\n       implementation 'com.android.support:support-v4:28.0.0'\n        implementation 'com.google.firebase:firebase-messaging:17.3.4'//Mandatory for using FCM push notifications, skip if not using FCM\n        implementation 'com.android.support:appcompat-v7:28.0.0'//MANDATORY for App Inbox\n        implementation 'com.android.support:design:28.0.0'//MANDATORY for App Inbox\n        implementation 'com.github.bumptech.glide:glide:4.9.0'//MANDATORY for App Inbox\n    //For CleverTap Android SDK v3.6.4 and above add the following -\n        implementation 'com.android.installreferrer:installreferrer:1.0'\n    //Optional ExoPlayer Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies\n        implementation 'com.google.android.exoplayer:exoplayer:2.8.4'\n        implementation 'com.google.android.exoplayer:exoplayer-hls:2.8.4'\n        implementation 'com.google.android.exoplayer:exoplayer-ui:2.8.4'\n",
      "language": "groovy"
    }
  ]
}
[/block]
At the end of the `app/build.gradle` file add the following 

[block:code]
{
  "codes": [
    {
      "code": "\n    apply plugin: 'com.google.gms.google-services' //skip if not using FCM\n\n",
      "language": "groovy"
    }
  ]
}
[/block]
In your app's Android Application class add the following code.


[block:code]
{
  "codes": [
    {
      "code": "\n\n    public class MyApplication extends FlutterApplication {\n        @java.lang.Override\n        public void onCreate() {\n            ActivityLifecycleCallback.register(this); //<--- Add this before super.onCreate()\n            super.onCreate();\n        }\n    }\n\n\n  ",
      "language": "java"
    }
  ]
}
[/block]
If you do not have an Application class, add this to your `AndroidManifest.xml`

[block:code]
{
  "codes": [
    {
      "code": "\n    <application\n        android:label=\"@string/app_name\"\n        android:icon=\"@drawable/ic_launcher\"\n        android:name=\"com.clevertap.android.sdk.Application\"> \n      ",
      "language": "xml"
    }
  ]
}
[/block]
Add the following permissions which are needed by the CleverTap SDK


[block:code]
{
  "codes": [
    {
      "code": "\n    <!-- Required to allow the app to send events and user profile information -->\n    <uses-permission android:name=\"android.permission.INTERNET\"/>\n    <!-- Recommended so that CleverTap knows when to attempt a network call -->\n    <uses-permission android:name=\"android.permission.ACCESS_NETWORK_STATE\"/>\n",
      "language": "xml"
    }
  ]
}
[/block]
Add your CleverTap Account ID and Token to your `AndroidManifest.xml`, within the <application></application> tags.



[block:code]
{
  "codes": [
    {
      "code": "\n    <meta-data\n        android:name=\"CLEVERTAP_ACCOUNT_ID\"\n        android:value=\"Your CleverTap Account ID\"/>\n    <meta-data\n        android:name=\"CLEVERTAP_TOKEN\"\n        android:value=\"Your CleverTap Account Token\"/>\n    <!-- IMPORTANT: To force use Google AD ID to uniquely identify  users, use the following meta tag. GDPR mandates that if you are using this tag, there is prominent disclousure to your end customer in their application. Read more about GDPR here - https://clevertap.com/blog/in-preparation-of-gdpr-compliance/ -->\n    <meta-data\n        android:name=\"CLEVERTAP_USE_GOOGLE_AD_ID\"\n        android:value=\"1\"/> \n\n\n",
      "language": "xml"
    }
  ]
}
[/block]

## iOS

After install, you will need to integrate the CleverTap SDK into your apps.

1. Follow the integration instructions [starting with Step 2 here](https://developer.clevertap.com/docs/ios-quickstart-guide#section-step-2-add-clever-tap-credentials).
2. In your `AppDelegate didFinishLaunchingWithOptions:` notify the CleverTap Flutter SDK of application launch:

[block:code]
{
  "codes": [
    {
      "code": "\n[CleverTap autoIntegrate]; // integrate CleverTap SDK using the autoIntegrate option\n[[CleverTapPlugin sharedInstance] applicationDidLaunchWithOptions:launchOptions];\n\n  ",
      "language": "objectivec"
    },
    {
      "code": "CleverTap.autoIntegrate()\nCleverTapPlugin.sharedInstance()?.applicationDidLaunch(options: launchOptions)\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Note",
  "body": "Check that you add the CleverTap imports at the top of the file."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "\n#import \"CleverTap.h\"\n#import \"CleverTapPlugin.h\"\n\n",
      "language": "objectivec"
    },
    {
      "code": "import CleverTapSDK\nimport clevertap_plugin\n",
      "language": "swift"
    }
  ]
}
[/block]
## Example Dart Usage

Please check out the [example](https://github.com/CleverTap/clevertap-flutter/tree/master/example) directory for the sample app.

### For more: 

For more information you can checkout the [developer docs](https://developer.clevertap.com/docs/ "CleverTap Technical Documentation")