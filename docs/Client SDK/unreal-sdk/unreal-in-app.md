---
title: Unreal In-App
excerpt: Learn how to handle In-App notifications in Unreal SDK.
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

In-App notifications are contextual messages that are shown while users are actively using your app. They are perfect for:

* Promoting offers
* Nudging user actions
* Delivering real-time updates without disrupting the experience

CleverTap Unreal SDK fully supports In-App notifications, with [In-App Notification Control](doc:unreal-in-app#in-app-notification-callback) and [In-App Notification Callbacks](doc:unreal-in-app#in-app-notification-controls) for display, interaction, and dismissal.

# In-App Notification Setup

In-App notifications are suspended by default when the app starts. To start displaying them, first register your In-App callbacks (for display, clicks, and dismiss events). Then explicitly resume In-App delivery by calling:

```cplusplus
ICleverTapInstance::ResumeInAppNotifications();
```

This ensures your app is ready to handle user interactions and avoids showing notifications before the handlers are set.

### Payload Structure

All In-App notification callbacks return a flattened key-value map, where nested `JSON` objects are represented using dot-separated keys. For example:

```json
{
  "a": {
    "b": 3.14
  }
}
```

Is flattened into:

```cplusplus
Payload[FString{TEXT("a.b")}] == FCleverTapPropertyValue{3.14};
```

> 📘 Note
>
> This structure simplifies accessing deeply nested parameters within your notification payload.

# In-App Notification Callback

Use the following callbacks to track user interaction and dismissal behavior for In-App messages.

## OnInAppNotificationShown

Triggered when an In-App notification is shown.

```cplusplus
CleverTapSys = GEngine->GetEngineSubsystem<UCleverTapSubsystem>();
ICleverTapInstance& CleverTap = CleverTapSys->SharedInstance();

CleverTap.OnInAppNotificationShown.AddLambda([](const FCleverTapProperties& Payload)
{
    UE_LOG(LogTemp, Log, TEXT("An In-App notification was shown: %s"), *ToDebugString(Payload));
});
```

## OnInAppNotificationButtonClicked

Triggered when the user taps a button within the in-app message.

```cplusplus
CleverTapSys = GEngine->GetEngineSubsystem<UCleverTapSubsystem>();
ICleverTapInstance& CleverTap = CleverTapSys->SharedInstance();

CleverTap.OnInAppNotificationButtonClicked.AddLambda([](const FCleverTapProperties& Payload)
{
    UE_LOG(LogTemp, Log, TEXT("In-app notification button clicked: %s"), *ToDebugString(Payload));
});
```

## OnInAppNotificationDismissed

Triggered when an In-App message is dismissed. Includes both extra and action-specific key-value pairs.

```cplusplus
CleverTapSys = GEngine->GetEngineSubsystem<UCleverTapSubsystem>();
ICleverTapInstance& CleverTap = CleverTapSys->SharedInstance();

CleverTap.OnInAppNotificationDismissed.AddLambda([](const FCleverTapProperties& Extras, const FCleverTapProperties& ActionExtras)
{
    UE_LOG(LogTemp, Log, TEXT("In-app notification dismissed. Extras: %s, ActionExtras: %s"),
        *ToDebugString(Extras), *ToDebugString(ActionExtras));
});
```

# In-App Notification Controls

In-app notifications are *suspended by default* when the app is initialized. You can control their behavior using the following methods:

## Discard Queued Notifications

To discard notifications queued during the suspended state, call:

```cplusplus
ICleverTapInstance::DiscardInAppNotifications();
```

## Resume Notifications

To resume displaying In-App notifications, use:

```cplusplus
ICleverTapInstance::ResumeInAppNotifications();
```

Notifications queued while suspended (and not explicitly discarded) will be shown to the user.

## Suspend Notifications Again

To suspend In-App notifications at any point, call:

```cplusplus
ICleverTapInstance::SuspendInAppNotifications();
```

> 📘 Minimize User Disruption
>
> Suspending notifications is useful in scenarios such as onboarding flows or payment screens, where interruptions should be minimized.
