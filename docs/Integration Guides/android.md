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
[block:code]
{
  "codes": [
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nHashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Name\", \"Jack Montana\");                  // String\nprofileUpdate.put(\"Identity\", 61026032);                    // String or number\nprofileUpdate.put(\"Email\", \"jack@gmail.com\");               // Email address of the user\nprofileUpdate.put(\"Phone\", \"+14155551234\");                 // Phone (with the country code, starting with +)\nprofileUpdate.put(\"Gender\", \"M\");                           // Can be either M or F\nprofileUpdate.put(\"Employed\", \"Y\");                         // Can be either Y or N\nprofileUpdate.put(\"Education\", \"Graduate\");                 // Can be either Graduate, College or School\nprofileUpdate.put(\"Married\", \"Y\");                          // Can be either Y or N\nprofileUpdate.put(\"DOB\", new Date());                       // Date of Birth. Set the Date object to the appropriate value first\nprofileUpdate.put(\"Age\", 28);                               // Not required if DOB is set\nprofileUpdate.put(\"Tz\", \"Asia/Kolkata\");                    //an abbreviation such as \"PST\", a full name such as \"America/Los_Angeles\", \n                                                            //or a custom ID such as \"GMT-8:00\"\nprofileUpdate.put(\"Photo\", \"www.foobar.com/image.jpeg\");    // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate.put(\"MSG-email\", false);                      // Disable email notifications\nprofileUpdate.put(\"MSG-push\", true);                        // Enable push notifications\nprofileUpdate.put(\"MSG-sms\", false);                        // Disable SMS notifications\n\nArrayList<String> stuff = new ArrayList<String>();\nstuff.add(\"bag\");\nstuff.add(\"shoes\");\nprofileUpdate.put(\"MyStuff\", stuff);                        //ArrayList of Strings\n\nString[] otherStuff = {\"Jeans\",\"Perfume\"};\nprofileUpdate.put(\"MyStuff\", otherStuff);                   //String Array\n\ncleverTap.profile.push(profileUpdate);",
      "language": "java",
      "name": "Predefined"
    },
    {
      "code": "HashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Customer Type\", \"Silver\");\nprofileUpdate.put(\"Prefered Language\", \"English\");\n\ncleverTap.profile.push(profileUpdate);\n\n/**\n * Data types\n * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n */",
      "language": "text",
      "name": "Arbitrary Single Value"
    },
    {
      "code": "// To set a multi-value property\nArrayList<String> stuff = new ArrayList<String>();\nstuff.add(\"bag\");\nstuff.add(\"shoes\");\ncleverTap.profile.setMultiValuesForKey(\"mystuff\", stuff);\n\n// To add an additional value(s) to a multi-value property\ncleverTap.profile.addMultiValueForKey(\"mystuff\", \"coat\");\n// or\nArrayList<String> newStuff = new ArrayList<String>();\nnewStuff.add(\"socks\");\nnewStuff.add(\"scarf\");\ncleverTap.profile.addMultiValuesForKey(\"mystuff\", newStuff);\n\n\n//To remove a value(s) from a multi-value property\ncleverTap.profile.removeMultiValueForKey(\"mystuff\", \"bag\");\n// or\nArrayList<String> oldStuff = new ArrayList<String>();\noldStuff.add(\"shoes\");\noldStuff.add(\"coat\");\ncleverTap.profile.removeMultiValuesForKey(\"mystuff\", oldStuff);\n\n//To remove the value of a property (scalar or multi-value)\ncleverTap.profile.removeValueForKey(\"mystuff\");",
      "language": "text",
      "name": "Arbitrary Multi Value"
    }
  ]
}
[/block]
CleverTap provides easy ways to enrich the user profile with data from sources like Facebook or Google Plus. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

For complete User Profile documentation, please see the [User Profile API Endpoints](doc:user-profile-object).

