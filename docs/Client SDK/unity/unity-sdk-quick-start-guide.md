---
title: Unity SDK Quick Start Guide (Legacy)
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

This section shows how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard in less than ten minutes. CleverTap provides Unity SDK that enables app developers to track, segment, and engage their users.

# Install

To install the SDK:

1. Import the `CleverTapUnityPlugin.unitypackage` into your Unity Project (_Assets_ >  _Import Package_ > _Custom Package_) or manually copy `Plugin/CleverTapUnity`, `Plugin/PlayServicesResolver` and `Plugin/Plugins/Android` (or copy the files in `Plugin/Plugins/Android` to your existing Assets > Plugins > Android\` directory) into the Assets directory of your Unity Project.
2. Create an empty game object (GameObject > Create Empty) and rename it as `CleverTapUnity`. Add `Assets/CleverTapUnity/CleverTapUnity-Scripts/CleverTapUnity.cs` as a component of the `CleverTapUnity GameObject`.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f9829e4-ct-ios-idfv.png",
        "Sample(CleverTapUnity) Game Object",
        2880
      ],
      "align": "center",
      "border": true,
      "caption": "Create a Sample(CleverTapUnity) Game Object"
    }
  ]
}
[/block]


3. Select the `CleverTapUnity GameObject` that you created in the _Hierarchy_ pane and add your CleverTap settings inside the _Inspector_ window. You must include your `CleverTap Account ID` and `CleverTap Account Token` from the [_Settings_](https://dashboard.clevertap.com/x/settings.html) page.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/48b3b93-227.png",
        "Add CleverTap Account and Preferences",
        1449
      ],
      "align": "center",
      "border": true,
      "caption": "Add CleverTap Account and Preferences"
    }
  ]
}
[/block]


4. Edit `Assets/CleverTapUnity/CleverTapUnity-Scripts/CleverTapUnity.cs` to add your calls to the CleverTap SDK.  See usage examples in [example/CleverTapUnity.cs](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/example/CleverTapUnity.cs). For more information, refer to the [Developer Guide](https://developer.clevertap.com/docs).

> 📘 IDFV Usage for CleverTap ID
> 
> Starting from CleverTap Unity SDK 2.1.2, you can optionally disable the usage of IDFV for CleverTap ID by selecting CLEVERTAP_DISABLE_IDFV. We recommend this for apps that send data from different iOS apps to a common CleverTap account. By default, the CLEVERTAP_DISABLE_IDFV checkbox is cleared.

### Add Region Code

To know how to add region code in Unity SDK, refer to [Region Codes.](doc:idc#unity)

# iOS Specific Instructions

1. To enable Push Notifications, ensure to add the Push Notifications capability to your Xcode project.  

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/56c0242-unity2.jpg",
        "Enable Push Notifications",
        1748
      ],
      "align": "center",
      "border": true,
      "caption": "Enable Push Notifications"
    }
  ]
}
[/block]


2. Add a run script to your build phases. In Xcode, go to your _Targets_, under your app’s name, select _Build Phases_ after embed frameworks, add a run script phase, and set it to use `/bin/sh` and the [iOS script](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/Plugin/CleverTapUnity/iOS/strip.sh):

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ad68118-ct_script_ios.jpg",
        "Add a Run Script",
        2554
      ],
      "align": "center",
      "border": true,
      "caption": "Add a Run Script"
    }
  ]
}
[/block]


The script scans your built application’s `Frameworks` folder and strips the unnecessary simulator architectures from the `CleverTakSDK.framework` prior to archiving/submitting the app store.

3. Build and run your iOS project.

# Android Specific Instructions

> 📘 Steps for Android X Dependencies
> 
> Perform these additional steps only if you are using Android X dependencies. 
> 
> 1. Go to _File_ > _Build Settings_ >  _Android_ > _Player Settings_ > _Publishing Settings_ > _Build_. 
> 2. Ensure the `Custom Gradle Template` option is selected.
> 3. Go to _Assets_ > _Play Services Resolver_ > _Android Resolver_ > _Settings_. 
> 4. Ensure that `Use Jetifier` and `Patch mainTemplate.gradle` options are selected.

1. To enable Push Notifications, add the Firebase Unity SDK to your app. For more information, refer to [Firebase Unity Setup Docs](https://firebase.google.com/docs/unity/setup).

> 📘 Note
> 
> The Firebase Unity SDK may override your `AndroidManifest.xml` file. If that happens, revert to your original manifest file.

2. Run _Assets_ > _Play Services Resolver_ > _Android Resolver_ > _Resolve Client Jars_ from the Unity menu bar to install the required google play services and android support library dependencies.
3. Edit the `AndroidManifest.xml` file in `Assets/Plugins/Android` to add your Bundle Identifier, FCM Sender ID, CleverTap Account Id, CleverTap Token, and Deep Link URL scheme (if applicable):

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="YOUR_BUNDLE_IDENTIFIER" android:versionName="1.0" android:versionCode="1" android:installLocation="preferExternal"> <supports-screens android:smallScreens="true" android:normalScreens="true" android:largeScreens="true" android:xlargeScreens="true" android:anyDensity="true" />
```

```xml
<meta-data
    android:name="CLEVERTAP_ACCOUNT_ID"
    android:value="Your CleverTap Account ID"/>
<meta-data
    android:name="CLEVERTAP_TOKEN"
    android:value="Your CleverTap Account Token"/>
```

```xml
<!-- Deep Links uncomment and replace YOUR_URL_SCHEME, if applicable, or remove if not supporting deep links-->
<!--
    <intent-filter android:label="@string/app_name">
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="YOUR_URL_SCHEME" />
    </intent-filter>
-->
```

4. Add the following in the `AndroidManifest.xml` file:

```xml
<service
        android:name="com.clevertap.android.sdk.pushnotification.fcm.FcmMessageListenerService">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT"/>
        </intent-filter>
    </service>
```

5. Add your `google-services.json` file to the project's Assets folder.
6. Build your app or Android project as usual.

# Initialize CleverTap SDK

```csharp
#if (UNITY_IPHONE)
    CleverTapBinding.LaunchWithCredentials(CLEVERTAP_ACCOUNT_ID, CLEVERTAP_ACCOUNT_TOKEN);
#endif

#if (UNITY_ANDROID)
    CleverTapBinding.Initialize(CLEVERTAP_ACCOUNT_ID, CLEVERTAP_ACCOUNT_TOKEN);
#endif
```

# Track User Profiles

Create a User profile when the user logs in (On User Login):

```csharp C#
Dictionary<string, string> newProps = new Dictionary<string, string>();
newProps.Add("Name", "Jack Montana");    // String
newProps.Add("Identity", "61026032");      // String
newProps.Add("Email", "jack@gmail.com"); // Email address of the user
newProps.Add("Phone", "+14155551234");   // Phone (with the country code, starting with +)
newProps.Add("Gender", "M");             // Can be either M or F
CleverTapBinding.OnUserLogin(newProps);
```

# Track User Events

Record an event without event data:

```csharp C#
// record basic event with no properties
CleverTapBinding.RecordEvent("testEvent");
```

Record an event with event data:

```csharp
// record event with properties
Dictionary<string, object> Props = new Dictionary<string, object>();
Props.Add("testKey", "testValue");
CleverTapBinding.RecordEvent("testEventWithProps", Props);
```