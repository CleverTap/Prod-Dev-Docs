---
title: Discontinuation of Xiaomi Push Service
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Xiaomi Corporation made a significant announcement recently, notifying users about discontinuing the Mi Push service beyond Mainland China due to operational concerns. You might have already received communication regarding this matter. Refer to the following image:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/216611a-Discontinuation_Message.png",
        "",
        "Mi Push Service Discontinuation Notification"
      ],
      "align": "center",
      "border": true,
      "caption": "Xiaomi Push Service Discontinuation Notification"
    }
  ]
}
[/block]


Read the official announcement from the Xiaomi Corporation [here](https://dev.mi.com/distribute/doc/details?pId=1555).

# Who Does this Shutdown Affect?

The change solely affects App Push Notifications sent to Xiaomi devices via XPS.

# Impact of Shutdown

With the Mi Push service's closure, CleverTap will cease offering Mi Push support for Xiaomi devices. After the shutdown, Xiaomi devices will still receive push notifications through Firebase Cloud Messaging (FCM).

You can still use the CleverTap Push and Rendermax<sup>TM</sup> to send push notifications to Xiaomi devices along with all other OEM devices.

**Other changes that CleverTap would be making regarding Mi Push deprecation**

Starting from Android SDK version v6.1.1, CleverTap will discontinue the support for Mi Push. As a result, CleverTap will remove all the existing credentials and also the provision to set up these credentials from the CleverTap dashboard.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/403a8ce-image.png",
        null,
        "Mi Push Settings"
      ],
      "align": "center",
      "border": true,
      "caption": "Mi Push Settings"
    }
  ]
}
[/block]


# When Does CleverTap Plan to Implement the Changes?

To ensure a smooth transition and help our customers adapt to forthcoming adjustments, we will continue the Mi Push service for Xiaomi devices until March 30th, 2024. Starting March 31st, 2024, App Push on Xiaomi devices will be directed through the FCM service instead.

# Action Required

Upgrade your app to CleverTap Android SDK v6.1.1 and above and promptly remove the XPS SDK usage. This must be done immediately to prevent any potential errors or crashes. Following are the steps to remove the SDK usage manually - 

### Steps to remove the Xiaomi SDK usage :-

1. Remove the clevertap-xiaomi dependecy from build.gradle inside the app module. 
   ```Text Gradle
   implementation "com.clevertap.android:clevertap-xiaomi-sdk:x.x.x"
   ```
2. Remove the [Xiaomi Push Library](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev5.2.0_hmsv1.3.3_xpsv1.5.3) dependency declaration  from build.gradle inside the app module. 
   ```groovy Gradle
   implementation files("libs/MiPush_SDK_Client_.aar") 
   ```
3. Delete the[ Xiaomi Push Library](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev5.2.0_hmsv1.3.3_xpsv1.5.3) from the libs folder. 

   [block:image]{"images":[{"image":["https://files.readme.io/9c1ca1a-Screenshot_2024-02-22_at_5.58.40_PM.png","",""],"align":"center"}]}[/block]
4. Remove any obfuscation rules related to Xiaomi from the app’s proguard file. 

   [block:image]{"images":[{"image":["https://files.readme.io/c7ee907-Screenshot_2024-02-22_at_6.00.49_PM.png","",""],"align":"center"}]}[/block]
5. Remove `CLEVERTAP_XIAOMI_APP_KEY` and `"CLEVERTAP_XIAOMI_APP_ID"` meta data from `AndroidManifest.xml` 

   [block:image]{"images":[{"image":["https://files.readme.io/3e7db0a-Screenshot_2024-02-22_at_6.03.49_PM.png","",""],"align":"center"}]}[/block]
6. Remove any usage of Xiaomi related API (now deprecated) from ClevertapAPI.java
   - `CleverTapAPI.changeXiaomiCredentials(String xiaomiAppID, String xiaomiAppKey)`
   - `CleverTapAPI.enableXiaomiPushOn(PushConstants.XIAOMI_MIUI_DEVICES);`
   - `CleverTapAPI.pushXiaomiRegistrationId(String regId,@NonNull String region, boolean register);`
   - `CleverTapAPI.getEnableXiaomiPushOn();`
7. Remove Manual Configuration of Xiaomi Push **if added**

   1. Remove Xiaomi related services, permissions and receivers from the `AndroidManifest.xml`
   2. Remove Xiaomi related code from `MainActivity`

      [block:image]{"images":[{"image":["https://files.readme.io/08add73-Screenshot_2024-02-22_at_6.14.02_PM.png","",""],"align":"center"}]}[/block]
   3. Delete the custom Xiaomi BroadcastReceiver that extends `PushMessageReceiver`