# User Events
A User Event is an action a user takes in your mobile application. CleverTap records the event on the User Profile, using an Event Name and optional associated key:value-based Event Properties. You can then segment users, target and personalize messaging based on both the Event Name and specific Event Properties.

An example of recording a User Event called Product Viewed.
[block:code]
{
  "codes": [
    {
      "code": "// event without properties\ncleverTap.event.push(\"Product viewed\");",
      "language": "java"
    }
  ]
}
[/block]
An example of recording a User Event called Product Viewed with Properties:
[block:code]
{
  "codes": [
    {
      "code": "// event with properties\nHashMap<String, Object> prodViewedAction = new HashMap<String, Object>();\nprodViewedAction.put(\"Product Name\", \"Casio Chronograph Watch\");\nprodViewedAction.put(\"Category\", \"Mens Accessories\");\nprodViewedAction.put(\"Price\", 59.99);\nprodViewedAction.put(\"Date\", new java.util.Date());\n\ncleverTap.event.push(\"Product viewed\", prodViewedAction);\n\n/**\n * Data types\n * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n *\n * Date object\n * When a property value is of type Date, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "java"
    }
  ]
}
[/block]
Events help you understand how your users interact with your app. CleverTap tracks certain common User Events automatically, while giving you the flexibility to record business specific events.

For a complete guide to recording Events, see: Working with Events; to capture customer transactions, see: Recording Customer Purchases.

# Push Notifications

The easiest way to send push notifications is to use the default implementation by CleverTap. However, if you have your own implementation, or use another push provider see the Custom Handling Push Notifications section

Starting with v3.0.0, the SDK supports FCM as an alternative to GCM.

**If using FCM:**

Inside the <application></application> tags, register the following services.
[block:code]
{
  "codes": [
    {
      "code": "<service\n    android:name=\"com.clevertap.android.sdk.FcmTokenListenerService\">\n    <intent-filter>\n        <action android:name=\"com.google.firebase.INSTANCE_ID_EVENT\"/>\n    </intent-filter>\n</service>\n\n<service\n    android:name=\"com.clevertap.android.sdk.FcmMessageListenerService\">\n    <intent-filter>\n        <action android:name=\"com.google.firebase.MESSAGING_EVENT\"/>\n    </intent-filter>\n</service>",
      "language": "xml"
    }
  ]
}
[/block]
**If using GCM:**

Add the following GCM Permissions, replacing com.your.package below with your application’s package name.
[block:code]
{
  "codes": [
    {
      "code": "<uses-permission\n    android:name=\"com.google.android.c2dm.permission.RECEIVE\"/>\n<uses-permission \n    android:name=\"android.permission.WAKE_LOCK\" /> <!--if using CleverTap SDK v3.0.0+-->\n<permission\n    android:name=\"com.your.package.permission.C2D_MESSAGE\"\n    android:protectionLevel=\"signature\"/>\n<uses-permission\n    android:name=\"com.your.package.permission.C2D_MESSAGE\"/>",
      "language": "xml"
    }
  ]
}
[/block]
If using CleverTap SDK v3.0.0 or higher:

