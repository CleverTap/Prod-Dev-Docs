---
title: Cordova In-App
excerpt: ''
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

## In-App Notification Button `onClick` callback

To handle In-App Notification Button `onClick` callback, add the following code snippet:

```javascript JavaScript
document.addEventListener('onCleverTapInAppButtonClick', e => {
            log("onCleverTapInAppButtonClick")
            log(e.customExtras)
        }
    )
```

## In-App Notification `onDismissed` callback

To handle `onDismissed` callback:

```javascript JavaScript
document.addEventListener('onCleverTapInAppNotificationDismissed', e => {
            log("onCleverTapInAppNotificationDismissed")
        }
    )
```

## Control In-App Notifications

### Suspend

Suspends and saves in-app notifications until `resumeInAppNotifications` method is called for the current session.

```javascript JavaScript
CleverTap.suspendInAppNotifications()
```

### Discard

Suspends the display of in-app notifications and discards the display of any new in-app notification. It also discards in-app notifications until `resumeInAppNotifications` method is called for the current session.

```javascript JavaScript
CleverTap.discardInAppNotifications()
```

### Resume

The `resumeInAppNotifications` method resumes displaying in-app notifications. If you call this method after the `discardInAppNotifications()` method, it resumes the in-app notifications for events raised after the call is performed.

However, if you call the `resumeInAppNotifications` method after the `suspendInAppNotifications()` method, then it displays all queued in-app notifications and also resumes in-app notifications for events raised after the call is performed.

```javascript JavaScript
CleverTap.resumeInAppNotifications()
```