---
title: Unity
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
CleverTap provides a [Unity Plugin](https://github.com/CleverTap/clevertap-unity-sdk) that enables app developers to track, segment, and engage their users. 

> 📘 Note
>
> If you are using AndroidX dependencies in your project, refer [v2.0.0](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/2.0.0)

# Install

1. Import the CleverTapUnityPlugin.unitypackage into your Unity Project (`Assets` > `Import Package` > `Custom Package`) or manually copy `Plugin/CleverTapUnity`, `Plugin/PlayServicesResolver` and `Plugin/Plugins/Android` (or copy the files in `Plugin/Plugins/Android` to your existing `Assets/Plugins/Android` directory) into the Assets directory of your Unity Project.

2. Create an empty game object (GameObject > Create Empty) and rename it as `CleverTapUnity`. Add `Assets/CleverTapUnity/CleverTapUnity-Scripts/CleverTapUnity.cs` as a component of the `CleverTapUnity GameObject`.

<Image title="unity_gameobj.jpg" alt={1000} className="border" border={true} src="https://files.readme.io/5ab2721-unity_gameobj.jpg" />

3. Select the `CleverTapUnity GameObject` that you created in the Hierarchy pane and add your CleverTap settings inside the Inspector window. You must include your `CleverTap Account ID` and `CleverTap Account Token` from your [CleverTap Dashboard -> Settings](https://dashboard.clevertap.com/x/settings.html).

<Image title="2,27.png" alt={1449} className="border" border={true} src="https://files.readme.io/48b3b93-227.png" />

4. Edit `Assets/CleverTapUnity/CleverTapUnity-Scripts/CleverTapUnity.cs` to add your calls to the CleverTap SDK.  See usage examples in [example/CleverTapUnity.cs](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/example/CleverTapUnity.cs). For more information, see our [developer guide](https://developer.clevertap.com/docs).

# iOS Specific Instructions

* If you want to enable Push Notifications, be sure to add the Push Notifications capability to your Xcode project.  

<Image title="unity2.jpg" alt={1748} className="border" border={true} src="https://files.readme.io/56c0242-unity2.jpg" />

* Add a run script to your build phases. In Xcode, go to your Targets, under your app’s name, select Build Phases after embed frameworks, add a run script phase, and set it to use `/bin/sh` and the [iOS script](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/Plugin/CleverTapUnity/iOS/strip.sh):

<Image title="ct_script_ios.jpg" alt={2554} className="border" border={true} src="https://files.readme.io/ad68118-ct_script_ios.jpg" />

The script scans your built application’s `Frameworks` folder and strips the unnecessary simulator architectures from the CleverTakSDK.framework prior to archiving/submitting the app store.

* Build and run your iOS project.

# Android Specific Instructions

> 📘 Steps for Android X
>
> Use these additional steps only if you are using Android X dependencies. 
>
> * Go to `File` > `Build Settings` > `Android` > `Player Settings` > `Publishing Settings` > `Build`.\
>   Ensure the `Custom Gradle Template` option is checked.
>
> * Go to `Assets` > `Play Services Resolver` > `Android Resolver` > `Settings`.\
>   Ensure that `Use Jetifier` and `Patch mainTemplate.gradle` options are checked.

* To enable Push Notifications, add the Firebase Unity SDK to your app. For more information, see [Firebase Unity Setup Docs](https://firebase.google.com/docs/unity/setup).

> 📘 Note
>
> The Firebase Unity SDK may override your AndroidManifest.xml file. If that happens, revert to your original manifest file.

* Run `Assets` > `Play Services Resolver` > `Android Resolver` > `Resolve Client Jars` from the Unity menu bar to install the required google play services and android support library dependencies.

* Edit the `AndroidManifest.xml` file in `Assets/Plugins/Android` to add your Bundle Identifier, FCM Sender ID, CleverTap Account Id, CleverTap Token and Deep Link url scheme (if applicable):

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="YOUR_BUNDLE_IDENTIFIER"
```

```xml
android:versionName="1.0" android:versionCode="1" android:installLocation="preferExternal"> <supports-screens android:smallScreens="true" android:normalScreens="true" android:largeScreens="true" android:xlargeScreens="true" android:anyDensity="true" />
<meta-data
    android:name="GCM_SENDER_ID"
    android:value="id:YOUR_FCM_SENDER_ID"/>
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

* Add the following in the `AndroidManifest.xml` file:

```xml
<service
        android:name="com.clevertap.android.sdk.FcmTokenListenerService">
        <intent-filter>
            <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
        </intent-filter>
    </service>

    <service
        android:name="com.clevertap.android.sdk.FcmMessageListenerService">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT"/>
        </intent-filter>
    </service>
```

* Add your `google-services.json` file to the Assets folder of the project.

* To enable A/B UI editor, edit `Assets/CleverTapUnity/CleverTapUnity-Scripts/CleverTapBinding.cs` and `CleverTapAPI.CallStatic("setUIEditorConnectionEnabled", true)` call just before getting the clevertap instance.

```csharp
public static AndroidJavaObject CleverTap {
        get {
            if (clevertap == null) {
                AndroidJavaObject context = unityCurrentActivity.Call<AndroidJavaObject>("getApplicationContext");
                
                //set the UI editor flag before getting the Clevertap instance, defaults to false.
                CleverTapAPI.CallStatic("setUIEditorConnectionEnabled", true);
                
                clevertap = CleverTapAPI.CallStatic<AndroidJavaObject>("getInstance", context);
            }
            return clevertap;
        }
    }
```

* Build your app or Android project as usual.