Inside the <application></application> tags, register the following receiver and services, and specify your GCM project number, replacing com.your.package with your application’s package name.
[block:code]
{
  "codes": [
    {
      "code": "<!-- replace below value with your GCM project number. Separate multiple sender IDs with a comma -->\n<meta-data android:name=\"GCM_SENDER_ID\" android:value=\"id:1234567890\"/> \n\n<meta-data\n    android:name=\"com.google.android.gms.version\"\n    android:value=\"@integer/google_play_services_version\"/>\n\n<!-- replace com.your.package below with your package name-->\n<receiver\n    android:name=\"com.google.android.gms.gcm.GcmReceiver\"\n    android:exported=\"true\"\n    android:permission=\"com.google.android.c2dm.permission.SEND\" >\n    <intent-filter>\n        <action android:name=\"com.google.android.c2dm.intent.RECEIVE\" />\n        <action android:name=\"com.google.android.c2dm.intent.REGISTRATION\" />\n        <category android:name=\"com.your.package\" />\n    </intent-filter>\n</receiver>\n\n<service\n    android:name=\"com.clevertap.android.sdk.GcmMessageListenerService\"\n    android:exported=\"false\" >\n    <intent-filter>\n        <action android:name=\"com.google.android.c2dm.intent.RECEIVE\" />\n    </intent-filter>\n</service>\n\n<service android:name=\"com.clevertap.android.sdk.GcmTokenListenerService\"\n    android:exported=\"false\">\n    <intent-filter>\n        <action android:name=\"com.google.android.gms.iid.InstanceID\"/>\n    </intent-filter>\n</service>",
      "language": "xml"
    }
  ]
}
[/block]
Opening a notification using CleverTap’s implementation will cause your app to be launched. If you’re sending a deep link, then the link will be opened

#**Set the Small Notification Icon**

With the launch of Android 5 (Lollipop), the notification icons are rendered differently in the notification bar at the top. By default, our SDK uses the app’s icon for both, the notification icon as well as the notification bar icon.

However, since Android 5, all non-alpha channels are ignored while drawing the main notification icon. See the official Android documentation here for more details.

To set a custom notification icon (only for small icon), add the following meta data entry in your AndroidManifest.xml
[block:code]
{
  "codes": [
    {
      "code": "<meta-data\n    android:name=\"CLEVERTAP_NOTIFICATION_ICON\"\n    android:value=\"ic_stat_red_star\"/> <!-- name of your file in the drawable directory without the file extension. -->\n",
      "language": "xml"
    }
  ]
}
[/block]
Don’t use @drawable/ic_stat_red_star. It will not work.

# In-App Notifications 

In-App notifications are pop-ups that you can show to your users while they are in your application. To support in-app notifications, register the following activity in your AndroidManifest.xml:
[block:code]
{
  "codes": [
    {
      "code": "<activity\n    android:name=\"com.clevertap.android.sdk.InAppNotificationActivity\"\n    android:theme=\"@android:style/Theme.Translucent.NoTitleBar\"\n    android:configChanges=\"orientation|keyboardHidden\"/>\n\n<meta-data\n    android:name=\"CLEVERTAP_INAPP_EXCLUDE\"\n    android:value=\"YourSplashActivity1, YourSplashActivity2\" /> ",
      "language": "xml"
    }
  ]
}
[/block]
Mention the Activities on which you don’t want In-App notifications to show in the android:value of the metadata tag.

# Referral Tracking

Add the following between the <application></application> tags. This will enable you to capture UTM parameters for app installs
[block:code]
{
  "codes": [
    {
      "code": "<receiver\n    android:name=\"com.clevertap.android.sdk.InstallReferrerBroadcastReceiver\"\n    android:exported=\"true\">\n        <intent-filter>\n            <action android:name=\"com.android.vending.INSTALL_REFERRER\"/>\n        </intent-filter>\n</receiver>",
      "language": "xml"
    }
  ]
}
[/block]
# ProGuard

To be able to use ProGuard with the CleverTap SDK, you’ll need to add the following rule.
[block:code]
{
  "codes": [
    {
      "code": "# For CleverTap SDK\n-dontwarn com.clevertap.android.sdk.**",
      "language": "xml"
    }
  ]
}
[/block]
# Debugging

By default, CleverTap logs are set to CleverTap.Loglevel.INFO. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the Android logging system. This can be done by setting the debug level to CleverTap.Loglevel.DEBUG. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTap.Loglevel.OFF.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.INFO);    //Default Log level\n\nCleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG);   //Set Log level to DEBUG log warnings or other important messages\n\nCleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.OFF);     //Switch off logs for Production environment",
      "language": "java"
    }
  ]
}
[/block]
# Push Notifications for Android O

