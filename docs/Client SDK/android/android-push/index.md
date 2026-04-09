---
title: Android Push
excerpt: Learn how to deliver push notifications to your Android application.
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
      title: Baidu Push Integration
      url: https://developer.clevertap.com/docs/baidu-push-notifications
    - type: link
      title: Xiaomi Push Integration
      url: https://developer.clevertap.com/docs/xiaomi-push-notifications
    - type: basic
      slug: push-templates-android
      title: Android Push Templates
    - type: link
      title: Android In-App
      url: https://developer.clevertap.com/docs/android-in-app-notifications
    - type: link
      title: Android App Inbox
      url: https://developer.clevertap.com/docs/android-app-inbox
    - type: link
      title: Android Native Display
      url: https://developer.clevertap.com/docs/android-native-display
    - type: link
      title: Android Product Experiences
      url: https://developer.clevertap.com/docs/product-experiences-android
    - type: link
      title: Multi-Instance Support Android
      url: https://developer.clevertap.com/docs/multi-instance-support-android
    - type: link
      title: Android Advanced Settings
      url: https://developer.clevertap.com/docs/android-advanced-settings
    - type: link
      title: Android Resources
      url: https://developer.clevertap.com/docs/android-resources
---
# Overview

CleverTap allows you to send push notifications to your applications from our dashboard. The following are the two ways to implement push notifications on an Android device:

* Default CleverTap Implementation - This is the easiest way to implement push notifications. For more information, refer to [Adding CleverTap's listener service](https://developer.clevertap.com/docs/android-push#section-add-clever-taps-fcm-message-listener-service).

* Custom Android Push - Use it only if you already have your implementation or use another push provider. For more information, refer to [Custom Android Push Notifications Handling](https://developer.clevertap.com/docs/android-push#custom-android-push-notification-handling).

# Configure Push Notifications

The following are the steps to configure push notifications for your Android app:

1. [Find FCM Credentials](doc:android-push#find-fcm-credentials).
2. [Set Up FCM Credentials](doc:android-push#set-up-fcm-credentials-on-clevertap).  
   OR  
   [Safeguard Admin Credentials and Upload Credentials File to CleverTap](doc:android-push#safeguard-admin-credentials).

> 📘 FCM Update: Migrate to HTTP v1 API
>
> Firebase will deprecate its legacy APIs starting June 20, 2023, and discontinue them from June 2024. To avoid disruptions in push notifications, you must migrate to the new HTTP v1 API by June 20, 2024. Failure to do so may result in all push notifications not being delivered to users and result in errors instead.
>
> For detailed migration steps, refer to  [Configure Push Notifications](doc:android-push#configure-push-notifications). CleverTap has already completed this migration to ensure uninterrupted service.
>
> To ensure uninterrupted delivery of the push notifications, you must [migrate to the latest FCM APIs](https://firebase.google.com/docs/cloud-messaging/migrate-v1).

## Find FCM Credentials

Firebase Cloud Messaging is a service that allows you to send notifications to your applications and receive information from them. Your FCM Sender ID and API key authenticate against CleverTap, enabling you to send notifications to your users from CleverTap.

To find the FCM credentials:

1. Log in to the [Firebase Developer Console](https://console.firebase.google.com/) and go to Open _Settings_ > _Service Accounts_.
2. To obtain the FCM Credential File, click **Generate new private key**.

<Image align="center" alt="Generate New Private Key" border={true} caption="Generate New Private Key" src="https://files.readme.io/c15f649-image.png" />

3. Confirm your action by clicking **Generate Key**.

## Set Up FCM Credentials on CleverTap

To ensure your campaigns are delivered, you must set up FCM Credentials on the CleverTap dashboard:

1. Log in to the CleverTap dashboard and go to _Settings_ > _Channels_ > _Mobile Push_.
2. Click the _FCM credentials_ section and click **Upload FCM Credential File**, as shown in the following image:

<Image align="center" alt="Upload FCM Credential File" border={true} caption="Upload FCM Credential File" src="https://files.readme.io/bc384f2-Upload_FCM_Credential_File_to_CleverTap.png" width="85% " />

3. Click **Save**.

<Image align="center" alt="Push Notifications Credentials" border={true} caption="FCM Credentials Saved to CleverTap Dashboard" src="https://files.readme.io/42a202a-FCM_Credential_File_Uploaded_Succesfully.png" width="85% " />

4. Send a [personalized Push campaign](https://docs.clevertap.com/docs/personalize-messages-push) to internal users to verify if the configuration is successful.

## Safeguard Admin Credentials

If your privacy policy requires safeguarding your admin credentials shared in the JSON file with CleverTap, follow the steps listed below:

1. Open your Firebase project and go to the _Service accounts_ tab.
2. Click the _service accounts_ list, as shown in the following figure:

<Image align="center" alt="Click Service Accounts List" border={true} caption="Click Service Accounts List" src="https://files.readme.io/ec87236-Click_Service_Accounts.png" width="90% " />

3. Select your project from the _Service Accounts_ page and click **+ CREATE SERVICE ACCOUNT**.

<Image align="center" alt="Create Service Account " border={true} caption="Create Service Account" src="https://files.readme.io/10ec3dd-Create_a_Service_Account.png" width="90% " />

4. Add the Service account name and Grant _Firebase Cloud Messaging API Admin_ role to the service account and click **Done**, as shown in the following figure:

<Image align="center" alt="Assign Role to Service Account" border={true} caption="Assign Role to Service Account" src="https://files.readme.io/9cf29b3-Assign_Role_to_Service_Account.png" width="90% " />

5. Download your Credentials File:
   1. Navigate to the _Keys_ tab from your service account and click **ADD KEY**.
   2. Select the Key type as _JSON_ and click **Create**. The credentials file is downloaded.

<Image align="center" alt="Download Credential File" border={true} caption="Download Credential File" src="https://files.readme.io/eb0435d-Download_credentials_file_and_upload_to_CleverTap_dashboard.png" width="90% " />

6. Use these credentials on a test account. Once you have validated it, you can use the same process for live account key generation and [upload it to the CleverTap dashboard](doc:android-push#set-up-fcm-credentials-on-clevertap).

> 📘 Prerequisite
>
> To enable Firebase Cloud Messaging API, do the following:
>
> 1. Go to [console.cloud.google.com](http://console.cloud.google.com/) and click **API & Services**.
>
> <Image align="center" border={true} src="https://files.readme.io/b4d5fd4-image.png" className="border" />
>
> 2. Click **Cloud Messaging API** link from the left menu.
> 3. Click **ENABLE** if it is not already enabled.
>
> <Image align="center" border={true} src="https://files.readme.io/ca7a60d-image.png" className="border" />

# HTTP v1 API Rate Limits

With the new HTTP v1 API, the rate at which FCM sends requests per minute has changed. Each project now has a strictly enforced threshold for the number of requests allowed. The HTTP v1 API allocates 600,000 tokens per 1-minute token bucket, which replenishes completely at the end of each 1-minute interval.

## Verify API Rate Limits

To verify the API Limits for your account:

1. Navigate to the Firebase Cloud Messaging API Management menu:

   1. Go to [console.cloud.google.com](http://console.cloud.google.com/) and select your project from the dropdown if not already selected.
   2. Search and select _Firebase Cloud Messaging API_.
   3. Click **Manage**.

   <br />

   <Image align="center" alt="Navigate to Firebase Cloud Messaging API Management Menu" border={true} caption="Navigate to Firebase Cloud Messaging API Management Menu" src="https://files.readme.io/6c74eaa-Naviagte_Firebase_Cloud_Messaging_API_Management_menu.png" width="90% " />
2. Find your Send Requests per Minute Quota:

   1. Navigate to the _Quotas and System Limits_ tab.
   2. Check your _Send Requests per Minute_ Quota, as shown in the following image:

   <Image align="center" alt="Find Send Requests per Minute Quota For Your Account" border={true} caption="Find Send Requests per Minute Quota For Your Account" src="https://files.readme.io/28c4d22-Find_your_Send_Requests_per_Minute_Quota.png" width="90% " />

## API Rate Limit Exceeded

Consider an example where your account is granted a quota of 1000 requests per minute, and your target segment consists of 5000 users. FCM expects the throttle rate to adhere strictly to this limit. Failure to comply with this throttle rate may result in push notifications not being delivered to users and instead resulting in errors. It also triggers a non-compliance email from the FCM support team. However, CleverTap attempts to resend the request up to three times before marking it as a failed delivery and raises the _FCM Rate limit exceeded_ error.

To optimize this, you can use CleverTap's throttle feature under the Campaign Limits section to help you stay within your allocated quota. This limit is set globally and can be configured by navigating to  _Settings_ > _Setup_ >  _Campaign Limits_ and modifying the throttle limit.

<Image align="center" alt="Set Up Throttle Limit" border={true} caption="Set Up Throttle Limit" src="https://files.readme.io/d804aa6-Set_Up_Throttle_Limit.png" width="90% " />

If you encounter any issues, refer to the [frequently asked questions](doc:android-push#faqs)  section.

> 🚧 Campaign Throttle Limit
>
> The FCM quota limit applies only to Android push notifications. However, CleverTap's campaign throttle limit applies at the campaign level. If there are multiple campaigns or if you want to deliver notifications to both Android and iOS devices, or use other FCM services, the campaign throttle limit must be set below the FCM quota limit to ensure efficient delivery.

# Add CleverTap's FcmMessageListenerService

Inside the <application /> tags, register the following services:

```xml
<service
    android:name="com.clevertap.android.sdk.pushnotification.fcm.FcmMessageListenerService"
android:exported="true">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter>
</service>
```

Opening a notification using CleverTap’s implementation launches your app. If you send a deeplink, the link will be opened.

# Custom Android Push Notification Handling

**Step 1. Using Your Custom Implementation**  
If you have your custom implementation for managing Android push notifications, you can inform CleverTap about the user’s FCM registration ID.

Follow the instructions below if you are using FCM.

```java JAVA
String fcmRegId = FirebaseInstanceId.getInstance().getToken();
clevertapDefaultInstance.pushFcmRegistrationId(fcmRegId,true);
```
```kotlin Kotlin
val fcmRegId = FirebaseInstanceId.getInstance().token
clevertapDefaultInstance?.pushFcmRegistrationId(fcmRegId, true)
```

**Step 2: Using Multiple Android Push Notification Providers**  
CleverTap plays well with other Android push notification providers. You can configure your app to work with multiple push notification providers.

If you are using FCM, please follow these instructions:

1. Subclass `FirebaseMessagingService` as shown below:

```java JAVA
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

                if (info.fromCleverTap) {
                    CleverTapAPI.createNotification(getApplicationContext(), extras);
                } else {
                    // not from CleverTap handle yourself or pass to another provider
                }
            }
        } catch (Throwable t) {
           Log.d("MYFCMLIST", "Error parsing FCM message", t);
        }
    }
}
```
```kotlin Kotlin
class MyFcmMessageListenerService : FirebaseMessagingService() {

override fun onMessageReceived(message: RemoteMessage) {

    message.data.apply {
        try {
            if (size > 0) {
                val extras = Bundle()
                for ((key, value) in this) {
                    extras.putString(key, value)
                }
                val info = CleverTapAPI.getNotificationInfo(extras)
                if (info.fromCleverTap) {
                    CleverTapAPI.createNotification(applicationContext, extras)
                } else {
                    // not from CleverTap handle yourself or pass to another provider
                }
            }
        } catch (t: Throwable) {
            Log.d("MYFCMLIST", "Error parsing FCM message", t)
        }
    }
    }
}
```

If you are using CleverTap Android SDK v4.4.0 and above, you can add the following code instead of the above code:

```java JAVA
public class MyFcmMessageListenerService extends FirebaseMessagingService {

    @Override
    public void onMessageReceived(RemoteMessage message){
        CTFcmMessageHandler()
                .createNotification(getApplicationContext(), message);
    }
}
```
```kotlin Kotlin
class MyFcmMessageListenerService : FirebaseMessagingService() {

    @Override
    public void onMessageReceived(RemoteMessage message){
        CTFcmMessageHandler()
                .createNotification(applicationContext, message)
    }
}
```

> 📘 Note
>
> From CleverTap Android SDK v5.1.0 onwards, the following API now runs on the caller's thread.  Ensure to call it in `onMessageReceive()` of messaging service:
>
> * `CTFcmMessageHandler().createNotification(getApplicationContext(), message)`
> * `CleverTapAPI.createNotification(getApplicationContext(),extras);`

2. Add your service to the `AndroidManifest.xml` in place of the CleverTap `FcmMessageListenerService`.

```xml
<service
    android:name="com.your.package.MyFcmMessageListenerService"
    android:exported="true">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter>
</service>
```

3. Replace `com.your.package` with the correct package name which contains the newly created class, `MyFcmMessageListenerService`.

4. To track the push notification events and deeplinks add the following receiver in your `AndroidManifest.xml`:

```xml
<receiver
    android:name="com.clevertap.android.sdk.pushnotification.CTPushNotificationReceiver"
    android:exported="false"
    android:enabled="true">
</receiver>
```

**Step 3: Structure of CleverTap FCM Payload**

Here is the structure of the payload with a CleverTap FCM push notification.

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Key
      </th>

      <th>
        Type
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        wzrk_pn
      </td>

      <td>
        N/A
      </td>

      <td>
        If present, this notification is sent from CleverTap.
      </td>
    </tr>

    <tr>
      <td>
        wzrk_id
      </td>

      <td>
        string
      </td>

      <td>
        Open rate tracking ID (can be empty or it might not be present).
      </td>
    </tr>

    <tr>
      <td>
        wzrk_bp
      </td>

      <td>
        string
      </td>

      <td>
        If present, the value will be the URL of an image that needs to be displayed in the notification.
      </td>
    </tr>

    <tr>
      <td>
        wzrk_sound
      </td>

      <td>
        boolean or string
      </td>

      <td>
        If present, it signifies that the default or custom Android notification sound must be played.
      </td>
    </tr>

    <tr>
      <td>
        nt
      </td>

      <td>
        string
      </td>

      <td>
        Notification Title, if absent or empty, falls back to the app name.
      </td>
    </tr>

    <tr>
      <td>
        nm
      </td>

      <td>
        string
      </td>

      <td>
        Notification Body, if absent or empty, ignore this notification.
      </td>
    </tr>

    <tr>
      <td>
        wzrk_dl
      </td>

      <td>
        string
      </td>

      <td>
        If present, this is a deeplink that must be followed at the time of notification open.
      </td>
    </tr>

    <tr>
      <td>
        wzrk_d
      </td>

      <td>
        N/A
      </td>

      <td>
        If present, ignore this notification.
      </td>
    </tr>

    <tr>
      <td>
        ico
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, it contains the URL of an image that must be used as the small notification icon.
      </td>
    </tr>

    <tr>
      <td>
        wzrk_pivot
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, it signifies the type of variant in A/B campaigns.
      </td>
    </tr>

    <tr>
      <td>
        wzrk_rnv
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, it will raise the Push Impressions event.
      </td>
    </tr>

    <tr>
      <td>
        wzrk_nms
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, it contains the summary text to be displayed along with the image.
      </td>
    </tr>

    <tr>
      <td>
        wzrk_st
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, it contains the subtitle text, which is displayed next to the App name.
      </td>
    </tr>

    <tr>
      <td>
        wzrk_clr
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, it contains the Hex value of the color to be applied on the small icon (and app name for Android versions below Pie)
      </td>
    </tr>

    <tr>
      <td>
        wzrk_bpds
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, indicate the image rendering status in the push notification. Its values indicate the following statuses:  
        -`NO_IMAGE`: The push payload does not include an image.  
        -`SUCCESS`: The push payload contains an image that was downloaded successfully.  
        -`NO_NETWORK`: The push payload includes an image, but the user's device does not have network connectivity.  
        -`DOWNLOAD_FAILED`: The push payload contains an image that could not be downloaded due to an external error.
      </td>
    </tr>
  </tbody>
</Table>

If the field nm is empty, ignore the notification. CleverTap sends out a dummy notification with the nm field empty to test for app uninstalls. In addition to the above, attach all keys starting with _wzrk_ to your notification extras (For example, `wzrk_id` is used to track notification opens).

# Set the Small Notification Icon

The small icon is a notification bar icon displayed in the push notifications, as shown in the image below:

<Image align="center" alt={1080} border={true} caption="Set the Small Notification Icon" title="Set the Small Notification Icon" src="https://files.readme.io/fd3de03-Campaigns_Push_Small_Icon.png" width="40% " />

With the release of Android 5 (Lollipop), the notification icons are rendered differently in the notification bar at the top. By default, our SDK uses the app's icon for both the notification icon and the notification bar icon; however, since Android 5, all non-alpha channels are ignored while drawing the main notification icon. For more information, refer to [Android Notification Guide](https://developer.android.com/design/ui/mobile/guides/home-screen/notifications) in the Android documentation.

The small icon must be an _alpha only_ image, which means it must have only one color and be transparent for the rest. The _alpha only_ icon image combines the colored and transparent regions.

In general, the logo displayed on the icon has a single color, and the background is transparent. The alpha channel provides the ability to embed transparency within an image. This can be accomplished with tools such as Adobe Photoshop, Illustrator, and so on. You can also change the colored region of the small icon from the CleverTap dashboard.

The CleverTap SDK fills the transparent section of the small icon with the selected color:

<Image align="center" alt={2736} border={true} caption="Set Small App Icon Color" title="Set Small App Icon Color" src="https://files.readme.io/87250b9-13.png" />

Following is a small icon sample:

<Image align="center" alt="Example of a Small Icon" border={true} caption="Example of a Small Icon" title="Example of a Small Icon" src="https://files.readme.io/4b90e04-Burger_icon.png" />

After creating an icon with the alpha channel, you can use it for your project in Android Studio. Save the icon file in the Drawable folder of your project.

To set a custom notification icon (only for the small icon), add the following metadata entry in your AndroidManifest.xml.

```xml
<meta-data
    android:name="CLEVERTAP_NOTIFICATION_ICON"
    android:value="ic_stat_red_star"/> <!-- name of your file in the drawable directory without the file extension. -->
```

> ❗️ Warning
>
> Use only the file name. Do not use the path such as _@drawable/ic_stat_red_star_ .

> 📘 Device and Android Limitation
>
> Push notification icons may be displayed in black and white on OnePlus, Oppo, and Realme devices due to support limitations of these device manufacturers for Android 12 and 13 versions.

# Push Notification Callback

The CleverTap Android SDK v3.8.1 and above gives you a callback when a Push Notification is clicked along with the entire payload. Implement the `CTPushNotificationListener` in your Application class or Main Activity to receive this payload.

To set the listener, use the following code -

```java JAVA
CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());
cleverTapAPI.setCTPushNotificationListener(this);
```
```kotlin Kotlin
CleverTapAPI.getDefaultInstance(applicationContext)?.apply {
	ctPushNotificationListener = this@YourAndroidActivity
}
```

Implement the following override method to receive the payload as a Map on the click of a notification -

```java JAVA
@Override
public void onNotificationClickedPayloadReceived(HashMap<String, Object> payload) {
   //Use your custom logic for  the payload 
}
```
```kotlin Kotlin
override fun onNotificationClickedPayloadReceived(payload: HashMap<String, Any>?) {
    //Use your custom logic for  the payload 
}
```

# Push Impressions

You can raise and record push notifications delivered to your users’ Android devices.

1. Navigate to _Settings_ > _Schema_ > _Events_.
2. Search for _Push Impressions_, then click on the vertical ellipsis.

<Image align="center" alt="Push Impressions" border={true} caption="Push Impressions" title="Push Impressions" src="https://files.readme.io/ebf8ae7-Push_Impression_event_FAQ.png" />

3. Click on **Setup push impressions**. A new window displays.
4. Click on **Save** to enable the option.

If you are custom-handling push notifications, add the following code after you render the push notifications:

```java JAVA
CleverTapAPI.getDefaultInstance(this).pushNotificationViewedEvent(extras);
```
```kotlin Kotlin
CleverTapAPI.getDefaultInstance(this@YourAndroidActivity)?.pushNotificationViewedEvent(extras)
```

# Pull Notification

Starting with v3.4.0, the SDK supports Pull Notification. This feature provides the capability to reach users on devices that suppress notifications via GCM/FCM.

To allow your app to use CleverTap's Pull Notification via background ping service, add the following fields in your app's `AndroidManifest.xml`:

```xml
<meta-data
    android:name="CLEVERTAP_BACKGROUND_SYNC"
    android:value="1"/>
```

> 🚧 Custom Handling
>
> Additional integration is required if the app is rendering push notifications and not the CleverTap SDK. For more information, see [custom handling of pull notification](https://developer.clevertap.com/docs/android-push#custom-handling-of-pull-notification).

## Custom Handling of Pull Notification

1. In your custom FCM Receiver class, add the following code snippet when you receive the RemoteMessage object from Firebase. This method allows you to ensure that duplicate notification is not rendered on your device via Pull Notification.

```java JAVA
if (message.getData().size() > 0) {
                Bundle extras = new Bundle();
                Iterator var = message.getData().entrySet().iterator();

                while(var.hasNext()) {
                    Map.Entry entry = (Map.Entry)var.next();
                    extras.putString((String)entry.getKey(), (String)entry.getValue());
                }
                    CleverTapAPI.processPushNotification(getApplicationContext(),extras);
                }
            }
```
```kotlin Kotlin
if (message.getData().size() > 0)
{
  val extras = Bundle()
  val `var` = message.getData().entrySet().iterator()
  while (`var`.hasNext())
  {
    val entry = `var`.next() as Map.Entry
    extras.putString(entry.getKey() as String, entry.getValue() as String)
  }
  CleverTapAPI.processPushNotification(getApplicationContext(), extras)
}
```

2. In your Application class, implement the CTPushAmpListener interface.

```java JAVA
public class MyApplication extends Application implements CTPushAmpListener {

// your application class code
}
```
```kotlin Kotlin
class MyApplication:Application(), CTPushAmpListener// your application class code
```

3. In the onCreate() method of your Application class, create an instance of CleverTapAPI and set the `CTPushAmpListener` as following:

```java JAVA
CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());
cleverTapAPI.setCTPushAmpListener(this);
```
```kotlin Kotlin
val cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext())
cleverTapAPI.setCTPushAmpListener(this)
```

4. After the listener is set, whenever a notification is sent to the device via Pull Notification, the app will receive the payload in the onPushPayloadReceived method. Implement this method as follows and ensure that your rendering logic of the Push Notification is written here:

```java JAVA
@Override
public void onPushPayloadReceived(Bundle bundle) {
  CTFcmMessageHandler()
                .createNotification(getApplicationContext(), message);
  //OR
  //write push notification rendering logic here
}
```
```kotlin Kotlin
fun onPushPayloadReceived(bundle:Bundle) {
  CTFcmMessageHandler()
                .createNotification(getApplicationContext(), message)
 //OR
 //write push notification rendering logic here
}
```

# FAQs

### Why do I see a `CHANNEL_UNSUBSCRIBED`Event in a Multi-Process Android App using FCM?

You may encounter the `CHANNEL_UNSUBSCRIBED` event in multi-process Android applications where Firebase is initialized in multiple processes. Although Firebase Cloud Messaging (FCM) supports multi-process apps, it does not automatically handle cross-process initialization.

If Firebase is not explicitly initialized in each process where it is accessed, it may lead to inconsistent behavior, including unexpectedly triggering the `CHANNEL_UNSUBSCRIBED` event. Therefore, Firebase initialization must be handled manually and carefully in multi-process apps.

For more information, refer to the following resources:

* [Firebase Initialize on Android](https://firebase.blog/posts/2016/12/how-does-firebase-initialize-on-android)
* [Challenges of Building a Multi-Process Android App](https://medium.com/@elvischidera/challenges-of-building-a-multi-process-android-app-38fe4464da15)

### What are the steps to migrate from the Legacy API to the FCM v1 APIs?

For detailed steps, refer to [Configure Push Notifications](doc:android-push#configure-push-notifications). You can also check our video, which gives a quick walkthrough of the steps for migrating to the new FCM APIs.

### What is the new API rate limit? How does it impact push notification deliverability?

Each Firebase project using the HTTP v1 APIs is strictly limited to a specific number of requests per minute. For more information, refer to [HTTP v1 API Rate Limits](doc:android-push#http-v1-api-rate-limits).

### How do I resolve the "Wrong FCM API Key" error?

Ensure that you create a new Service Account from the correct Firebase project on the Firebase Console and assign it the _Firebase Cloud Messaging API Admin_ role, not the _Firebase Cloud Messaging Admin_ role. For more information, refer to [Safeguard Admin Credentials](doc:android-push#safeguard-admin-credentials).

<Image align="center" alt="Firebase Cloud Messaging API Admin" border={true} caption="Firebase Cloud Messaging API Admin" src="https://files.readme.io/0ae33f5-Assign_Role_to_Service_Account.png" width="85% " />

### Do we need to update the CleverTap SDK to migrate to HTTP v1 API?

No. You do not need to update the CleverTap SDK. You just need to upload the FCM Credentials on the CleverTap Dashboard.

### I have already uploaded the credentials. Is there anything else I need to do to migrate to the HTTP v1 API?

No further steps are required. Once credentials are added, CleverTap automatically transitions from legacy APIs to v1 APIs. Ensure that you always add throttle for Push Notification to keep the rate limit in check.

### What happens if I fail to migrate to new v1 APIs before June 2024?

You will not be able to send Push Notifications via legacy credentials; instead, it results in an error.

### How can I increase the rate limit for my account?

To request an increase in the rate limit, contact [Firebase Support](https://firebase.google.com/support).

### Why does FCM fail when using a custom `FirebaseOptions.Builder`, and how do I fix it?

When Firebase is initialized using a custom `FirebaseOptions.Builder`, Firebase Cloud Messaging (FCM) requires the **GCM/FCM Sender ID** to be explicitly set. If `setGcmSenderId()` is missing, FCM cannot register correctly, which causes push notification issues.

To fix this, add the Sender ID (from the Firebase console) to your builder:

```java
builder.setGcmSenderId(YOUR_FCM_SENDER_ID);
```

This resolves FCM issues when using the CleverTap Android SDK with custom Firebase initialization.
