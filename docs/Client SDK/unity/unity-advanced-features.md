---
title: Unity Advanced Features
excerpt: >-
  Learn how to enable debugging, personalization, GDPR controls, and native
  display units using CleverTap’s Unity SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Advanced Features

Access advanced configuration options in the CleverTap Unity SDK to fine-tune logging, event tracking, personalization, attribution, and privacy settings.

This document covers Unity SDK features that help enhance:

- [Debugging](doc:unity-advanced-features#debugging): Diagnose SDK activity during development.
- [Native Display](doc:unity-advanced-features#native-display): Track and render In-App banners or cards.
- [App Personalization](doc:unity-advanced-features#app-personalization): Deliver tailored experiences to users.
- [Install Attribution](doc:unity-advanced-features#install-attribution): Measure campaign effectiveness using UTM data.
- [GDPR Controls](doc:unity-advanced-features#gdpr-controls): Manage data privacy and opt-out preferences.

## Debugging

Enable CleverTap’s DEBUG mode during development to log SDK activity and troubleshoot integration issues. You can control this by setting the debug level as shown below.

### Set Debug Level

Set the debug level to control the verbosity of CleverTap logs during development:

- `-1`: Disables all debugging. You can set the `debugLevel` to -1 if you want to disable CleverTap logs for the production environment.
- `0`: Default, shows minimal SDK integration related logging.
- `2`: Shows debug output.
- `3`: Shows verbose output.

```csharp
CleverTap.SetDebugLevel(debugLevel);
```

> 🚧 Platform Specific Logs
> 
> To view SDK logs when the app is in the killed state, refer to:
> 
> - For Android platform, refer to [Android Debugging](doc:android-advanced-features#debugging).
> - For iOS platform, refer to [iOS Debugging](doc:advanced-options-ios#debugging).

## Native Display

Native Display Units are customizable In-App banners or cards triggered by user behavior or campaign logic.

### On Display Units Updated

Registers a callback to listen for when Native Display Units are updated from CleverTap.

```csharp
CleverTap.OnCleverTapNativeDisplayUnitsUpdated += CleverTapNativeDisplayUnitsUpdated;

void CleverTapNativeDisplayUnitsUpdated(string message)
{
    // 1. Parse the "message" string received from CleverTap into a JSON array of display units.
    // 2. Create a JSON object with a key "displayUnits" and assign the parsed array.
}
```

### Get All Display Units

Fetches all available Native Display Units currently loaded in the app session.

```csharp
CleverTap.GetAllDisplayUnits();
```

### Display Unit Viewed Event for ID

Tracks when a specific Native Display Unit is viewed by the user.

```csharp
CleverTap.RecordDisplayUnitViewedEventForID('Display Unit Id');
```

### Display Unit Clicked Event for ID

Tracks when a specific Native Display Unit is clicked by the user. Use this method to track engagement if your app uses a custom UI to render display units.

```csharp
CleverTap.RecordDisplayUnitClickedEventForID('Display Unit Id');
```

## App Personalization

Enable or disable user-level personalization based on CleverTap recommendations and segmentation.

### Enable Personalization

Activates CleverTap’s personalization engine to tailor content and recommendations for the user.

```csharp
CleverTap.EnablePersonalization();
```

### Disable Personalization

Turns off personalized recommendations and message targeting for the current user.

```csharp
CleverTap.DisablePersonalization();
```

> 📘 Note
> 
> Toggle this setting after login, logout, or consent updates.

## Install Attribution

Track install attribution using UTM parameters to measure the effectiveness of acquisition campaigns.

### Push Install Referrer

Pass campaign parameters (source, medium, campaign) to attribute installs accurately.

```csharp
CleverTap.PushInstallReferrerSource("source", "medium", "campaign");
// Call this after capturing campaign parameters using attribution SDKs like InstallReferrerClient for Android.
```

## GDPR Controls

Manage user opt-in and opt-out preferences and control device-level tracking to ensure compliance with GDPR privacy regulations.

### Set Opt Out

To help you comply with GDPR and user privacy requirements, CleverTap offers a method to control whether a user’s data is tracked.

#### Opt Out from All Tracking and Communication

To completely disable event tracking and stop sending any communication (personalized or generic) to the user.

```csharp
CleverTap.SetOptOut(true); // Will opt out the user to send data to CleverTap
```

#### Opt Out from Tracking Only, but Allow Generic Communication

To disable event tracking, but still allow you to send non-personalized messages (such as broadcast announcements) to the user.

```csharp
CleverTap.SetOptOut(true, true); // Only stop tracking, allow generic communication
```

#### Opt In to Tracking and Communication

To re-enable event tracking and allow sending personalized messages to the user, assuming prior consent was collected.

```csharp
CleverTap.SetOptOut(false); // Will opt in the user to send data to CleverTap
```

### Enable Device Networking Info Reporting

By default, in compliance with GDPR, the CleverTap SDK does not collect device-level information such as Wi-Fi, Bluetooth, network details, or IP address.

To turn this data collection on or off in your Unity, use the following method:

```csharp
// Turn OFF device network info collection
CleverTap.EnableDeviceNetworkInfoReporting(false);
// Turn ON device network info collection
CleverTap.EnableDeviceNetworkInfoReporting(true);
```

### Set Offline

CleverTap Unity SDK provides a method to set the user as offline. Setting the user offline means no events/profile information will be sent to CleverTap. You can set the user as offline/online using the following method:

```csharp
// Set user online (enable real-time data transmission)
CleverTap.SetOffline(false);
// Set user offline (pause data transmission)
CleverTap.SetOffline(true);
```