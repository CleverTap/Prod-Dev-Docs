---
title: Cordova Quick Start Guide
excerpt: Learn how to install and integrate your app with Cordova SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Steps to Install

Grab the Account ID and Token values from the _Settings_ page of your [CleverTap Dashboard](https://dashboard.clevertap.com).

## Cordova Integration

Follow the steps below to get started with SDK integration for your Cordova app:

### Step 1: Add the Cordova plugin using the following command in your command line interface:

```shell
cordova plugin add https://github.com/CleverTap/clevertap-cordova.git --variable CLEVERTAP_ACCOUNT_ID="YOUR CLEVERTAP ACCOUNT ID" --variable CLEVERTAP_TOKEN="YOUR CLEVERTAP ACCOUNT TOKEN"
```

### Step 2: Add the following XML tags to your `www/config.xml` file.

```xml
<preference name="android-build-tool" value="gradle" />

<gap:plugin name="clevertap-cordova" source="npm">
    <param name="CLEVERTAP_ACCOUNT_ID" value="YOUR CLEVERTAP ACCOUNT ID" />
    <param name="CLEVERTAP_TOKEN" value="YOUR CLEVERTAP ACCOUNT TOKEN" />
    <param name="GCM_PROJECT_NUMBER" value="YOUR GCM PROJECT NUMBER" /> // for v1.2.5 and lower of the plugin
</gap:plugin>
```

### Step 3: Add `CleverTap.notifyDeviceReady();` to your `onDeviceReady` callback in `www/js/index.js`

```javascript
onDeviceReady: function() {
    app.receivedEvent('deviceready');
    CleverTap.notifyDeviceReady();
    ...
},
```

## Set Up CleverTap in Ionic (Cordova Projects)

Use this method if your Ionic app is based on Cordova.

### Step 1:  Install the CleverTap Cordova Plugin

Install the CleverTap plugin and its Ionic Native wrapper:

```shell
# Install Ionic Native TypeScript wrapper for CleverTap
npm install @awesome-cordova-plugins/clevertap

# Install the CleverTap Cordova plugin with your account credentials
ionic cordova plugin add https://github.com/CleverTap/clevertap-cordova.git \
--variable CLEVERTAP_ACCOUNT_ID="YOUR_CLEVERTAP_ACCOUNT_ID" \
--variable CLEVERTAP_TOKEN="YOUR_CLEVERTAP_ACCOUNT_TOKEN"
```

### Step 2: Initialize CleverTap in Your App

Initialize CleverTap after the platform is ready to ensure plugin readiness.

```typescript
import { CleverTap } from '@awesome-cordova-plugins/clevertap/ngx';

export class AppComponent {
  constructor(
    private platform: Platform,
    private clevertap: CleverTap
  ) {
    this.platform.ready().then(() => {
      this.clevertap.notifyDeviceReady();
    });
  }
}
```

### Step 3: Sync and Launch Your App

Prepare and open your Cordova project for platform-specific testing.

```shell
ionic cordova prepare
ionic cordova open android
# or
ionic cordova open ios
```

## Set Up CleverTap in Ionic + Capacitor Projects

Use this method if your Ionic project uses _Capacitor_ instead of Cordova.

### Step 1: Install the CleverTap Plugin for Capacitor

Install the plugin and sync with Capacitor:

```shell
npm install @awesome-cordova-plugins/clevertap
npm install https://github.com/CleverTap/clevertap-cordova.git
npx cap sync
```

### Step 2: Initialize CleverTap in `app.component.ts`

Use `initializeApp()` to trigger CleverTap setup after the platform loads.

```typescript
import { CleverTap } from '@awesome-cordova-plugins/clevertap/ngx';

export class AppComponent {
  constructor(
    private platform: Platform,
    private clevertap: CleverTap
  ) {
    this.initializeApp();
  }

  initializeApp() {
    this.platform.ready().then(() => {
      this.clevertap.notifyDeviceReady();
    });
  }
}
```

### Step 3: Sync and Open the Native Project

Sync all Capacitor plugins and open your platform project in the native IDE.

```shell
ionic capacitor sync
ionic capacitor open android
# or
ionic capacitor open ios
```

## iOS

Check your _.plist_ file for the added Account ID and Account Token.

<Image align="center" alt={574} border={true} caption="Information Property List-Info.plist File" title="Info.plist" src="https://files.readme.io/7c74e73-19c6c39-image4.png" />

### Add Region Code

To know how to add region code for iOS in Cordova SDK, refer to [Region Codes.](doc:idc#ios)

## Android

Ensure that the following metadata tags are added inside the `<application> </application>` tags of your `AndroidManifest.xml`:

```xml
    <meta-data  
        android:name="CLEVERTAP_ACCOUNT_ID"  
        android:value="Your CleverTap Project ID"/>  
    <meta-data  
        android:name="CLEVERTAP_TOKEN"  
        android:value="Your CleverTap Project Token"/>
```

### Add Region Code

To know how to add region code for Android in Cordova SDK, refer to [Region Codes.](doc:idc#android)

### Set Lifecycle Callback

You can add CleverTap’s class as your application class in `AndroidManifest.xml` file to capture system events correctly.

```xml
<application
        android:name="com.clevertap.android.sdk.Application">
```

The above step is **extremely important** and enables CleverTap to track notification opens, display in-app notifications, track deep links, and other important user behavior.

### Add Permissions

Ensure to request the following permissions for your app:

```xml
<!-- Required to allow the app to send events -->
    <uses-permission android:name="android.permission.INTERNET"/>
    <!-- Recommended so that we can be smart about when to send the data -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.WAKE_LOCK" />
```

Refer to the example [AndroidManifest.xml](https://github.com/CleverTap/clevertap-cordova/blob/master/Samples/Cordova/ExampleProject/platforms/android/app/src/main/AndroidManifest.xml).

### Add Dependencies

Check that your `build.gradle` file includes the play-services and support library dependencies:

> 📘 Migrating from ExoPlayer to AndroidX Media3
>
> Starting from CleverTap Cordova SDK v3.3.0, CleverTap supports [AndroidX Media3](https://developer.android.com/media/media3/exoplayer/migration-guide), replacing the deprecated ExoPlayer libraries as of CleverTap Android SDK v8.1.0. Support for [ExoPlayer](https://github.com/google/ExoPlayer) will be removed after upgrading to v6.0.0. For a smoother migration to AndroidX Media3, update the following dependencies:

For a smoother migration to AndroidX Media3. Update the following dependencies:

```groovy Dependencies with Exoplayer Libraries
implementation fileTree(dir: 'libs', include: '*.jar')
debugCompile(project(path: "CordovaLib", configuration: "debug"))
releaseCompile(project(path: "CordovaLib", configuration: "release"))

// SUB-PROJECT DEPENDENCIES START
implementation "com.google.firebase:firebase-core:+"
implementation "com.google.firebase:firebase-messaging:23.0.6"
implementation 'androidx.core:core:1.3.0'
implementation 'androidx.fragment:fragment:1.3.6'
implementation "com.android.installreferrer:installreferrer:2.2" //Mandatory for v2.1.8 and above

// MANDATORY for App Inbox
implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'

// Optional ExoPlayer Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies
implementation 'com.google.android.exoplayer:exoplayer:2.19.1'
implementation 'com.google.android.exoplayer:exoplayer-hls:2.19.1'
implementation 'com.google.android.exoplayer:exoplayer-ui:2.19.1'

```
```coffeescript Dependencies with AndroidX Media3
implementation fileTree(dir: 'libs', include: '*.jar')
debugCompile(project(path: "CordovaLib", configuration: "debug"))
releaseCompile(project(path: "CordovaLib", configuration: "release"))

// SUB-PROJECT DEPENDENCIES START
implementation "com.google.firebase:firebase-core:+"
implementation "com.google.firebase:firebase-messaging:23.0.6"
implementation 'androidx.core:core:1.3.0'
implementation 'androidx.fragment:fragment:1.3.6'
implementation "com.android.installreferrer:installreferrer:2.2" //Mandatory for v2.1.8 and above

// MANDATORY for App Inbox
implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'

// Optional AndroidX Media3 Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies
implementation "androidx.media3:media3-exoplayer:1.1.1"
implementation "androidx.media3:media3-exoplayer-hls:1.1.1"
implementation "androidx.media3:media3-ui:1.1.1"
```

### Support AndroidX

To support AndroidX libraries, add the following to your `config.xml` file -

```xml
<preference name="AndroidXEnabled" value="true" />
```

## Integrate Javascript with CleverTap's Cordova Plugin

After integrating, all calls to the CleverTap SDK must be made from your Javascript.

Add the following listeners to your Javascript:

```javascript
document.addEventListener('deviceready', this.onDeviceReady, false);
document.addEventListener('onCleverTapProfileSync', this.onCleverTapProfileSync, false); // optional: to be notified of CleverTap user profile synchronization updates
document.addEventListener('onCleverTapProfileDidInitialize', this.onCleverTapProfileDidInitialize, false); // optional, to be notified when the CleverTap user profile is initialized
document.addEventListener('onCleverTapInAppNotificationDismissed', this.onCleverTapInAppNotificationDismissed, false); // optional, to be receive a callback with custom in-app notification click data
document.addEventListener('onDeepLink', this.onDeepLink, false); // optional, register to receive deep links.
document.addEventListener('onPushNotification', this.onPushNotification, false); // optional, register to receive push notification payloads.
document.addEventListener('onCleverTapInboxDidInitialize', this.onCleverTapInboxDidInitialize, false); // optional, to check if CleverTap Inbox intialized
document.addEventListener('onCleverTapInboxMessagesDidUpdate', this.onCleverTapInboxMessagesDidUpdate, false); // optional, to check if CleverTap Inbox Messages were updated
document.addEventListener('onCleverTapInboxButtonClick', this.onCleverTapInboxButtonClick, false); // optional, to check if Inbox button was clicked with custom payload
document.addEventListener('onCleverTapInAppButtonClick', this.onCleverTapInAppButtonClick, false); // optional, to check if InApp button was clicked with custom payload
document.addEventListener('onCleverTapFeatureFlagsDidUpdate', this.onCleverTapFeatureFlagsDidUpdate, false); // optional, to check if Feature Flags were updated
document.addEventListener('onCleverTapProductConfigDidInitialize', this.onCleverTapProductConfigDidInitialize, false); // optional, to check if Product Config was initialized
document.addEventListener('onCleverTapProductConfigDidFetch', this.onCleverTapProductConfigDidFetch, false); // optional, to check if Product Configs were updated
document.addEventListener('onCleverTapProductConfigDidActivate', this.onCleverTapProductConfigDidActivate, false); // optional, to check if Product Configs were activated
document.addEventListener('onCleverTapDisplayUnitsLoaded', this.onCleverTapDisplayUnitsLoaded, false); // optional, to check if Native Display units were loaded


// deep link handling  
onDeepLink: function(e) {
    console.log(e.deeplink);  
},

// push notification data handling
onPushNotification: function(e) {
    console.log(JSON.stringify(e.notification));
},

onCleverTapInboxDidInitialize: function() {
    CleverTap.showInbox({"navBarTitle":"My App Inbox","tabs": ["tag1", "tag2"],"navBarColor":"#FF0000"});
},
    
onCleverTapInboxMessagesDidUpdate: function() {
    CleverTap.getInboxMessageUnreadCount(function(val) {console.log("Inbox unread message count"+val);})
    CleverTap.getInboxMessageCount(function(val) {console.log("Inbox read message count"+val);});
},

onCleverTapInAppButtonClick: function(e) {
    console.log("onCleverTapInAppButtonClick");
    console.log(e.customExtras);
},

onCleverTapInboxButtonClick: function(e) {
    console.log("onCleverTapInboxButtonClick");
    console.log(e.customExtras);
},

onCleverTapFeatureFlagsDidUpdate: function() {
    console.log("onCleverTapFeatureFlagsDidUpdate");
},

onCleverTapProductConfigDidInitialize: function() {
    console.log("onCleverTapProductConfigDidInitialize");
},

onCleverTapProductConfigDidFetch: function() {
    console.log("onCleverTapProductConfigDidFetch");
},

onCleverTapProductConfigDidActivate: function() {
    console.log("onCleverTapProductConfigDidActivate");
},

onCleverTapDisplayUnitsLoaded: function(e) {
    console.log("onCleverTapDisplayUnitsLoaded");
    console.log(e.units);
},
```

## Track User Profiles

Create a user profile (`onUserLogin` method):

```javascript JavaScript
var stuff = ["bags", "shoes"];
var profile = {
    'Name': 'Captain America',
    'Identity': '100',
    'Email': 'captain@america.com',
    'Phone': '+14155551234',
    'stuff': stuff
}
CleverTap.onUserLogin(profile);
```

## Track User Events

Record an event with event data:

```javascript JavaScript
var eventData = {
    // Key:    Value
   'first': 'partridge',
   'second': 'turtledoves'
}
CleverTap.recordEventWithName('Cordova Event', eventData);
```

Record an event without event data:

```javascript JavaScript
CleverTap.recordEventWithName('Cordova Event');
```
