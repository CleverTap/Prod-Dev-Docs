---
title: Unity Native SDK Quick Start Guide
excerpt: Learn how to install and set up the CleverTap's Unity Native SDK in your app.
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

This document guides you on how to install the CleverTap SDK, track your first user event, and view this information within the CleverTap dashboard in less than ten minutes. The plugin includes the Unity Native SDK, which is packaged within the same Unity module and extends support for multiple platforms, including macOS, Windows, Linux, and WebGL.

# Install SDK

The installation involves the following major steps:

1. [Import the CleverTap Unity Package](doc:unity-native-sdk-quick-start-guide#import-the-clevertap-unity-package).
2. [Set Up the Unity SDK](doc:unity-native-sdk-quick-start-guide#set-up-unity-sdk).
3. [Initialize CleverTap SDK](doc:unity-native-sdk-quick-start-guide#initialize-clevertap-sdk).

## Import the CleverTap Unity Package

To import the CleverTap Unity Package into your project:

1. Download the latest CleverTap Unity package from the [GitHub Releases page](https://github.com/CleverTap/clevertap-unity-sdk/releases). Then, in Unity, go to _Assets_ > _Import Package_ > _Custom Package_, and select the downloaded `.unitypackage` file.
2. Add the **PlayServiceResolver** and the **ExternalDependencyManager** folders in your project. These folders install the **EDM4U** plugin, which automatically adds all the Android and iOS dependencies during the build process.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3c7ec95-Screenshot_2024-08-09_at_4.07.16_PM.png",
        "",
        ""
      ],
      "align": "center",
      "border": true
    }
  ]
}
[/block]


3. Check that the `AndroidPostImport` script is added, as it sets up `clevertap-android-wrapper` library for Android.

### Optional

You can install the CleverTap Unity SDK using the .unitypackage file or as a local dependency via the Unity Package Manager (UPM).

To install via UPM:

1. Clone the latest release of the CleverTap Unity SDK.
2. Open Unity and go to _Window_ > _Package Manager_.
3. Click the _+ icon_ > _Add package_ from disk.
4. Select the `package.json` file from the cloned repo folder.

## Set Up Unity SDK

CleverTap API can be accessed anywhere in your project by calling the static CleverTap class. You need not create a _GameObject_ with the _CleverTapUnity_ name or attach any script. The new architecture handles the following: 

- Instantiation of platform-specific binding
- Creation of `GameObject`
- Script attachment

## Initialize CleverTap SDK

The `WebGL` initialization requires [region code](https://developer.clevertap.com/docs/idc). Use the **LaunchWithCredentialsForRegion** method to initialize with the correct region code.

```csharp
// Initialize CleverTap
#if UNITY_WEBGL
	CleverTap.LaunchWithCredentialsForRegion({YOUR_CLEVERTAP_ACCOUNT_ID}, {YOUR_CLEVERTAP_ACCOUNT_TOKEN}, {CLEVERTAP_ACCOUNT_REGION});
#endif
```

### Find CleverTap Account ID and Token

You can view your `CleverTap Account ID` and `CleverTap Account Token` from the CleverTap dashboard by navigating to the _Settings_ > _Project_ page on the CleverTap dashboard.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4e89ad8-Project_Details.png",
        "Add CleverTap Account and Preferences",
        1449
      ],
      "align": "center",
      "sizing": "100% ",
      "border": true,
      "caption": "Add CleverTap Account and Preferences"
    }
  ]
}
[/block]


> 📘 CORS Credentials Configuration
> 
> If you are using **JavaScript Fetch** to call APIs from your client, ensure that requests to **CleverTap domains do not include credentials**. Setting `"credentials": "include"` on such requests may lead to CORS issues.
> 
> Use the following snippet to override the default behavior and selectively apply credentials only for non-CleverTap domains:
> 
> ```javascript
> fetch = function (url, data) {
>     if (url.indexOf('clevertap') == -1) {
>         data.credentials = 'include';
>     }
>     return originalFetch(url, data);
> };
> ```
> 
> This ensures that cross-origin calls to CleverTap avoid unnecessary credential sharing and CORS conflicts.

# Callbacks

A new mechanism to handle callbacks is introduced. You can now add an event listener for a callback directly through the CleverTap static events. You need not set all callbacks in the `CleverTapUnity.cs` anymore.

```csharp
CleverTap.OnCleverTapProfileInitializedCallback += YOUR_CALLBACK_METHOD;  
CleverTap.OnCleverTapProfileUpdatesCallback += YOUR_CALLBACK_METHOD;
```

This introduces breaking changes in the `CleverTapUnity.cs`, rendering all existing callback methods obsolete, and these methods will not be called anymore. If you still want to use the callbacks through the `CleverTapUnity.cs` methods, each method must subscribe to a new event listener found at `CleverTap.On{CleveTapUnity_Callback_MethodName}`.

The following is a sample code for subscribing to new event listeners with existing methods in `CleverTapUnity.cs`:

```csharp
CleverTap.OnCleverTapProfileInitializedCallback += CleverTapProfileInitializedCallback;
CleverTap.OnCleverTapProfileUpdatesCallback += CleverTapProfileUpdatesCallback;
CleverTap.OnCleverTapInitCleverTapIdCallback += CleverTapInitCleverTapIdCallback;
```

# Track User Profiles

Create a User profile when the user logs in with the `OnUserLogin` method:

```csharp C#
Dictionary<string, string> newProps = new Dictionary<string, string>();
newProps.Add("email", "test@test.com");
newProps.Add("Identity", "123456");
newProps.put("Name", "Jack Montana");    // String
newProps.put("Identity", "61026032");      // String
newProps.put("Email", "jack@gmail.com"); // Email address of the user
newProps.put("Phone", "+14155551234");   // Phone (with the country code, starting with +)
newProps.put("Gender", "M");             // Can be either M or F
CleverTap.OnUserLogin(newProps);
```

# Track User Events

- Record an event without event data:

```csharp C#
// record basic event with no properties
CleverTap.RecordEvent("testEvent");
```

- Record an event with event data as follows:

```csharp
// record event with properties
Dictionary<string, object> Props = new Dictionary<string, object>();
Props.Add("testKey", "testValue");
CleverTap.RecordEvent("testEventWithProps", Props);
```