---
title: Android SDK_In_app_WIP
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
[block:api-header]
{
  "title": "Overview"
}
[/block]
This section shows how to track custom events, enrich user profiles, and send push notifications with the CleverTap SDK in your Android app.  Before getting started, complete the [Android SDK Quick Start Guide](doc:android-quickstart-guide). The quick start guide shows how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard.

# User Profiles
CleverTap stores the user’s demographic data (gender, age, location), app and website interactions, campaign visits, and transaction history to give you a complete picture for every user.

A user profile is automatically created for every user launching your mobile application whether logged in or not.

At first, the user profile starts out as *Anonymous* which means that the profile does not contain any identifiable information about the user yet. You can choose to enrich the profile with attributes, such as name, age, and customer ID.

CleverTap provides pre-defined profile properties, such as name, phone, gender, and age to represent well-known properties associated with a profile. It is strongly recommended to use these standard property names. A list of all pre-defined property names is available. In addition, we also support arbitrary single and multi-value profile properties. 

Below are examples of updating these type of properties:
[block:code]
{
  "codes": [
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nHashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Name\", \"Jack Montana\");                  // String\nprofileUpdate.put(\"Identity\", 61026032);                    // String or number\nprofileUpdate.put(\"Email\", \"jack@gmail.com\");               // Email address of the user\nprofileUpdate.put(\"Phone\", \"+14155551234\");                 // Phone (with the country code, starting with +)\nprofileUpdate.put(\"Gender\", \"M\");                           // Can be either M or F\nprofileUpdate.put(\"Employed\", \"Y\");                         // Can be either Y or N\nprofileUpdate.put(\"Education\", \"Graduate\");                 // Can be either Graduate, College or School\nprofileUpdate.put(\"Married\", \"Y\");                          // Can be either Y or N\nprofileUpdate.put(\"DOB\", new Date());                       // Date of Birth. Set the Date object to the appropriate value first\nprofileUpdate.put(\"Tz\", \"Asia/Kolkata\");                    //an abbreviation such as \"PST\", a full name such as \"America/Los_Angeles\", \n                                                            //or a custom ID such as \"GMT-8:00\"\nprofileUpdate.put(\"Photo\", \"www.foobar.com/image.jpeg\");    // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate.put(\"MSG-email\", false);                      // Disable email notifications\nprofileUpdate.put(\"MSG-push\", true);                        // Enable push notifications\nprofileUpdate.put(\"MSG-sms\", false);                        // Disable SMS notifications\n profileUpdate.put(\"MSG-dndPhone\", true);                  // Opt out phone                                                                    number from SMS                                                                  notifications\n profileUpdate.put(\"MSG-dndEmail\", true);                  // Opt out phone                                                                    number from SMS                                                                  notifications\nArrayList<String> stuff = new ArrayList<String>();\nstuff.add(\"bag\");\nstuff.add(\"shoes\");\nprofileUpdate.put(\"MyStuff\", stuff);                        //ArrayList of Strings\n\nString[] otherStuff = {\"Jeans\",\"Perfume\"};\nprofileUpdate.put(\"MyStuff\", otherStuff);                   //String Array\n\nclevertapDefaultInstance.pushProfile(profileUpdate);",
      "language": "java",
      "name": "Predefined - JAVA"
    },
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nval profileUpdate = HashMap<String, Any>()\nprofileUpdate[\"Name\"] = \"Jack Montana\" // String\nprofileUpdate[\"Identity\"] = 61026032 // String or number\nprofileUpdate[\"Email\"] = \"jack@gmail.com\" // Email address of the user\nprofileUpdate[\"Phone\"] = \"+14155551234\" // Phone (with the country code, starting with +)\nprofileUpdate[\"Gender\"] = \"M\" // Can be either M or F\nprofileUpdate[\"Employed\"] = \"Y\" // Can be either Y or N\nprofileUpdate[\"Education\"] = \"Graduate\" // Can be either Graduate, College or School\nprofileUpdate[\"Married\"] = \"Y\" // Can be either Y or N\nprofileUpdate[\"DOB\"] = Date() // Date of Birth. Set the Date object to the appropriate value first\nprofileUpdate[\"Tz\"] = \"Asia/Kolkata\" //an abbreviation such as \"PST\", a full name such as \"America/Los_Angeles\" or a custom ID such as \"GMT-8:00\"\nprofileUpdate[\"Photo\"] = \"www.foobar.com/image.jpeg\" // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate[\"MSG-email\"] = false // Disable email notifications\nprofileUpdate[\"MSG-push\"] = true // Enable push notifications\nprofileUpdate[\"MSG-sms\"] = false // Disable SMS notifications\nprofileUpdate[\"MSG-dndPhone\"] = true // Opt out phone\nprofileUpdate[\"MSG-dndEmail\"] = true // Opt out email\nprofileUpdate[\"MyStuff\"] = arrayListOf(\"bag\", \"shoes\") //ArrayList of Strings\nprofileUpdate[\"MyStuff\"] = arrayOf(\"Jeans\", \"Perfume\") //String Array\nclevertapDefaultInstance?.pushProfile(profileUpdate)\n",
      "language": "kotlin",
      "name": "Predefined - Kotlin"
    },
    {
      "code": "HashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Customer Type\", \"Silver\");\nprofileUpdate.put(\"Prefered Language\", \"English\");\n\nclevertapDefaultInstance.pushEvent(profileUpdate);\n\n/**\n * Data types\n * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n */",
      "language": "java",
      "name": "Arbitrary Single Value - JAVA"
    },
    {
      "code": "// To set a multi-value property\nclevertapDefaultInstance?.setMultiValuesForKey(\"mystuff\", arrayListOf(\"bag\",\"shoes\"))\n // To add an additional value(s) to a multi-value property\nclevertapDefaultInstance?.addMultiValueForKey(\"mystuff\", \"coat\")\n// or\nclevertapDefaultInstance?.addMultiValuesForKey(\"mystuff\", arrayListOf(\"socks\", \"scarf\"))\n// To remove a value(s) from a multi-value property\nclevertapDefaultInstance?.removeMultiValueForKey(\"mystuff\", \"bag\")\n// or\nclevertapDefaultInstance?.removeMultiValuesForKey(\"mystuff\", arrayListOf(\"shoes\", \"coat\"))\n//To remove the value of a property (scalar or multi-value)\nclevertapDefaultInstance?.removeValueForKey(\"mystuff\")",
      "language": "kotlin",
      "name": "Arbitrary Multi Value - Kotlin"
    },
    {
      "code": "// To set a multi-value property\nArrayList<String> stuff = new ArrayList<String>();\nstuff.add(\"bag\");\nstuff.add(\"shoes\");\nclevertapDefaultInstance.setMultiValuesForKey(\"mystuff\", stuff);\n\n// To add an additional value(s) to a multi-value property\nclevertapDefaultInstance.addMultiValueForKey(\"mystuff\", \"coat\");\n// or\nArrayList<String> newStuff = new ArrayList<String>();\nnewStuff.add(\"socks\");\nnewStuff.add(\"scarf\");\nclevertapDefaultInstance.addMultiValuesForKey(\"mystuff\", newStuff);\n\n\n//To remove a value(s) from a multi-value property\nclevertapDefaultInstance.removeMultiValueForKey(\"mystuff\", \"bag\");\n// or\nArrayList<String> oldStuff = new ArrayList<String>();\noldStuff.add(\"shoes\");\noldStuff.add(\"coat\");\nclevertapDefaultInstance.removeMultiValuesForKey(\"mystuff\", oldStuff);\n\n//To remove the value of a property (scalar or multi-value)\nclevertapDefaultInstance.removeValueForKey(\"mystuff\");",
      "language": "java",
      "name": "Arbitrary Multi Value - JAVA"
    },
    {
      "code": "val profileUpdate = HashMap<String, Any>()\nprofileUpdate[\"Customer Type\"] = \"Silver\"\nprofileUpdate[\"Prefered Language\"] = \"English\"\nclevertapDefaultInstance?.pushEvent(profileUpdate)",
      "language": "kotlin",
      "name": "Arbitrary Single Value - Kotlin"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "You do not require to pass *Age* as a profile property in the API since it is automatically calculated and stored."
}
[/block]
CleverTap provides easy ways to enrich the user profile with data from sources, such as Facebook. You can also store custom attributes in a user profile which can be used to segment users later.

## DND 
You can opt out a user by either phone or email.

###Phone
You can opt out a user or a phone number:

  * To opt-out a specific user when multiple users share the same phone number, you can disable SMS for a specific user. Set the MSG-sms flag to *false* and the specified user stops receiving SMS; however, all other users sharing the number continue to receive messages if they have not opted out.  
  
  * To opt out a phone number and all the users associated with it, set the MSG-dndPhone flag to *true*.

For complete user profile documentation, refer to [User Profile API Endpoints](doc:user-profile-object).

###Email
You can opt-out a user or an email address: 

  * To opt-out a specific user when multiple users share the same email address, you can disable email for a specific user. Set the MSG-email flag to *false* and the specified user stops receiving messages; however, all other users sharing the email address continue to receive messages if they have not opted out.  
  
  * To opt-out an email address and all the users associated with it, set the MSG-dndEmail flag to *true*.

For complete user profile documentation, refer to [User Profile API Endpoints](doc:user-profile-object).

# User Events
A user event is an action a user takes in your mobile application. CleverTap records the event on the user profile using an event name, and optional associated key:value-based event properties. You can then segment users, target, and personalize messaging based on both the event name and specific event properties.

Below is an example of recording a user event called *Product Viewed*: 
[block:code]
{
  "codes": [
    {
      "code": "// event without properties\ncleverTap.pushEvent(\"Product viewed\");",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "// event without properties\nclevertapDefaultInstance?.pushEvent(\"Product viewed\")",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
Below is an example of recording a user event called *Product Viewed with Properties*:
[block:code]
{
  "codes": [
    {
      "code": "// event with properties\nHashMap<String, Object> prodViewedAction = new HashMap<String, Object>();\nprodViewedAction.put(\"Product Name\", \"Casio Chronograph Watch\");\nprodViewedAction.put(\"Category\", \"Mens Accessories\");\nprodViewedAction.put(\"Price\", 59.99);\nprodViewedAction.put(\"Date\", new java.util.Date());\n\nclevertapDefaultInstance.pushEvent(\"Product viewed\", prodViewedAction);\n\n/**\n * Data types\n * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n *\n * Date object\n * When a property value is of type Date, the date and time are both recorded to the second.\n * This can be later used for targeting scenarios.\n * For e.g. if you are recording the time of the flight as an event property,\n * you can send a message to the user just before their flight takes off.\n */",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "// event with properties\nval prodViewedAction = mapOf(\n\"Product Name\" to \"Casio Chronograph Watch\",\n\"Category\" to \"Mens Accessories\",\n\"Price\" to 59.99,\n\"Date\" to Date())\nclevertapDefaultInstance?.pushEvent(\"Product viewed\", prodViewedAction)\n/**\n* Data types\n* The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,\n* a Float, a Character, a String, or a Boolean.\n*\n* Date object\n* When a property value is of type Date, the date and time are both recorded to the second.\n* This can be later used for targeting scenarios.\n* For e.g. if you are recording the time of the flight as an event property,\n* you can send a message to the user just before their flight takes off.\n*/",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
Events help you understand how your users interact with your app. CleverTap tracks certain common user events automatically while giving you the flexibility to record business-specific events.

For a complete guide to recording events, refer to [Events](https://developer.clevertap.com/docs/concepts-events).

# Push Notifications

The easiest way to send push notifications is to use the default implementation by CleverTap, however, if you have your own implementation or use another push provider, refer to [Custom Android Push Notifications Handling](doc:android#section-custom-android-push-notifications-handling).

Starting with v3.0.0, the SDK supports FCM as an alternative to GCM.

If using FCM, inside the <application></application> tags, register the following services:
[block:code]
{
  "codes": [
    {
      "code": "<service\n    android:name=\"com.clevertap.android.sdk.pushnotification.fcm.FcmMessageListenerService\">\n    <intent-filter>\n        <action android:name=\"com.google.firebase.MESSAGING_EVENT\"/>\n    </intent-filter>\n</service>",
      "language": "xml"
    }
  ]
}
[/block]
If using CleverTap SDK v3.4.3 and above, the CleverTap SDK supports specifying a custom FCM Sender ID. The SDK will request for a token with the specified FCM Sender ID if it is present in the `AndroidManifest.xml`. If the FCM Sender ID is not present in the `AndroidManifest.xml` file, then the SDK will request for the token in the default manner which uses the app's `google-services.json` file. To use a custom FCM Sender ID, add the following field in your app's `AndroidManifest.xml`:
[block:code]
{
  "codes": [
    {
      "code": "<!-- replace below value with your FCM project number.-->\n<meta-data\n    android:name=\"FCM_SENDER_ID\"\n    android:value=\"id:1234567890\"/>",
      "language": "xml"
    }
  ]
}
[/block]
Opening a notification using CleverTap’s implementation will cause your app to be launched. If you are sending a deep link, then the link will be opened.

##Enable Push Notifications from the Dashboard

Follow these steps to enable push notifications: 
1. Navigate to *Settings* > *Channel* > *Mobile Push*.
2. Enter the credentials.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ea19d95-Push_Notification_Dashboard.png",
        "Push_Notification_Dashboard.png",
        1031,
        700,
        "#fafafa"
      ],
      "border": true
    }
  ]
}
[/block]
# Push Amplification
Starting with v3.4.0, the SDK supports push amplification. Push amplification provides the capability to reach users on devices that suppress notifications via GCM/FCM.

To allow your app to use CleverTap's push amplification via background ping service, add the following fields in your app's `AndroidManifest.xml`:
[block:code]
{
  "codes": [
    {
      "code": "<meta-data\n    android:name=\"CLEVERTAP_BACKGROUND_SYNC\"\n    android:value=\"1\"/>\n",
      "language": "xml"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Custom Handling",
  "body": "Additional integration is required if the app is rendering push notifications and not the CleverTap SDK. For more information, see [custom handling push amplification](https://developer.clevertap.com/docs/custom-handling-push-amplification)."
}
[/block]

## FAQ for Push Notification Errors in Android

###Invalid FCM API Key

An invalid FCM API key error is displayed when the FCM API key in the CleverTap dashboard is incorrect. You will not be able to send any notifications until the FCM API key is added to your settings.

Follow the steps below to update the FCM API key:
1. Navigate to *Settings* > *Engage* > *Channels* > *Mobile Push*. 
2. Update the valid FCM API key as it is displayed in your Firebase project. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/da79c70-FAQ_FCM_Credentials.png",
        "FAQ_FCM_Credentials.png",
        972,
        591,
        "#f9fafb"
      ],
      "caption": "",
      "border": true,
      "sizing": "full"
    }
  ]
}
[/block]
For more information, refer to [Find Your FCM Sender ID & FCM Server API Key](https://developer.clevertap.com/docs/find-your-fcm-sender-id-fcm-server-api-key).

###FCM Token Invalid

The FCM token is not linked to any device that has your app. If the token is not valid, notifications are not delivered. Check the format of the FCM project number and registration token you pass on to CleverTap.

For more information, refer to [Troubleshooting Push Notifications](https://developer.clevertap.com/docs/troubleshooting-push-notifications).

###FCM Error

This is a Firebase (Android) error. The FCM error is not part of the standard published list and is sometimes incorrectly identified as an Android bug.

###FCM Not Registered

Most likely, the users have uninstalled the app from their device. One of the common non-technical errors is *Undelivered due to App Uninstall* in campaign reports. This error occurs when the users uninstall your app while you are trying to reach them via a push notification.

The *FCM Not Registered* error occurs in two scenarios:

  * When the users have uninstalled your app and you are trying to schedule a push notification to them.
  * When users have cleared the device cache memory, which in turn removes the token from the device.

###FCM Mismatch Sender ID

An FCM sender ID mismatch occurs when the sender ID entered in the CleverTap dashboard under *Push Notification* settings does not match with the sender ID in the FCM project for your app created on Firebase.

You must match the sender ID/server keys at the following three locations:

  *  App-level which registers token with the said FCM sender ID mentioned.
  *  In the CleverTap dashboard.
  *  FCM project.

If you have checked the sender ID for mismatch and the error still persists, check the following: 

  * Does the app build and the CleverTap account contain mismatched sender IDs?
  * Do you have multiple Firebase projects?
  * Do you have multiple Platform IDs if your app is using two Firebase projects?

# Push Impressions
You can raise and record push notifications delivered onto your users’ Android devices. 

1. Navigate to *Settings* > *Schema* > *Events*.
2. Search for *Push Impressions*, then click on the vertical ellipsis.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ebf8ae7-Push_Impression_event_FAQ.png",
        "Push Impression event FAQ.png",
        1161,
        454,
        "#f7f7f9"
      ]
    }
  ]
}
[/block]
3. Click on **Setup push impressions**. A new window displays.
4. Click on **Save** to enable the option.

If you are custom-handling push notifications, add the following code after you render the push notifications: 
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI.getDefaultInstance(this).pushNotificationViewedEvent(extras);",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.getDefaultInstance(this@YourAndroidActivity)?.pushNotificationViewedEvent(extras)",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]

