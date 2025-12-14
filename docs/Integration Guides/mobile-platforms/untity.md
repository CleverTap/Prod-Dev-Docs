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
CleverTap provides a [Unity Plugin](https://github.com/CleverTap/clevertap-react-native) that enables app developers to track, segment, and engage their users. 

# Install

1.  Import the [CleverTapUnityPlugin.unitypackage](https://github.com/CleverTap/clevertap-unity-sdk) into your Unity Project (`Assets` > `Import Package` > `Custom Package`) or manually Copy `Plugin/CleverTapUnity`, `Plugin/PlayServicesResolver` and `Plugin/Plugins/Android` (or copy the files in `Plugin/Plugins/Android` to your existing `Assets/Plugins/Android` directory) into the Assets directory of your Unity Project.

2. Create an empty game object (GameObject -> Create Empty) and rename it `CleverTapUnity`. Add `Assets/CleverTapUnity/CleverTapUnity-Scripts/CleverTapUnity.cs` as a component of the `CleverTapUnity GameObject`.
    

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6fe0e21-unity1.jpg",
        "unity1.jpg",
        1000,
        361,
        "#959391"
      ]
    }
  ]
}
[/block]
3. Select the `CleverTapUnity GameObject` you created in the Hierarchy pane and add your CleverTap settings inside the Inspector window. You must include your `CleverTap Account ID` and `CleverTap Account Token` from your [CleverTap Dashboard -> Settings](https://dashboard.clevertap.com/x/settings.html).

4. Edit `Assets/CleverTapUnity/CleverTapUnity-Scripts/CleverTapUnity.cs` to add your calls to CleverTap SDK.  See usage examples in [example/CleverTapUnity.cs](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/example/CleverTapUnity.cs).  

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
      ]
    }
  ]
}
[/block]
- Build and run your iOS project.

# Android Specific Instructions
- Run `Assets` > `Play Services Resolver` > `Android Resolver` > `Resolve Client Jars` from the Unity menu bar to install the required google play services and android support library dependencies.

- Edit the `AndroidManifest.xml` file in `Assets/Plugins/Android` to add your Bundle Identifier, FCM Sender ID and Deep Link url scheme (if applicable): 
[block:code]
{
  "codes": [
    {
      "code": "    <manifest xmlns:android=\"http://schemas.android.com/apk/res/android\" package=\"YOUR_BUNDLE_IDENTIFIER\" android:versionName=\"1.0\" android:versionCode=\"1\" android:installLocation=\"preferExternal\"> <supports-screens android:smallScreens=\"true\" android:normalScreens=\"true\" android:largeScreens=\"true\" android:xlargeScreens=\"true\" android:anyDensity=\"true\" />",
      "language": "xml"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "    <permission android:protectionLevel=\"signature\" android:name=\"YOUR_BUNDLE_IDENTIFIER.permission.C2D_MESSAGE\" />\n    <uses-permission android:name=\"YOUR_BUNDLE_IDENTIFIER.permission.C2D_MESSAGE\" />",
      "language": "xml"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "    <receiver\n        android:name=\"com.google.android.gms.gcm.GcmReceiver\"\n        android:exported=\"true\"\n        android:permission=\"com.google.android.c2dm.permission.SEND\" >\n        <intent-filter>\n            <action android:name=\"com.google.android.c2dm.intent.RECEIVE\" />\n            <action android:name=\"com.google.android.c2dm.intent.REGISTRATION\" />\n            <category android:name=\"YOUR_BUNDLE_IDENTIFIER\" />\n        </intent-filter>\n    </receiver>",
      "language": "xml"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "  <meta-data\n        android:name=\"GCM_SENDER_ID\"\n        android:value=\"id:YOUR_FCM_SENDER_ID\"/>",
      "language": "xml"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "    <!-- Deep Links uncomment and replace YOUR_URL_SCHEME, if applicable, or remove if not supporting deep links-->\n    <!--\n        <intent-filter android:label=\"@string/app_name\">\n        <action android:name=\"android.intent.action.VIEW\" />\n        <category android:name=\"android.intent.category.DEFAULT\" />\n        <category android:name=\"android.intent.category.BROWSABLE\" />\n        <data android:scheme=\"YOUR_URL_SCHEME\" />\n        </intent-filter>\n    -->  ",
      "language": "xml"
    }
  ]
}
[/block]

- Build your app or Android project as usual.