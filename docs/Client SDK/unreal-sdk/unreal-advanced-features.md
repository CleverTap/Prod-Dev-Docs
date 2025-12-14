---
title: Unreal Advanced Features
excerpt: Learn how to configure advanced settings for the CleverTap Unreal SDK.
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

The Advanced Setup for Unreal SDK guide is intended for developers who need deeper control over how the CleverTap Unreal SDK behaves across Android and iOS. This guide focuses on advanced configuration topics such as encryption, notification customization, deep link handling, and platform-specific engine patches.

For initial setup instructions, plugin installation, and core usage, refer to the [Unreal Quick Start Guide](doc:unreal-quick-start-guide) or the [CleverTap Unreal SDK GitHub README](https://github.com/CleverTap/clevertap-unreal-sdk).

This guide is intended for you if:

- You use multiple third-party plugins that interfere with CleverTap’s default behavior.
- You require custom Firebase or APNs setups.
- You want to localize or override CleverTap resources for push or In-App messaging.

# Advanced Features

This section provides detailed instructions for configuring advanced CleverTap SDK behaviors. These include security settings, deep linking, push notification handling, and SDK customization options for Android and iOS.

The following features are covered:

1. [Encrypt PII data](doc:unreal-advanced-features#encrypt-pii-data)
2. [Handle Android Push Notification](doc:unreal-advanced-features#handle-android-push-notification)
3. [Set Up Deep Linking (Android and iOS)](doc:unreal-advanced-features#set-up-deep-linking-android-and-ios)
4. [Configure iOS push notifications](doc:unreal-advanced-features#ios-push-notifications-and-patches)
5. [Track Push Notification Clicks (Android and iOS)](doc:unreal-advanced-features#track-push-notification-clicks-android-and-ios)
6. [Customize Android Notifications](doc:unreal-advanced-features#customize-android-notifications)

## Encrypt PII data

You can control how the SDK stores Personally Identifiable Information (PII) using the EncryptionLevel configuration.

Available options:

- `None`: Stores PII as plain text (default).
- `Medium`: Encrypts PII at rest.

Set this in your Unreal config:

```text ini
[/Script/CleverTap.CleverTapConfig]
EncryptionLevel=Medium
```

To override at the instance level:

```cplusplus
FCleverTapInstanceConfig.EncryptionLevel = ECleverTapEncryptionLevel::Medium;
```

## Handle Android Push Notification

If you use multiple plugins that implement Firebase services (like OneSignal, GameAnalytics, and so on), they can conflict with CleverTap’s FCM registration. To avoid this:

1. Disable CleverTap’s default Firebase integration:

```ini ini
[/Script/CleverTap.CleverTapConfig]
bAndroidIntegrateFirebase=False
```

2. Implement a custom multiplexer service in Java:

This service ensures only CleverTap messages are passed to the CleverTap handler, while others are routed to their respective plugin handlers.

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
        return data != null && data.containsKey("wzrk_pn");
    }
}
```

## Set Up Deep Linking (Android and iOS)

Configure deep linking to ensure CleverTap can handle incoming URLs and route them correctly on both Android and iOS. This is essential for driving in-app navigation from push notifications, emails, or other external sources.

- [Android Deep Linking](doc:unreal-advanced-features#android-deep-linking)
- [iOS Deep Linking](doc:unreal-advanced-features#ios-deep-linking)

### Android Deep Linking

Enable CleverTap to handle incoming app links using intent filters and scheme/host/path matching:

```ini ini
[/Script/CleverTap.CleverTapConfig]
bIntegrateOpenUrl=True
DeepLinkSchemeFilterSlot1=clevertap-unreal-sample
DeepLinkSchemeFilterSlot2=clevertap
AndroidIntentFilterSlot2_Host=unreal.com
AndroidIntentFilterSlot2_PathPrefix=/sample
```

This setup ensures:

- CleverTap messages are routed to their handler.
- Other messages are handled by the plugin you extended.
- Conflicts are avoided when multiple plugins register Firebase services.

#### Simulate with ADB

Run the following command to test deep link handling on an Android device or emulator.

```bash bash
adb shell am start -a android.intent.action.VIEW -d "clevertap-unreal-sample://test/path"
```

#### Example: HTTP/HTTPS Link Handling with AutoVerify

This configuration enables verified HTTP/HTTPS app links that Android automatically validates.

```ini ini
DeepLinkSchemeFilterSlot3=http
AndroidIntentFilterSlot3_Host=clevertap.com
AndroidIntentFilterSlot3_PathPrefix=/unreal-sample
AndroidIntentFilterSlot3_AutoVerify=True
```

> 🚧 Digital Link Verification Required
> 
> HTTP/HTTPS schemes require digital verification via `assetlinks.json` hosted on the target domain.  
> For more information, refer to [Verify Android App Links](https://developer.android.com/training/app-links/verify-android-applinks).

### iOS Deep Linking

Support app link handling using scheme-based filters for iOS targets.

```ini ini
[/Script/CleverTap.CleverTapConfig]
bIntegrateOpenUrl=True
DeepLinkSchemeFilterSlot1=clevertap-unreal-sample
DeepLinkSchemeFilterSlot2=clevertap
```

Use `RegisterCleverTapUrlHandler()` for custom filtering.

## iOS Push Notifications and Patches

Configure APNs support and foreground delivery behavior for iOS devices.

> 📘 APNs Registration
> 
> Before configuring push notifications in Unreal, follow the [CleverTap APNs setup guide](https://developer.clevertap.com/docs/ios-push-notifications) to register your app with Apple Push Notification service (APNs).

```ini ini
[/Script/IOSRuntimeSettings.IOSRuntimeSettings]
bEnableRemoteNotificationsSupport=True

[/Script/CleverTap.CleverTapConfig]
bIOSPresentPushNotificationsInForeground=True
```

### Engine Patches (iOS)

To support background push delivery and rich media, apply the following engine patches.

#### Push Notification Callback Patch

This patch allows push messages to be delivered even when the application is closed:

```bash ini
patch -p1 -u -i /path/to/iOSSavedRemoteNotifications.patch
```

> 📘 Patch File Path Instructions
> 
> Replace /path/to/ with the actual path to your patch file.

#### Rich Push Support Patch

While the SDK doesn’t support rich push notifications, this patch adds limited support for Unreal Engine 4.27.

```bash bash
patch -p1 -u -i /path/to/UE4.27_ExtensionSupport.patch
```

Then configure the provisioning profile:

```ini ini
[/Script/IOSRuntimeSettings.IOSRuntimeSettings]
MobileProvision_CTNotificationService=YourProvisioning.mobileprovision
```

This is a workaround for iOS restrictions and works only on UE 4.27.

## Track Push Notification Clicks (Android and iOS)

Capture user interactions with push notifications by using the `OnPushNotificationClicked` event provided by the CleverTap Unreal SDK. This event works on Android and iOS, allowing you to respond when a user taps a notification, even when the app launches from a terminated state (cold start).

Register the listener before enabling the event to ensure proper handling of cold-start launches.

```cpp cpp
CleverTap.OnPushNotificationClicked.AddLambda([](const FCleverTapProperties& Payload) {
    UE_LOG(LogTemp, Log, TEXT("Push clicked"));
});
CleverTap.EnableOnPushNotificationClicked();
```

## Customize Android Notifications

Customize how push notifications appear on Android devices, including icons, sounds, images, channels, and localization options.

```ini ini
AndroidSmallNotificationIconPath=Config/Android/sample_small_notification_icon.png
AndroidImagesDir=Config/Android/Images
AndroidSoundsDir=Config/Android/Sounds
```

> 📘 File Naming Rules
> 
> Filenames must follow Android naming conventions: lowercase, digits, underscores only.

### Android Notification Channels and Groups

Define multiple notification channels and groups to categorize messages.

#### Default Notification Channel

Specify the default notification channel to use if the channel provided in the push payload is not registered in your application. If the SDK cannot find the specified default channel ID, it will automatically fall back to using a channel named `Miscellaneous`.

```ini ini
AndroidDefaultNotificationChannel=general
```

#### Define Channels

Define the notification channels that determine how notifications are grouped and displayed on the device.

```ini ini
AndroidNotificationChannelSlot1=ID="general" | Name="General" | Importance=IMPORTANCE_DEFAULT
```

#### Define Groups

Organize related notification channels into groups to simplify notification management.

```ini ini
AndroidNotificationChannelGroupSlot1=ID="general" | Name="General"
AndroidNotificationChannelSlot1=ID="general" | Group="general"
```

#### Localizing Channel and Group Names

You can set the display name and description of channels and groups at runtime using Unreal's localization system.

```cplusplus C++ Example: Channel Localization
CleverTapSys = GEngine->GetEngineSubsystem<UCleverTapSubsystem>();
ICleverTapInstance& CleverTap = CleverTapSys->SharedInstance();
CleverTap.LocalizeAndroidNotificationChannel(
    TEXT("general"),
    NSLOCTEXT("CleverTapSample", "ChannelName_general", "General"),
    NSLOCTEXT("CleverTapSample", "ChannelDesc_general", "General Notifications")
);
```
```cplusplus C++ Example: Group Localization
CleverTapSys = GEngine->GetEngineSubsystem<UCleverTapSubsystem>();
ICleverTapInstance& CleverTap = CleverTapSys->SharedInstance();
CleverTap.LocalizeAndroidNotificationChannelGroup(
    TEXT("general"),
    NSLOCTEXT("CleverTapSample", "ChannelGroupName_general", "General")
);
```

By implementing these advanced setup steps, you can fully customize the CleverTap Unreal SDK for complex project requirements while ensuring compatibility across Android and iOS platforms.