Android has modified the way push notifications can be sent to an app in its latest version Oreo. Starting with CleverTap SDK 3.1.7, we support Android Oreo’s latest features like Notification Channels and Notification Badges.

**Step 1: Prerequisites**
Since Notification channels and badges are the features of the latest Android version, you as a developer will have to update your SDK and Build Tools to the latest versions. The latest SDK tools version is 26.0.2 and the latest Build Tools version is 26.0.1. You will have to make sure that in your build.gradle file, the compileSdkVersion is 26 and buildToolsVersion is 26.0.1
[block:code]
{
  "codes": [
    {
      "code": "android {\n   compileSdkVersion 26\n   buildToolsVersion \"26.0.1\"\n}",
      "language": "xml"
    }
  ]
}
[/block]
**Step 2: Creating Notification Channel Groups**
Once you have the CleverTap Android SDK integrated successfully, you can create a notification channel group. Notification channel groups allow you to manage multiple notification channels with identical names within a single app, which can be useful if your app supports multiple accounts. You can create a notification group using the following line of code –
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getInstance(getApplicationContext());\ncleverTapAPI.createNotificationChannelGroup(getApplicationContext(),\"YourGroupId\",\"YourGroupName\");",
      "language": "java"
    }
  ]
}
[/block]
**Step 3: Creating Notification Channels with Notification Groups**
Once you have created a notification channel group you can use the group Id to create notification channels for that specific group. You can create a notification channel specifying the group id using the following line of code.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getInstance(getApplicationContext());\ncleverTapAPI.createNotificationChannel(getApplicationContext(),\"YourChannelId\",\"YourChannelName\",\"YourChannelDescription\",NotificationManager.IMPORTANCE_MAX,\"YourGroupId\",true);",
      "language": "java"
    }
  ]
}
[/block]
**Step 4: Creating Notification Channels**
You can create stand-alone notification channels in your app also, by using the following line of code.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getInstance(getApplicationContext());\ncleverTapAPI.createNotificationChannel(getApplicationContext(),\"YourChannelId\",\"Your Channel Name\",\"Your Channel Description\",NotificationManager.IMPORTANCE_MAX,true);",
      "language": "java"
    }
  ]
}
[/block]
You can create more than one channel using the above line of code, just make sure that the channel ID differs in every Notification Channel. Also, this Channel ID will be used to send Push Notifications using the CleverTap dashboard.

**Step 5: Deleting Notification Channels**
You can delete the notification channels created previously in your app. There is no error thrown when you try to delete a notification channel which doesn’t exist. You can delete the notification channels using the following line of code.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getInstance(getApplicationContext());\ncleverTapAPI.deleteNotificationChannel(getApplicationContext(),\"YourChannelId\");",
      "language": "java"
    }
  ]
}
[/block]
**Step 6: Deleting Notification Groups**
CleverTap SDK also allows you to remove the notification groups you have created previously. Please note that you will need to delete all the channels associated with a group prior to deleting a group. You can delete a notification group using the following line of code.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getInstance(getApplicationContext());\ncleverTapAPI.deleteNotificationChannelGroup(getApplicationContext(),\"YourGroupId\");",
      "language": "java"
    }
  ]
}
[/block]
**Step 7: Sending Notifications via Dashboard**
To send Push Notifications, login to the CleverTap Dashboard and on the left navigation menu click on Messages. Then click on +Message button on the top right to select Mobile Push. Now, set up your Push Notification campaign by selecting when you want to send out your notification, who do you want to send it out to (in this case it will be Android users) and what kind of push notification you want to send out. Once you reach the what section, select the Single Message option. Select the checkbox which says Send Message to Android O.

Fill in the Channel with the channel id you used to create the notification channel in your app. This field is mandatory as Android O requires a valid channel to send notifications. Badge Icon can be set as the app icon or the small icon used to denote push notifications. The default is app icon. Badge ID is the number you want to set the Notification Badge with, by default it auto-increments. Both Badge Icon and Badge ID are optional.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b2c3f30-dashboard1.png",
        "dashboard1.png",
        706,
        283,
        "#fbfbfb"
      ]
    }
  ]
}
[/block]
# Custom Android Push Notifications Handling

