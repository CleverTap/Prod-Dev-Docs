---
title: Android 12 Updates
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
# Notification Trampolines

The system prevents components that start other activities inside services or broadcast receivers when a notification is opened. The system will prevent the activity from starting if this requirement is not met. The most prominent one is restrictions on [Notification trampoline](https://developer.android.com/about/versions/12/behavior-changes-12#notification-trampolines). 

## SDK Changes

The new restrictions on Notification Trampolines have changed how CleverTap handles some actions in SDK version 4.3.0. The most important change is the handling of push notifications. 

### Notification Body Click Handling

The push notification body is handled differently after the Android 12 update. 

#### Android 11 and Lower

The `PendingIntent` is a unique instance that the CleverTap SDK creates inside each push notification. The Android OS identifies this instance to open the `CTPushNotificationReceiver` broadcast receiver.  After opening the broadcast receiver, the CleverTap SDK completes the processing, such as starting the activity/deep-link and tracking the Notification Clicked event, and so on.

#### Android 12 and Above

Android has restricted the usage of [Notification Trampolines](https://developer.android.com/about/versions/12/behavior-changes-12). The push notification must start the activity directly after the notification tap. It means that the broadcast receiver or service is now redundant for opening the activity.  

* Automatic Handling by CleverTap SDK - When there is no target activity in the activity stack, the  CleverTap SDK uses the `onActivityCreated()` method from `com.clevertap.android.sdk.ActivityLifecycleCallback` class to perform the following:

  * Raise the Notification Clicked event
  * Provide a callback to the application

* Manual Handling by the Application - If a target activity is available in the activity stack, then you must handle these actions manually. For example, the payment screen is visible to the user, and the target activity of the deeplink is the payment screen. The `onNewIntent()` method can process the Notification Clicked event and provide the callback to the application using the`clevertapApi.pushNotificationClickedEvent()` method. 

#### Example

The following is an example of how you can set up actions in your application. Set up the automatic handling followed by the manual handling. 

1. For automatic handling, register the activity lifecycle callback using the `ActivityLifecycleCallback.register(this)` method.
2. For manual handling, override the `onNewIntent()` method in all your activities or the base activity and call the `clevertapApi.pushNotificationClickedEvent()`.

```kotlin
class HomeScreenActivity : AppCompatActivity(){

override fun onNewIntent(intent: Intent?) {
   super.onNewIntent(intent)
   
   // On Android 12, Raise notification clicked event when Activity is already running in activity backstack
   if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
       cleverTapDefaultInstance?.pushNotificationClickedEvent(intent!!.extras)
   		}
	}
}
```
```java
class HomeScreenActivity extends AppCompatActivity {

   @Override
   protected void onNewIntent(final Intent intent) {
       super.onNewIntent(intent);
       /**
        * On Android 12, Raise notification clicked event when Activity is already running in activity backstack
        */
       if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
           cleverTapDefaultInstance.pushNotificationClickedEvent(intent.getExtras());
       }   
   }
}
```

The tables below show Notification Clicked Handling in Android 12.

* Without a deeplink or with an internal deeplink:

| Application Running Status | Activity in Backstack                                                                                                                     | Activity not in Backstack                                                                                                                                                               |
| :------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Foreground/Background      | Call the `clevertapApi.pushNotificationClickedEvent()` method to raise notification clicks and callback  from the `onNewIntent` callback. | The `onActivityCreated` callback from the `com.clevertap.android.sdk.ActivityLifecycleCallback` class raises the Notification Clicked event and provides a callback to the application. |
| Killed                     | Not Applicable.                                                                                                                           | The `onActivityCreated` callback from the `com.clevertap.android.sdk.ActivityLifecycleCallback` class raises the Notification Clicked event and provides a callback to the application. |

* With an external deeplink:

| Application Running Status | Activity in Backstack           | Activity not in Backstack       |
| :------------------------- | :------------------------------ | :------------------------------ |
| Foreground/Background      | No Notification Clicks tracked. | No Notification Clicks tracked. |
| Killed                     | Not Applicable                  | No Notification Clicks tracked. |

> 📘 Tracking Deeplinks
>
> The notification trampoline restrictions mean that we cannot track the Notification Clicked events when the deeplinks point to a third-party application.

### Notification CTA Click Handling

The handling for Push Notifications with an action button is the same as the handling for notifications body click. Additionally, when the Notification Clicked event is triggered, the event properties indicate the exact button click.  

#### Android 11 and Lower

The `PendingIntent` is a unique instance that the CleverTap SDK creates inside each push notification. The PendingIntent identifies this instance to open the `CTNotificationIntentService`  intent service.  After opening the broadcast receiver, the CleverTap SDK completes the processing, such as starting the activity/deep-link and tracking the Notification Clicked event, and so on.

#### Android 12 and Higher

Android has restricted the usage of [Notification Trampolines](https://developer.android.com/about/versions/12/behavior-changes-12) in all OS versions above version 12. The push notification must start the activity directly after the notification tap. It means that the broadcast receiver or service is now redundant for opening the activity.  

* Automatic Handling by CleverTap SDK - When there is no target activity in the activity stack, the  CleverTap SDK uses the `onActivityCreated()` method from the `com.clevertap.android.sdk.ActivityLifecycleCallback` class to perform the following:

* Raise the Notification Clicked event

* Provide a callback to the application

* Manual Handling by the Application - If a target activity is available in the activity stack, then you must handle these actions manually. For example, the payment screen is visible to the user, and the target activity of deeplink is the payment screen. The `onNewIntent()` method can process the Notification Clicked event and provide the callback to the application using the`clevertapApi.pushNotificationClickedEvent()` method. 

#### Example

The following is an example of how you can set up actions in your application. Set up the automatic handling followed by the manual handling. 

1. For automatic handling, register the activity lifecycle callback using the `ActivityLifecycleCallback.register(this)` method.
2. For manual handling, override the `onNewIntent()` method in all your activities or the base activity and call the `clevertapApi.pushNotificationClickedEvent()` method.

```kotlin
class HomeScreenActivity : AppCompatActivity(){

override fun onNewIntent(intent: Intent?) {
   super.onNewIntent(intent)
   
   // On Android 12, Raise notification clicked event when Activity is already running in activity backstack
   if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
       cleverTapDefaultInstance?.pushNotificationClickedEvent(intent!!.extras)
   		}
	}
}
```
```java
class HomeScreenActivity extends AppCompatActivity {

   @Override
   protected void onNewIntent(final Intent intent) {
       super.onNewIntent(intent);
       /**
        * On Android 12, Raise notification clicked event when Activity is already running in activity backstack
        */
       if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
           cleverTapDefaultInstance.pushNotificationClickedEvent(intent.getExtras());
       }
   	}
}
```

The tables below show Notification Clicked Handling in Android 12.

* Without a deeplink or with an internal deeplink:

| Application Running Status | Activity in Backstack                                                                                                                     | Activity not in Backstack                                                                                                                                                               |
| :------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Foreground/Background      | Call the `clevertapApi.pushNotificationClickedEvent()` method to raise notification clicks and callback  from the `onNewIntent` callback. | The `onActivityCreated` callback from the `com.clevertap.android.sdk.ActivityLifecycleCallback` class raises the Notification Clicked event and provides a callback to the application. |
| Killed                     | Not Applicable.                                                                                                                           | The `onActivityCreated` callback from the `com.clevertap.android.sdk.ActivityLifecycleCallback` class raises the Notification Clicked event and provides a callback to the application. |

* With an external deeplink:

| Application Running Status | Activity in Backstack           | Activity not in Backstack       |
| :------------------------- | :------------------------------ | :------------------------------ |
| Foreground/Background      | No Notification Clicks tracked. | No Notification Clicks tracked. |
| Killed                     | Not Applicable.                 | No Notification Clicks tracked. |

> 📘 Tracking Deeplinks
>
> The notification trampoline restrictions mean that we cannot track the Notification Clicked events when the deeplinks point to a third-party application.

### Clear Notification on CTA Click

The clearing of notifications on CTA click is handled differently after the Android 12 update. The application must now handle these actions. 

#### Android 11 and Lower

The `PendingIntent` is a unique instance that the CleverTap SDK creates inside each push notification. The Android OS identifies this instance to open the `CTNotificationIntentService` intent service.  The CleverTap SDK then clears the notification.

#### Android 12 and Higher

 Android has restricted the usage of [Notification Trampolines](https://developer.android.com/about/versions/12/behavior-changes-12) in all OS versions above version 12. The push notification must start the activity directly after the notification tap. It means that the broadcast receiver or service is now redundant for opening the activity.

* Automatic handling by CleverTap SDK - This handling is not applicable for clearing a notification.

* Manual handling by the application - There are two cases of manual handling for clearing notifications:

* Target activity available in the activity stack - You must handle these actions manually. For example, the payment screen is visible to the user, and the target activity of deeplink is the payment screen.  The `onNewIntent()` method can process the clearing of notifications.

* Target activity not available in the activity stack - Register the activity lifecycle callbacks of the `android.app.Application.ActivityLifecycleCallbacks` class to clear the notification in the `onActivityCreated` method.

#### Example

The following is an example of how you can clear notifications in your application:

```kotlin
class HomeScreenActivity : AppCompatActivity(){

override fun onNewIntent(intent: Intent?) {
   super.onNewIntent(intent)

   // On Android 12, clear notification on CTA click when Activity is already running in activity backstack
   if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
   NotificationUtils.dismissNotification(intent, applicationContext)
					}
			}
}

class MyApplication : MultiDexApplication(),
ActivityLifecycleCallbacks {

override fun onCreate() {
    registerActivityLifecycleCallbacks(this)
}

override fun onActivityCreated(activity: Activity, savedInstanceState: Bundle?) {

// On Android 12, clear notification on CTA click when Activity is already running in activity backstack
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
   NotificationUtils.dismissNotification(intent, applicationContext)
				}
			}
}

object NotificationUtils {

   //Require to close notification on action button click
   fun dismissNotification(intent: Intent?, applicationContext: Context){
       intent?.extras?.apply {
            var autoCancel = true
            var notificationId = -1

            getString("actionId")?.let {
                Log.d("ACTION_ID", it)
                autoCancel = getBoolean("autoCancel", true)
                notificationId = getInt("notificationId", -1)
            }
            /**
             * If using InputBox template, add ptDismissOnClick flag to not dismiss notification
             * if pt_dismiss_on_click is false in InputBox template payload. Alternatively if normal
             * notification is raised then we dismiss notification.
             */
            val ptDismissOnClick = intent.extras!!.getString(PTConstants.PT_DISMISS_ON_CLICK,"")

            if (autoCancel && notificationId > -1 && ptDismissOnClick.isNullOrEmpty()) {
                val notifyMgr: NotificationManager =
                    applicationContext.getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
                notifyMgr.cancel(notificationId)
            }
        }
   }
}
```
```java
class HomeScreenActivity extends AppCompatActivity {

   @Override
   protected void onNewIntent(final Intent intent) {
       super.onNewIntent(intent);
       
       /**
        * On Android 12, clear notification on CTA click when Activity is already running in activity backstack
        */
       if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
           NotificationUtils.dismissNotification(intent, applicationContext);
       }
   }
}

class MyApplication extends MultiDexApplication implements ActivityLifecycleCallbacks {

   @Override
   public void onCreate(){
   registerActivityLifecycleCallbacks(this);
}

   @Override
   public void onActivityCreated(@NonNull final Activity activity, @Nullable final Bundle savedInstanceState) {

/**
        * On Android 12, clear notification on CTA click when Activity is already running in activity backstack
        */
       if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
           NotificationUtils.dismissNotification(intent, applicationContext);
       }

   }

}

public class NotificationUtils {

//Require to close notification on action button click
   public static void dismissNotification( Intent intent, Context applicationContext){
       Bundle extras = intent.getExtras();
       if (extras != null) {
           String actionId = extras.getString("actionId");
           if (actionId != null) {
               boolean autoCancel = extras.getBoolean("autoCancel", true);
               int notificationId = extras.getInt("notificationId", -1);
               if (autoCancel && notificationId > -1) {
                   NotificationManager notifyMgr =
                           (NotificationManager) applicationContext.getSystemService(Context.NOTIFICATION_SERVICE);
                   notifyMgr.cancel(notificationId);                }
              
           }
       }
   }
}
```

# Pending Intents Mutability

To improve the security of your app, you must [specify the mutability](https://developer.android.com/about/versions/12/behavior-changes-12#pending-intent-mutability) of each `PendingIntent` object that your app creates. CleverTap Android SDK 4.3.0 complies with this.

### Safer Component Exporting

You must explicitly declare the `android:exported` attribute if your app contains activities, services, or broadcast receivers that use intent filters. Refer to [Safer Component Exporting](https://developer.android.com/about/versions/12/behavior-changes-12#exported). CleverTap Android SDK 4.3.0 complies with this.

### Custom Notifications

To streamline User Experience when apps use entirely Custom Notification layouts, Android 12 will prevent such notifications from using the whole notification area.\
Android 12 introduces a standard template for notifications, ensuring the same decoration for all notifications. It has a fixed header with the app name and a consistent expand/collapse appearance.\
Detailed documentation and illustration are available in the [Android user experience documentation](https://developer.android.com/about/versions/12/behavior-changes-12#custom-notifications).\
CleverTap Push Templates SDK will be released soon, supporting Android 12 changes related to Custom Notifications.
