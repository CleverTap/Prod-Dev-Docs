---
title: Use FCM Version 18.0.0 and Above
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
For Firebase Messaging version 18 and above, you can use your own service to check for tokens and send it to CleverTap. Use the following steps to create a service:

# Step 1: Create a MyFirebaseMessagingService Java file in your project

Following is an example:

```java JAVA
import android.os.Bundle;
import android.util.Log;
import com.clevertap.android.sdk.CleverTapAPI;
import com.clevertap.android.sdk.NotificationInfo;
import com.google.firebase.messaging.FirebaseMessagingService;
import com.google.firebase.messaging.RemoteMessage;
import java.util.Map;
public class MyFirebaseMessagingService extends FirebaseMessagingService {
    @Override
    public void onMessageReceived(RemoteMessage message){
        try {
            if (message.getData().size() > 0) {
                Bundle extras = new Bundle();
                for (Map.Entry<String, String> entry : message.getData().entrySet()) {
                    extras.putString(entry.getKey(), entry.getValue());
                }
                Log.e("TAG","onReceived Mesaage Called");
                NotificationInfo info = CleverTapAPI.getNotificationInfo(extras);
                if (info.fromCleverTap) {
                    CleverTapAPI.createNotification(getApplicationContext(), extras);
                }
            }
        } catch (Throwable t) {
            Log.d("MYFCMLIST", "Error parsing FCM message", t);
        }
    }
    @Override
    public void onNewToken(String token) {
        CleverTapAPI.getDefaultInstance(this).pushFcmRegistrationId(token,true);
        // If you want to send messages to this application instance or
        // manage this apps subscriptions on the server side, send the
        // Instance ID token to your app server.
        //sendRegistrationToServer(token);
    }
}
```
```kotlin Kotlin
import android.os.Bundle
import android.util.Log
import com.clevertap.android.sdk.CleverTapAPI
import com.clevertap.android.sdk.NotificationInfo
import com.google.firebase.messaging.FirebaseMessagingService
import com.google.firebase.messaging.RemoteMessage

class MyFirebaseMessagingService:FirebaseMessagingService() {
  fun onMessageReceived(message:RemoteMessage) {
    try
    {
      if (message.getData().size() > 0)
      {
        val extras = Bundle()
        for (entry in message.getData().entrySet())
        {
          extras.putString(entry.key, entry.value)
        }
        Log.e("TAG", "onReceived Mesaage Called")
        val info = CleverTapAPI.getNotificationInfo(extras)
        if (info.fromCleverTap)
        {
          CleverTapAPI.createNotification(getApplicationContext(), extras)
        }
      }
    }
    catch (t:Throwable) {
      Log.d("MYFCMLIST", "Error parsing FCM message", t)
    }
  }
  fun onNewToken(token:String) {
    CleverTapAPI.getDefaultInstance(this).pushFcmRegistrationId(token, true)
    // If you want to send messages to this application instance or
    // manage this apps subscriptions on the server side, send the
    // Instance ID token to your app server.
    //sendRegistrationToServer(token);
  }
}
```

# Step 2: Append your Android manifest file

Add the following code:

```xml
<service android:name=".MyFirebaseMessagingService">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter>
</service>
```

# Step 3 : Remove CleverTap's FCMTokenListenerService from your AndroidManifest.xml file

Since you have upgraded to FCM v18.0.0 and above, the `FCMTokenListenerService` will now cause a crash when you run your app. So please, remove the following code from your `AndroidManifest.xml` file.

```xml
<service
    android:name="com.clevertap.android.sdk.FcmTokenListenerService">
    <intent-filter>
        <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
    </intent-filter>
</service>
```
