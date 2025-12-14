---
title: Enable RenderMax with Android
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
# Overview

Starting from CleverTap Core SDK version 5.1.0, the RenderMax Push SDK functionality is now supported directly within the CleverTap Core SDK. We strongly recommend updating to CleverTap Core SDK 5.1.0 to take advantage of this integration and to ensure stability.

# Custom Rendering with RenderMax Push SDK

To implement custom handling with RenderMax Push SDK, please note the following updates:

> 🚧 Handling APIs
>
> In CleverTap Core SDK 5.1.0 and above, the following APIs now run on the caller's thread. Make sure to call it in `onMessageReceive()` of messaging service:
>
> * `CTFcmMessageHandler().createNotification(getApplicationContext(), message)`
> * `CleverTapAPI.createNotification(getApplicationContext(), extras)`
> * `CTXiaomiMessageHandler().createNotification(getApplicationContext(), message)`
>
> Ensure that `CTHmsMessageHandler().createNotification(getApplicationContext(), message)` API is  always called on a background thread.

Please ensure that these APIs are called first thing within the `onMessageRecieved` callback of your Firebase Messaging Service. For example,

```java
public class MyFcmMessageListenerService extends FirebaseMessagingService {

    @Override
    public void onMessageReceived(RemoteMessage message){
        try {
            if (message.getData().size() > 0) {
                Bundle extras = new Bundle();
                for (Map.Entry<String, String> entry : message.getData().entrySet()) {
                    extras.putString(entry.getKey(), entry.getValue());
                }

                NotificationInfo info = CleverTapAPI.getNotificationInfo(extras);
	    // Do not do any network/db IO before this
                if (info.fromCleverTap) {
                    CleverTapAPI.createNotification(getApplicationContext(), extras);
	 // Do any other work
                } else {
                    // not from CleverTap handle yourself or pass to another provider
                }
	 // Do any other work
            }
        } catch (Throwable t) {
           Log.d("MYFCMLIST", "Error parsing FCM message", t);
        }
    }
}

```

If you are custom rendering the notification:

* Notification should render within 9 seconds of receiving the payload.
* All network IO operations should have a timeout in such a way that the 9-second limit is not breached.
* The CleverTap SDK should be notified to raise the Push Impression event on the same thread and within 9 seconds of receiving the notification.

By following these guidelines, you can ensure a smooth integration of the RenderMax Push SDK with your Android app, leveraging the enhanced capabilities provided by CleverTap Core SDK 5.1.0.

In addition to the RenderMax Push SDK integration, we have introduced two new APIs in CleverTapAPI that help custom rendering.

Below are the details of the new APIs:

This API allows you to retrieve a notification bitmap from the specified `bitmapSrcUrl` with a specified timeout and size. In case the bitmap retrieval fails, you can choose to fallback to the app icon by setting the `fallbackToAppIcon` parameter. This API provides more control over the bitmap retrieval process for custom rendering.

```java
@Override
public void onMessageReceived(RemoteMessage message) {
        Bundle messageBundle = mParser.toBundle(message);
        // this method must be called on background thread
        // context, messageBundle must be non null.
        // timeout must be in range of 1 - 20000 millis.
        Bitmap notificationBitmap = CleverTapAPI.getNotificationBitmapWithTimeout(
context,messageBundle, "https://www.pushicons.com/icon",
       true, 5000);
// Pass the retrieved bitmap to your notification builder.
        }
```

Below API extends the functionality of the previous one by additionally allowing you to specify the desired size in bytes for the retrieved bitmap.

```java
@Override
public void onMessageReceived(RemoteMessage message) {
        Bundle messageBundle = mParser.toBundle(message);
        // this method must be called on background thread
        // context, messageBundle must be non null.
        // timeout must be in range of 1 - 20000 millis and size must be greater than 0.
        Bitmap notificationBitmap = CleverTapAPI.getNotificationBitmapWithTimeout(
context,messageBundle, "https://www.pushicons.com/icon",
       true, 5000);
// Pass the retrieved bitmap to your notification builder.
        }
```

This is useful when you need to limit the size of the bitmap to optimize memory usage.\
By utilizing these new APIs, you can enhance the push delivery experience for custom rendering and ensure efficient handling of notification bitmaps in your Android app.

> 📘 Security Concerns for Broadcast Receiver
>
> The Broadcast receiver, as a part of the **com.clevertap.android.sdk.pushnotification.fcm** package, is the Broadcast Receiver used by our RenderMax technology. It complies with Android rules and causes no security concerns because the intent will only be passed to the App’s Broadcast Receiver and not any other app.
