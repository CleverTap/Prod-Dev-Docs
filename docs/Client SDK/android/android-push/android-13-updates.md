---
title: Android 13 Updates
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

Android 13 (API 33) has rolled out new features and API changes. Refer to [Android Behavior changes for Apps targeting Android 13](https://developer.android.com/about/versions/13/behavior-changes-13).

There are changes for all apps, whether targeting API 33 or just running on Android 13. Some of these changes include runtime notification permission, Foreground Service Task Manager, and Battery Resource Utilization. For a complete list of these changes, refer to [Android Developer](https://developer.android.com/about/versions/13/behavior-changes-all) documentation.

> 📘 CleverTap Android SDK 4.7.0 supports Android 13
>
> If you plan to increase the target API of your app to 33, you must upgrade the CleverTap Android SDK to version 4.7.0 or higher. The   CleverTap SDK version 4.7.0 and higher now support Android SDK from API 19(Android 4.4) and higher.

# Push Notification Permission

All apps targeting Android 13 and above must request permission from the user before pushing notifications. All newly-installed apps must get user permission before they can send notifications. This means that you have one chance to ask for permission from the user. However, existing apps will get grandfathered in after a short grace period until the next app launch on Android 13. For more information, refer to [Android Developer Documentation - notification permission](https://developer.android.com/develop/ui/views/notifications/notification-permission).

With the latest update, Android 13 allows you to monitor opt-in rates for Push Notifications. To access this feature, ensure you upgrade to SDK version 50100 or higher. As soon as the user grants permission to receive push notifications on the push primer, CleverTap raises an event named *Channel Subscribed*. And the *Channel Unsubscribed* event is raised when end-users unsubscribe from the push campaigns. For more information about the event and its properties, refer to [System Events](https://docs.clevertap.com/docs/events#system-events).

A *Push Primer* can explain to your users the need for push notifications and help with your engagement rates. It is an InApp notification that you can show to your users before requesting notification permission. It helps with the following :

* Allows you to educate your users on why you are asking for this permission before invoking a system dialog that asks the user to allow or deny it.
* Acts as a precursor to the hard system dialog and thus allows you to ask for permission multiple times if previously denied without making your users search the device settings.

The following are the three ways to handle the new push notification changes:

* **Use a push primer using[Half-Interstitial InApp notification](https://developer.clevertap.com/docs/android-in-app-notifications#push-primer-using-half-interstitial-inapp)**

<Image title="Half Interstitial InApp Notification" alt={398} align="center" width="30% " border={true} src="https://files.readme.io/96df198-push_primer_half_interstitial.png">
  Half Interstitial InApp Notification
</Image>

* **Use a push primer using[In-App alert](https://developer.clevertap.com/docs/android-in-app-notifications#push-primer-using-alert-inapp)**

<Image title="In App Alert Notification" alt={398} align="center" width="smart" border={true} src="https://files.readme.io/b711a89-Alert_push_primer.png">
  In-App Alert Notification
</Image>

* **[Without a push primer](https://developer.clevertap.com/docs/android-push#invoke-the-android-permission-dialog-without-push-primer)**

<Image title="Push Notification without Push Primer" alt={293} align="center" width="smart" border={true} src="https://files.readme.io/9e9a15b-without_primer.png">
  Push Notification without Push Primer
</Image>

### Invoke Android Permission Dialog (Without Push Primer)

The`promptForPushPermission(boolean)` method is a public method that can be used to request permission on behalf of your application directly. If `showFallbackSettings` is true, then it shows an alert dialog that routes to the app's notification settings page. 

```java
cleverTapAPI.promptForPushPermission(true) //Takes boolean as a parameter. If true and the permission  is denied then we fallback to app’s notification settings, if it’s false then we just throw a verbose log saying permission is denied.
```
```kotlin
cleverTapAPI.promptForPushPermission(true) // Takes boolean as a parameter. If true and the permission is denied then we fallback to app’s notification settings, if it’s false then we just throw a verbose log saying permission is denied.
```

### Check Status of Push Permission

The `isPushPermissionGranted` method can be used to check the status of the push notification permission for your application.

```java
cleverTapAPI.isPushPermissionGranted() // Returns true if permission is granted, else returns false if permission is denied.
```
```kotlin
cleverTapAPI.isPushPermissionGranted // Returns true if permission is granted, else returns false if permission is denied.
```

### Invoke Push Primer Flow

`promptPushPrimer(JSONObject)`- Calls the push primer flow for Android 13 and above devices.

```java
cleverTapAPI.promptPushPrimer(jsonObject);
```

## Available Callbacks for Push Primer

Based on notification permission grant/deny, we provide a callback on `PushPermissionResponse`(boolean). 

* Register `PushPermissionResponseListener` instance to receive the Push Primer result:\
        The `CleverTapAPI` class exposes a `registerPushPermissionNotificationResponseListener(PushPermissionResponseListener)` method. Use this method to register your `PushPermissionResponseListener` instance. Each `PushPermissionResponseListener` instance passed in this method is added to a `List` of the `PushPermissionResponseListener` type, and the Push Primer result is notified to all the elements of that list.

* Unregister `PushPermissionResponseListener` instance to stop receiving the Push Primer result:\
    The `CleverTapAPI` class exposes a `unregisterPushPermissionNotificationResponseListener(PushPermissionResponseListener)` method.\
  Use this method to unregister your previously registered `PushPermissionResponseListener` instance.

> 📘 Note
>
> Call `registerPushPermissionNotificationResponseListener(PushPermissionResponseListener)` only from the `onCreate()` method of the Activity/Fragment and unregister the listener instance from the `onDestroy()` method using `unregisterPushPermissionNotificationResponseListener(PushPermissionResponseListener)`.

The following is a sample implementation to get the permission result:

```java
public class MainActivity extends AppCompatActivity implements PushPermissionResponseListener {
    private CleverTapAPI cleverTapDefaultInstance;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(this);
        if (cleverTapDefaultInstance != null) {
            cleverTapDefaultInstance.registerPushPermissionNotificationResponseListener(this);
        }
    }

    @Override
    public void onPushPermissionResponse(boolean accepted) {
        Log.i(TAG, "onPushPermissionResponse :  InApp---> response() called accepted="+accepted);
        if(accepted){
            CleverTapAPI.createNotificationChannel(getApplicationContext(), "BRTesting", "Testing Channel",
                    "Testing Channel for BR", NotificationManager.IMPORTANCE_HIGH, true);
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (cleverTapDefaultInstance != null) {
            cleverTapDefaultInstance.unregisterPushPermissionNotificationResponseListener(this);
        }
    }
}
```
```kotlin
class MainActivity : AppCompatActivity(), PushPermissionResponseListener {
    var cleverTapDefaultInstance: CleverTapAPI? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(this)
        cleverTapDefaultInstance?.registerPushPermissionNotificationResponseListener(this)
    }

    override fun onPushPermissionResponse(accepted: Boolean) {
        Log.i(TAG, "onPushPermissionResponse :  InApp---> response() called accepted=$accepted")
        if (accepted) {
            CleverTapAPI.createNotificationChannel(getApplicationContext(), "BRTesting", "Testing Channel",
                "Testing Channel for BR", NotificationManager.IMPORTANCE_HIGH, true);
        }
    }

    override fun onDestroy() {
        super.onDestroy()
        cleverTapDefaultInstance?.unregisterPushPermissionNotificationResponseListener(this)
    }
}
```

> 📘 New Method for InAppNotificationListener
>
> Use the following method from CleverTap SDK version 4.7.0 and above:
>
> ```java
> @Override  
> public void onShow(CTInAppNotification ctInAppNotification) {
> }
> ```