# Set the Small Notification Icon

The small icon is a notification bar icon displayed in the push notifications as shown in the image below:


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/fd3de03-Campaigns_Push_Small_Icon.png",
        "Campaigns_Push_Small_Icon.png",
        1080,
        2340,
        "#151516"
      ]
    }
  ]
}
[/block]
With the launch of Android 5 (Lollipop), the notification icons are rendered differently in the notification bar at the top. By default, our SDK uses the app’s icon for both, the notification icon as well as the notification bar icon; however, since Android 5, all non-alpha channels are ignored while drawing the main notification icon. For more information, refer to [Material design style](https://developer.android.com/about/versions/android-5.0-changes.html#NotificationsMaterialDesignStyle) in the Android documentation.

The small icon must to be an *alpha only* image which means the icon must have only one color and the rest of the image must transparent. The *alpha only* icon image is a combination of the colored region and the transparent region. 

In general, the logo displayed in the icon has a single color and the background is transparent. The alpha channel provides the ability to embed transparency within an image. This can be accomplished in tools such as Adobe Photoshop, Illustrator, and so on. You can also change the colored region of the small icon from the CleverTap dashboard. 

The CleverTap SDK fills the transparent section of the small icon with the selected color:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/87250b9-13.png",
        "13.png",
        2736,
        1578,
        "#e7e7eb"
      ],
      "border": true
    }
  ]
}
[/block]
Following is a small icon sample:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4b90e04-Burger_icon.png",
        "Burger_icon.png",
        36,
        30,
        "#f74b8a"
      ]
    }
  ]
}
[/block]
After you create an icon with the alpha channel, you can use it for your project in Android Studio. Simply save the icon file in the Drawable folder of your project. 

