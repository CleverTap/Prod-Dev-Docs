---
title: KaiOS Push Notifications
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
#Register for Push Notifications

Call the following method to register for push notifications: clevertap.registerCTNotifications(arg1) 
  * arg1 = Clevertap ServiceWorker path (if not provided, /serviceWorker.js in the root directory will be considered)

This registers the service worker and updates the push token for the profile.

#Un-Register for Push Notifications

Call the following method to un-register the service worker: 
clevertap.unregisterCTNotifications(arg1) 
  * arg1 = Clevertap ServiceWorker path (if not provided, /serviceWorker.js in the root directory will be considered)

#Add the Service Worker File
Push notifications on KaiOS use a special script called a service worker that listens for pushes. Since the execution of these scripts is managed by the app in the background, users can receive push notifications from your app even when they are not online. To get started, download the service worker file we have provided and copy it in the document root of your app in order to make it publicly accessible.

Download the service worker file [from this JavaScript code](https://github.com/CleverTap/clevertap-fxos/blob/master/ctServiceWorker.js). As mentioned above, you will have to host this file in your document root.