---
title: Unity In-App Notifications
excerpt: Learn how to handle In-App notifications in Unity.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# In-App Notifications

In-App notifications are contextual pop-ups shown while users are actively using your app. They are ideal for promoting offers, nudging user actions, or delivering real-time updates without disrupting the user experience.

This section outlines the key implementation areas to help you integrate and control CleverTap In-App notifications effectively:

* [In-App Notification Callback](doc:unity-inapp#in-app-notification-callback)
* [In-App Notification Controls](doc:unity-inapp#in-app-notification-controls)
* [Header and Footer for Android](doc:unity-inapp#header-and-footer-in-apps-for-android)

## In-App Notification Callback

Use the following callbacks to track user interaction and dismissal behavior for In-App messages.

### In-App Notification Button `onClick` callback

To handle an In-App button click, add the following code snippet:

```csharp C#
CleverTap.OnCleverTapInAppNotificationButtonTapped += CleverTapInAppNotificationButtonTapped;

void CleverTapInAppNotificationButtonTapped(string message)
{
    Debug.Log("unity received inapp notification button clicked: " + (!String.IsNullOrEmpty(message) ? message : "NULL"));
}
```

### In-App Notification `onDismissed` callback

To handle the dismissed callback, use the following code:

```csharp C#
CleverTap.OnCleverTapInAppNotificationDismissedCallback += CleverTapInAppNotificationDismissedCallback;

void CleverTapInAppNotificationDismissedCallback(string message)
{
    Debug.Log("unity received inapp notification dismissed: " + (!String.IsNullOrEmpty(message) ? message : "NULL"));
}
```

### In-App Notification`OnCleverTapInAppNotificationShow`callback

Fires each time an in-app notification is displayed to the user.

```csharp
csharp
// Subscribe to the callback
CleverTap.OnCleverTapInAppNotificationShowCallback += CleverTapInAppNotificationShowCallback;

// Handler
private void CleverTapInAppNotificationShowCallback(string message)
{
  Debug.Log($"InAppNotification Show callback: {message}");
}
```

* Parameters: It contains a key-value pair (Message-JSON string).  The JSON string contains the in-app notification payload (campaign ID, type, custom key-value extras, and so on.)
* Platform support: Android and iOS only.  In-App notifications not supported on Desktop or WebGL platforms.

<Callout icon="📘" theme="info">
  Note

  Check that you subscribe to the `OnCleverTapInAppNotificationShow` callback before the `LaunchWithCredentials SDK` initialization call to avoid missing the first show event.
</Callout>

## In-App Notification Controls

Control how and when In-App notifications appear in your app.

### Suspend In-App Notifications

Pause In-App notifications temporarily. Use this to stop notifications during specific app states.

```csharp C#
CleverTap.SuspendInAppNotifications();
```

### Discard In-App Notifications

Remove all pending In-App notifications permanently. Use this to clear irrelevant or outdated notifications.

```csharp C#
CleverTap.DiscardInAppNotifications();
```

### Resume In-App Notifications

Resume paused In-App notifications. Use this to show notifications again when appropriate.

```csharp C#
CleverTap.ResumeInAppNotifications();
```

## Header and Footer In-Apps for Android

Unity generates Android apps using the`UnityPlayerActivity`, which extends the Android Activity class. However, CleverTap’s header and footer In-App notifications require the activity to extend `FragmentActivity`. This limitation prevents these In-App notifications from working by default in Unity-based Android apps.

To support these notifications, you can export your Unity project and modify the base activity to extend `FragmentActivity`.

### Enable Header/Footer In-App Notifications

To enable header and footer In-Apps in your Unity Android app, follow these steps:

1. In the Unity Editor, go to _File_ >_Build Settings_, select the Android platform, and check the Export Project option. Click **Export**.

<Image align="center" alt="Exporting Unity project for Android Studio" border={true} caption="Exporting Unity project for Android Studio" src="https://files.readme.io/f6e5759fd7b42d99f0756d71ac47da3f970bb178f1a2b1050bea83bcc29e031e-image.png" width="75% " />

2. Open the exported project in Android Studio and locate the `UnityPlayerActivity.java` file.
3. Update the class to extend `FragmentActivity` instead of `Activity`, and add the required import:

```coffeescript Java
// Make sure the correct import is also added
import androidx.fragment.app.FragmentActivity;

public class UnityPlayerActivity extends FragmentActivity {
    // existing code
}
```

4. Build and run the project from Android Studio.

The header and footer In-App notifications should now appear as expected.
