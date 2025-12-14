---
title: Unreal Quick Start Guide
excerpt: Learn how to quickly get started with CleverTap Unreal SDK
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

This guide walks you through integrating the CleverTap Unreal SDK into your Unreal Engine project, configuring it for your platform, and enabling core features such as user tracking and event logging.

Follow these steps to complete the integration:

1. [Enable the Plugin](doc:unreal-quick-start-guide#enable-the-plugin)
2. [Add Dependencies](doc:unreal-quick-start-guide#add-dependencies)
3. [Configure Credentials](doc:unreal-quick-start-guide#configure-credentials)
4. [Initialize the SDK](doc:unreal-quick-start-guide#initialize-the-sdk)
5. [Configure Push Notifications](doc:unreal-quick-start-guide#configure-push-notifications)

## Enable the Plugin

Add the CleverTap plugin to your Unreal project's `.uproject` file.

```json
{
  "Plugins": [
    {
      "Name": "CleverTap",
      "Enabled": true
    }
  ]
}
```

## Add Dependencies

To link CleverTap with your C++ module, update your build targets by adding the appropriate dependency type in the module’s `*.Target.cs` file.

Use one of the following options based on how CleverTap is used in your module:

1. **If CleverTap is used only in internal logic (private source files)**, add it to the `PrivateDependencyModuleNames` list:
   ```cplusplus
   PrivateDependencyModuleNames.Add("CleverTap");
   ```

2. **If you use CleverTap classes or functions in your module's public headers**, add it to the `PublicDependencyModuleNames` list:
   ```cplusplus
   PublicDependencyModuleNames.Add("CleverTap");
   ```

> 📘 Public vs Private Dependencies
> 
> Use `PublicDependencyModuleNames` only if CleverTap types (such as `UCleverTapManager`) are included in public headers exposed to other modules.

## Configure Credentials

Add your CleverTap project credentials to the `Config/DefaultEngine.ini`:

You can find the CleverTap Account ID, CleverTap Account token, and CleverTap Region on the [Settings](https://dashboard.clevertap.com/x/settings.html) page of the CleverTap dashboard.

> 🚧 Credentials and Access
> 
> The **CleverTap Account ID** and **CleverTap Token** are available as **Project ID** and **Project Token** respectively on the CleverTap dashboard. Member and Creator roles in the project do not have access to view the account Passcode and Project Token on the CleverTap dashboard.

To know how to add a region code, refer to [Region Codes](doc:idc#enable-clevertap-region).

```csharp INI
[/Script/CleverTap.CleverTapConfig]
ProjectId= ; Add your Project ID from CleverTap dashboard
ProjectToken= ; Add your Project Token
RegionCode= ; Refer to region code list
```

You can also set these from UE4Editor Plugin Settings.

## Initialize the SDK

By default, the CleverTap SDK is initialized automatically when your project starts.

This behavior is controlled by the following setting in `Config/DefaultEngine.ini`:

```csharp INI
[/Script/CleverTap.CleverTapConfig]
bAutoInitializeSharedInstance=true
```

When `bAutoInitializeSharedInstance` is set to `true`, the `UCleverTapSubsystem` initializes the CleverTap SDK and the default shared instance using the credentials configured in the INI file.

> 🚧 Platform Limitation
> 
> Auto-initialization is required on Android. Deferred initialization is supported only on iOS.

### iOS: Defer Initialization (Optional)

To disable auto-initialization:

```csharp INI
bAutoInitializeSharedInstance=false
```

You can then manually initialize the SDK using:

```csharp INI
GEngine->GetEngineSubsystem<UCleverTapSubsystem>()->InitializeSharedInstance();
```

Or use explicit configuration at runtime:

```csharp INI
FCleverTapInstanceConfig Config;
Config.ProjectId = TEXT("YOUR_PROJECT_ID");
Config.ProjectToken = TEXT("YOUR_PROJECT_TOKEN");
Config.RegionCode = TEXT("YOUR_REGION_CODE");
Config.LogLevel = ECleverTapLogLevel::Debug;

GEngine->GetEngineSubsystem<UCleverTapSubsystem>()->InitializeSharedInstance(Config);
```

> 📘 Set Log Level for Debugging
> 
> Use `ECleverTapLogLevel` to control log verbosity (`Off`, `Info`, `Debug`, `Verbose`).

## Configure Push Notifications

CleverTap supports sending push notifications to your app users from the dashboard after they grant permission via a call to `PromptForPushPermission()`.

### Platform-Specific Setup

Push notification configuration differs slightly between Android and iOS platforms.

### Android: Configure Firebase Cloud Messaging (FCM)

To use CleverTap's default FCM implementation:

1. [Register your Firebase credentials](doc:android-push#configure-push-notifications) and add them to the CleverTap dashboard.
2. Download the `google-services.json` file and place it in a directory within your project (for example, `Config/`).
3. Update `Config/DefaultEngine.ini` as follows:

```coffeescript INI
[/Script/CleverTap.CleverTapConfig]
bAndroidIntegrateFirebase=True
AndroidGoogleServicesJsonPath=Config/google-services.json
```

### Custom Android Notification Handling

Android only allows one `FirebaseMessagingService` per app. If your project uses another plugin that already defines its own FCM service (such as the Unreal Firebase plugin), the default CleverTap integration may conflict.

To resolve this:

1. Disable CleverTap’s default Firebase integration:

```coffeescript INI
bAndroidIntegrateFirebase=False
```

2. Implement a custom Java-based FCM message multiplexer:

For example:

```java
public class UnifiedMessagingService extends SomeOtherPluginMessagingService {
    @Override
    public void onMessageReceived(RemoteMessage message) {
        if (isCleverTapMessage(message)) {
            new CTFcmMessageHandler().createNotification(getApplicationContext(), message);
        } else {
            super.onMessageReceived(message);
        }
    }

    private boolean isCleverTapMessage(RemoteMessage message) {
        Map<String, String> data = message.getData();
        return data != null && data.containsKey("wzrk_pn"); // CleverTap payload identifier
    }
}
```

> 📘 Compatibility with Other FCM Plugins
> 
> Using this setup allows CleverTap to work alongside your existing FCM integration without causing service conflicts..

For more details, refer to [Custom Android Push Notification Handling](https://developer.clevertap.com/docs/android-push#custom-android-push-notification-handling).