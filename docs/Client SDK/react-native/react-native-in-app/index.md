---
title: React Native In-App
excerpt: Learn how to handle In-App in React Native.
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
      title: React Native App Inbox
      url: https://developer.clevertap.com/docs/react-native-app-inbox
    - type: link
      title: React Native Advanced Features
      url: https://developer.clevertap.com/docs/react-native-advanced-features
    - type: link
      title: React Native Resources
      url: https://developer.clevertap.com/docs/react-native-resources
---
# In-App Notifications

In-app notifications are pop-ups that you can show to your users while they are in your application.

## In-App Notification Button `onClick` callback

To handle the In-App Button `onClick` add the following code snippet:

```javascript JavaScript
CleverTap.addListener(CleverTap.CleverTapInAppNotificationButtonTapped, (event) => {
        _handleCleverTapEvent(CleverTap.CleverTapInAppNotificationButtonTapped, event);
    });

function _handleCleverTapEvent(eventName, event) {
    console.log('CleverTap Event called - ', eventName, event);
}
```

## In-App Notification `onDismissed` callback

To handle `onDismissed` callback

```javascript JavaScript
CleverTap.addListener(CleverTap.CleverTapInAppNotificationDismissed, (event) => {
        _handleCleverTapEvent(CleverTap.CleverTapInAppNotificationDismissed, event);
    });

function _handleCleverTapEvent(eventName, event) {
    console.log('CleverTap Event called - ', eventName, event);
}
```

## In-App Notification `onShow` callback

To handle `onShow` callback

```javascript JavaScript
CleverTap.addListener(CleverTap.CleverTapInAppNotificationShowed, (event) => {
        _handleCleverTapEvent(CleverTap.CleverTapInAppNotificationShowed, event);
    });

function _handleCleverTapEvent(eventName, event) {
    console.log('CleverTap Event called - ', eventName, event);
}
```

> 📘 Note
>
> `CleverTapInAppNotificationShowed` on iOS requires React Native SDK v4.0.0+. 

## Control In-App Notifications

### Suspend

Suspends and saves In-App notifications until `resumeInAppNotifications` method is called for the current session.

```javascript JavaScript
CleverTap.suspendInAppNotifications();
```

### Discard

Suspends the display of In-App Notifications and discards the display of any new In-App notification. It also discards In-App notifications until `resumeInAppNotifications` method is called for the current session.

```javascript JavaScript
CleverTap.discardInAppNotifications();
```

### Resume

The `resumeInAppNotifications` method resumes displaying In-App notifications.

If you call this method after the `discardInAppNotifications()` method, it resumes the In-App notifications for events raised after the call is performed.

However, if you call the `resumeInAppNotifications` method after the `suspendInAppNotifications()` method, then it displays all queued In-App notifications and also resumes In-App notifications for events raised after the call is performed.

```javascript JavaScript
CleverTap.resumeInAppNotifications();
```
