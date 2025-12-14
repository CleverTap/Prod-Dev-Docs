---
title: Unreal Push Notifications
excerpt: Learn how to configure push notifications.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Push Notifications

Push notifications are a powerful tool for real-time user engagement. With the CleverTap Unreal SDK, you can configure and manage push notifications for Android and iOS platforms. This guide walks you through the platform-specific setup, notification behavior, and prerequisites.

CleverTap allows you to send push notifications from its dashboard after the user grants permission through the `PromptForPushPermission()` method.

# Prerequisites

The following are the prerequisites:

- The CleverTap [Unreal SDK is integrated](doc:unreal-quick-start-guide).
- For Android, Firebase Cloud Messaging (FCM) is configured. Refer to the [Android Push Setup Guide](https://developer.clevertap.com/docs/android-push).
- For iOS, Apple Push Notification service (APNs) is configured.

# Android Setup

The following steps explain how to configure Firebase, notification channels, and app behavior for delivering push notifications on Android using the CleverTap Unreal SDK.

1. [Configure Firebase Cloud Messaging (FCM)](doc:unreal-push#configure-firebase-cloud-messaging-fcm)
2. [Configure Push Notification Channels](doc:unreal-push#configure-push-notification-channels)

## Configure Firebase Cloud Messaging (FCM)

To use CleverTap's default notification implementation with Firebase:

1. Follow the [Android Push Setup Guide](https://developer.clevertap.com/docs/android-push) to register Firebase credentials on the CleverTap dashboard.
2. Copy `google-services.json` to your project directory (for example, `Config`).
3. In your project’s `Config/DefaultEngine.ini`, set `bAndroidIntegrateFirebase` to `True`, and specify `AndroidGoogleServicesJsonPath` as the relative path to the `google-services.json` file that you copied in the earlier step.

```text .ini
[/Script/CleverTap.CleverTapConfig]
bAndroidIntegrateFirebase=True
AndroidGoogleServicesJsonPath=Config/Android/google-services.json
```

## Configure Push Notification Channels

Android requires push notifications to be delivered through predefined channels. These channels allow users to control how notifications behave, such as setting the sound, vibration, or visibility preferences.

Since channels must be registered during Java's `GameApplication.onCreate()` (before Unreal Engine has initialized), they cannot be created dynamically in `C++`.

Therefore, all required channels must be predefined in `Config/DefaultEngine.ini`.

### Basic Setup

You can define up to 100 notification channels in your `DefaultEngine.ini` file. Each channel must include an ID field, which acts as the unique identifier for that channel. Android uses this ID to route and manage push notifications for the respective channel configuration.

```ini .ini
[/Script/CleverTap.CleverTapConfig]
AndroidNotificationChannelSlot1=ID="general" | Name="General" | Description="General Notifications" | Importance=IMPORTANCE_DEFAULT | bShowBadge=True
AndroidNotificationChannelSlot2=ID="news" | Name="News Updates" | Description="Important news and alerts" | Importance=IMPORTANCE_HIGH | bShowBadge=True | Sound="news_alert.wav"
```

You can assign an `Importance` level to each channel to control how prominently notifications appear. Valid values include:  
`IMPORTANCE_NONE`, `IMPORTANCE_MIN`, `IMPORTANCE_LOW`, `IMPORTANCE_DEFAULT`, `IMPORTANCE_HIGH`, and `IMPORTANCE_MAX`.

The sound files must be added to the APK's `res/raw` directory. Use `AndroidSoundsDir` to configure the path.

> 📘 Note
> 
> - The `CleverTap_Android_UPL.xml` reads the `AndroidNotificationChannelSlot` entries to inject the required Java code into `GameApplication.onCreate()`. If the `ini` syntax contains errors, the build will fail during compilation of `GameApplication.java`.
> - You must configure `AndroidNotificationChannelSlot` settings directly in your project’s `Config/DefaultEngine.ini` file. These settings are not exposed in the Project Settings GUI.

### Channel Localization

You can set a notification channel’s display name and description at runtime using Unreal's localization system. If you localize the channel dynamically, you do not need to include `Name` or `Description` in the `.ini` configuration.

##### Minimal `.ini` Setup

Define the notification channel with only the essential parameters in your `DefaultEngine.ini file`:

```ini .ini
AndroidNotificationChannelSlot1=ID="general" | Importance=IMPORTANCE_DEFAULT | bShowBadge=True
```

##### Runtime Localization in `C++`

You can localize the channel name and description at runtime using the following API:

```cplusplus
CleverTapSys = GEngine->GetEngineSubsystem<UCleverTapSubsystem>();
ICleverTapInstance& CleverTap = CleverTapSys->SharedInstance();
CleverTap.LocalizeAndroidNotificationChannel(TEXT("general"),
        NSLOCTEXT("CleverTapSample", "ChannelName_general", "General"),
        NSLOCTEXT("CleverTapSample", "ChannelDesc_general", "General Notifications"));
```

### Channel Groups

To organize related channels into categories, you can define up to 10 notification channel groups. Each group is declared in your `.ini` file, and channels can reference it using Group=`"group_id."`

##### `.ini` Configuration Example

```ini .ini
[/Script/CleverTap.CleverTapConfig]
AndroidNotificationChannelGroupSlot1=ID="general" | Name="General"
AndroidNotificationChannelSlot1=ID="general" | Group="general" | Importance=IMPORTANCE_DEFAULT | bShowBadge=True
```

Group names can be localized at runtime using Unreal's localization system. Call `LocalizeAndroidNotificationChannelGroup()` during app initialization (after setting up the CleverTap subsystem) and again if the app detects a locale change.

```cplusplus
CleverTapSys = GEngine->GetEngineSubsystem<UCleverTapSubsystem>();
ICleverTapInstance& CleverTap = CleverTapSys->SharedInstance();

CleverTap.LocalizeAndroidNotificationChannelGroup(
    TEXT("general"), 
    NSLOCTEXT("CleverTapSample", "ChannelGroupName_general", "General")
); // Using NSLOCTEXT() enables translation of group names using Unreal's localization pipeline.
```

### Default Android Notification Channel

You can define a _default notification channel_ that CleverTap will use when the push payload refers to a channel that is _not registered_ in your app. This ensures that push notifications are delivered consistently, even if specific channels are missing.

If the SDK cannot find the default channel ID specified in the manifest, it will automatically fall back to using a built-in channel called `Miscellaneous`.

#### To specify the app’s preferred default channel:

```ini .ini
[/Script/CleverTap.CleverTapConfig]
AndroidDefaultNotificationChannel=general
```

> 📘 Note
> 
> Make sure the channel ID you specify (for example, `general`) defined in the `AndroidNotificationChannelSlot` entries in your `DefaultEngine.ini`.

### Android Small Notification Icon

By default, the CleverTap SDK uses your app’s icon for both the notification and the status bar icons. However, starting with Android 5.0 (Lollipop), non-alpha (non-transparent) icons are ignored when rendering the small notification icon.

To customize the icon, you can provide a project-relative path to an alpha-only `.png` file:

```ini .ini
[/Script/CleverTap.CleverTapConfig]
AndroidSmallNotificationIconPath=Config/Android/sample_small_notification_icon.png
```

> 📘 Note
> 
> The base filename must contain only lowercase letters (`a–z`), digits (`0–9`), or underscores (`_`). Avoid capital letters, dashes, or spaces.

For more information about image requirements, refer to the [Android Push Notification- Set the Small Notification Icon](<>).

### Android Sound and Image Resources

You can include additional images and custom sound files in your APK for direct use by CleverTap on Android.

These resources are not part of Unreal's asset system. Instead, they are copied directly into the APK and must follow Android's resource naming conventions:

- File names must use only lowercase letters `a–z`, digits `0–9`, or underscores `_`.
- Avoid using capital letters, spaces, or special characters.

Android supports the following audio formats for custom sounds:

- `.mp3`
- `.ogg`
- `.wav`

#### Define Resource Directories

Set the directories in your `DefaultEngine.ini`:

```ini .ini
[/Script/CleverTap.CleverTapConfig]
AndroidImagesDir=Config/Android/Images
AndroidSoundsDir=Config/Android/Sounds
```

### Custom Android Notification Handling

Due to Android's limitation of allowing only one `FirebaseMessagingService` per app, the CleverTap SDK cannot coexist cleanly with other Unreal plugins that declare their own FCM service (for example, Unreal Firebase plugin).

If your project uses another Firebase plugin and requires CleverTap push notifications, you must:

1. Disable CleverTap's Firebase integration:

```ini
bAndroidIntegrateFirebase=False
```

2. Create a custom multiplexer in `Java` that forwards CleverTap messages manually.

#### Example: Unified FCM Handler

```java
public class UnifiedMessagingService extends SomeOtherPluginMessagingService {
    @Override
    public void onMessageReceived(RemoteMessage message) {
        if (isCleverTapMessage(message)) {
            new CTFcmMessageHandler().createNotification(getApplicationContext(), message);
        } else {
            // Let the base class handle it
            super.onMessageReceived(message);
        }
    }

    private boolean isCleverTapMessage(RemoteMessage message) {
        Map<String, String> data = message.getData();
        return data != null && data.containsKey("wzrk_pn"); // CleverTap magic key
    }
}
```

This approach ensures that CleverTap and other Firebase plugins can handle push messages without conflict.

For more information, refer to the [Custom Android Push Notification Handling](doc:android-push#custom-android-push-notification-handling).

### Android OpenUrl Configuration

To handle incoming app links such as deep links, enable `bIntegrateOpenUrl` and define up to _four slot-based URL filters_ in your project’s `.ini` file.

#### Example Configuration

```ini .ini
[/Script/CleverTap.CleverTapConfig]
bIntegrateOpenUrl=True

; Slot 1 - catches clevertap-unreal-sample://
DeepLinkSchemeFilterSlot1=clevertap-unreal-sample 

; Slot 2 - catches clevertap://unreal.com/sample
DeepLinkSchemeFilterSlot2=clevertap
AndroidIntentFilterSlot2_Host=unreal.com
AndroidIntentFilterSlot2_PathPrefix=/sample

; Slot 3 - catches http://clevertap.com/unreal-sample (requires digital verification)
DeepLinkSchemeFilterSlot3=http
AndroidIntentFilterSlot3_Host=clevertap.com
AndroidIntentFilterSlot3_PathPrefix=/unreal-sample
AndroidIntentFilterSlot3_AutoVerify=True
```

> 📘 Note
> 
> The `http` and `https` schemes require _digital verification_ using an `assetlinks.json` file hosted on the target domain. For more information, refer to the [Verify Android App Links](https://developer.android.com/training/app-links/verify-android-applinks).

#### Test Deep Link with `adb`

You can simulate a deep link click on a connected Android device using the following command:

```shell
adb shell am start -a android.intent.action.VIEW -d "clevertap-unreal-sample://test/path"
```

#### Custom Deep Link Handling

If you need full control over how your app handles deep links:

- Disable the built-in `OpenUrlActivity` integration.
- Define your activity and intent filters via _custom UPL rules_.
- If your custom activity forwards the intent to `GameActivity`, the event will still reach the `OnOpenURL` delegate in Unreal.

# iOS Setup

CleverTap's Unreal SDK provides robust support for handling push notifications and deep links on iOS. This section walks you through configuring URL handling, enabling Apple Push Notification service (APNs), applying optional engine patches, and handling push interactions.

## iOS - Configuring Apple Push Notifications (APNs)

To enable push notification support on iOS using CleverTap, you must first configure APNs and ensure the correct Unreal Engine settings are in place.

1. Follow the [CleverTap iOS Push Guide](doc:push-notifications-ios#step-1-configure-push-notifications) to configure Apple Push Notification service (APNs).
2. In your project’s `Config/DefaultEngine.ini`, check that the following setting is enabled:

```ini .ini
[/Script/IOSRuntimeSettings.IOSRuntimeSettings]
bEnableRemoteNotificationsSupport=True
```

> 📘 Note
> 
> The CleverTap Unreal plugin does not support the following iOS features:
> 
> - [Push Impressions](https://developer.clevertap.com/docs/push-notifications-ios#push-impressions)
> - [Push Primers](doc:push-notifications-ios#ios-push-primer)
> - [Rich Push Notifications](doc:rich-push-notifications)

## iOS - Optional Engine Changes for Push Notifications

Push notification features on iOS, such as handling when the app is closed or supporting rich media, require optional engine-level patches to Unreal Engine.

#### Install Patch Tool (Windows Only)

On Windows, install the patch utility using:

```bash bash
winget install GnuWin32.Patch
```

#### Patch: Push Notification Callback When App is Closed

Unreal Engine does not forward push notifications to CleverTap when the app is fully closed (not in foreground/background). To fix this, apply the engine patch to `IOSAppDelegate.cpp`:

```bash bash
UnrealEngine % patch -p1 -u -i /path/to/CleverTapSample/EnginePatches/iOSSavedRemoteNotifications.patch
```

#### Patch: Rich Push Notification Support

To support [Rich Push Notifications](doc:rich-push-notifications) on iOS, CleverTap provides a [Notification Service Extension](https://developer.apple.com/reference/usernotifications/unnotificationserviceextension). However, Unreal Engine does not natively support iOS app extensions.

The `EnginePatches/UE4.27_ExtensionSupport.patch`  file provides an example patch that adds this support. This patch modifies Unreal Engine's build process to include app extensions and must be applied from the Unreal Engine root directory using the Linux patch command.

##### Apply the Engine Patch

```bash bash
UnrealEngine % patch -p1 -u -i /path/to/CleverTapSample/EnginePatches/UE4.27_ExtensionSupport.patch
```

Once patched, the extension located at [Plugins/CleverTap/Source/ThirdParty/IOS/Extensions/CTNotificationService](https://github.com/CleverTap/clevertap-unreal-sdk/tree/dev/Plugins/CleverTap/Source/ThirdParty/IOS/Extensions/CTNotificationService) will be included in the build. A custom signing provision can be specified for the extension via changes to the `Config/DefaultEngine.ini` file.

```ini .ini
[/Script/IOSRuntimeSettings.IOSRuntimeSettings]
MobileProvision_CTNotificationService=YourProvisioning.mobileprovision
```

Check that the specified `.mobileprovision` file is installed before building.

> 📘 Note
> 
> This engine patch adds support for app extensions located in:
> 
> - `{ProjectFolder}/Build/IOS/Extensions`
> - `{ProjectFolder}/Plugins/{PluginName}/Source/ThirdParty/IOS/Extensions`
> 
> Currently, this integration only supports _Unreal Engine 4.27_ and has been tested exclusively with the `CTNotificationService` extension provided by CleverTap.

### Enabling `OnPushNotificationClicked`

After setting up your shared CleverTap instance, enable the `OnPushNotificationClicked` delegate by calling:

```cplusplus
EnableOnPushNotificationClicked();
```

Make sure to register your delegate listener before invoking this method.

On iOS, if the engine patch has been applied for push notifications that launch the app, this can trigger attempting to broadcast the push notification that launched the app.

```cplusplus
CleverTapSys = GEngine->GetEngineSubsystem<UCleverTapSubsystem>();
ICleverTapInstance& CleverTap = CleverTapSys->SharedInstance();

CleverTap.OnPushNotificationClicked.AddLambda([](const FCleverTapProperties& NotificationPayload)
{
    UE_LOG(LogTemp, Log, TEXT("Push notification was tapped"));
});

CleverTap.EnableOnPushNotificationClicked();
```

This delegate is triggered when a user taps on a push notification, allowing you to dynamically handle deep links, In-App events, or other logic.

### 🔗 iOS OpenUrl Configuration

To handle incoming app links on iOS, enable `bIntegrateOpenUrl` and define up to _four slot-based URL scheme filters_ in your `.ini` file.

#### Example Configuration

```ini .ini
[/Script/CleverTap.CleverTapConfig]
bIntegrateOpenUrl=True

; Slot 1 - catches clevertap-unreal-sample://
DeepLinkSchemeFilterSlot1=clevertap-unreal-sample 

; Slot 2 - catches clevertap://
DeepLinkSchemeFilterSlot2=clevertap

; Slot 3 - catches http://
DeepLinkSchemeFilterSlot3=http
```

To further filter incoming URLs or handle them manually, use the `RegisterCleverTapUrlHandler()` method to register a delegate.