To set a custom notification icon (only for the small icon), add the following metadata entry in your AndroidManifest.xml.
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

[block:callout]
{
  "type": "danger",
  "body": "Only use the file name. Do not use the path such as @drawable/ic_stat_red_star as it will not work.",
  "title": "Warning"
}
[/block]
# In-App Notifications 

In-app notifications are pop-ups that you can show to your users while they are in your application. 


[block:callout]
{
  "type": "info",
  "title": "Activity Tag",
  "body": "The <activity> tag is not required for CleverTap SDK 3.5.1 and higher."
}
[/block]
To support in-app notifications, register the following activity in your AndroidManifest.xml:
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
Mention the activities on which you do not want in-app notifications to show in the android:value of the metadata tag.

Add the following implementation to your app's build.gradle file:
[block:code]
{
  "codes": [
    {
      "code": "implementation 'androidx.fragment:fragment:1.1.0'\n",
      "language": "groovy"
    }
  ]
}
[/block]
##Javascript Support in In-App Notifications
Android SDK v3.5.0 and above supports embedding JavaScript code inside custom in-apps. To make sure your JavaScript code works on the app while creating the in-app campaign, select the checkbox for *Enabling JavaScript during the in-app campaign* creation.

Below are all methods explained with examples:
[block:code]
{
  "codes": [
    {
      "code": "//Recording a User Event called Product Viewed in JS enabled custom In-Apps\nif (window.CleverTap) {\n  // Call Android interface             \n CleverTap.pushEvent(\"Product Viewed\");          \n}\n\n//Recording a User Event called Product Viewed with properties, in JS enabled custom In-Apps\nvar props = {foo: 'xyz', lang: 'French'};\nif (window.CleverTap) {\n  // Call Android interface             \n CleverTap.pushEvent(\"Product Viewed\",JSON.stringify(props));\n}\n\n//Recording a CHARGED Event with charge details and items, in in JS enabled custom In-Apps\nvar chargeDetails = {Amount: 300, Payment: 'Card'};\nvar items = [{Category: 'Books', Name: 'Book name', Quantity: 1},{Category: 'Clothes', Name: 'Cloth name', Quantity: 1},{Category: 'Food', Name: 'Food name', Quantity: 1}]\n\nif (window.CleverTap) {\n  // Call Android interface             \nCleverTap.pushChargedEvent(JSON.stringify(chargeDetails),JSON.stringify(items))           \n}\n\n//Updating profile properties of the User in JS enabled custom In-Apps\nvar props = {foo: 'xyz', lang: 'French'};\nif (window.CleverTap) {\n  // Call Android interface             \n CleverTap.pushProfile(JSON.stringify(props));          \n}\n\n//Adding user property for the User in JS enabled custom In-Apps\nif (window.CleverTap) {\n  // Call Android interface \nCleverTap.addMultiValueForKey('membership', 'gold');\n}\n\n//Adding multiple user properties for the User in JS enabled custom In-Apps\nvar cars = ['Saab', 'Volvo', 'BMW', 'Kia'];\nif (window.CleverTap) {\n  // Call Android interface \nCleverTap.addMultiValuesForKey('Cars', JSON.stringify(cars));\n}\n\n//removing a user property for a specific key in JS enabled custom In-Apps\nif (window.CleverTap) {\n// Call Android interface \nCleverTap.removeMultiValueForKey(‘Cars’, 'Saab');\n}\n\n//Removing multiple user properties for a specific key in JS enabled custom In-Apps\nvar cars = ['BMW', 'Kia'];\nif (window.CleverTap) {\n  // Call Android interface \nCleverTap.removeMultiValuesForKey('Cars', JSON.stringify(cars));\n}\n\n//Removing a user property by specifying a key in JS enabled custom In-Apps\nif (window.CleverTap) {\n  // Call Android interface \nCleverTap.removeValueForKey('Cars');\n}\n\n//Setting a user property by specifying the key in JS enabled custom In-Apps\n\nvar values = ['Mercedes','Bentley']\nif (window.CleverTap) {\n  // Call Android interface \nCleverTap.setMultiValueForKey('Cars', JSON.stringify(values));\n}",
      "language": "javascript",
      "name": "Javascript"
    }
  ]
}
[/block]
## Control In-App Notifications

With CleverTap Android SDK 4.2.0 and above, you can control the rendering of in-app notifications by calling the following methods: 
### `suspendInAppNotifications`
Suspends and saves in-app notifications until `resumeInAppNotifications` method is called for the current session.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapSingleton.getDefaultInstance(ctx).suspendInAppNotifications();\n\n\n",
      "language": "java"
    },
    {
      "code": "CleverTapAPI.getDefaultInstance(ctx)!!.suspendInAppNotifications()\n\n  ",
      "language": "kotlin"
    }
  ]
}
[/block]
### `discardInAppNotifications`
Suspends the display of in-app Notifications and discards the display of any new in-app notification. It also discards in-app notifications until `resumeInAppNotifications`  method is called for the current session.

[block:code]
{
  "codes": [
    {
      "code": "CleverTapSingleton.getDefaultInstance(ctx).discardInAppNotifications();\n\n",
      "language": "java"
    },
    {
      "code": "CleverTapAPI.getDefaultInstance(ctx)!!.discardInAppNotifications()\n  ",
      "language": "kotlin"
    }
  ]
}
[/block]
### `resumeInAppNotifications` API

Resumes displaying in-app notifications.

If you call this API after the `discardInAppNotifications()` method, it resumes the in-app notifications for events raised after the call is performed. 

