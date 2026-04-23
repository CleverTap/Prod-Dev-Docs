---
title: Flutter Quick Start Guide
excerpt: Learn how to quickly get started with your Flutter Integration.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: link
      title: Flutter Push
      url: https://developer.clevertap.com/docs/flutter-push-notification
    - type: link
      title: Flutter In-App
      url: https://developer.clevertap.com/docs/flutter-in-app
    - type: link
      title: Flutter App Inbox
      url: https://developer.clevertap.com/docs/flutter-app-inbox
    - type: link
      title: Flutter Advanced Features
      url: https://developer.clevertap.com/docs/flutter-advanced-features
    - type: link
      title: Flutter Resources
      url: https://developer.clevertap.com/docs/flutter-resources
---
# Overview

The following section shows how to install, set up, and test your Flutter application.

# Install

To add the CleverTap Flutter SDK to your project, edit your project's `pubspec.yaml` file:

```yaml
dependencies:
    clevertap_plugin: 3.6.0
```

Run `flutter packages get` to install the SDK.

Now, in your Dart code, you can use:

```javascript Dart
import 'package:clevertap_plugin/clevertap_plugin.dart';
```

# Android Integration

## Add CleverTap Credentials

To associate your Android app with your CleverTap account, you must add your CleverTap credentials to the application's `AndroidManifest.xml` file. Follow the steps to add CleverTap credentials:

Add your CleverTap Account ID and Token to your AndroidManifest.xml within the <application /> tags.

```xml
<meta-data
    android:name="CLEVERTAP_ACCOUNT_ID"
    android:value="Your CleverTap Account ID"/>
<meta-data
    android:name="CLEVERTAP_TOKEN"
    android:value="Your CleverTap Account Token"/>
<!-- IMPORTANT: To force use Google AD ID to uniquely identify  users, use the following meta tag. GDPR mandates that if you are using this tag, there is prominent disclousure to your end customer in their application. Read more about GDPR here - https://clevertap.com/blog/in-preparation-of-gdpr-compliance/ -->
<meta-data
    android:name="CLEVERTAP_USE_GOOGLE_AD_ID"
    android:value="1"/>
```

> 🚧 Credentials and Access
>
> The **CleverTap Account ID** and **CleverTap Token** are available as **Project ID** and **Project Token** respectively on the CleverTap dashboard. Member and Creator roles in the project don't have access to view the Passcode and Project Token of the account on the dashboard.

### Add Region Code

