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
[block:callout]
{
  "type": "info",
  "body": "If you are using AndroidX dependencies in your project, refer [v2.0.0](https://github.com/CleverTap/clevertap-unity-sdk/releases/tag/2.0.0)",
  "title": "Note"
}
[/block]
# Install

1. Import the CleverTapUnityPlugin.unitypackage into your Unity Project (`Assets` > `Import Package` > `Custom Package`) or manually copy `Plugin/CleverTapUnity`, `Plugin/PlayServicesResolver` and `Plugin/Plugins/Android` (or copy the files in `Plugin/Plugins/Android` to your existing `Assets/Plugins/Android` directory) into the Assets directory of your Unity Project.

2. Create an empty game object (GameObject > Create Empty) and rename it as `CleverTapUnity`. Add `Assets/CleverTapUnity/CleverTapUnity-Scripts/CleverTapUnity.cs` as a component of the `CleverTapUnity GameObject`.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/5ab2721-unity_gameobj.jpg",
        "unity_gameobj.jpg",
        1000,
        361,
        "#959391"
      ],
      "border": true
    }
  ]
}
[/block]
3. Select the `CleverTapUnity GameObject` that you created in the Hierarchy pane and add your CleverTap settings inside the Inspector window. You must include your `CleverTap Account ID` and `CleverTap Account Token` from your [CleverTap Dashboard -> Settings](https://dashboard.clevertap.com/x/settings.html).
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/48b3b93-227.png",
        "2,27.png",
        1449,
        1419,
        "#f9f9fb"
      ],
      "border": true
    }
  ]
}
[/block]
4. Edit `Assets/CleverTapUnity/CleverTapUnity-Scripts/CleverTapUnity.cs` to add your calls to the CleverTap SDK.  See usage examples in [example/CleverTapUnity.cs](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/example/CleverTapUnity.cs). For more information, see our [developer guide](https://developer.clevertap.com/docs).

# iOS Specific Instructions

- If you want to enable Push Notifications, be sure to add the Push Notifications capability to your Xcode project.  
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/56c0242-unity2.jpg",
        "unity2.jpg",
        1748,
        400,
        "#f4f6f8"
      ],
      "border": true
    }
  ]
}
[/block]
* Add a run script to your build phases. In Xcode, go to your Targets, under your app’s name, select Build Phases after embed frameworks, add a run script phase, and set it to use `/bin/sh` and the [iOS script](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/Plugin/CleverTapUnity/iOS/strip.sh):
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ad68118-ct_script_ios.jpg",
        "ct_script_ios.jpg",
        2554,
        1368,
        "#f5f2f2"
      ],
      "border": true
    }
  ]
}
[/block]
The script scans your built application’s `Frameworks` folder and strips the unnecessary simulator architectures from the CleverTakSDK.framework prior to archiving/submitting the app store.

- Build and run your iOS project.

# Android Specific Instructions

[block:callout]
{
  "type": "info",
  "title": "Steps for Android X",
  "body": "Use these additional steps only if you are using Android X dependencies. \n\n* Go to `File` > `Build Settings` > `Android` > `Player Settings` > `Publishing Settings` > `Build`. \nEnsure the `Custom Gradle Template` option is checked.\n\n* Go to `Assets` > `Play Services Resolver` > `Android Resolver` > `Settings`. \nEnsure that `Use Jetifier` and `Patch mainTemplate.gradle` options are checked."
}
[/block]
- To enable Push Notifications, add the Firebase Unity SDK to your app. For more information, see [Firebase Unity Setup Docs](https://firebase.google.com/docs/unity/setup).
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The Firebase Unity SDK may override your AndroidManifest.xml file. If that happens, revert to your original manifest file."
}
[/block]
- Run `Assets` > `Play Services Resolver` > `Android Resolver` > `Resolve Client Jars` from the Unity menu bar to install the required google play services and android support library dependencies.

- Edit the `AndroidManifest.xml` file in `Assets/Plugins/Android` to add your Bundle Identifier, FCM Sender ID, CleverTap Account Id, CleverTap Token and Deep Link url scheme (if applicable):
[block:code]
{
  "codes": [
    {
      "code": "<manifest xmlns:android=\"http://schemas.android.com/apk/res/android\" package=\"YOUR_BUNDLE_IDENTIFIER\"\n          \n          ",
      "language": "xml"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "android:versionName=\"1.0\" android:versionCode=\"1\" android:installLocation=\"preferExternal\"> <supports-screens android:smallScreens=\"true\" android:normalScreens=\"true\" android:largeScreens=\"true\" android:xlargeScreens=\"true\" android:anyDensity=\"true\" />\n<meta-data\n    android:name=\"GCM_SENDER_ID\"\n    android:value=\"id:YOUR_FCM_SENDER_ID\"/>\n<meta-data\n    android:name=\"CLEVERTAP_ACCOUNT_ID\"\n    android:value=\"Your CleverTap Account ID\"/>\n<meta-data\n    android:name=\"CLEVERTAP_TOKEN\"\n    android:value=\"Your CleverTap Account Token\"/>",
      "language": "xml"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "<!-- Deep Links uncomment and replace YOUR_URL_SCHEME, if applicable, or remove if not supporting deep links-->\n<!--\n    <intent-filter android:label=\"@string/app_name\">\n    <action android:name=\"android.intent.action.VIEW\" />\n    <category android:name=\"android.intent.category.DEFAULT\" />\n    <category android:name=\"android.intent.category.BROWSABLE\" />\n    <data android:scheme=\"YOUR_URL_SCHEME\" />\n    </intent-filter>\n-->  \n\n\n",
      "language": "xml"
    }
  ]
}
[/block]

- Add the following in the `AndroidManifest.xml` file:

[block:code]
{
  "codes": [
    {
      "code": "    <service\n        android:name=\"com.clevertap.android.sdk.FcmTokenListenerService\">\n        <intent-filter>\n            <action android:name=\"com.google.firebase.INSTANCE_ID_EVENT\"/>\n        </intent-filter>\n    </service>\n\n    <service\n        android:name=\"com.clevertap.android.sdk.FcmMessageListenerService\">\n        <intent-filter>\n            <action android:name=\"com.google.firebase.MESSAGING_EVENT\"/>\n        </intent-filter>\n    </service>\n\n\n\n",
      "language": "xml"
    }
  ]
}
[/block]

- Add your `google-services.json` file to the Assets folder of the project.

- To enable A/B UI editor, edit `Assets/CleverTapUnity/CleverTapUnity-Scripts/CleverTapBinding.cs` and `CleverTapAPI.CallStatic("setUIEditorConnectionEnabled", true)` call just before getting the clevertap instance.
[block:code]
{
  "codes": [
    {
      "code": "public static AndroidJavaObject CleverTap {\n        get {\n            if (clevertap == null) {\n                AndroidJavaObject context = unityCurrentActivity.Call<AndroidJavaObject>(\"getApplicationContext\");\n                \n                //set the UI editor flag before getting the Clevertap instance, defaults to false.\n                CleverTapAPI.CallStatic(\"setUIEditorConnectionEnabled\", true);\n                \n                clevertap = CleverTapAPI.CallStatic<AndroidJavaObject>(\"getInstance\", context);\n            }\n            return clevertap;\n        }\n    }\n\n\n",
      "language": "csharp"
    }
  ]
}
[/block]

- Build your app or Android project as usual.