**Step 1. Using Your Custom Implementation**
If you have your custom implementation for managing Android push notifications, you can inform CleverTap about the user’s GCM/FCM registration ID.

Please follow the instructions below if you are using GCM. If you are using FCM skip that to that section further below.
[block:code]
{
  "codes": [
    {
      "code": "String gcmRegId = gcm.register(projectNumber);\ncleverTap.data.pushGcmRegistrationId(gcmRegId, true);",
      "language": "java"
    }
  ]
}
[/block]
Add the following code to your GCM broadcast receiver.
[block:code]
{
  "codes": [
    {
      "code": "Bundle extras = intent.getExtras();\nGoogleCloudMessaging gcm = GoogleCloudMessaging.getInstance(context);\nString messageType = gcm.getMessageType(intent);\nif (GoogleCloudMessaging. MESSAGE_TYPE_MESSAGE.equals(messageType)) {\n  CleverTapAPI.createNotification(context, extras);\n}",
      "language": "java"
    }
  ]
}
[/block]
Please follow the instructions below if you are using FCM.
[block:code]
{
  "codes": [
    {
      "code": "String fcmRegId = FirebaseInstanceId.getInstance().getToken();\ncleverTapAPI.data.pushFcmRegistrationId(fcmRegId,true);",
      "language": "java"
    }
  ]
}
[/block]
For tracking Android push notifications, please ensure that the following method is called in every calling activity to enable tracking.

[block:code]
{
  "codes": [
    {
      "code": "@Override\nprotected void onNewIntent(Intent intent) {\n     try {\n         CleverTapAPI.getInstance(this).event.pushNotificationEvent(intent.getExtras());\n     } catch (Throwable t) {\n         t.printStackTrace();\n     }\n     super.onNewIntent(intent);\n}",
      "language": "java"
    }
  ]
}
[/block]
**Step 2: Using Multiple Android Push Notification Providers**
CleverTap plays well with other Android push notification providers. You can configure your app to work with multiple push notification providers.

If you are using GCM, please follow the instructions below. If you are using FCM, please follow the instructions included further below.

In the below example, we use Parse as the other provider. The idea is to create your own GCM broadcast receiver, and then notify both CleverTap and Parse upon receiving a push message.

Subclass WakefulBroadcastReceiver as shown below. 
[block:code]
{
  "codes": [
    {
      "code": "public class MyCustomGcmBroadcastReceiver extends WakefulBroadcastReceiver {\n\n    @Override\n    public void onReceive(Context context, Intent intent) {\n        CleverTapAPI.createNotification(context, intent.getExtras());\n        new com.parse.GcmBroadcastReceiver().onReceive(context, intent);\n    }\n}",
      "language": "java"
    }
  ]
}
[/block]
Remove the CleverTap and Parse GCM broadcast receivers from your AndroidManifest.xml.

Now, add your new broadcast receiver.
[block:code]
{
  "codes": [
    {
      "code": "<receiver\n        android:name=“com.mypackage.path.to.MyCustomGcmBroadcastReceiver\"\n        android:permission=\"com.google.android.c2dm.permission.SEND\">\n    <intent-filter>\n        <action android:name=\"com.google.android.c2dm.intent.RECEIVE\"/>\n        <action android:name=\"com.google.android.c2dm.intent.REGISTRATION\"/>\n\n        <category android:name=\"com.mypackage\"/>\n    </intent-filter>\n</receiver>",
      "language": "xml"
    }
  ]
}
[/block]
Replace com.mypackage with your app package name. Replace com.mypackage.path.to with the correct package name which contains the newly created class, MyCustomGcmBroadcastReceiver.

You should now be able to confirm that push notifications from CleverTap and Parse are now working.

If you are using FCM, please follow these instructions.

