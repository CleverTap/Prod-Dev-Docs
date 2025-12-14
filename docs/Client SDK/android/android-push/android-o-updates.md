---
title: Android O Updates
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

Android has modified sending push notifications to an app on the Oreo version. Starting with CleverTap SDK v3.1.7, we support Android Oreo's latest features like Notification Channels and Notification Badges.

# Prerequisites

As Notification channels and badges are the features of the latest Android version, you must update your SDK and Build Tools to the latest versions. Ensure that in your build.gradle file, the `compileSdkVersion` is 26 (or above) and `buildToolsVersion` is  26.0.1 (or above).

```groovy
android {
   compileSdkVersion 26
   buildToolsVersion "26.0.1"
}
```

# Create Notification Channels

Once the CleverTap Android SDK is integrated successfully, You can create stand-alone notification channels in your app by using the following line of code.

```java JAVA
CleverTapAPI.createNotificationChannel(getApplicationContext(),"YourChannelId","Your Channel Name","Your Channel Description",NotificationManager.IMPORTANCE_MAX,true);

// Creating a Notification Channel With Sound Support
CleverTapAPI.createNotificationChannel(getApplicationContext(),"got","Game of Thrones","Game Of Thrones",NotificationManager.IMPORTANCE_MAX,true,"gameofthrones.mp3");

// How to add a sound file to your app : https://developer.clevertap.com/docs/add-a-sound-file-to-your-android-app
```
```kotlin Kotlin
CleverTapAPI.createNotificationChannel(getApplicationContext(),"YourChannelId","Your Channel Name","Your Channel Description",NotificationManager.IMPORTANCE_MAX,true)

// Creating a Notification Channel With Sound Support
CleverTapAPI.createNotificationChannel(getApplicationContext(),"got","Game of Thrones","Game Of Thrones",NotificationManager.IMPORTANCE_MAX,true,"gameofthrones.mp3")

// How to add a sound file to your app : https://developer.clevertap.com/docs/add-a-sound-file-to-your-android-app
```

#### Notification Channel Importance Levels

The notification channel's importance levels allow you to customize app notifications based on user preferences. The following are the different levels and their usage:

| Notification Channel Importance                                                                                             | Description                                                                                             |
| :-------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------ |
| [IMPORTANCE_MIN](https://developer.android.com/reference/android/app/NotificationManager?hl=en#IMPORTANCE_MIN) or 1         | Minimum notification importance. It only shows in the shade, below the fold in the Notification Drawer. |
| [IMPORTANCE_LOW](https://developer.android.com/reference/android/app/NotificationManager?hl=en#IMPORTANCE_LOW) or 2         | Low notification importance. It shows in the shade and potentially in the status bar.                   |
| [IMPORTANCE_DEFAULT](https://developer.android.com/reference/android/app/NotificationManager?hl=en#IMPORTANCE_DEFAULT) or 3 | Default notification importance. It shows everywhere and makes noise but does not visually intrude.     |
| [IMPORTANCE_HIGH](https://developer.android.com/reference/android/app/NotificationManager?hl=en#IMPORTANCE_HIGH) or 4       | Higher notification importance. It shows everywhere, makes noise, and peeks.                            |
| [IMPORTANCE_MAX](https://developer.android.com/reference/android/app/NotificationManager?hl=en#IMPORTANCE_MAX) or 5         | Deprecated in Android 8.0. Use IMPORTANCE_HIGH or 4                                                     |

# Create Notification Channel Groups

Additionally, you can also create a notification channel group. Notification channel groups allow you to manage multiple notification channels with identical names within a single app, which is helpful if your app supports multiple accounts. You can create a notification group using the following line of code:

```java JAVA
CleverTapAPI.createNotificationChannelGroup(getApplicationContext(),"YourGroupId","YourGroupName");
```
```kotlin Kotlin
CleverTapAPI.createNotificationChannelGroup(getApplicationContext(), "YourGroupId", "YourGroupName")
```

Once you have created a notification channel group, you can use the group ID to create notification channels for that specific group. You can create a notification channel specifying the group id using the following line of code.

```java JAVA
CleverTapAPI.createNotificationChannel(getApplicationContext(),"YourChannelId","YourChannelName","YourChannelDescription",NotificationManager.IMPORTANCE_MAX,"YourGroupId",true);
```
```text Kotlin
CleverTapAPI.createNotificationChannel(getApplicationContext(), "YourChannelId", "YourChannelName", "YourChannelDescription", NotificationManager.IMPORTANCE_MAX, "YourGroupId", true)
```

You can create more than one channel using the above lines of code. Just make sure that the channel ID differs in every Notification Channel. Also, this Channel ID will be used to send Push Notifications using the CleverTap dashboard.

# Delete Notification Channels

You can delete the notification channels created previously in your app. There is no error thrown when you try to delete a notification channel that doesn't exist. You can delete the notification channels using the following line of code.

```java JAVA
CleverTapAPI.deleteNotificationChannel(getApplicationContext(),"YourChannelId");
```
```kotlin Kotlin
CleverTapAPI.deleteNotificationChannel(getApplicationContext(),"YourChannelId")
```

# Delete Notification Groups

CleverTap SDK also allows you to remove the notification groups you have created previously. Note that you will need to delete all the channels associated with a group before deleting a group. You can delete a notification group using the following line of code:

```java JAVA
CleverTapAPI.deleteNotificationChannelGroup(getApplicationContext(),"YourGroupId");
```
```kotlin Kotlin
CleverTapAPI.deleteNotificationChannelGroup(getApplicationContext(),"YourGroupId")
```

# Send Notifications via Dashboard

To send Push Notifications, login to the CleverTap Dashboard and perform the following steps:

1. Go to _Messages_ > _Campaigns_ from the left navigation. 
2. Click **+ Campaign** and select _Mobile Push_. 
3. Set up your Push Notification campaign by defining when you want to send out your notification, who you want to send it out to (in this case, it will be Android users), and what kind of push notification you want to send. 
4. Under the _What_ section, select the _Single Message_ option and then select  _Send Message to Android O_.
5. Fill in the Channel with the channel ID you used to create the notification channel in your app. This field is mandatory, as Android O requires a valid channel to send notifications. The Badge Icon can be set as the app icon or a small icon to denote push notifications. The default is the app icon. The Badge ID is the number you want to set the Notification Badge with. By default, it auto-increments. Both Badge Icon and Badge ID are optional.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/5d161a5-14.png",
        "Settings for Android 8.0 and Above",
        "Settings for Android 8.0 and Above"
      ],
      "align": "center",
      "sizing": "90% ",
      "border": true,
      "caption": "Settings for Android 8.0 and Above"
    }
  ]
}
[/block]


# Save a Channel ID

You can choose a _Notification channel Id_ as optional or mandatory for creating a campaign. Select _Add notification channels for Android 8.0 and above_ option. It is available from _Settings_ > _Channel_ > _Mobile Push_. You can create a selection list of channel IDs for your messages.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/078c833-15.png",
        "Save the Notification Settings",
        "Save the Notification Settings"
      ],
      "align": "center",
      "border": true,
      "caption": "Save the Notification Settings"
    }
  ]
}
[/block]


# Add a Custom Notification Icon

If your app targets devices with Android version Lollipop and above, you can set up a custom notification icon for your notifications. For more information, refer to  [Set the Small Icon Notification](https://developer.clevertap.com/docs/android-push#set-the-small-notification-icon).

# Default Notification Channel

Starting from `core v5.1.0`we have introduced a new feature that allows developers to define a  
default notification channel for their app. This feature provides flexibility in handling push  
notifications. 

> 📘 Note
> 
> Please note that this is only supported for CleverTap core notifications. Support for  
> push templates will be released soon.

To specify the default notification channel ID, you can add the following metadata to your app's  
manifest file:

```xml
<meta-data
    android:name="CLEVERTAP_DEFAULT_CHANNEL_ID"
    android:value="your_default_channel_id" />
```

By including this metadata, you can define a specific notification channel that CleverTap will use  
if the channel provided in the push payload is not registered by your app. This ensures that push  
notifications are displayed consistently even if the app's notification channels are not set up.

In case the SDK does not find the default channel ID specified in the manifest, it will  
automatically fall back to using a default channel called _Miscellaneous_. It ensures that push  
notifications are still delivered, even if no specific default channel is specified in the manifest.

This enhancement provides developers with greater control over the default notification channel used  
by CleverTap for push notifications, ensuring a seamless and customizable user experience.