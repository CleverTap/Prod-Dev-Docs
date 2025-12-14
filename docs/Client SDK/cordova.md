---
title: Cordova SDK
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
CleverTap provides a [Cordova Plugin](https://github.com/CleverTap/clevertap-cordova) that enables app developers to track, segment, and engage their users. 


# Supported Versions

Tested on Cordova 10.0.0

- [CleverTap Android SDK version 4.1.1](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/core-v4.1.1)
- [CleverTap iOS SDK version 3.9.4](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/3.9.4)

# Installation Steps

To integrate CleverTap for Cordova:
  * Set up and register for Push notifications, if required.
  * Integrate your Javascript with the CleverTap Plugin.

# Step 1: Install the Plugin

Grab the Account ID and Token values from your CleverTap [Dashboard](https://dashboard.clevertap.com) -> Settings.

### For Android *Important*
Starting with v2.0.0, the plugin uses FCM rather than GCM.  To configure FCM, add your google-services.json to the root of your Cordova project **before you add the plugin**.  

The plugin uses an `after plugin add` hook script to configure your project for FCM.  
If the google-services.json file is not present in your project when the script runs, FCM will not be configured properly and will not work. 

### Using Cordova  

[block:code]
{
  "codes": [
    {
      "code": "cordova plugin add https://github.com/CleverTap/clevertap-cordova.git --variable CLEVERTAP_ACCOUNT_ID=\"YOUR CLEVERTAP ACCOUNT ID\" --variable CLEVERTAP_TOKEN=\"YOUR CLEVERTAP ACCOUNT TOKEN\"\n\n",
      "language": "text"
    }
  ]
}
[/block]
### Using Ionic  


[block:code]
{
  "codes": [
    {
      "code": "ionic plugin add https://github.com/CleverTap/clevertap-cordova.git --variable CLEVERTAP_ACCOUNT_ID=\"YOUR CLEVERTAP ACCOUNT ID\" --variable CLEVERTAP_TOKEN=\"YOUR CELVERTAP ACCOUNT TOKEN\" \n\n",
      "language": "text"
    }
  ]
}
[/block]
### Using PhoneGap Build

**Starting with v2.0.0, the plugin drops official support for PhoneGap Build.**

This is because PhoneGap Build does not support install hooks and a hook is required to configure FCM.

It might be possible by forking this plugin and replacing the placeholder google-services.json in src/android with yours, and then hard coding your google app id and API key in plugin.xml, but you're on your own there.

When using the plugin with PhoneGap Build: 

Add the following to your `www/config.xml` file:
         
[block:code]
{
  "codes": [
    {
      "code": "<preference name=\"android-build-tool\" value=\"gradle\" />\n\n<gap:plugin name=\"clevertap-cordova\" source=\"npm\">\n    <param name=\"CLEVERTAP_ACCOUNT_ID\" value=\"YOUR CLEVERTAP ACCOUNT ID\" />\n    <param name=\"CLEVERTAP_TOKEN\" value=\"YOUR CLEVERTAP ACCOUNT TOKEN\" />\n    <param name=\"GCM_PROJECT_NUMBER\" value=\"YOUR GCM PROJECT NUMBER\" /> // for v1.2.5 and lower of the plugin\n</gap:plugin>\n  \n",
      "language": "xml"
    }
  ]
}
[/block]
**Extremely Important** *For PhoneGap Build Android projects*, add `CleverTap.notifyDeviceReady();` to your onDeviceReady callback in `www/js/index.js`:
[block:code]
{
  "codes": [
    {
      "code": "onDeviceReady: function() {\n    app.receivedEvent('deviceready');\n    CleverTap.notifyDeviceReady();\n    ...\n},\n",
      "language": "javascript"
    }
  ]
}
[/block]
### iOS

Check your .plist file:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7c74e73-19c6c39-image4.png",
        "19c6c39-image4.png",
        574,
        358,
        "#e5e8ec"
      ],
      "border": true
    }
  ]
}
[/block]
### Android