Subclass FirebaseMessagingService as shown below.
[block:code]
{
  "codes": [
    {
      "code": "public class MyFcmMessageListenerService extends FirebaseMessagingService {\n\n    @Override\n    public void onMessageReceived(RemoteMessage message){\n        try {\n            if (message.getData().size() > 0) {\n                Bundle extras = new Bundle();\n                for (Map.Entry<String, String> entry : message.getData().entrySet()) {\n                    extras.putString(entry.getKey(), entry.getValue());\n                }\n\n                NotificationInfo info = CleverTapAPI.getNotificationInfo(extras);\n\n                if (info.fromCleverTap) {\n                    CleverTapAPI.createNotification(getApplicationContext(), extras);\n                } else {\n                    // not from CleverTap handle yourself or pass to another provider\n                }\n            }\n        } catch (Throwable t) {\n           Log.d(\"MYFCMLIST\", \"Error parsing FCM message\", t);\n        }\n    }\n}",
      "language": "java"
    }
  ]
}
[/block]
Add your service to the AndroidManifest.xml in place of the CleverTap FcmMessageListenerService.
[block:code]
{
  "codes": [
    {
      "code": "<service\n    android:name=\"com.your.package.MyFcmMessageListenerService\">\n    <intent-filter>\n        <action android:name=\"com.google.firebase.MESSAGING_EVENT\"/>\n    </intent-filter>\n</service>",
      "language": "xml"
    }
  ]
}
[/block]
Replace com.your.package with the correct package name which contains the newly created class, MyFcmMessageListenerService.

To track the push notification events and deeplinks add the following receiver in your AndroidManifest.xml.
[block:code]
{
  "codes": [
    {
      "code": "<receiver\n    android:name=\"com.clevertap.android.sdk.CTPushNotificationReceiver\"\n    android:exported=\"false\"\n    android:enabled=\"true\">\n</receiver>",
      "language": "xml"
    }
  ]
}
[/block]
# **Step 3: Structure of CleverTap GCM Payload** 

Here is the structure of the payload with a CleverTap GCM push notification. 
[block:parameters]
{
  "data": {
    "h-0": "Key",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "wzrk_pn",
    "0-1": "N/A",
    "0-2": "If present, this notification is sent from CleverTap.",
    "1-2": "Open rate tracking ID (can be empty or it might not be present).",
    "1-1": "string",
    "2-1": "string",
    "1-0": "wzrk_id",
    "2-0": "wzrk_bp",
    "3-0": "wzrk_sound",
    "3-2": "If present, it signifies that the default or custom Android notification sound must be played.",
    "2-2": "If present, the value will be a URL of an image, that needs to be displayed in the notification.",
    "3-1": "boolean or string",
    "4-0": "nt",
    "5-0": "nm",
    "6-0": "wzrk_dl",
    "7-0": "wzrk_d",
    "8-0": "ico",
    "9-0": "wzrk_pivot",
    "4-1": "string",
    "5-1": "string",
    "6-1": "string",
    "7-1": "N/A",
    "8-1": "string",
    "9-1": "string",
    "9-2": "If present and not empty, it contains the URL of an image, it signifies the type of variant in A/B campaigns.",
    "8-2": "If present and not empty, it contains the URL of an image, that must be used as the small notification icon.",
    "7-2": "If present, ignore this notification.",
    "6-2": "If present, this is a deep link that must be followed at the time of notification open.",
    "5-2": "Notification Body, if absent or empty, ignore this notification.",
    "4-2": "Notification Title, if absent or empty, fallback to the app name."
  },
  "cols": 3,
  "rows": 10
}
[/block]
If the field nm is empty, ignore the notification. CleverTap sends out a dummy notification with the nm field empty to test for app uninstalls. In addition to the above, attach all keys starting with wzrk_ to your notification extras (For example, wzrk_id is used to track notification opens).

# Advanced Android Push Notification Options