However, if you call this API after the `suspendInAppNotifications()` method, then it displays all queued in-app notifications and also resumes in-app notifications for events raised after the call is performed. 
[block:code]
{
  "codes": [
    {
      "code": "CleverTapSingleton.getDefaultInstance(ctx).resumeInAppNotifications();\n\n\n",
      "language": "java"
    },
    {
      "code": "CleverTapAPI.getDefaultInstance(ctx)!!.resumeInAppNotifications()\n\n  ",
      "language": "kotlin"
    }
  ]
}
[/block]
##In-App Notification Button onClick Callbacks
Android SDK v3.6.1 and above supports callback on the click of in-app notification buttons by returning a map of key value pairs. To use this, make sure your activity implements the `InAppNotificationButtonListener` and override the following method: 

[block:code]
{
  "codes": [
    {
      "code": "@Override\npublic void onInAppButtonClick(HashMap<String, String> hashMap) {\n  if(hashMap != null){\n    //Read the values\n  }\n}",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "override fun onInAppButtonClick(hashMap: HashMap<String, String>?) {\n        // Read the values\n    }",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
Set the `InAppNotificationButtonListener` using the following code:
[block:code]
{
  "codes": [
    {
      "code": "cleverTapInstance.setInAppNotificationButtonListener(this);",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "cleverTapDefaultInstance?.apply {\n       setInAppNotificationButtonListener(this@YourAndroidActivity)\n   }",
      "language": "text",
      "name": "Kotlin"
    }
  ]
}
[/block]
##FAQ for In-App

###Q. How do I exclude in-app from the Android activity?

If your application has a splash screen (for example, logo screen or loading page) displayed on app launch, then the in-app triggered on *App Launch* would be attempted to be displayed on this screen. As soon as this screen is dismissed, the in-app will be dismissed too.

In these types of cases, the splash screen must be excluded. Mention the *Activities* on which you do not want in-app notifications to show in the android:value of the metadata tag in the AndroidManifest.xml file.
[block:code]
{
  "codes": [
    {
      "code": "<meta-data\n    android:name=\"CLEVERTAP_INAPP_EXCLUDE\"\n    android:value=\"YourSplashActivity1, YourSplashActivity2\" /> ",
      "language": "xml"
    }
  ]
}
[/block]

# App Inbox
  * The CleverTap SDK 3.4.0 and above provide the capability to create App Inbox notifications for your users.
  * You can use the App Inbox provided by CleverTap or create your own.
  * You can design App Inbox notifications right from the dashboard.

1. Add the following inbox dependencies in your app's build.gradle.
[block:code]
{
  "codes": [
    {
      "code": "//MANDATORY for App Inbox\n\nimplementation 'androidx.appcompat:appcompat:1.2.0'\nimplementation 'androidx.recyclerview:recyclerview:1.1.0'\nimplementation 'androidx.viewpager:viewpager:1.0.0'\nimplementation 'com.google.android.material:material:1.2.1'\nimplementation 'com.github.bumptech.glide:glide:4.11.0'\n\n//Optional ExoPlayer Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies\n\nimplementation 'com.google.android.exoplayer:exoplayer:2.11.5'\nimplementation 'com.google.android.exoplayer:exoplayer-hls:2.11.5'\nimplementation 'com.google.android.exoplayer:exoplayer-ui:2.11.5'",
      "language": "groovy"
    }
  ]
}
[/block]
2. Initializing the Inbox provides a callback to two methods `inboxDidInitialize()` AND `inboxMessagesDidUpdate()`
[block:code]
{
  "codes": [
    {
      "code": "import com.clevertap.android.sdk.CTInboxActivity;\nimport com.clevertap.android.sdk.CTInboxListener;\nimport com.clevertap.android.sdk.CTInboxStyleConfig;\nimport com.clevertap.android.sdk.CleverTapAPI;\nimport com.clevertap.android.sdk.CleverTapInstanceConfig;\n\npublic class MainActivity extends AppCompatActivity implements CTInboxListener {\n    @Override\n    protected void onCreate(Bundle savedInstanceState) {\n         private CleverTapAPI cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(this);\n         if (cleverTapDefaultInstance != null) {\n            //Set the Notification Inbox Listener\n            cleverTapDefaultInstance.setCTNotificationInboxListener(this);\n            //Initialize the inbox and wait for callbacks on overridden methods\n            cleverTapDefaultInstance.initializeInbox();\n        }\n    }\n}",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "import com.clevertap.android.sdk.CTInboxListener\nimport com.clevertap.android.sdk.CTInboxStyleConfig\nimport com.clevertap.android.sdk.CleverTapAPI\nimport com.clevertap.android.sdk.CleverTapInstanceConfig\n\nclass MainActivity:AppCompatActivity(), CTInboxListener {\noverride fun onCreate(savedInstanceState: Bundle?) {\n        super.onCreate(savedInstanceState)\n        val cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(this@MainActivity)\n\n        cleverTapDefaultInstance?.apply {\n            \n            ctNotificationInboxListener = this@MainActivity\n            \n            //Initialize the inbox and wait for callbacks on overridden methods\n            initializeInbox()\n        }\n    }\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
3. Configure styling and showing the inbox by:
  * Customize the config object and call the Inbox in the `inboxDidInitialize()` method
  * Call this method on the button click which opens the CleverTap Inbox for your App
[block:code]
{
  "codes": [
    {
      "code": "@Override\n    public void inboxDidInitialize() {\n        \n        yourInboxButton.setOnClickListener(v -> {\n            ArrayList<String> tabs = new ArrayList<>();\n            tabs.add(\"Promotions\");\n            tabs.add(\"Offers\");//We support upto 2 tabs only. Additional tabs will be ignored\n            \n            CTInboxStyleConfig styleConfig = new CTInboxStyleConfig();\n            styleConfig.setFirstTabTitle(\"First Tab\");\n            styleConfig.setTabs(tabs);//Do not use this if you don't want to use tabs\n            styleConfig.setTabBackgroundColor(\"#FF0000\");\n            styleConfig.setSelectedTabIndicatorColor(\"#0000FF\");\n            styleConfig.setSelectedTabColor(\"#0000FF\");\n            styleConfig.setUnselectedTabColor(\"#FFFFFF\");\n            styleConfig.setBackButtonColor(\"#FF0000\");\n            styleConfig.setNavBarTitleColor(\"#FF0000\");\n            styleConfig.setNavBarTitle(\"MY INBOX\");\n            styleConfig.setNavBarColor(\"#FFFFFF\");\n            styleConfig.setInboxBackgroundColor(\"#ADD8E6\");\n            if (ct != null) {\n                ct.showAppInbox(styleConfig); //With Tabs\n            }\n            //ct.showAppInbox();//Opens Activity with default style configs\n        });\n    }",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "fun inboxDidInitialize() {\n  yourInboxButton.setOnClickListener(object:View.OnClickListener() {\n    fun onClick(v:View) {\n      val inboxTabs =\n                    arrayListOf(\"Promotions\", \"Offers\", \"Others\")//Anything after the first 2 will be ignored\n                CTInboxStyleConfig().apply {\n                    tabs = inboxTabs //Do not use this if you don't want to use tabs\n                    tabBackgroundColor = \"#FF0000\"\n                    selectedTabIndicatorColor = \"#0000FF\"\n                    selectedTabColor = \"#000000\"\n                    unselectedTabColor = \"#FFFFFF\"\n                    backButtonColor = \"#FF0000\"\n                    navBarTitleColor = \"#FF0000\"\n                    navBarTitle = \"MY INBOX\"\n                    navBarColor = \"#FFFFFF\"\n                    inboxBackgroundColor = \"#00FF00\"\n                    firstTabTitle = \"First Tab\"\n                    cleverTapAPI?.showAppInbox(this) //Opens activity With Tabs\n                }\n      //OR\n      cleverTapDefaultInstance.showAppInbox()//Opens Activity with default style config\n    }\n  })\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
## Creating your own App Inbox
  * You can choose to create your own App Inbox by calling the following APIs
[block:code]
{
  "codes": [
    {
      "code": "//Initialize App Inbox\ncleverTapDefaultInstance.initializeInbox();\n\n//Get Inbox Message Count\ncleverTapDefaultInstance.getInboxMessageCount();\n\n//Get Inbox Unread Count\ncleverTapDefaultInstance.getInboxMessageUnreadCount();\n\n//Get All messages\ncleverTapDefaultInstance.getAllInboxMessages();\n\n//Get only Unread messages\ncleverTapDefaultInstance.getUnreadInboxMessages();\n\n//Get message object belonging to the given message id only. Message id should be a String\ncleverTapDefaultInstance.getInboxMessageForId(messageId);\n\n//Delete message from the Inbox. Message id should be a String\ncleverTapDefaultInstance.deleteInboxMessage(messageId);\n\n//Delete message from the Inbox. Message should object of CTInboxMessage\ncleverTapDefaultInstance.deleteInboxMessage(message);\n\n//Mark Message as Read. Message id should be a String\ncleverTapDefaultInstance.markReadInboxMessage(messageId);\n\n//Mark message as Read. Message should object of CTInboxMessage\ncleverTapDefaultInstance.markReadInboxMessage(message);\n\n//Raise Notification Viewed event for Inbox Message. Message id should be a String\ncleverTapDefaultInstance.pushInboxNotificationViewedEvent(messageId);\n\n//Raise Notification Clicked event for Inbox Message. Message id should be a String\ncleverTapDefaultInstance.pushInboxNotificationClickedEvent(messageId);\n\n//Callback on Inbox Message update/delete/read (any activity)\n@Override\npublic void inboxMessagesDidUpdate() {    }",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "cleverTapDefaultInstance?.apply {\n        //Initialize App Inbox\n        initializeInbox()\n\n        //Get Inbox Message Count\n        val msgCount = inboxMessageCount\n\n        //Get Inbox Unread Count\n        val unreadCount = inboxMessageUnreadCount\n\n        //Get All messages\n        allInboxMessages?.forEach {\n            println(\"inbox messages ID = ${it.messageId}\")\n        }\n\n        //Get All Unread messages\n        unreadInboxMessages?.forEach {\n            println(\"unread inbox messages ID = ${it.messageId}\")\n        }\n\n        //Get message object belonging to the given message id only. Message id should be a String\n        val inboxMessageForId = getInboxMessageForId(messageId)\n\n        //Delete message from the Inbox. Message id should be a String\n        deleteInboxMessage(messageId)\n\n        //Delete message from the Inbox. Message should object of CTInboxMessage\n        deleteInboxMessage(message)\n\n        //Mark Message as Read. Message id should be a String\n        markReadInboxMessage(messageId)\n\n        //Mark message as Read. Message should object of CTInboxMessage\n        markReadInboxMessage(message)\n\n        //Raise Notification Viewed event for Inbox Message. Message id should be a String\n        pushInboxNotificationViewedEvent(messageId)\n\n        //Raise Notification Clicked event for Inbox Message. Message id should be a String\n        pushInboxNotificationClickedEvent(messageId)\n    }\n\noverride fun inboxMessagesDidUpdate() {}\n",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
**App Inbox Button onClick Callbacks**
Android SDK v3.6.1 and above supports callback on the click of App Inbox Buttons by returning a Map of Key-Value pairs. To use this, make sure your activity implements the `InboxMessageButtonListener` and override the following method - 

[block:code]
{
  "codes": [
    {
      "code": "@Override\n    public void onInboxButtonClick(HashMap<String, String> hashMap) {\n        if(hashMap != null){\n            //Read the values\n        }\n    }",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "override fun onInboxButtonClick(payload: HashMap<String, String>?) {\n\tpayload?.apply { \n\t\tval key1 = get(\"key1\")\n\t\tval key2 = get(\"key2\")\n\t}\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
# Referral Tracking


From **CleverTap SDK v3.6.4** onwards, add the following Gradle dependency to capture UTM details, app-install time, referrer click time and other metrics provided by Google Install Referrer Library.
[block:code]
{
  "codes": [
    {
      "code": "implementation 'com.android.installreferrer:installreferrer:2.1'",
      "language": "groovy"
    }
  ]
}
[/block]
If you are upgrading from a lower CleverTap version to **v3.6.4 or above**, then please **remove** the following code from your `AndroidManifest.xml` file - 
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
# Debugging

By default, CleverTap logs are set to CleverTap.Loglevel.INFO. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the Android logging system. This can be done by setting the debug level to CleverTap.Loglevel.DEBUG. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTap.Loglevel.OFF.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.INFO);    //Default Log level\n\nCleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG);   //Set Log level to DEBUG log warnings or other important messages\n\nCleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.OFF);     //Switch off logs for Production environment",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.INFO) //Default Log level\n\nCleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG) //Set Log level to DEBUG log warnings or other important messages\n\nCleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.OFF) //Switch off logs for Production environment}",
      "language": "kotlin",
      "name": "Kotlin"
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
      "code": "android {\n   compileSdkVersion 26\n   buildToolsVersion \"26.0.1\"\n}\n",
      "language": "groovy"
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
      "code": "CleverTapAPI.createNotificationChannelGroup(getApplicationContext(),\"YourGroupId\",\"YourGroupName\");",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.createNotificationChannelGroup(getApplicationContext(), \"YourGroupId\", \"YourGroupName\")",
      "language": "kotlin",
      "name": "Kotlin"
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
      "code": "CleverTapAPI.createNotificationChannel(getApplicationContext(),\"YourChannelId\",\"YourChannelName\",\"YourChannelDescription\",NotificationManager.IMPORTANCE_MAX,\"YourGroupId\",true);\n\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.createNotificationChannel(getApplicationContext(), \"YourChannelId\", \"YourChannelName\", \"YourChannelDescription\", NotificationManager.IMPORTANCE_MAX, \"YourGroupId\", true)",
      "language": "text",
      "name": "Kotlin"
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
      "code": "CleverTapAPI.createNotificationChannel(getApplicationContext(),\"YourChannelId\",\"Your Channel Name\",\"Your Channel Description\",NotificationManager.IMPORTANCE_MAX,true);\n\n// Creating a Notification Channel With Sound Support\nCleverTapAPI.createNotificationChannel(getApplicationContext(),\"got\",\"Game of Thrones\",\"Game Of Thrones\",NotificationManager.IMPORTANCE_MAX,true,\"gameofthrones.mp3\");\n\n// How to add a sound file to your app : https://developer.clevertap.com/docs/add-a-sound-file-to-your-android-app\n\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.createNotificationChannel(getApplicationContext(),\"YourChannelId\",\"Your Channel Name\",\"Your Channel Description\",NotificationManager.IMPORTANCE_MAX,true)\n\n// Creating a Notification Channel With Sound Support\nCleverTapAPI.createNotificationChannel(getApplicationContext(),\"got\",\"Game of Thrones\",\"Game Of Thrones\",NotificationManager.IMPORTANCE_MAX,true,\"gameofthrones.mp3\")\n\n// How to add a sound file to your app : https://developer.clevertap.com/docs/add-a-sound-file-to-your-android-app\n",
      "language": "kotlin",
      "name": "Kotlin"
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
      "code": "CleverTapAPI.deleteNotificationChannel(getApplicationContext(),\"YourChannelId\");",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.deleteNotificationChannel(getApplicationContext(),\"YourChannelId\")",
      "language": "kotlin",
      "name": "Kotlin"
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
      "code": "CleverTapAPI.deleteNotificationChannelGroup(getApplicationContext(),\"YourGroupId\");",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.deleteNotificationChannelGroup(getApplicationContext(),\"YourGroupId\")",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
**Step 7: Sending Notifications via Dashboard**
To send Push Notifications, login to the CleverTap Dashboard and on the left navigation menu click on Engage > Campaigns. Then click on +Campaign button on the top right to select Mobile Push. Now, set up your Push Notification campaign by selecting when you want to send out your notification, who do you want to send it out to (in this case it will be Android users) and what kind of push notification you want to send out. Once you reach the what section, select the Single Message option. Select the checkbox which says Send Message to Android O.

Fill in the Channel with the channel id you used to create the notification channel in your app. This field is mandatory as Android O requires a valid channel to send notifications. Badge Icon can be set as the app icon or the small icon used to denote push notifications. The default is app icon. Badge ID is the number you want to set the Notification Badge with, by default it auto-increments. Both Badge Icon and Badge ID are optional.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/5d161a5-14.png",
        "14.png",
        1560,
        766,
        "#fbfcfd"
      ]
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Notification Channel ID settings",
  "body": "You can choose to have a notification channel ID as optional or mandatory for creating a campaign. Select 'Add notification channels for Android 8.0 and above' option. It is available at Settings>Integration>Push Notification. You can create a selection list of channel IDs for your messages."
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/078c833-15.png",
        "15.png",
        1460,
        740,
        "#f7f7f7"
      ]
    }
  ]
}
[/block]
# Custom Android Push Notifications Handling

**Step 1. Using Your Custom Implementation**
If you have your custom implementation for managing Android push notifications, you can inform CleverTap about the user’s FCM registration ID.

Follow the instructions below if you are using FCM.
[block:code]
{
  "codes": [
    {
      "code": "String fcmRegId = FirebaseInstanceId.getInstance().getToken();\nclevertapDefaultInstance.pushFcmRegistrationId(fcmRegId,true);",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "val fcmRegId = FirebaseInstanceId.getInstance().token\nclevertapDefaultInstance?.pushFcmRegistrationId(fcmRegId, true)",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]

**Step 2: Using Multiple Android Push Notification Providers**
CleverTap plays well with other Android push notification providers. You can configure your app to work with multiple push notification providers.

If you are using FCM, please follow these instructions.

Subclass FirebaseMessagingService as shown below.
[block:code]
{
  "codes": [
    {
      "code": "public class MyFcmMessageListenerService extends FirebaseMessagingService {\n\n    @Override\n    public void onMessageReceived(RemoteMessage message){\n        try {\n            if (message.getData().size() > 0) {\n                Bundle extras = new Bundle();\n                for (Map.Entry<String, String> entry : message.getData().entrySet()) {\n                    extras.putString(entry.getKey(), entry.getValue());\n                }\n\n                NotificationInfo info = CleverTapAPI.getNotificationInfo(extras);\n\n                if (info.fromCleverTap) {\n                    CleverTapAPI.createNotification(getApplicationContext(), extras);\n                } else {\n                    // not from CleverTap handle yourself or pass to another provider\n                }\n            }\n        } catch (Throwable t) {\n           Log.d(\"MYFCMLIST\", \"Error parsing FCM message\", t);\n        }\n    }\n}",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "class MyFcmMessageListenerService : FirebaseMessagingService() {\n\noverride fun onMessageReceived(message: RemoteMessage) {\n\n    message.data.apply {\n        try {\n            if (size > 0) {\n                val extras = Bundle()\n                for ((key, value) in this) {\n                    extras.putString(key, value)\n                }\n                val info = CleverTapAPI.getNotificationInfo(extras)\n                if (info.fromCleverTap) {\n                    CleverTapAPI.createNotification(applicationContext, extras)\n                } else {\n                    // not from CleverTap handle yourself or pass to another provider\n                }\n            }\n        } catch (t: Throwable) {\n            Log.d(\"MYFCMLIST\", \"Error parsing FCM message\", t)\n        }\n    }\n    }\n}",
      "language": "kotlin",
      "name": "Kotlin"
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
      "code": "<receiver\n    android:name=\"com.clevertap.android.sdk.pushnotification.CTPushNotificationReceiver\"\n    android:exported=\"false\"\n    android:enabled=\"true\">\n</receiver>",
      "language": "xml"
    }
  ]
}
[/block]
**Step 3: Structure of CleverTap FCM Payload** 

Here is the structure of the payload with a CleverTap FCM push notification. 
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
    "4-2": "Notification Title, if absent or empty, fallback to the app name.",
    "10-0": "wzrk_rnv",
    "10-1": "string",
    "10-2": "If present and not empty, it will raise Push Impressions event",
    "11-0": "wzrk_nms",
    "12-0": "wzrk_st",
    "13-0": "wzrk_clr",
    "11-1": "string",
    "11-2": "If present and not empty, it contains the summary text to be displayed along with the image.",
    "12-1": "string",
    "12-2": "If present and not empty, it contains the subtitle text which is displayed next to the App name.",
    "13-1": "string",
    "13-2": "If present and not empty, it contains the Hex value of the colour to be applied on the small icon (and app name for Android versions below Pie)"
  },
  "cols": 3,
  "rows": 14
}
[/block]
If the field nm is empty, ignore the notification. CleverTap sends out a dummy notification with the nm field empty to test for app uninstalls. In addition to the above, attach all keys starting with wzrk_ to your notification extras (For example, wzrk_id is used to track notification opens).


#Push Notification Callback

The CleverTap Android SDK v3.8.1 SDK gives you a callback when a Push Notification is clicked along with the entire payload. To receive this payload, implement the `CTPushNotificationListener` in your Application class or Main Activity.

To set the listener, use the following code - 
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());\ncleverTapAPI.setCTPushNotificationListener(this);\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.getDefaultInstance(applicationContext)?.apply {\n\tctPushNotificationListener = this@YourAndroidActivity\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]

Implement the following override method to receive the payload as a Map on the click of a notification -
[block:code]
{
  "codes": [
    {
      "code": "@Override\npublic void onNotificationClickedPayloadReceived(HashMap<String, Object> payload) {\n   //Use your custom logic for  the payload \n}\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "override fun onNotificationClickedPayloadReceived(payload: HashMap<String, Any>?) {\n    //Use your custom logic for  the payload \n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
# Advanced Android Push Notification Options

Apart from Title and Message, you have the below mentioned options to add to your Android push notification. Please note that each of these are optional.

##**Image URL**
If an image link is specified, a large image is added to your push notification.
  * Recommended resolution: 600×300
  * Max size: 40 kb
  * Supported file formats are .jpg and .png

##**Large Icon URL**
If a large icon link is specified, the large icon will be appended to the push notification. Based on the device, the large icon will be displayed either far left or far right.
  * Max resolution – 72×72
  * Max size – 1 kb

##**Deep Link/External URL**
Deep link helps you open a particular activity in your app on click of the notification. If left empty, the notification on click will open the launcher activity of the app. If you wish to use external URLs, then please whitelist the IPs or provide http/https before the URL so that they can be handled properly by the SDK.

##**Action Buttons**
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
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "intent.extras?.apply {\n            getString(\"actionId\")?.let {\n                Log.d(\"ACTION_ID\", it)\n                val autoCancel = getBoolean(\"autoCancel\", true)\n                val notificationId = getInt(\"notificationId\", -1)\n\n                if (autoCancel && notificationId > -1) {\n                    val notifyMgr: NotificationManager =\n                        applicationContext.getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager\n                    notifyMgr.cancel(notificationId) // the bit that cancels the notification\n                }\n                Toast.makeText(baseContext, \"Action ID is: $it\", Toast.LENGTH_SHORT).show()\n            }\n        }",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
The second option is that CleverTap handles closing the notification. You will have to add an additional CleverTap IntentService to your Manifest.xml and the SDK will do it for you automatically.
[block:code]
{
  "codes": [
    {
      "code": "<service\n    android:name=\"com.clevertap.android.sdk.pushnotification.CTNotificationIntentService\"\n    android:exported=\"false\">\n    <intent-filter>\n        <action android:name=\"com.clevertap.PUSH_EVENT\" />\n    </intent-filter>\n</service>",
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
##**Sound Files**
You can choose to have no notification sound, the default OS sound or use a custom sound. It has to be present in the resources folder of your app. Please see the [Android sound guide](doc:add-a-sound-file-to-your-android-app) to learn how to add a sound file to your android app. Android only supports .mp3, .ogg and .wav files.

##**Notification Tray Priority**
Relative priority for this notification in the device tray. Priority is an indication of how much of the user’s valuable attention should be consumed by this notification.
  * MAXIMUM: Use for critical and urgent notifications that alert the user to a condition that is time-critical or needs to be resolved before they can continue with a particular task. A notification with priority set to the maximum will be a heads up notification, and will always be at the top in the notification tray.
  * HIGH: Use primarily for important communication, such as message or chat events with content that is particularly interesting for the user. High-priority notifications trigger the heads-up notification display. A notification with priority set to high will be a heads up notification.
  * DEFAULT: Use for all notifications that don’t fall into any of the other priorities. A notification with default priority will simply show up in the notification tray and its order in the notification tray is subject to the presence of other notifications.


[block:callout]
{
  "type": "info",
  "body": "Notification tray priority is only applicable for Android 7.1 and below. For Android 8 and above, the notification tray priority is based on the notification channel.",
  "title": "Note"
}
[/block]
##**Notification Delivery Priority**
You have two options for assigning delivery priority to downstream messages on Android: normal and high priority. 

Normal priority - This is the default priority for messages on Android. Normal priority messages are delivered immediately when the app is in the foreground. When the device is in Doze, the delivery may be delayed to conserve battery. For less time-sensitive messages, such as notifications of new email, keeping your UI in sync, or syncing app data in the background, choose normal delivery priority.

High priority - FCM attempts to deliver high priority messages immediately, allowing the FCM service to wake a sleeping device when necessary and to run some limited processing (including very limited network access). High priority messages generally should result in user interaction with your app or its notifications. If FCM detects a pattern in which they don't, your messages may be de-prioritized. 

#Setting Native Display in Android
We support Native Display for CleverTap Android SDK version 3.6.2 and above. Follow the steps to integrate Native Display with your App. 

##Step 1: Register the Native Display Listener
The Activity class must implement DisplayUnitListener on which you are going to show the NativeDisplayUnits. Register the activity to the Clevertap’s instance using the setDisplayUnitListener method.
[block:code]
{
  "codes": [
    {
      "code": "import com.clevertap.android.sdk.CleverTapAPI;\nimport com.clevertap.android.sdk.displayunits.DisplayUnitListener;\nimport com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit;\nimport java.util.ArrayList;\npublic class MainActivity extends AppCompatActivity implements DisplayUnitListener {\n    @Override\n    protected void onCreate(Bundle savedInstanceState){\n        super.onCreate(savedInstanceState);\n        CleverTapAPI.getDefaultInstance(this).setDisplayUnitListener(this);\n    }\n    @Override\n    public void onDisplayUnitsLoaded(ArrayList<CleverTapDisplayUnit> units) {\n        // you will get display units here\n    }\n}\n\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "import android.os.Bundle\nimport androidx.appcompat.app.AppCompatActivity\nimport com.clevertap.android.sdk.CleverTapAPI\nimport com.clevertap.android.sdk.displayunits.DisplayUnitListener\nimport com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit\nimport java.util.ArrayList\n\nclass MainActivity: AppCompatActivity(), DisplayUnitListener {\n    override fun onCreate(savedInstanceState: Bundle?) {\n        super.onCreate(savedInstanceState)\n        val cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(this@MainActivity)\n\n        cleverTapDefaultInstance?.apply {\n            setDisplayUnitListener(this@MainActivity)\n        }\n    }\n    override fun onDisplayUnitsLoaded(units: ArrayList<CleverTapDisplayUnit>?) {\n        // you will get display units here\n    }\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
##Step 2: Get the NativeDisplayUnits

You can raise the event for the Native Display Campaign.
[block:code]
{
  "codes": [
    {
      "code": " CleverTapAPI.getDefaultInstance(this).pushEvent(\"Your Event Name\");\n\n",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.getDefaultInstance(this).pushEvent(\"Your Event Name\")",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
You receive the list of CleverTapDisplayUnits via the registered callback for all active Native Display campaigns. Implement your logic to create Native Display views using these CleverTapDisplayUnits.
[block:code]
{
  "codes": [
    {
      "code": "@Override\npublic void onDisplayUnitsLoaded(ArrayList<CleverTapDisplayUnit> units) {\n    // you will get display units here\n    // implement your logic to create your display views using these Display Units here\n    for (int i = 0; i <units.size() ; i++) {\n        CleverTapDisplayUnit unit = units.get(i);\n        prepareDisplayView(unit);\n    }\n}",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "override fun onDisplayUnitsLoaded(units:ArrayList<CleverTapDisplayUnit>) {\n  // you will get display units here\n  // implement your logic to create your display views using these Display Units here\n  for (i in 0 until units.size())\n  {\n    val unit = units.get(i)\n    prepareDisplayView(unit)\n  }\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
Refer to our model classes CleverTapDisplayUnit and CleverTapDisplayUnitContent for creating the Native Display view.
[block:code]
{
  "codes": [
    {
      "code": "/**\n * This model class holds the data of an individual Display Unit.\n */\npublic class CleverTapDisplayUnit implements Parcelable {\n    /**\n     * Display unit identifier\n     */\n    private String unitID;\n    /**\n     * Display Type Could be (banner,carousel,custom key value etc.)\n     */\n    private CTDisplayUnitType type;\n    /**\n     * Background Color\n     */\n    private String bgColor;\n    /**\n     * List of Display Content Items\n     */\n    private ArrayList<CleverTapDisplayUnitContent> contents;\n    /**\n     * Custom Key Value Pairs\n     */\n    private HashMap<String, String> customExtras;\n    /**\n     * Getter for the unitId of the Display Unit\n     *\n     * @return String\n     */\n    public String getUnitID() {\n        return unitID;\n    }\n    /**\n     * Getter for the Key Value pairs of the Display Unit\n     *\n     * @return HashMap<String, String>\n     */\n    public HashMap<String, String> getCustomExtras() {\n        return customExtras;\n    }\n    /**\n     * Getter for the hex-value background color of the Display Unit e.g. #000000\n     *\n     * @return String\n     */\n    public String getBgColor() {\n        return bgColor;\n    }\n    /**\n     * Getter for the DisplayUnitType of the Display Unit, Refer{@link CTDisplayUnitType}\n     *\n     * @return CTDisplayUnitType\n     */\n    public CTDisplayUnitType getType() {\n        return type;\n    }\n    /**\n     * Getter for the list of Content Display Unit Items.\n     *\n     * @return ArrayList<CleverTapDisplayUnitContent>\n     */\n    public ArrayList<CleverTapDisplayUnitContent> getContents() {\n        return contents;\n    }\n}",
      "language": "java",
      "name": "CleverTapDisplayUnit"
    },
    {
      "code": "/**\n * Content class for holding Display Unit Content Data\n */\npublic class CleverTapDisplayUnitContent implements Parcelable {\n    private String title;\n    private String titleColor;\n    private String message;\n    private String messageColor;\n    private String media;\n    private String contentType;\n    private String posterUrl;\n    private String actionUrl;\n    private String icon;\n    /**\n     * Getter for the title section of the Display Unit Content\n     * @return String\n     */\n    public String getTitle() {\n        return title;\n    }\n    /**\n     * Getter for the message section of the Display Unit Content\n     * @return String\n     */\n    public String getMessage() {\n        return message;\n    }\n    /**\n     * Getter for the media URL of the Display Unit Content\n     * @return String\n     */\n    public String getMedia() {\n        return media;\n    }\n    /**\n     * Getter for the action URL of the body of the Display Unit Content\n     * @return String\n     */\n    public String getActionUrl() {\n        return actionUrl;\n    }\n    /**\n     * Getter for the URL as String for the icon in case of Icon Message template\n     * @return String\n     */\n    public String getIcon() {\n        return icon;\n    }\n    /**\n     * Getter for the hex-code value of the title color e.g. #000000\n     * @return String\n     */\n    public String getTitleColor() {\n        return titleColor;\n    }\n    /**\n     * Getter for the hex-code value of the message color e.g. #000000\n     * @return String\n     */\n    public String getMessageColor() {\n        return messageColor;\n    }\n    /**\n     * Getter for the URL for the thumbnail of the video\n     * @return String\n     */\n    public String getPosterUrl() {\n        return posterUrl;\n    }\n    /**\n     * Getter for the content type of the media(image/gif/audio/video etc.)\n     *\n     * Refer{@link #mediaIsImage()}, {@link #mediaIsGIF()},\n     *      {@link #mediaIsAudio()} ,{@link #mediaIsVideo()}\n     * @return String\n     */\n    public String getContentType() {\n        return contentType;\n    }\n    /**\n     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is an image.\n     *\n     * @return boolean - | true, if the media type is image\n     *                   | false, if the media type is not an image\n     */\n    public boolean mediaIsImage() {\n        return contentType != null && this.media != null && contentType.startsWith(\"image\") && !contentType.equals(\"image/gif\");\n    }\n    /**\n     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is a GIF.\n     *\n     * @return boolean - | true, if the media type is GIF\n     *                   | false, if the media type is not a GIF\n     */\n    public boolean mediaIsGIF() {\n        return contentType != null && this.media != null && contentType.equals(\"image/gif\");\n    }\n    /**\n     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is a video.\n     *\n     * @return boolean - | true, if the media type is video\n     *                   | false, if the media type is not a video\n     */\n    public boolean mediaIsVideo() {\n        return contentType != null && this.media != null && contentType.startsWith(\"video\");\n    }\n    /**\n     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is an audio.\n     *\n     * @return boolean - | true, if the media type is audio\n     *                   | false, if the media type is not an audio\n     */\n    public boolean mediaIsAudio() {\n        return contentType != null && this.media != null && contentType.startsWith(\"audio\");\n    }\n}\n\n\n  ",
      "language": "java",
      "name": "CleverTapDisplayUnitContent"
    }
  ]
}
[/block]
We have also provided a few utility methods to get Adunits corresponding to the last event raised.
[block:code]
{
  "codes": [
    {
      "code": "/**\n * Getter for retrieving all the Display Units.\n *\n * @return ArrayList<CleverTapDisplayUnit> - could be null, if there is no Display Unit campaigns\n */\n@Nullable\npublic ArrayList<CleverTapDisplayUnit> getAllDisplayUnits();",
      "language": "java",
      "name": "Getting all the Display Units"
    },
    {
      "code": "/**\n * Getter for retrieving Display Unit using the unitID\n *\n * @param unitID - unitID of the Display Unit {@link CleverTapDisplayUnit#getUnitID()}\n * @return CleverTapDisplayUnit - could be null, if there is no Display Unit campaign with the identifier\n */\n@Nullable\npublic CleverTapDisplayUnit getDisplayUnitForId(String unitID);\n\n",
      "language": "java",
      "name": "Getting Display Unit using unitID"
    }
  ]
}
[/block]
##Step 3: Raise Events for Tracking
You can raise the following events for tracking:

###Notification Viewed Event
You can raise Notification Viewed event using pushDisplayUnitViewedEventForID method, whenever the user sees the Native Display. You must provide the unitID of the CleverTapDisplayUnit corresponding to the Native Display.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI.getDefaultInstance(this).pushDisplayUnitViewedEventForID(adUnit.getUnitID());",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.getDefaultInstance(this).pushDisplayUnitViewedEventForID(adUnit.getUnitID())",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
###Notification Clicked Event

You can raise the Notification Clicked event using the pushDisplayUnitClickedEventForID method whenever the Native Display is clicked by the user.  You must provide the unitID of the CleverTapDisplayUnit corresponding to the Native Display.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI.getDefaultInstance(this).pushDisplayUnitClickedEventForID(adUnit.getUnitID();;",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.getDefaultInstance(this).pushDisplayUnitClickedEventForID(adUnit.getUnitID())",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "The events that you raise are accepted automatically by the CleverTap dashboard.\nFor example, you have implemented the Native Display using RecyclerView and you raise a Notification Viewed event whenever the Native Display appears on the browser's viewport. If the users scroll through the app multiple times, then multiple Notification Viewed events are raised. \n\nIn this case, you might not want to raise multiple events. You can handle this scenario using the android framework callbacks.",
  "title": "Custom Handling"
}
[/block]
##Step 4: Get Native Display Units using Send Test

In the Send Test flow, the CleverTapDisplayUnit is delivered via a Push notification. As soon we receive the notification, we parse the notification payload containing the CleverTapDisplayUnit and keep them in memory.

You can get the CleverTapDisplayUnit payload by either of the following methods:

###Method 1: By getting all the Display Units
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI.getDefaultInstance(this).getAllDisplayUnits();",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "CleverTapAPI.getDefaultInstance(this).getAllDisplayUnits()",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
OR

###Method 2: Register the DisplayUnitListener in the Application Class

You will receive the CleverTapDisplayUnit.
[block:code]
{
  "codes": [
    {
      "code": "import com.clevertap.android.sdk.CleverTapAPI;\nimport com.clevertap.android.sdk.displayunits.DisplayUnitListener;\nimport com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit;\nimport java.util.ArrayList;\n\npublic class MyApplication extends Application implements DisplayUnitListener {\n    @Override\n    public void onCreate(){\n        super.onCreate();\n        CleverTapAPI.getDefaultInstance(this).setDisplayUnitListener(this);\n    }\n    @Override\n    public void onDisplayUnitsLoaded(ArrayList<CleverTapDisplayUnit> units) {\n        // you will get send test display units here\n    }\n}",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "import com.clevertap.android.sdk.CleverTapAPI\nimport com.clevertap.android.sdk.displayunits.DisplayUnitListener\nimport com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit\nimport java.util.ArrayList\n\nclass MyApplication:Application(), DisplayUnitListener {\n  fun onCreate() {\n    super.onCreate()\n    CleverTapAPI.getDefaultInstance(this).setDisplayUnitListener(this)\n  }\n  override fun onDisplayUnitsLoaded(units:ArrayList<CleverTapDisplayUnit>) {\n    // you will get send test display units here\n  }\n}",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]

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
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "import android.app.Application\nimport com.clevertap.android.sdk.ActivityLifecycleCallback\nimport com.clevertap.android.sdk.CleverTapAPI\n\nclass MyApplication:Application() {\n  override fun onCreate() {\n    CleverTapAPI.changeCredentials(\"Your account ID here\", \"Your account token here\")\n    ActivityLifecycleCallback.register(this) // Must be called before super.onCreate()\n    super.onCreate()\n  }\n}",
      "language": "kotlin",
      "name": "Kotlin"
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
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());\ncleverTapAPI.setLocation(location); //android.location.Location",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "val cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext())\ncleverTapAPI.setLocation(location) //android.location.Location}",
      "language": "kotlin",
      "name": "Kotlin"
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
      "code": "application.registerActivityLifecycleCallbacks(\n        new android.app.Application.ActivityLifecycleCallbacks() {\n        \n            @Override\n            public void onActivityCreated(Activity activity, Bundle bundle) {\n                CleverTapAPI.setAppForeground(true);\n                try {\n                    CleverTapAPI.getDefaultInstance(application).pushNotificationClickedEvent(activity.getIntent().getExtras());\n                } catch (Throwable t) {\n                    // Ignore\n                }\n                try {\n                    Intent intent = activity.getIntent();\n                    Uri data = intent.getData();\n                    CleverTapAPI.getDefaultInstance(application).pushDeepLink(data);\n                } catch (Throwable t) {\n                    // Ignore\n                }\n            }\n            \n            @Override\n            public void onActivityStarted(Activity activity) {\n            }\n            \n            @Override\n            public void onActivityResumed(Activity activity) {\n                try {\n                    CleverTapAPI.getDefaultInstance(application).onActivityResumed(activity);\n                } catch (Throwable t) {\n                    // Ignore\n                }\n            }\n            \n            @Override\n            public void onActivityPaused(Activity activity) {\n                try {\n                    CleverTapAPI.getDefaultInstance(application).onActivityPaused();\n                } catch (Throwable t) {\n                    // Ignore\n                }\n            }\n            \n            @Override\n            public void onActivityStopped(Activity activity) {\n            }\n            \n            @Override\n            public void onActivitySaveInstanceState(Activity activity, Bundle bundle) {\n            }\n            \n            @Override\n            public void onActivityDestroyed(Activity activity) {\n            }\n        }\n);",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "registerActivityLifecycleCallbacks(object : android.app.Application.ActivityLifecycleCallbacks {\n            override fun onActivityCreated(activity: Activity, savedInstanceState: Bundle?) {\n                CleverTapAPI.setAppForeground(true)\n                try\n                {\n                    CleverTapAPI.getDefaultInstance(applicationContext)?.pushNotificationClickedEvent(activity.intent.extras)\n                }\n                catch (t:Throwable) {\n                    // Ignore\n                }\n                try\n                {\n                    val intent = activity.intent\n                    val data = intent.data\n                    CleverTapAPI.getDefaultInstance(applicationContext)?.pushDeepLink(data)\n                }\n                catch (t:Throwable) {\n                    // Ignore\n                }\n            }\n            override fun onActivityStarted(activity: Activity) {\n            }\n            override fun onActivityResumed(activity: Activity) {\n                try\n                {\n                    CleverTapAPI.onActivityResumed(activity)\n                }\n                catch (t:Throwable) {\n                    // Ignore\n                }\n            }\n            override fun onActivityPaused(activity: Activity) {\n                try\n                {\n                    CleverTapAPI.onActivityPaused()\n                }\n                catch (t:Throwable) {\n                    // Ignore\n                }\n            }\n            override fun onActivityStopped(activity: Activity) {\n            }\n\n            override fun onActivitySaveInstanceState(activity: Activity, outState: Bundle){\n            }\n            override fun onActivityDestroyed(activity: Activity) {\n            }\n        })",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]