To know how to add region code for Android in Flutter SDK, refer to [Region Codes.](doc:idc#android)

## Enable Tracking by Adding Permissions

Add the snippet below in the `AndroidManifest.xml` file so the CleverTap SDK can access the internet.

```xml
<!-- Required to allow the app to send events and user profile information -->
<uses-permission android:name="android.permission.INTERNET"/>
<!-- Recommended so that CleverTap knows when to attempt a network call -->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

## Add Dependencies

> 📘 Plugin Domain-Specific Language (DSL)
>
> To build a Flutter app for Android, you must apply Flutter's Gradle plugins. Previously, you would be required to use Gradle's legacy imperative `apply` script method to build a Flutter app for Android.  
> However, since the release of Flutter 3.16, you can apply these plugins using Gradle's declarative `plugins {} `block which simplifies and standardizes the configuration process, reducing the chances of errors.
>
> For more information, refer to the [official Flutter documentation](https://docs.flutter.dev/release/breaking-changes/flutter-gradle-plugin-apply) and check out the [CleverTap-Flutter sample app](https://github.com/CleverTap/clevertap-flutter/tree/master/example/android) for guidance on migration.

1. Add the following to your `dependencies` section in `project/build.gradle`:

```groovy
dependencies {
            classpath 'com.android.tools.build:gradle:8.6.1'
            classpath 'com.google.gms:google-services:4.4.1' //<--- Mandatory for using Firebase Messaging, skip if not using FCM
        }
```

2. Add the following to your `dependencies` section in `app/build.gradle`:

> 📘 Migrating from ExoPlayer to AndroidX Media3
>
> Starting from CleverTap Flutter SDK v2.5.0, CleverTap now supports [AndroidX Media3](https://developer.android.com/media/media3/exoplayer/migration-guide), replacing the deprecated ExoPlayer libraries. Support for [ExoPlayer](https://github.com/google/ExoPlayer) will be removed after upgrading to v9.0.0. For a smoother migration to AndroidX Media3, update the following dependencies:

```groovy Dependencies with AndroidX Media3
implementation 'com.google.firebase:firebase-messaging:21.0.0'
implementation 'androidx.core:core:1.3.0'
implementation 'androidx.fragment:fragment:1.3.6'
        
//MANDATORY for App Inbox
implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'
        
//For CleverTap Android SDK v3.6.4 and above, add the following line of code
implementation 'com.android.installreferrer:installreferrer:2.2'

//Optional AndroidX Media3 Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies

implementation "androidx.media3:media3-exoplayer:1.1.1"
implementation "androidx.media3:media3-exoplayer-hls:1.1.1"
implementation "androidx.media3:media3-ui:1.1.1"
```
```groovy Dependencies with Exoplayer Libraries
implementation 'com.google.firebase:firebase-messaging:21.0.0'
implementation 'androidx.core:core:1.3.0'
implementation 'androidx.fragment:fragment:1.3.6'
        
//MANDATORY for App Inbox
implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'
        
//For CleverTap Android SDK v3.6.4 and above, add the following line of code
implementation 'com.android.installreferrer:installreferrer:2.2'
        
//Optional ExoPlayer Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies

implementation "com.google.android.exoplayer:exoplayer:2.19.1"
implementation "com.google.android.exoplayer:exoplayer-hls:2.19.1"
implementation "com.google.android.exoplayer:exoplayer-ui:2.19.1"
```

3. At the end of the `app/build.gradle` file, add the following:

```groovy
apply plugin: 'com.google.gms.google-services' //skip if not using FCM
```

## Set Up Android's Application class

In your app's `Android Application class`, add the following code:

```java
public class MyApplication extends FlutterApplication {
        @java.lang.Override
        public void onCreate() {
            ActivityLifecycleCallback.register(this); //<--- Must call this before super.onCreate()
            super.onCreate();
        }
    }
```
```kotlin
class MyApplication : FlutterApplication() {
    override fun onCreate() {
        ActivityLifecycleCallback.register(this)//<--- Add this before super.onCreate()
        super.onCreate()
    }
}
```

If you do not have an Application class, add this to your `AndroidManifest.xml` file:

```xml
<application
        android:label="@string/app_name"
        android:icon="@drawable/ic_launcher"
        android:name="com.clevertap.android.sdk.Application">
```

# iOS integration

## Add CleverTap Credentials

To link your iOS app with your CleverTap account, add your CleverTap credentials to your application's `Info.plist` file.

Perform the following steps:

1. Navigate to the` Info.plist` file in your project navigator.

<Image align="center" alt={574} border={true} caption="Information Property List in Info.plist File" title="Information Property List in Info.plist File" src="https://files.readme.io/19c6c39-image4.png" />

2. Create a key called _CleverTapAccountID_ with the type _string_.
3. Create a key called _CleverTapToken_ with the type _string_.
4. Insert the account ID and account token values from your _CleverTap account_. These values are available on the _Settings_ page on the CleverTap dashboard.

### Add Region Code

To know how to add region code for iOS in Flutter SDK, refer to [Region Codes.](doc:idc#ios)

## Initialize CleverTap SDK

1. Initialize CleverTap SDK by adding the following code snippet to import the CleverTap header in your AppDelegate file:

```objectivec
#import "CleverTap.h"
 #import "CleverTapPlugin.h"
```
```swift
import CleverTapSDK
import clevertap_plugin
```

2. In your `didFinishLaunchingWithOptions:` method, notify the CleverTap Flutter SDK of application launch:

```objectivec
[CleverTap autoIntegrate]; // integrate CleverTap SDK using the autoIntegrate option
[[CleverTapPlugin sharedInstance] applicationDidLaunchWithOptions:launchOptions];
```
```swift
CleverTap.autoIntegrate() // integrate CleverTap SDK using the autoIntegrate option
CleverTapPlugin.sharedInstance()?.applicationDidLaunch(options: launchOptions)
```

> 🚧 Note
>
> If you want to handle the clicks on the iOS Push notifications manually, remove the `[CleverTap autoIntegrate];` line from your integration.

# Web Integration

## Initialize Flutter Web SDK

1. Add the below script in the `index.html` file:
   ```javascript
   <script src="./assets/packages/clevertap_plugin/assets/clevertap.js"></script>
   ```
2. Initialize CleverTap Flutter Web SDK using the `CleverTapPlugin.init` method:
   ```d Dart
    CleverTapPlugin.init("CLEVERTAP_ACCOUNT_ID", "CLEVERTAP_REGION", "CLEVERTAP_TARGET_DOMAIN");
   ```

Following are the parameter details:

* **CLEVERTAP_ACCOUNT_ID** (Mandatory): Obtain this value from the Projects page on the CleverTap dashboard.
* **CLEVERTAP_REGION** (Optional): This should match the region of the CleverTap dashboard. Possible values include **in1**, **us1**, **sg1**, **aps3** and **mec1**.
* **CLEVERTAP_TARGET_DOMAIN** (Optional): The domain of the proxy server.

### Add Region Code

To know how to add region code for Web in Flutter SDK, refer to [Region Codes.](doc:idc#web)

# Run Your Application

Run your Flutter application and navigate to your CleverTap dashboard. If you integrate the CleverTap SDK successfully, you will see a new active user on the dashboard.

<Image align="center" alt={1405} border={true} caption="View Active User on the Dashboard" title="View Active User on the Dashboard" src="https://files.readme.io/f8a92ef-135b863-1_New_mobile_users.png" />

# Flutter Integration

## Track User Profiles

A user profile is automatically created in CleverTap for each user launching your application.

Initially, the user profile starts out as anonymous which means the profile does not contain any identifiable information about the user. You can enrich the profile with pre-defined attributes from the CleverTap data model, such as name and email. You can also add custom attributes that you define to extend the CleverTap data model. Create a User profile when the user logs in (On User Login):

```javascript Dart
var stuff = ["bags", "shoes"];
var profile = {
    'Name': 'Captain America',
    'Identity': '100',
    'Email': 'captain@america.com',
    'Phone': '+14155551234',
    'stuff': stuff
};
CleverTapPlugin.onUserLogin(profile);
```

When the `Onuserlogin` method is called, the user profile information is sent to CleverTap.

To see how this information displays within the CleverTap dashboard:

1. Log in to the CleverTap dashboard.
2. Click `Find People` under the `Segment` tab. In the By Identity box, enter the email you set on the user profile record and click Find.

<Image align="center" alt={1407} border={true} caption="Find People" title="Find People" src="https://files.readme.io/8e785f5-eac3675-2_By_identity_search_box.png" />

If CleverTap finds a user profile with this email, the user record is displayed. On that page, you will see name and email as pre-defined fields and any other custom fields.

<Image align="center" alt={2912} border={true} caption="CleverTap User Profile" title="CleverTap User Profile" src="https://files.readme.io/ab96437-7bdfec6-3__User_Profile.png" />

## Update the User Profile

The `Onuserlogin()` method identifies the individual users on the device. However, you may need to add additional user properties to the user profile, such as gender, date of birth, and so on. You can update these properties with the `profilePush()` method. For more information, refer to our [User Documentation](https://developer.clevertap.com/docs/concepts-user-profiles).

## Track User Events

After you integrate the CleverTap SDK, we automatically start tracking events, such as App Launch and Notification Viewed. In addition to the [default events tracked](https://developer.clevertap.com/docs/events) by CleverTap, you can also track custom events.

To send custom events to CleverTap using our Flutter SDK, you will have to call the recordEvent method with the name of the custom event you want to track.  
Record an event without event data:

```javascript Dart
CleverTapPlugin.recordEvent("Product Viewed",{});
```

<Image align="center" alt={2722} border={true} caption="Track User Events" title="Track User Events" src="https://files.readme.io/8759e00-ae28962-Screenshot_2022-08-02_at_1.44.51_PM.png" />

Record an event with event data:

```javascript Dart
var eventData = {
    // Key:    Value
   'first': 'partridge',
   'second': 'turtledoves'
};
CleverTapPlugin.recordEvent("Flutter Event", eventData);
```

## Debugging the Flutter Application with CleverTap APIs

In order to log warnings or other important messages to the logging system, please refer to [Flutter Debugging](https://developer.clevertap.com/docs/flutter-advanced-features#debugging)

# GDPR Compliance

CleverTap provides APIs to support GDPR compliance. For more information, refer to [Flutter GDPR Compliance](https://developer.clevertap.com/docs/flutter-advanced-features#gdpr)

# Sample Project Demonstrating Direct Implementation of CleverTap Features

For an example project of integrating CleverTap Flutter SDK and using all its features in your Flutter application, refer to the [Github Flutter Example Project](https://github.com/CleverTap/clevertap-flutter/tree/master/example).

# Next Steps

By completing this guide, you are now automatically tracking user events, such as app launches, and associating that information with profiles for each user. You have also learned how to debug, how to add information to a user profile, and how to track custom events.
