---
title: Cordova
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

Tested on Cordova 9.0.0

* [CleverTap Android SDK version 3.6.4](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/3.6.4)
* [CleverTap iOS SDK version 3.7.2](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/3.7.2)

# Installation Steps

To integrate CleverTap for Cordova:

* Set up and register for Push notifications, if required.
* Integrate your Javascript with the CleverTap Plugin.

# Step 1: Install the Plugin

Grab the Account ID and Token values from your CleverTap [Dashboard](https://dashboard.clevertap.com) -> Settings.

### For Android *Important*

Starting with v2.0.0, the plugin uses FCM rather than GCM.  To configure FCM, add your google-services.json to the root of your Cordova project **before you add the plugin**.  

The plugin uses an `after plugin add` hook script to configure your project for FCM.\
If the google-services.json file is not present in your project when the script runs, FCM will not be configured properly and will not work. 

### Using Cordova

```text
cordova plugin add https://github.com/CleverTap/clevertap-cordova.git --variable CLEVERTAP_ACCOUNT_ID="YOUR CLEVERTAP ACCOUNT ID" --variable CLEVERTAP_TOKEN="YOUR CLEVERTAP ACCOUNT TOKEN"
```

### Using Ionic

For iOS:

```text
ionic plugin add https://github.com/CleverTap/clevertap-cordova.git --variable CLEVERTAP_ACCOUNT_ID="YOUR CLEVERTAP ACCOUNT ID" --variable CLEVERTAP_TOKEN="YOUR CELVERTAP ACCOUNT TOKEN"
```

### Using PhoneGap Build

**Starting with v2.0.0, the plugin drops official support for PhoneGap Build.**

This is because PhoneGap Build does not support install hooks and a hook is required to configure FCM.

It might be possible by forking this plugin and replacing the placeholder google-services.json in src/android with yours, and then hard coding your google app id and API key in plugin.xml, but you're on your own there.

When using the plugin with PhoneGap Build: 

Add the following to your `www/config.xml` file:

```xml
<preference name="android-build-tool" value="gradle" />

<gap:plugin name="clevertap-cordova" source="npm">
    <param name="CLEVERTAP_ACCOUNT_ID" value="YOUR CLEVERTAP ACCOUNT ID" />
    <param name="CLEVERTAP_TOKEN" value="YOUR CLEVERTAP ACCOUNT TOKEN" />
    <param name="GCM_PROJECT_NUMBER" value="YOUR GCM PROJECT NUMBER" /> // for v1.2.5 and lower of the plugin
</gap:plugin>
```

**Extremely Important** *For PhoneGap Build Android projects*, add `CleverTap.notifyDeviceReady();` to your onDeviceReady callback in `www/js/index.js`:

```javascript
onDeviceReady: function() {
    app.receivedEvent('deviceready');
    CleverTap.notifyDeviceReady();
    ...
},
```

### iOS

Check your .plist file:

<Image title="19c6c39-image4.png" alt={574} className="border" border={true} src="https://files.readme.io/7c74e73-19c6c39-image4.png" />

### Android

Check that the following is inside the `<application></application>` tags of your AndroidManifest.xml:  

```xml
<meta-data  
        android:name="CLEVERTAP_ACCOUNT_ID"  
        android:value="Your CleverTap Account ID"/>  
    <meta-data  
        android:name="CLEVERTAP_TOKEN"  
        android:value="Your CleverTap Account Token"/>
```

Replace "Your CleverTap Account ID" and "Your CleverTap Account Token" with actual values from your CleverTap [Dashboard](https://dashboard.clevertap.com) -> Settings.

**Set the Lifecycle Callback** 

IMPORTANT!

Check the `android:name` property of the `<application>` tag of our AndroidManifest.xml:

```xml
<application
        android:name="com.clevertap.android.sdk.Application">
```

**Note:** The above step is **extremely important** and enables CleverTap to track notification opens, display in-app notifications, track deep links, and other important **user behaviour**.

**Add Permissions**

Please ensure that you're requesting the following permissions for your app:

```xml
<!-- Required to allow the app to send events -->
    <uses-permission android:name="android.permission.INTERNET"/>
    <!-- Recommended so that we can be smart about when to send the data -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.WAKE_LOCK" />
```

[Please see the example AndroidManifest.xml here](https://github.com/CleverTap/clevertap-cordova/blob/master/Starter/platforms/android/AndroidManifest.xml).

**Add Dependencies**

Make sure your build.gradle file includes the play-services and support library dependencies:  

```java
dependencies {
        implementation fileTree(dir: 'libs', include: '*.jar'  )
        debugimplementation(project(path: "CordovaLib", configuration: "debug"))
        releaseimplementation(project(path: "CordovaLib", configuration: "release"))
        // SUB-PROJECT DEPENDENCIES START
        implementation "com.google.firebase:firebase-core:+"
        implementation "com.google.firebase:firebase-messaging:17.3.3"
        implementation "com.android.support:support-v4:+"
         //Mandatory for v2.1.8 and above
        implementation "com.android.installreferrer:installreferrer:1.0"
        // SUB-PROJECT DEPENDENCIES END
```

# 2. Set Up and Register for Push Notifications and Deep Links

### iOS

[Set up push notifications for your app](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6).

If you plan on using deep links, [please register your custom URL scheme as described here](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html#//apple_ref/doc/uid/TP40007072-CH6-SW1).

Call the following from your Javascript.

```javascript
CleverTap.registerPush();
```

### Android

Add your custom URL scheme to the AndroidManifest.xml.

```xml
<intent-filter android:label="@string/app_name">
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="clevertapstarter" />
     </intent-filter>
```

See [example AndroidManifest.xml](ihttps://github.com/CleverTap/clevertap-cordova/blob/master/Starter/platforms/android/AndroidManifest.xml).

# 3. Integrate Javascript with the Plugin

After integrating, all calls to the CleverTap SDK should be made from your Javascript.

Start by adding the following listeners to your Javascript:

```javascript
document.addEventListener('deviceready', this.onDeviceReady, false);
    document.addEventListener('onCleverTapProfileSync', this.onCleverTapProfileSync, false); // optional: to be notified of CleverTap user profile synchronization updates 
    document.addEventListener('onCleverTapProfileDidInitialize', this.onCleverTapProfileDidInitialize, false); // optional, to be notified when the CleverTap user profile is initialized
    document.addEventListener('onCleverTapInAppNotificationDismissed', this.onCleverTapInAppNotificationDismissed, false); // optional, to be receive a callback with custom in-app notification click data
    document.addEventListener('onDeepLink', this.onDeepLink, false); // optional, register to receive deep links.
    document.addEventListener('onPushNotification', this.onPushNotification, false); // optional, register to receive push notification payloads.


    // deep link handling  
    onDeepLink: function(e) {
        console.log(e.deeplink);  
    },  

    // push notification data handling
    onPushNotification: function(e) {
        console.log(JSON.stringify(e.notification));
    },
```

# Step 4: Example Projects

To see how to use the CleverTap SDK in your app, please see these example projects:

* [ iOS Starter Cordova project](https://github.com/CleverTap/clevertap-cordova/blob/master/ExampleProject/platforms/ios/www/js/index.js).   

* [Android Starter Cordova project](https://github.com/CleverTap/clevertap-cordova/tree/master/ExampleProject) .