Apart from Title and Message, you have the below mentioned options to add to your Android push notification. Please note that each of these are optional.

**Image URL**
If an image link is specified, a large image is added to your push notification.
  * Recommended resolution: 600×300
  * Max size: 40 kb
  * Supported file formats are .jpg and .png

**Large Icon URL**
If a large icon link is specified, the large icon will be appended to the push notification. Based on the device, the large icon will be displayed either far left or far right.
  * Max resolution – 72×72
  * Max size – 1 kb

**Deep Link/External URL**
Deep link helps you open a particular activity in your app on click of the notification. If left empty, the notification on click will open the launcher activity of the app. If you wish to use external URLs, then please whitelist the IPs or provide http/https before the URL so that they can be handled properly by the SDK.

**Action Buttons**
You can add upto 3 call to action buttons for every push notification. For every action button, you have the following options:
  * Title: contains the Call to Action text (mandatory)
  * Deep link: the deep link that should open on click of that button
  * Action ID: a user defined string (applicable to apps custom handling their android push notifications: This string will be available as an extra on the notification click intent for you to identify the action button clicked) This is a mandatory field.
  * Icon Resource Identifier: A drawable icon in your app’s resources folder to display the icon along with the notification for Android devices below OS version Nougat. Android Nougat does not display icons by default to give more room for buttons.

If the user clicks on the main body of the notification, the app will open and the notification will disappear. If the user clicks on one of the action buttons, then by default android will not remove the notification from the tray. We provide two user options for this.

The first option is to handle closing the notification yourself (applies to apps that are custom handling the push notification). To accomplish this, the click intent has the notification Id in its extras. So to close, please add the following code in the activity that would get called.
[block:code]
{
  "codes": [
    {
      "code": "Bundle extras = intent.getExtras();\nif (extras != null) {\n    String actionId = extras.getString(\"actionId\");\n    if (actionId != null) {\n        Log.d(\"ACTION_ID\", actionId);\n        boolean autoCancel = extras.getBoolean(\"autoCancel\", true);\n        int notificationId = extras.getInt(\"notificationId\", -1);\n        if (autoCancel && notificationId > -1) {\n            NotificationManager notifyMgr =\n                    (NotificationManager) getApplicationContext().getSystemService(Context.NOTIFICATION_SERVICE);\n            notifyMgr.cancel(notificationId);  // the bit that cancels the notification\n        }\n        Toast.makeText(getBaseContext(),\"Action ID is: \"+actionId,\n                Toast.LENGTH_SHORT).show();\n    }\n}",
      "language": "java"
    }
  ]
}
[/block]
The second option is that CleverTap handles closing the notification. You will have to add an additional CleverTap IntentService to your Manifest.xml and the SDK will do it for you automatically.
[block:code]
{
  "codes": [
    {
      "code": "<service\n     android:name=\"com.clevertap.android.sdk.CTNotificationIntentService\"\n     android:exported=\"false\">\n     <intent-filter>\n         <action android:name=\"com.clevertap.PUSH_EVENT\"/>\n     </intent-filter>\n </service>",
      "language": "xml"
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2c29710-push1.png",
        "push1.png",
        996,
        865,
        "#c4c6c7"
      ],
      "border": true
    }
  ]
}
[/block]
**Sound Files**
You can choose to have no notification sound, the default OS sound or use a custom sound. It has to be present in the resources folder of your app. Please see the [Android sound guide](doc:add-a-sound-file-to-your-android-app) to learn how to add a sound file to your android app. Android only supports .mp3, .ogg and .wav files.

**Setting Priority**
Relative priority for this notification. Priority is an indication of how much of the user’s valuable attention should be consumed by this notification.
  * MAXIMUM: Use for critical and urgent notifications that alert the user to a condition that is time-critical or needs to be resolved before they can continue with a particular task. A notification with priority set to maximum will be a heads up notification, and will always be at the top in the notification tray.
  * HIGH: Use primarily for important communication, such as message or chat events with content that is particularly interesting for the user. High-priority notifications trigger the heads-up notification display. A notification with priority set to high will be a heads up notification.
  * DEFAULT: Use for all notifications that don’t fall into any of the other priorities. A notification with default priority will simply show up in the notification tray and its order in the notification tray is subject to presence of other notifications.
