---
title: Android
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
This guide will show you how to how to track custom events, enrich user profiles, and send push notifications with the CleverTap SDK in your Android app. 

If you haven’t already completed the [Android quickstart guide](doc:android-quickstart-guide), please complete that first before following this guide. The quickstart guide will show you how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard.

# User Profiles

CleverTap stores the user’s demographic data (gender, age, location), app and website interactions, campaign visits and transaction history to give you a complete picture for every user.

A User Profile is automatically created for every user launching your mobile application – whether logged in or not.

Initially, the User Profile starts out as “Anonymous” – which means that the profile does not yet contain any identifiable information about the user. You can choose to enrich the profile with attributes like name, age, customer id, etc.

CleverTap provides pre-defined profile properties such as name, phone, gender, age etc to represent well know properties associated with a profile. It is strongly recommended to use these standard property names. A list of all pre-defined property names is available here. In addition, we also support arbitrary single and multi value profile properties. Examples of updating these type of properties is documented below.

```java Predefined
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Name", "Jack Montana");                  // String
profileUpdate.put("Identity", 61026032);                    // String or number
profileUpdate.put("Email", "jack@gmail.com");               // Email address of the user
profileUpdate.put("Phone", "+14155551234");                 // Phone (with the country code, starting with +)
profileUpdate.put("Gender", "M");                           // Can be either M or F
profileUpdate.put("Employed", "Y");                         // Can be either Y or N
profileUpdate.put("Education", "Graduate");                 // Can be either Graduate, College or School
profileUpdate.put("Married", "Y");                          // Can be either Y or N
profileUpdate.put("DOB", new Date());                       // Date of Birth. Set the Date object to the appropriate value first
profileUpdate.put("Age", 28);                               // Not required if DOB is set
profileUpdate.put("Tz", "Asia/Kolkata");                    //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                                            //or a custom ID such as "GMT-8:00"
profileUpdate.put("Photo", "www.foobar.com/image.jpeg");    // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.put("MSG-email", false);                      // Disable email notifications
profileUpdate.put("MSG-push", true);                        // Enable push notifications
profileUpdate.put("MSG-sms", false);                        // Disable SMS notifications

ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
profileUpdate.put("MyStuff", stuff);                        //ArrayList of Strings

String[] otherStuff = {"Jeans","Perfume"};
profileUpdate.put("MyStuff", otherStuff);                   //String Array

cleverTap.profile.push(profileUpdate);
```
```text Arbitrary Single Value
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Customer Type", "Silver");
profileUpdate.put("Prefered Language", "English");

cleverTap.profile.push(profileUpdate);

/**
 * Data types
 * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 */
```
```text Arbitrary Multi Value
// To set a multi-value property
ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
cleverTap.profile.setMultiValuesForKey("mystuff", stuff);

// To add an additional value(s) to a multi-value property
cleverTap.profile.addMultiValueForKey("mystuff", "coat");
// or
ArrayList<String> newStuff = new ArrayList<String>();
newStuff.add("socks");
newStuff.add("scarf");
cleverTap.profile.addMultiValuesForKey("mystuff", newStuff);


//To remove a value(s) from a multi-value property
cleverTap.profile.removeMultiValueForKey("mystuff", "bag");
// or
ArrayList<String> oldStuff = new ArrayList<String>();
oldStuff.add("shoes");
oldStuff.add("coat");
cleverTap.profile.removeMultiValuesForKey("mystuff", oldStuff);

//To remove the value of a property (scalar or multi-value)
cleverTap.profile.removeValueForKey("mystuff");
```

CleverTap provides easy ways to enrich the user profile with data from sources like Facebook or Google Plus. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

For complete User Profile documentation, please see the [User Profile API Endpoints](doc:user-profile-object).

# User Events

A User Event is an action a user takes in your mobile application. CleverTap records the event on the User Profile, using an Event Name and optional associated key:value-based Event Properties. You can then segment users, target and personalize messaging based on both the Event Name and specific Event Properties.

An example of recording a User Event called Product Viewed.

```java
// event without properties
cleverTap.event.push("Product viewed");
```

An example of recording a User Event called Product Viewed with Properties:

```java
// event with properties
HashMap<String, Object> prodViewedAction = new HashMap<String, Object>();
prodViewedAction.put("Product Name", "Casio Chronograph Watch");
prodViewedAction.put("Category", "Mens Accessories");
prodViewedAction.put("Price", 59.99);
prodViewedAction.put("Date", new java.util.Date());

cleverTap.event.push("Product viewed", prodViewedAction);

/**
 * Data types
 * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 *
 * Date object
 * When a property value is of type Date, the date and time are both recorded to the second.
 * This can be later used for targeting scenarios.
 * For e.g. if you are recording the time of the flight as an event property,
 * you can send a message to the user just before their flight takes off.
 */
```

Events help you understand how your users interact with your app. CleverTap tracks certain common User Events automatically, while giving you the flexibility to record business specific events.

For a complete guide to recording Events, see: Working with Events; to capture customer transactions, see: Recording Customer Purchases.

# Push Notifications

The easiest way to send push notifications is to use the default implementation by CleverTap. However, if you have your own implementation, or use another push provider see the Custom Handling Push Notifications section

Starting with v3.0.0, the SDK supports FCM as an alternative to GCM.

**If using FCM:**

Inside the <application></application> tags, register the following services.

```xml
<service
    android:name="com.clevertap.android.sdk.FcmTokenListenerService">
    <intent-filter>
        <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
    </intent-filter>
</service>

<service
    android:name="com.clevertap.android.sdk.FcmMessageListenerService">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter>
</service>
```

**If using GCM:**

Add the following GCM Permissions, replacing com.your.package below with your application’s package name.

```xml
<uses-permission
    android:name="com.google.android.c2dm.permission.RECEIVE"/>
<uses-permission 
    android:name="android.permission.WAKE_LOCK" /> <!--if using CleverTap SDK v3.0.0+-->
<permission
    android:name="com.your.package.permission.C2D_MESSAGE"
    android:protectionLevel="signature"/>
<uses-permission
    android:name="com.your.package.permission.C2D_MESSAGE"/>
```

If using CleverTap SDK v3.0.0 or higher:

Inside the <application></application> tags, register the following receiver and services, and specify your GCM project number, replacing com.your.package with your application’s package name.

```xml
<!-- replace below value with your GCM project number. Separate multiple sender IDs with a comma -->
<meta-data android:name="GCM_SENDER_ID" android:value="id:1234567890"/> 

<meta-data
    android:name="com.google.android.gms.version"
    android:value="@integer/google_play_services_version"/>

<!-- replace com.your.package below with your package name-->
<receiver
    android:name="com.google.android.gms.gcm.GcmReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND" >
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="com.your.package" />
    </intent-filter>
</receiver>

<service
    android:name="com.clevertap.android.sdk.GcmMessageListenerService"
    android:exported="false" >
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    </intent-filter>
</service>

<service android:name="com.clevertap.android.sdk.GcmTokenListenerService"
    android:exported="false">
    <intent-filter>
        <action android:name="com.google.android.gms.iid.InstanceID"/>
    </intent-filter>
</service>
```

Opening a notification using CleverTap’s implementation will cause your app to be launched. If you’re sending a deep link, then the link will be opened

# **Set the Small Notification Icon**

With the launch of Android 5 (Lollipop), the notification icons are rendered differently in the notification bar at the top. By default, our SDK uses the app’s icon for both, the notification icon as well as the notification bar icon.

However, since Android 5, all non-alpha channels are ignored while drawing the main notification icon. See the official Android documentation here for more details.

To set a custom notification icon (only for small icon), add the following meta data entry in your AndroidManifest.xml

```xml
<meta-data
    android:name="CLEVERTAP_NOTIFICATION_ICON"
    android:value="ic_stat_red_star"/> <!-- name of your file in the drawable directory without the file extension. -->
```

Don’t use @drawable/ic\_stat\_red\_star. It will not work.

# In-App Notifications

In-App notifications are pop-ups that you can show to your users while they are in your application. To support in-app notifications, register the following activity in your AndroidManifest.xml:

```xml
<activity
    android:name="com.clevertap.android.sdk.InAppNotificationActivity"
    android:theme="@android:style/Theme.Translucent.NoTitleBar"
    android:configChanges="orientation|keyboardHidden"/>

<meta-data
    android:name="CLEVERTAP_INAPP_EXCLUDE"
    android:value="YourSplashActivity1, YourSplashActivity2" />
```

Mention the Activities on which you don’t want In-App notifications to show in the android:value of the metadata tag.

# Referral Tracking

Add the following between the <application></application> tags. This will enable you to capture UTM parameters for app installs

```xml
<receiver
    android:name="com.clevertap.android.sdk.InstallReferrerBroadcastReceiver"
    android:exported="true">
        <intent-filter>
            <action android:name="com.android.vending.INSTALL_REFERRER"/>
        </intent-filter>
</receiver>
```

# ProGuard

To be able to use ProGuard with the CleverTap SDK, you’ll need to add the following rule.

```xml
# For CleverTap SDK
-dontwarn com.clevertap.android.sdk.**
```

# Debugging

By default, CleverTap logs are set to CleverTap.Loglevel.INFO. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the Android logging system. This can be done by setting the debug level to CleverTap.Loglevel.DEBUG. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTap.Loglevel.OFF.

```java
CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.INFO);    //Default Log level

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG);   //Set Log level to DEBUG log warnings or other important messages

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.OFF);     //Switch off logs for Production environment
```

# Push Notifications for Android O

Android has modified the way push notifications can be sent to an app in its latest version Oreo. Starting with CleverTap SDK 3.1.7, we support Android Oreo’s latest features like Notification Channels and Notification Badges.

**Step 1: Prerequisites**\
Since Notification channels and badges are the features of the latest Android version, you as a developer will have to update your SDK and Build Tools to the latest versions. The latest SDK tools version is 26.0.2 and the latest Build Tools version is 26.0.1. You will have to make sure that in your build.gradle file, the compileSdkVersion is 26 and buildToolsVersion is 26.0.1

```xml
android {
   compileSdkVersion 26
   buildToolsVersion "26.0.1"
}
```

**Step 2: Creating Notification Channel Groups**\
Once you have the CleverTap Android SDK integrated successfully, you can create a notification channel group. Notification channel groups allow you to manage multiple notification channels with identical names within a single app, which can be useful if your app supports multiple accounts. You can create a notification group using the following line of code –

```java
CleverTapAPI cleverTapAPI = CleverTapAPI.getInstance(getApplicationContext());
cleverTapAPI.createNotificationChannelGroup(getApplicationContext(),"YourGroupId","YourGroupName");
```

**Step 3: Creating Notification Channels with Notification Groups**\
Once you have created a notification channel group you can use the group Id to create notification channels for that specific group. You can create a notification channel specifying the group id using the following line of code.

```java
CleverTapAPI cleverTapAPI = CleverTapAPI.getInstance(getApplicationContext());
cleverTapAPI.createNotificationChannel(getApplicationContext(),"YourChannelId","YourChannelName","YourChannelDescription",NotificationManager.IMPORTANCE_MAX,"YourGroupId",true);
```

**Step 4: Creating Notification Channels**\
You can create stand-alone notification channels in your app also, by using the following line of code.

```java
CleverTapAPI cleverTapAPI = CleverTapAPI.getInstance(getApplicationContext());
cleverTapAPI.createNotificationChannel(getApplicationContext(),"YourChannelId","Your Channel Name","Your Channel Description",NotificationManager.IMPORTANCE_MAX,true);
```

You can create more than one channel using the above line of code, just make sure that the channel ID differs in every Notification Channel. Also, this Channel ID will be used to send Push Notifications using the CleverTap dashboard.

**Step 5: Deleting Notification Channels**\
You can delete the notification channels created previously in your app. There is no error thrown when you try to delete a notification channel which doesn’t exist. You can delete the notification channels using the following line of code.

```java
CleverTapAPI cleverTapAPI = CleverTapAPI.getInstance(getApplicationContext());
cleverTapAPI.deleteNotificationChannel(getApplicationContext(),"YourChannelId");
```

**Step 6: Deleting Notification Groups**\
CleverTap SDK also allows you to remove the notification groups you have created previously. Please note that you will need to delete all the channels associated with a group prior to deleting a group. You can delete a notification group using the following line of code.