Check that the following is inside the `<application></application>` tags of your AndroidManifest.xml:  
[block:code]
{
  "codes": [
    {
      "code": "    <meta-data  \n        android:name=\"CLEVERTAP_ACCOUNT_ID\"  \n        android:value=\"Your CleverTap Account ID\"/>  \n    <meta-data  \n        android:name=\"CLEVERTAP_TOKEN\"  \n        android:value=\"Your CleverTap Account Token\"/>\n",
      "language": "xml"
    }
  ]
}
[/block]
Replace "Your CleverTap Account ID" and "Your CleverTap Account Token" with actual values from your CleverTap [Dashboard](https://dashboard.clevertap.com) -> Settings.

**Set the Lifecycle Callback** 

IMPORTANT!

Check the `android:name` property of the `<application>` tag of our AndroidManifest.xml:
[block:code]
{
  "codes": [
    {
      "code": "    <application\n        android:name=\"com.clevertap.android.sdk.Application\">\n\n",
      "language": "xml"
    }
  ]
}
[/block]
**Note:** The above step is **extremely important** and enables CleverTap to track notification opens, display in-app notifications, track deep links, and other important **user behaviour**.

**Add Permissions**

Please ensure that you're requesting the following permissions for your app:

[block:code]
{
  "codes": [
    {
      "code": "    <!-- Required to allow the app to send events -->\n    <uses-permission android:name=\"android.permission.INTERNET\"/>\n    <!-- Recommended so that we can be smart about when to send the data -->\n    <uses-permission android:name=\"android.permission.ACCESS_NETWORK_STATE\"/>\n    <uses-permission android:name=\"android.permission.WAKE_LOCK\" />\n\n",
      "language": "xml"
    }
  ]
}
[/block]
[Please see the example AndroidManifest.xml here](https://github.com/CleverTap/clevertap-cordova/blob/master/Samples/Cordova/ExampleProject/platforms/android/app/src/main/AndroidManifest.xml).

**Add Dependencies**

Make sure your build.gradle file includes the play-services and support library dependencies:  
[block:code]
{
  "codes": [
    {
      "code": "    dependencies {\n        implementation fileTree(dir: 'libs', include: '*.jar'  )\n        debugimplementation(project(path: \"CordovaLib\", configuration: \"debug\"))\n        releaseimplementation(project(path: \"CordovaLib\", configuration: \"release\"))\n        // SUB-PROJECT DEPENDENCIES START\n        implementation \"com.google.firebase:firebase-core:+\"\n        implementation \"com.google.firebase:firebase-messaging:17.3.3\"\n        implementation \"com.android.support:support-v4:+\"\n         //Mandatory for v2.1.8 and above\n        implementation \"com.android.installreferrer:installreferrer:1.0\"\n        // SUB-PROJECT DEPENDENCIES END ",
      "language": "java"
    }
  ]
}
[/block]
# 2. Set Up and Register for Push Notifications and Deep Links

### iOS

[Set up push notifications for your app](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6).

If you plan on using deep links, [please register your custom URL scheme as described here](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html#//apple_ref/doc/uid/TP40007072-CH6-SW1).

Call the following from your Javascript.
[block:code]
{
  "codes": [
    {
      "code": "   CleverTap.registerPush();\n",
      "language": "javascript"
    }
  ]
}
[/block]
### Android

Add your custom URL scheme to the AndroidManifest.xml.

[block:code]
{
  "codes": [
    {
      "code": "\t <intent-filter android:label=\"@string/app_name\">\n                <action android:name=\"android.intent.action.VIEW\" />\n                <category android:name=\"android.intent.category.DEFAULT\" />\n                <category android:name=\"android.intent.category.BROWSABLE\" />\n                <data android:scheme=\"clevertapstarter\" />\n     </intent-filter>\n",
      "language": "xml"
    }
  ]
}
[/block]
See [example AndroidManifest.xml](https://github.com/CleverTap/clevertap-cordova/blob/master/Samples/Cordova/ExampleProject/platforms/android/app/src/main/AndroidManifest.xml).

# 3. Integrate Javascript with the Plugin

After integrating, all calls to the CleverTap SDK should be made from your Javascript.

Start by adding the following listeners to your Javascript:

[block:code]
{
  "codes": [
    {
      "code": "    document.addEventListener('deviceready', this.onDeviceReady, false);\n    document.addEventListener('onCleverTapProfileSync', this.onCleverTapProfileSync, false); // optional: to be notified of CleverTap user profile synchronization updates \n    document.addEventListener('onCleverTapProfileDidInitialize', this.onCleverTapProfileDidInitialize, false); // optional, to be notified when the CleverTap user profile is initialized\n    document.addEventListener('onCleverTapInAppNotificationDismissed', this.onCleverTapInAppNotificationDismissed, false); // optional, to be receive a callback with custom in-app notification click data\n    document.addEventListener('onDeepLink', this.onDeepLink, false); // optional, register to receive deep links.\n    document.addEventListener('onPushNotification', this.onPushNotification, false); // optional, register to receive push notification payloads.\n\n\n    // deep link handling  \n    onDeepLink: function(e) {\n        console.log(e.deeplink);  \n    },  \n\n    // push notification data handling\n    onPushNotification: function(e) {\n        console.log(JSON.stringify(e.notification));\n    }, \n      \n      ",
      "language": "javascript"
    }
  ]
}
[/block]
# Step 4: Example Projects

To see how to use the CleverTap SDK in your app, please see these example projects:
 [Starter Cordova project](https://github.com/CleverTap/clevertap-cordova/blob/master/Samples/Cordova/ExampleProject/www/js/index.js)
[Starter Ionic project](https://github.com/CleverTap/clevertap-cordova/blob/master/Samples/IonicCordova/IonicAngularProject/src/app/app.component.ts)