[block:api-header]
{
  "title": "Changing Account Credentials"
}
[/block]
If you’d not want to insert your account credentials in your app’s AndroidManifest.xml, or would like to change your account ID programmatically, you need to create a custom Application class (if you don’t have one already) with the following content.
[block:code]
{
  "codes": [
    {
      "code": "import android.app.Application;\nimport com.clevertap.android.sdk.ActivityLifecycleCallback;\nimport com.clevertap.android.sdk.CleverTapAPI;\n\npublic class MyApplication extends Application {\n    @Override\n    public void onCreate() {\n        CleverTapAPI.changeCredentials(\"Your account ID here\", \"Your account token here\");\n        ActivityLifecycleCallback.register(this); // Must be called before super.onCreate()\n        super.onCreate();\n    }\n}",
      "language": "java"
    }
  ]
}
[/block]
If you’ve just created this subclass of Application, update your AndroidManifest.xml to specify your subclass.
[block:code]
{
  "codes": [
    {
      "code": "<application\n    android:label=\"@string/app_name\"\n    android:icon=\"@drawable/ic_launcher\"\n    android:name=\".MyApplication\">",
      "language": "xml"
    }
  ]
}
[/block]
If you have used this method, to set your CleverTap Account ID and Token, please do not add these values to your manifest file.
[block:api-header]
{
  "title": "Manually Updating User Location"
}
[/block]
The application is responsible for requesting the user’s permission to use location. If this permission is granted, location can be passed to CleverTap.

The App can pass the location to the SDK using setLocation.
[block:code]
{
  "codes": [
    {
      "code": "clevertap = CleverTapAPI.getInstance(getApplicationContext());\nclevertap.setLocation(location); //android.location.Location",
      "language": "java"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Using Your Existing Activity Lifecycle Listener"
}
[/block]
If you’d like to use your own activity lifecycle listener, please update it to incorporate the following code.
[block:code]
{
  "codes": [
    {
      "code": "application.registerActivityLifecycleCallbacks(\n        new android.app.Application.ActivityLifecycleCallbacks() {\n        \n            @Override\n            public void onActivityCreated(Activity activity, Bundle bundle) {\n                CleverTapAPI.setAppForeground(true);\n                try {\n                    CleverTapAPI.getInstance(application).event.pushNotificationEvent(activity.getIntent().getExtras());\n                } catch (Throwable t) {\n                    // Ignore\n                }\n                try {\n                    Intent intent = activity.getIntent();\n                    Uri data = intent.getData();\n                    CleverTapAPI.getInstance(application).pushDeepLink(data);\n                } catch (Throwable t) {\n                    // Ignore\n                }\n            }\n            \n            @Override\n            public void onActivityStarted(Activity activity) {\n            }\n            \n            @Override\n            public void onActivityResumed(Activity activity) {\n                try {\n                    CleverTapAPI.getInstance(application).activityResumed(activity);\n                } catch (Throwable t) {\n                    // Ignore\n                }\n            }\n            \n            @Override\n            public void onActivityPaused(Activity activity) {\n                try {\n                    CleverTapAPI.getInstance(application).activityPaused(activity);\n                } catch (Throwable t) {\n                    // Ignore\n                }\n            }\n            \n            @Override\n            public void onActivityStopped(Activity activity) {\n            }\n            \n            @Override\n            public void onActivitySaveInstanceState(Activity activity, Bundle bundle) {\n            }\n            \n            @Override\n            public void onActivityDestroyed(Activity activity) {\n            }\n        }\n);",
      "language": "java"
    }
  ]
}
[/block]