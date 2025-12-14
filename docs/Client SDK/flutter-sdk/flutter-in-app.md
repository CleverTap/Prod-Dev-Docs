---
title: Flutter In-App
excerpt: Learn how to handle In-App notifications in Flutter.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: link
      title: Flutter App Inbox
      url: https://developer.clevertap.com/docs/flutter-app-inbox
    - type: link
      title: Flutter Advanced Features
      url: https://developer.clevertap.com/docs/flutter-advanced-features
    - type: link
      title: Flutter Resources
      url: https://developer.clevertap.com/docs/flutter-resources
---
# In-App Notifications

The CleverTap SDK allows you to show In-App notifications to your users and send In-App messages with images, GIFs, video, and audio. You can design In-App notifications right from the dashboard without writing a single line of code. No code is required for this feature. You can also do A/B testing on your In-App notifications.

> 📘 Header and Footer In-App Notification Template for Android
> 
> To use CleverTap's Header and Footer In-App Notification Templates, update the `MainActivity` class to inherit the `FlutterFragmentActivity` class instead of `FlutterActivity`.
> 
> ```
> public class MainActivity extends FlutterFragmentActivity {  
> //FlutterFragmentActivity supports Header & Footer InApp Notification Templates  
> }
> ```

## In-App Notification Button `onClick` Callbacks

To handle the In-App Button `onClick` add the following code snippet:

```javascript Dart
_clevertapPlugin.setCleverTapInAppNotificationButtonClickedHandler(inAppNotificationButtonClicked);

void inAppNotificationButtonClicked(Map<String, dynamic> map) {
    this.setState(() {
      print("inAppNotificationButtonClicked called = ${map.toString()}");
    });
}
```

## In-App Notification `onDismissed` callback

Called after the In-App notification is dismissed through user interaction or auto-close.

```javascript Dart
_clevertapPlugin.setCleverTapInAppNotificationDismissedHandler(inAppNotificationDismissed)

void inAppNotificationDismissed(Map<String, dynamic> map) {
    this.setState(() {
      print("inAppNotificationDismissed called");
    });
}
```

## In-App Notification `onShow` callback

Called when an In-App notification is displayed to the user.

```javascript Dart
_clevertapPlugin.setCleverTapInAppNotificationShowHandler(inAppNotificationShow);

void inAppNotificationShow(Map<String, dynamic> map) {
    this.setState(() {
      print("inAppNotificationShow called = ${map.toString()}");
    });
}
```

# Control In-App Notifications

You can suspend, discard, or resume In-App notifications.

> 📘 Note
> 
> The CleverTap SDK automatically resumes displaying the In-App notification after every new session.

## Suspend

This method suspends and saves In-App notifications until `resumeInAppNotifications` method is called for the current session.

```javascript Dart
CleverTapPlugin.suspendInAppNotifications();
```

## Discard

This method suspends the display of In-App Notifications and discards any new In-App notifications.  
In-app notifications are discarded until the `resumeInAppNotifications` method is called for the current session.

```javascript Dart
CleverTapPlugin.discardInAppNotifications();
```

## Resume

The `resumeInAppNotifications` method resumes displaying In-App notifications. If you call this method after the `discardInAppNotifications()` method, it resumes the In-App notifications for events raised after the call is performed.

However, if you call the `resumeInAppNotifications` method after the `suspendInAppNotifications()` method, then it displays all queued In-App notifications and also resumes In-App notifications for events raised after the call is performed.

```javascript Dart
CleverTapPlugin.resumeInAppNotifications();
```

# Web Pop-Ups

CleverTap Flutter SDK v2.0.0 onwards supports Web Pop-Ups