```java
CleverTapAPI cleverTapAPI = CleverTapAPI.getInstance(getApplicationContext());
cleverTapAPI.deleteNotificationChannelGroup(getApplicationContext(),"YourGroupId");
```

**Step 7: Sending Notifications via Dashboard**\
To send Push Notifications, login to the CleverTap Dashboard and on the left navigation menu click on Messages. Then click on +Message button on the top right to select Mobile Push. Now, set up your Push Notification campaign by selecting when you want to send out your notification, who do you want to send it out to (in this case it will be Android users) and what kind of push notification you want to send out. Once you reach the what section, select the Single Message option. Select the checkbox which says Send Message to Android O.

Fill in the Channel with the channel id you used to create the notification channel in your app. This field is mandatory as Android O requires a valid channel to send notifications. Badge Icon can be set as the app icon or the small icon used to denote push notifications. The default is app icon. Badge ID is the number you want to set the Notification Badge with, by default it auto-increments. Both Badge Icon and Badge ID are optional.

![706](https://files.readme.io/b2c3f30-dashboard1.png "dashboard1.png")

# Custom Android Push Notifications Handling

**Step 1. Using Your Custom Implementation**\
If you have your custom implementation for managing Android push notifications, you can inform CleverTap about the user’s GCM/FCM registration ID.

Please follow the instructions below if you are using GCM. If you are using FCM skip that to that section further below.

```java
String gcmRegId = gcm.register(projectNumber);
cleverTap.data.pushGcmRegistrationId(gcmRegId, true);
```

Add the following code to your GCM broadcast receiver.

```java
Bundle extras = intent.getExtras();
GoogleCloudMessaging gcm = GoogleCloudMessaging.getInstance(context);
String messageType = gcm.getMessageType(intent);
if (GoogleCloudMessaging. MESSAGE_TYPE_MESSAGE.equals(messageType)) {
  CleverTapAPI.createNotification(context, extras);
}
```

Please follow the instructions below if you are using FCM.

```java
String fcmRegId = FirebaseInstanceId.getInstance().getToken();
cleverTapAPI.data.pushFcmRegistrationId(fcmRegId,true);
```

For tracking Android push notifications, please ensure that the following method is called in every calling activity to enable tracking.

```java
@Override
protected void onNewIntent(Intent intent) {
     try {
         CleverTapAPI.getInstance(this).event.pushNotificationEvent(intent.getExtras());
     } catch (Throwable t) {
         t.printStackTrace();
     }
     super.onNewIntent(intent);
}
```

**Step 2: Using Multiple Android Push Notification Providers**\
CleverTap plays well with other Android push notification providers. You can configure your app to work with multiple push notification providers.

If you are using GCM, please follow the instructions below. If you are using FCM, please follow the instructions included further below.

In the below example, we use Parse as the other provider. The idea is to create your own GCM broadcast receiver, and then notify both CleverTap and Parse upon receiving a push message.

Subclass WakefulBroadcastReceiver as shown below. 

```java
public class MyCustomGcmBroadcastReceiver extends WakefulBroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        CleverTapAPI.createNotification(context, intent.getExtras());
        new com.parse.GcmBroadcastReceiver().onReceive(context, intent);
    }
}
```

Remove the CleverTap and Parse GCM broadcast receivers from your AndroidManifest.xml.

Now, add your new broadcast receiver.

```xml
<receiver
        android:name=“com.mypackage.path.to.MyCustomGcmBroadcastReceiver"
        android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
        <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>

        <category android:name="com.mypackage"/>
    </intent-filter>
</receiver>
```

Replace com.mypackage with your app package name. Replace com.mypackage.path.to with the correct package name which contains the newly created class, MyCustomGcmBroadcastReceiver.

You should now be able to confirm that push notifications from CleverTap and Parse are now working.

If you are using FCM, please follow these instructions.

Subclass FirebaseMessagingService as shown below.

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

Add your service to the AndroidManifest.xml in place of the CleverTap FcmMessageListenerService.

```xml
<service
    android:name="com.your.package.MyFcmMessageListenerService">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter>
</service>
```

Replace com.your.package with the correct package name which contains the newly created class, MyFcmMessageListenerService.

To track the push notification events and deeplinks add the following receiver in your AndroidManifest.xml.

```xml
<receiver
    android:name="com.clevertap.android.sdk.CTPushNotificationReceiver"
    android:exported="false"
    android:enabled="true">
</receiver>
```

# **Step 3: Structure of CleverTap GCM Payload**

Here is the structure of the payload with a CleverTap GCM push notification. 

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
        wzrk\_pn
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
        wzrk\_id
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
        wzrk\_bp
      </td>

      <td>
        string
      </td>

      <td>
        If present, the value will be a URL of an image, that needs to be displayed in the notification.
      </td>
    </tr>

    <tr>
      <td>
        wzrk\_sound
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
        Notification Title, if absent or empty, fallback to the app name.
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
        wzrk\_dl
      </td>

      <td>
        string
      </td>

      <td>
        If present, this is a deep link that must be followed at the time of notification open.
      </td>
    </tr>

    <tr>
      <td>
        wzrk\_d
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
        If present and not empty, it contains the URL of an image, that must be used as the small notification icon.
      </td>
    </tr>

    <tr>
      <td>
        wzrk\_pivot
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, it contains the URL of an image, it signifies the type of variant in A/B campaigns.
      </td>
    </tr>
  </tbody>
</Table>

If the field nm is empty, ignore the notification. CleverTap sends out a dummy notification with the nm field empty to test for app uninstalls. In addition to the above, attach all keys starting with wzrk\_ to your notification extras (For example, wzrk\_id is used to track notification opens).

# Advanced Android Push Notification Options

Apart from Title and Message, you have the below mentioned options to add to your Android push notification. Please note that each of these are optional.

**Image URL**\
If an image link is specified, a large image is added to your push notification.

* Recommended resolution: 600×300
* Max size: 40 kb
* Supported file formats are .jpg and .png

**Large Icon URL**\
If a large icon link is specified, the large icon will be appended to the push notification. Based on the device, the large icon will be displayed either far left or far right.

* Max resolution – 72×72
* Max size – 1 kb

**Deep Link/External URL**\
Deep link helps you open a particular activity in your app on click of the notification. If left empty, the notification on click will open the launcher activity of the app. If you wish to use external URLs, then please whitelist the IPs or provide http/https before the URL so that they can be handled properly by the SDK.

**Action Buttons**\
You can add upto 3 call to action buttons for every push notification. For every action button, you have the following options:

* Title: contains the Call to Action text (mandatory)
* Deep link: the deep link that should open on click of that button
* Action ID: a user defined string (applicable to apps custom handling their android push notifications: This string will be available as an extra on the notification click intent for you to identify the action button clicked) This is a mandatory field.
* Icon Resource Identifier: A drawable icon in your app’s resources folder to display the icon along with the notification for Android devices below OS version Nougat. Android Nougat does not display icons by default to give more room for buttons.

If the user clicks on the main body of the notification, the app will open and the notification will disappear. If the user clicks on one of the action buttons, then by default android will not remove the notification from the tray. We provide two user options for this.

The first option is to handle closing the notification yourself (applies to apps that are custom handling the push notification). To accomplish this, the click intent has the notification Id in its extras. So to close, please add the following code in the activity that would get called.

```java
Bundle extras = intent.getExtras();
if (extras != null) {
    String actionId = extras.getString("actionId");
    if (actionId != null) {
        Log.d("ACTION_ID", actionId);
        boolean autoCancel = extras.getBoolean("autoCancel", true);
        int notificationId = extras.getInt("notificationId", -1);
        if (autoCancel && notificationId > -1) {
            NotificationManager notifyMgr =
                    (NotificationManager) getApplicationContext().getSystemService(Context.NOTIFICATION_SERVICE);
            notifyMgr.cancel(notificationId);  // the bit that cancels the notification
        }
        Toast.makeText(getBaseContext(),"Action ID is: "+actionId,
                Toast.LENGTH_SHORT).show();
    }
}
```

The second option is that CleverTap handles closing the notification. You will have to add an additional CleverTap IntentService to your Manifest.xml and the SDK will do it for you automatically.

```xml
<service
     android:name="com.clevertap.android.sdk.CTNotificationIntentService"
     android:exported="false">
     <intent-filter>
         <action android:name="com.clevertap.PUSH_EVENT"/>
     </intent-filter>
 </service>
```

<Image title="push1.png" alt={996} className="border" border={true} src="https://files.readme.io/2c29710-push1.png" />

**Sound Files**\
You can choose to have no notification sound, the default OS sound or use a custom sound. It has to be present in the resources folder of your app. Please see the [Android sound guide](doc:add-a-sound-file-to-your-android-app) to learn how to add a sound file to your android app. Android only supports .mp3, .ogg and .wav files.

**Setting Priority**\
Relative priority for this notification. Priority is an indication of how much of the user’s valuable attention should be consumed by this notification.

* MAXIMUM: Use for critical and urgent notifications that alert the user to a condition that is time-critical or needs to be resolved before they can continue with a particular task. A notification with priority set to maximum will be a heads up notification, and will always be at the top in the notification tray.
* HIGH: Use primarily for important communication, such as message or chat events with content that is particularly interesting for the user. High-priority notifications trigger the heads-up notification display. A notification with priority set to high will be a heads up notification.
* DEFAULT: Use for all notifications that don’t fall into any of the other priorities. A notification with default priority will simply show up in the notification tray and its order in the notification tray is subject to presence of other notifications.

## Changing Account Credentials

If you’d not want to insert your account credentials in your app’s AndroidManifest.xml, or would like to change your account ID programmatically, you need to create a custom Application class (if you don’t have one already) with the following content.

```java
import android.app.Application;
import com.clevertap.android.sdk.ActivityLifecycleCallback;
import com.clevertap.android.sdk.CleverTapAPI;

public class MyApplication extends Application {
    @Override
    public void onCreate() {
        CleverTapAPI.changeCredentials("Your account ID here", "Your account token here");
        ActivityLifecycleCallback.register(this); // Must be called before super.onCreate()
        super.onCreate();
    }
}
```

If you’ve just created this subclass of Application, update your AndroidManifest.xml to specify your subclass.

```xml
<application
    android:label="@string/app_name"
    android:icon="@drawable/ic_launcher"
    android:name=".MyApplication">
```

If you have used this method, to set your CleverTap Account ID and Token, please do not add these values to your manifest file.

## Manually Updating User Location

The application is responsible for requesting the user’s permission to use location. If this permission is granted, location can be passed to CleverTap.

The App can pass the location to the SDK using setLocation.

```java
clevertap = CleverTapAPI.getInstance(getApplicationContext());
clevertap.setLocation(location); //android.location.Location
```

## Using Your Existing Activity Lifecycle Listener

If you’d like to use your own activity lifecycle listener, please update it to incorporate the following code.

```java
application.registerActivityLifecycleCallbacks(
        new android.app.Application.ActivityLifecycleCallbacks() {
        
            @Override
            public void onActivityCreated(Activity activity, Bundle bundle) {
                CleverTapAPI.setAppForeground(true);
                try {
                    CleverTapAPI.getInstance(application).event.pushNotificationEvent(activity.getIntent().getExtras());
                } catch (Throwable t) {
                    // Ignore
                }
                try {
                    Intent intent = activity.getIntent();
                    Uri data = intent.getData();
                    CleverTapAPI.getInstance(application).pushDeepLink(data);
                } catch (Throwable t) {
                    // Ignore
                }
            }
            
            @Override
            public void onActivityStarted(Activity activity) {
            }
            
            @Override
            public void onActivityResumed(Activity activity) {
                try {
                    CleverTapAPI.getInstance(application).activityResumed(activity);
                } catch (Throwable t) {
                    // Ignore
                }
            }
            
            @Override
            public void onActivityPaused(Activity activity) {
                try {
                    CleverTapAPI.getInstance(application).activityPaused(activity);
                } catch (Throwable t) {
                    // Ignore
                }
            }
            
            @Override
            public void onActivityStopped(Activity activity) {
            }
            
            @Override
            public void onActivitySaveInstanceState(Activity activity, Bundle bundle) {
            }
            
            @Override
            public void onActivityDestroyed(Activity activity) {
            }
        }
);
```
