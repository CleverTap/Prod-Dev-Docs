---
title: Android SDK
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
## Overview

This section shows how to track custom events, enrich user profiles, and send push notifications with the CleverTap SDK in your Android app.  Before getting started, complete the [Android SDK Quick Start Guide](doc:android-quickstart-guide). The quick start guide shows how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard.

# User Profiles

CleverTap stores the user’s demographic data (gender, age, location), app and website interactions, campaign visits, and transaction history to give you a complete picture for every user.

A user profile is automatically created for every user launching your mobile application whether logged in or not.

At first, the user profile starts out as *Anonymous* which means that the profile does not contain any identifiable information about the user yet. You can choose to enrich the profile with attributes, such as name, age, and customer ID.

CleverTap provides pre-defined profile properties, such as name, phone, gender, and age to represent well-known properties associated with a profile. It is strongly recommended to use these standard property names. A list of all pre-defined property names is available. In addition, we also support arbitrary single and multi-value profile properties. 

Below are examples of updating these type of properties:


```java Predefined - JAVA
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
profileUpdate.put("Tz", "Asia/Kolkata");                    //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                                            //or a custom ID such as "GMT-8:00"
profileUpdate.put("Photo", "www.foobar.com/image.jpeg");    // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.put("MSG-email", false);                      // Disable email notifications
profileUpdate.put("MSG-push", true);                        // Enable push notifications
profileUpdate.put("MSG-sms", false);                        // Disable SMS notifications
 profileUpdate.put("MSG-dndPhone", true);                  // Opt out phone                                                                    number from SMS                                                                  notifications
 profileUpdate.put("MSG-dndEmail", true);                  // Opt out phone                                                                    number from SMS                                                                  notifications
ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
profileUpdate.put("MyStuff", stuff);                        //ArrayList of Strings

String[] otherStuff = {"Jeans","Perfume"};
profileUpdate.put("MyStuff", otherStuff);                   //String Array

clevertapDefaultInstance.pushProfile(profileUpdate);
```
```kotlin Predefined - Kotlin
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
val profileUpdate = HashMap<String, Any>()
profileUpdate["Name"] = "Jack Montana" // String
profileUpdate["Identity"] = 61026032 // String or number
profileUpdate["Email"] = "jack@gmail.com" // Email address of the user
profileUpdate["Phone"] = "+14155551234" // Phone (with the country code, starting with +)
profileUpdate["Gender"] = "M" // Can be either M or F
profileUpdate["Employed"] = "Y" // Can be either Y or N
profileUpdate["Education"] = "Graduate" // Can be either Graduate, College or School
profileUpdate["Married"] = "Y" // Can be either Y or N
profileUpdate["DOB"] = Date() // Date of Birth. Set the Date object to the appropriate value first
profileUpdate["Tz"] = "Asia/Kolkata" //an abbreviation such as "PST", a full name such as "America/Los_Angeles" or a custom ID such as "GMT-8:00"
profileUpdate["Photo"] = "www.foobar.com/image.jpeg" // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate["MSG-email"] = false // Disable email notifications
profileUpdate["MSG-push"] = true // Enable push notifications
profileUpdate["MSG-sms"] = false // Disable SMS notifications
profileUpdate["MSG-dndPhone"] = true // Opt out phone
profileUpdate["MSG-dndEmail"] = true // Opt out email
profileUpdate["MyStuff"] = arrayListOf("bag", "shoes") //ArrayList of Strings
profileUpdate["MyStuff"] = arrayOf("Jeans", "Perfume") //String Array
clevertapDefaultInstance?.pushProfile(profileUpdate)
```
```java Arbitrary Single Value - JAVA
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Customer Type", "Silver");
profileUpdate.put("Prefered Language", "English");

clevertapDefaultInstance.pushEvent(profileUpdate);

/**
 * Data types
 * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 */
```
```kotlin Arbitrary Multi Value - Kotlin
// To set a multi-value property
clevertapDefaultInstance?.setMultiValuesForKey("mystuff", arrayListOf("bag","shoes"))
 // To add an additional value(s) to a multi-value property
clevertapDefaultInstance?.addMultiValueForKey("mystuff", "coat")
// or
clevertapDefaultInstance?.addMultiValuesForKey("mystuff", arrayListOf("socks", "scarf"))
// To remove a value(s) from a multi-value property
clevertapDefaultInstance?.removeMultiValueForKey("mystuff", "bag")
// or
clevertapDefaultInstance?.removeMultiValuesForKey("mystuff", arrayListOf("shoes", "coat"))
//To remove the value of a property (scalar or multi-value)
clevertapDefaultInstance?.removeValueForKey("mystuff")
```
```java Arbitrary Multi Value - JAVA
// To set a multi-value property
ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
clevertapDefaultInstance.setMultiValuesForKey("mystuff", stuff);

// To add an additional value(s) to a multi-value property
clevertapDefaultInstance.addMultiValueForKey("mystuff", "coat");
// or
ArrayList<String> newStuff = new ArrayList<String>();
newStuff.add("socks");
newStuff.add("scarf");
clevertapDefaultInstance.addMultiValuesForKey("mystuff", newStuff);


//To remove a value(s) from a multi-value property
clevertapDefaultInstance.removeMultiValueForKey("mystuff", "bag");
// or
ArrayList<String> oldStuff = new ArrayList<String>();
oldStuff.add("shoes");
oldStuff.add("coat");
clevertapDefaultInstance.removeMultiValuesForKey("mystuff", oldStuff);

//To remove the value of a property (scalar or multi-value)
clevertapDefaultInstance.removeValueForKey("mystuff");
```
```kotlin Arbitrary Single Value - Kotlin
val profileUpdate = HashMap<String, Any>()
profileUpdate["Customer Type"] = "Silver"
profileUpdate["Prefered Language"] = "English"
clevertapDefaultInstance?.pushEvent(profileUpdate)
```


> 📘 Note
>
> You do not require to pass *Age* as a profile property in the API since it is automatically calculated and stored.

CleverTap provides easy ways to enrich the user profile with data from sources, such as Facebook. You can also store custom attributes in a user profile which can be used to segment users later.

## DND

You can opt out a user by either phone or email.

### Phone

You can opt out a user or a phone number:

* To opt-out a specific user when multiple users share the same phone number, you can disable SMS for a specific user. Set the MSG-sms flag to *false* and the specified user stops receiving SMS; however, all other users sharing the number continue to receive messages if they have not opted out.  

* To opt out a phone number and all the users associated with it, set the MSG-dndPhone flag to *true*.

For complete user profile documentation, refer to [User Profile API Endpoints](doc:user-profile-object).

### Email

You can opt-out a user or an email address: 

* To opt-out a specific user when multiple users share the same email address, you can disable email for a specific user. Set the MSG-email flag to *false* and the specified user stops receiving messages; however, all other users sharing the email address continue to receive messages if they have not opted out.  

* To opt-out an email address and all the users associated with it, set the MSG-dndEmail flag to *true*.

For complete user profile documentation, refer to [User Profile API Endpoints](doc:user-profile-object).

# User Events

A user event is an action a user takes in your mobile application. CleverTap records the event on the user profile using an event name, and optional associated key:value-based event properties. You can then segment users, target, and personalize messaging based on both the event name and specific event properties.

Below is an example of recording a user event called *Product Viewed*: 

```java JAVA
// event without properties
cleverTap.pushEvent("Product viewed");
```
```kotlin Kotlin
// event without properties
clevertapDefaultInstance?.pushEvent("Product viewed")
```

Below is an example of recording a user event called *Product Viewed with Properties*:


```java JAVA
// event with properties
HashMap<String, Object> prodViewedAction = new HashMap<String, Object>();
prodViewedAction.put("Product Name", "Casio Chronograph Watch");
prodViewedAction.put("Category", "Mens Accessories");
prodViewedAction.put("Price", 59.99);
prodViewedAction.put("Date", new java.util.Date());

clevertapDefaultInstance.pushEvent("Product viewed", prodViewedAction);

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
```kotlin Kotlin
// event with properties
val prodViewedAction = mapOf(
"Product Name" to "Casio Chronograph Watch",
"Category" to "Mens Accessories",
"Price" to 59.99,
"Date" to Date())
clevertapDefaultInstance?.pushEvent("Product viewed", prodViewedAction)
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

Events help you understand how your users interact with your app. CleverTap tracks certain common user events automatically while giving you the flexibility to record business-specific events.

For a complete guide to recording events, refer to [Events](https://developer.clevertap.com/docs/concepts-events).

# Push Notifications

The easiest way to send push notifications is to use the default implementation by CleverTap, however, if you have your own implementation or use another push provider, refer to [Custom Android Push Notifications Handling](doc:android#section-custom-android-push-notifications-handling).

Starting with v3.0.0, the SDK supports FCM as an alternative to GCM.

If using FCM, inside the <application></application> tags, register the following services:













































































































































































































































If using CleverTap SDK v3.4.3 and above, the CleverTap SDK supports specifying a custom FCM Sender ID. The SDK will request for a token with the specified FCM Sender ID if it is present in the `AndroidManifest.xml`. If the FCM Sender ID is not present in the `AndroidManifest.xml` file, then the SDK will request for the token in the default manner which uses the app's `google-services.json` file. To use a custom FCM Sender ID, add the following field in your app's `AndroidManifest.xml`:























































































































































Opening a notification using CleverTap’s implementation will cause your app to be launched. If you are sending a deep link, then the link will be opened.

## Enable Push Notifications from the Dashboard

Follow these steps to enable push notifications: 

1. Navigate to *Settings* > *Channel* > *Mobile Push*.
2. Enter the credentials.

<Image title="Push_Notification_Dashboard.png" alt={1031} className="border" border={true} src="https://files.readme.io/ea19d95-Push_Notification_Dashboard.png" />

# Push Amplification

Starting with v3.4.0, the SDK supports push amplification. Push amplification provides the capability to reach users on devices that suppress notifications via GCM/FCM.

To allow your app to use CleverTap's push amplification via background ping service, add the following fields in your app's `AndroidManifest.xml`:





























































































> 🚧 Custom Handling
>
> Additional integration is required if the app is rendering push notifications and not the CleverTap SDK. For more information, see [custom handling push amplification](https://developer.clevertap.com/docs/custom-handling-push-amplification).

## FAQ for Push Notification Errors in Android

### Invalid FCM API Key

An invalid FCM API key error is displayed when the FCM API key in the CleverTap dashboard is incorrect. You will not be able to send any notifications until the FCM API key is added to your settings.

Follow the steps below to update the FCM API key:

1. Navigate to *Settings* > *Engage* > *Channels* > *Mobile Push*. 
2. Update the valid FCM API key as it is displayed in your Firebase project. 

<Image title="FAQ_FCM_Credentials.png" alt={972} className="border" width="100%" border={true} src="https://files.readme.io/da79c70-FAQ_FCM_Credentials.png" />


For more information, refer to [Find Your FCM Sender ID & FCM Server API Key](https://developer.clevertap.com/docs/find-your-fcm-sender-id-fcm-server-api-key).

### FCM Token Invalid

The FCM token is not linked to any device that has your app. If the token is not valid, notifications are not delivered. Check the format of the FCM project number and registration token you pass on to CleverTap.

For more information, refer to [Troubleshooting Push Notifications](https://developer.clevertap.com/docs/troubleshooting-push-notifications).

### FCM Error

This is a Firebase (Android) error. The FCM error is not part of the standard published list and is sometimes incorrectly identified as an Android bug.

### FCM Not Registered

Most likely, the users have uninstalled the app from their device. One of the common non-technical errors is *Undelivered due to App Uninstall* in campaign reports. This error occurs when the users uninstall your app while you are trying to reach them via a push notification.

The *FCM Not Registered* error occurs in two scenarios:

* When the users have uninstalled your app and you are trying to schedule a push notification to them.
* When users have cleared the device cache memory, which in turn removes the token from the device.

### FCM Mismatch Sender ID

An FCM sender ID mismatch occurs when the sender ID entered in the CleverTap dashboard under *Push Notification* settings does not match with the sender ID in the FCM project for your app created on Firebase.

You must match the sender ID/server keys at the following three locations:

* App-level which registers token with the said FCM sender ID mentioned.
* In the CleverTap dashboard.
* FCM project.

If you have checked the sender ID for mismatch and the error still persists, check the following: 

* Does the app build and the CleverTap account contain mismatched sender IDs?
* Do you have multiple Firebase projects?
* Do you have multiple Platform IDs if your app is using two Firebase projects?

# Push Impressions

You can raise and record push notifications delivered onto your users’ Android devices. 

1. Navigate to *Settings* > *Schema* > *Events*.
2. Search for *Push Impressions*, then click on the vertical ellipsis.

![1161](https://files.readme.io/ebf8ae7-Push_Impression_event_FAQ.png "Push Impression event FAQ.png")

3. Click on **Setup push impressions**. A new window displays.
4. Click on **Save** to enable the option.


If you are custom-handling push notifications, add the following code after you render the push notifications: 

```java JAVA
CleverTapAPI.getDefaultInstance(this).pushNotificationViewedEvent(extras);
```
```kotlin Kotlin
CleverTapAPI.getDefaultInstance(this@YourAndroidActivity)?.pushNotificationViewedEvent(extras)
```

# Set the Small Notification Icon

The small icon is a notification bar icon displayed in the push notifications as shown in the image below:

![1080](https://files.readme.io/fd3de03-Campaigns_Push_Small_Icon.png "Campaigns_Push_Small_Icon.png")

With the launch of Android 5 (Lollipop), the notification icons are rendered differently in the notification bar at the top. By default, our SDK uses the app’s icon for both, the notification icon as well as the notification bar icon; however, since Android 5, all non-alpha channels are ignored while drawing the main notification icon. For more information, refer to [Material design style](https://developer.android.com/about/versions/android-5.0-changes.html#NotificationsMaterialDesignStyle) in the Android documentation.

The small icon must to be an *alpha only* image which means the icon must have only one color and the rest of the image must transparent. The *alpha only* icon image is a combination of the colored region and the transparent region. 

In general, the logo displayed in the icon has a single color and the background is transparent. The alpha channel provides the ability to embed transparency within an image. This can be accomplished in tools such as Adobe Photoshop, Illustrator, and so on. You can also change the colored region of the small icon from the CleverTap dashboard. 

The CleverTap SDK fills the transparent section of the small icon with the selected color:

<Image title="13.png" alt={2736} className="border" border={true} src="https://files.readme.io/87250b9-13.png" />

Following is a small icon sample:

![36](https://files.readme.io/4b90e04-Burger_icon.png "Burger_icon.png")

After you create an icon with the alpha channel, you can use it for your project in Android Studio. Simply save the icon file in the Drawable folder of your project. 

To set a custom notification icon (only for the small icon), add the following metadata entry in your AndroidManifest.xml.
































































































































































































> ❗️ Warning
>
> Only use the file name. Do not use the path such as @drawable/ic\_stat\_red\_star as it will not work.

# In-App Notifications

In-app notifications are pop-ups that you can show to your users while they are in your application. 

> 📘 Activity Tag
>
> The &lt;activity&gt; tag is not required for CleverTap SDK 3.5.1 and higher.

To support in-app notifications, register the following activity in your AndroidManifest.xml:














































































































































































































































































































































Mention the activities on which you do not want in-app notifications to show in the android:value of the metadata tag.

Add the following implementation to your app's build.gradle file:


































































## Javascript Support in In-App Notifications

Android SDK v3.5.0 and above supports embedding JavaScript code inside custom in-apps. To make sure your JavaScript code works on the app while creating the in-app campaign, select the checkbox for *Enabling JavaScript during the in-app campaign* creation.

Below are all methods explained with examples:
## In-App Notification Button onClick Callbacks

Android SDK v3.6.1 and above supports callback on the click of in-app notification buttons by returning a map of key value pairs. To use this, make sure your activity implements the `InAppNotificationButtonListener` and override the following method: 

```java JAVA
@Override
public void onInAppButtonClick(HashMap<String, String> hashMap) {
  if(hashMap != null){
    //Read the values
  }
}
```
```kotlin Kotlin
override fun onInAppButtonClick(hashMap: HashMap<String, String>?) {
        // Read the values
    }
```

Set the `InAppNotificationButtonListener` using the following code:

```java JAVA
cleverTapInstance.setInAppNotificationButtonListener(this);
```
```text Kotlin
cleverTapDefaultInstance?.apply {
       setInAppNotificationButtonListener(this@YourAndroidActivity)
   }
```

## FAQ for In-App

### Q. How do I exclude in-app from the Android activity?

If your application has a splash screen (for example, logo screen or loading page) displayed on app launch, then the in-app triggered on *App Launch* would be attempted to be displayed on this screen. As soon as this screen is dismissed, the in-app will be dismissed too.

In these types of cases, the splash screen must be excluded. Mention the *Activities* on which you do not want in-app notifications to show in the android:value of the metadata tag in the AndroidManifest.xml file.



































































































































# App Inbox

* The CleverTap SDK 3.4.0 and above provide the capability to create App Inbox notifications for your users.
* You can use the App Inbox provided by CleverTap or create your own.
* You can design App Inbox notifications right from the dashboard.

1. Add the following inbox dependencies in your app's build.gradle.







































































































































































































































































































































































































































































































































































































































































2. Initializing the Inbox provides a callback to two methods `inboxDidInitialize()` AND `inboxMessagesDidUpdate()`

```java JAVA
import com.clevertap.android.sdk.CTInboxActivity;
import com.clevertap.android.sdk.CTInboxListener;
import com.clevertap.android.sdk.CTInboxStyleConfig;
import com.clevertap.android.sdk.CleverTapAPI;
import com.clevertap.android.sdk.CleverTapInstanceConfig;

public class MainActivity extends AppCompatActivity implements CTInboxListener {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
         private CleverTapAPI cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(this);
         if (cleverTapDefaultInstance != null) {
            //Set the Notification Inbox Listener
            cleverTapDefaultInstance.setCTNotificationInboxListener(this);
            //Initialize the inbox and wait for callbacks on overridden methods
            cleverTapDefaultInstance.initializeInbox();
        }
    }
}
```
```kotlin Kotlin
import com.clevertap.android.sdk.CTInboxListener
import com.clevertap.android.sdk.CTInboxStyleConfig
import com.clevertap.android.sdk.CleverTapAPI
import com.clevertap.android.sdk.CleverTapInstanceConfig

class MainActivity:AppCompatActivity(), CTInboxListener {
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(this@MainActivity)

        cleverTapDefaultInstance?.apply {
            
            ctNotificationInboxListener = this@MainActivity
            
            //Initialize the inbox and wait for callbacks on overridden methods
            initializeInbox()
        }
    }
}
```

3. Configure styling and showing the inbox by:

* Customize the config object and call the Inbox in the `inboxDidInitialize()` method
* Call this method on the button click which opens the CleverTap Inbox for your App


```java JAVA
@Override
    public void inboxDidInitialize() {
        
        yourInboxButton.setOnClickListener(v -> {
            ArrayList<String> tabs = new ArrayList<>();
            tabs.add("Promotions");
            tabs.add("Offers");//We support upto 2 tabs only. Additional tabs will be ignored
            
            CTInboxStyleConfig styleConfig = new CTInboxStyleConfig();
            styleConfig.setFirstTabTitle("First Tab");
            styleConfig.setTabs(tabs);//Do not use this if you don't want to use tabs
            styleConfig.setTabBackgroundColor("#FF0000");
            styleConfig.setSelectedTabIndicatorColor("#0000FF");
            styleConfig.setSelectedTabColor("#0000FF");
            styleConfig.setUnselectedTabColor("#FFFFFF");
            styleConfig.setBackButtonColor("#FF0000");
            styleConfig.setNavBarTitleColor("#FF0000");
            styleConfig.setNavBarTitle("MY INBOX");
            styleConfig.setNavBarColor("#FFFFFF");
            styleConfig.setInboxBackgroundColor("#ADD8E6");
            if (ct != null) {
                ct.showAppInbox(styleConfig); //With Tabs
            }
            //ct.showAppInbox();//Opens Activity with default style configs
        });
    }
```
```kotlin Kotlin
fun inboxDidInitialize() {
  yourInboxButton.setOnClickListener(object:View.OnClickListener() {
    fun onClick(v:View) {
      val inboxTabs =
                    arrayListOf("Promotions", "Offers", "Others")//Anything after the first 2 will be ignored
                CTInboxStyleConfig().apply {
                    tabs = inboxTabs //Do not use this if you don't want to use tabs
                    tabBackgroundColor = "#FF0000"
                    selectedTabIndicatorColor = "#0000FF"
                    selectedTabColor = "#000000"
                    unselectedTabColor = "#FFFFFF"
                    backButtonColor = "#FF0000"
                    navBarTitleColor = "#FF0000"
                    navBarTitle = "MY INBOX"
                    navBarColor = "#FFFFFF"
                    inboxBackgroundColor = "#00FF00"
                    firstTabTitle = "First Tab"
                    cleverTapAPI?.showAppInbox(this) //Opens activity With Tabs
                }
      //OR
      cleverTapDefaultInstance.showAppInbox()//Opens Activity with default style config
    }
  })
}
```

## Creating your own App Inbox

* You can choose to create your own App Inbox by calling the following APIs


```java JAVA
//Initialize App Inbox
cleverTapDefaultInstance.initializeInbox();

//Get Inbox Message Count
cleverTapDefaultInstance.getInboxMessageCount();

//Get Inbox Unread Count
cleverTapDefaultInstance.getInboxMessageUnreadCount();

//Get All messages
cleverTapDefaultInstance.getAllInboxMessages();

//Get only Unread messages
cleverTapDefaultInstance.getUnreadInboxMessages();

//Get message object belonging to the given message id only. Message id should be a String
cleverTapDefaultInstance.getInboxMessageForId(messageId);

//Delete message from the Inbox. Message id should be a String
cleverTapDefaultInstance.deleteInboxMessage(messageId);

//Delete message from the Inbox. Message should object of CTInboxMessage
cleverTapDefaultInstance.deleteInboxMessage(message);

//Mark Message as Read. Message id should be a String
cleverTapDefaultInstance.markReadInboxMessage(messageId);

//Mark message as Read. Message should object of CTInboxMessage
cleverTapDefaultInstance.markReadInboxMessage(message);

//Raise Notification Viewed event for Inbox Message. Message id should be a String
cleverTapDefaultInstance.pushInboxNotificationViewedEvent(messageId);

//Raise Notification Clicked event for Inbox Message. Message id should be a String
cleverTapDefaultInstance.pushInboxNotificationClickedEvent(messageId);

//Callback on Inbox Message update/delete/read (any activity)
@Override
public void inboxMessagesDidUpdate() {    }
```
```kotlin Kotlin
cleverTapDefaultInstance?.apply {
        //Initialize App Inbox
        initializeInbox()

        //Get Inbox Message Count
        val msgCount = inboxMessageCount

        //Get Inbox Unread Count
        val unreadCount = inboxMessageUnreadCount

        //Get All messages
        allInboxMessages?.forEach {
            println("inbox messages ID = ${it.messageId}")
        }

        //Get All Unread messages
        unreadInboxMessages?.forEach {
            println("unread inbox messages ID = ${it.messageId}")
        }

        //Get message object belonging to the given message id only. Message id should be a String
        val inboxMessageForId = getInboxMessageForId(messageId)

        //Delete message from the Inbox. Message id should be a String
        deleteInboxMessage(messageId)

        //Delete message from the Inbox. Message should object of CTInboxMessage
        deleteInboxMessage(message)

        //Mark Message as Read. Message id should be a String
        markReadInboxMessage(messageId)

        //Mark message as Read. Message should object of CTInboxMessage
        markReadInboxMessage(message)

        //Raise Notification Viewed event for Inbox Message. Message id should be a String
        pushInboxNotificationViewedEvent(messageId)

        //Raise Notification Clicked event for Inbox Message. Message id should be a String
        pushInboxNotificationClickedEvent(messageId)
    }

override fun inboxMessagesDidUpdate() {}
```


**App Inbox Button onClick Callbacks**\
Android SDK v3.6.1 and above supports callback on the click of App Inbox Buttons by returning a Map of Key-Value pairs. To use this, make sure your activity implements the `InboxMessageButtonListener` and override the following method - 

```java JAVA
@Override
    public void onInboxButtonClick(HashMap<String, String> hashMap) {
        if(hashMap != null){
            //Read the values
        }
    }
```
```kotlin Kotlin
override fun onInboxButtonClick(payload: HashMap<String, String>?) {
	payload?.apply { 
		val key1 = get("key1")
		val key2 = get("key2")
	}
}
```

# Referral Tracking

From **CleverTap SDK v3.6.4** onwards, add the following Gradle dependency to capture UTM details, app-install time, referrer click time and other metrics provided by Google Install Referrer Library.

















































































If you are upgrading from a lower CleverTap version to **v3.6.4 or above**, then please **remove** the following code from your `AndroidManifest.xml` file - 









































































































































































































































































# Debugging

By default, CleverTap logs are set to CleverTap.Loglevel.INFO. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the Android logging system. This can be done by setting the debug level to CleverTap.Loglevel.DEBUG. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTap.Loglevel.OFF.

```java JAVA
CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.INFO);    //Default Log level

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG);   //Set Log level to DEBUG log warnings or other important messages

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.OFF);     //Switch off logs for Production environment
```
```kotlin Kotlin
CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.INFO) //Default Log level

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.DEBUG) //Set Log level to DEBUG log warnings or other important messages

CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.OFF) //Switch off logs for Production environment}
```

# Push Notifications for Android O


Android has modified the way push notifications can be sent to an app in its latest version Oreo. Starting with CleverTap SDK 3.1.7, we support Android Oreo’s latest features like Notification Channels and Notification Badges.

**Step 1: Prerequisites**\
Since Notification channels and badges are the features of the latest Android version, you as a developer will have to update your SDK and Build Tools to the latest versions. The latest SDK tools version is 26.0.2 and the latest Build Tools version is 26.0.1. You will have to make sure that in your build.gradle file, the compileSdkVersion is 26 and buildToolsVersion is 26.0.1


















































































**Step 2: Creating Notification Channel Groups**\
Once you have the CleverTap Android SDK integrated successfully, you can create a notification channel group. Notification channel groups allow you to manage multiple notification channels with identical names within a single app, which can be useful if your app supports multiple accounts. You can create a notification group using the following line of code –

```java JAVA
CleverTapAPI.createNotificationChannelGroup(getApplicationContext(),"YourGroupId","YourGroupName");
```
```kotlin Kotlin
CleverTapAPI.createNotificationChannelGroup(getApplicationContext(), "YourGroupId", "YourGroupName")
```

**Step 3: Creating Notification Channels with Notification Groups**\
Once you have created a notification channel group you can use the group Id to create notification channels for that specific group. You can create a notification channel specifying the group id using the following line of code.

```java JAVA
CleverTapAPI.createNotificationChannel(getApplicationContext(),"YourChannelId","YourChannelName","YourChannelDescription",NotificationManager.IMPORTANCE_MAX,"YourGroupId",true);
```
```text Kotlin
CleverTapAPI.createNotificationChannel(getApplicationContext(), "YourChannelId", "YourChannelName", "YourChannelDescription", NotificationManager.IMPORTANCE_MAX, "YourGroupId", true)
```

**Step 4: Creating Notification Channels**\
You can create stand-alone notification channels in your app also, by using the following line of code.


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

You can create more than one channel using the above line of code, just make sure that the channel ID differs in every Notification Channel. Also, this Channel ID will be used to send Push Notifications using the CleverTap dashboard.

**Step 5: Deleting Notification Channels**\
You can delete the notification channels created previously in your app. There is no error thrown when you try to delete a notification channel which doesn’t exist. You can delete the notification channels using the following line of code.

```java JAVA
CleverTapAPI.deleteNotificationChannel(getApplicationContext(),"YourChannelId");
```
```kotlin Kotlin
CleverTapAPI.deleteNotificationChannel(getApplicationContext(),"YourChannelId")
```

**Step 6: Deleting Notification Groups**\
CleverTap SDK also allows you to remove the notification groups you have created previously. Please note that you will need to delete all the channels associated with a group prior to deleting a group. You can delete a notification group using the following line of code.

```java JAVA
CleverTapAPI.deleteNotificationChannelGroup(getApplicationContext(),"YourGroupId");
```
```kotlin Kotlin
CleverTapAPI.deleteNotificationChannelGroup(getApplicationContext(),"YourGroupId")
```


**Step 7: Sending Notifications via Dashboard**\
To send Push Notifications, login to the CleverTap Dashboard and on the left navigation menu click on Engage > Campaigns. Then click on +Campaign button on the top right to select Mobile Push. Now, set up your Push Notification campaign by selecting when you want to send out your notification, who do you want to send it out to (in this case it will be Android users) and what kind of push notification you want to send out. Once you reach the what section, select the Single Message option. Select the checkbox which says Send Message to Android O.

Fill in the Channel with the channel id you used to create the notification channel in your app. This field is mandatory as Android O requires a valid channel to send notifications. Badge Icon can be set as the app icon or the small icon used to denote push notifications. The default is app icon. Badge ID is the number you want to set the Notification Badge with, by default it auto-increments. Both Badge Icon and Badge ID are optional.

![1560](https://files.readme.io/5d161a5-14.png "14.png")

> 📘 Notification Channel ID settings
>
> You can choose to have a notification channel ID as optional or mandatory for creating a campaign. Select 'Add notification channels for Android 8.0 and above' option. It is available at Settings>Integration>Push Notification. You can create a selection list of channel IDs for your messages.

![1460](https://files.readme.io/078c833-15.png "15.png")

# Custom Android Push Notifications Handling

**Step 1. Using Your Custom Implementation**\
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

**Step 2: Using Multiple Android Push Notification Providers**\
CleverTap plays well with other Android push notification providers. You can configure your app to work with multiple push notification providers.

If you are using FCM, please follow these instructions.

Subclass FirebaseMessagingService as shown below.


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

Add your service to the AndroidManifest.xml in place of the CleverTap FcmMessageListenerService.
















































































































































































































Replace com.your.package with the correct package name which contains the newly created class, MyFcmMessageListenerService.

To track the push notification events and deeplinks add the following receiver in your AndroidManifest.xml.






















































































































































































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

    <tr>
      <td>
        wzrk\_rnv
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, it will raise Push Impressions event
      </td>
    </tr>

    <tr>
      <td>
        wzrk\_nms
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
        wzrk\_st
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, it contains the subtitle text which is displayed next to the App name.
      </td>
    </tr>

    <tr>
      <td>
        wzrk\_clr
      </td>

      <td>
        string
      </td>

      <td>
        If present and not empty, it contains the Hex value of the colour to be applied on the small icon (and app name for Android versions below Pie)
      </td>
    </tr>
  </tbody>
</Table>


If the field nm is empty, ignore the notification. CleverTap sends out a dummy notification with the nm field empty to test for app uninstalls. In addition to the above, attach all keys starting with wzrk\_ to your notification extras (For example, wzrk\_id is used to track notification opens).

# Push Notification Callback

The CleverTap Android SDK v3.8.1 SDK gives you a callback when a Push Notification is clicked along with the entire payload. To receive this payload, implement the `CTPushNotificationListener` in your Application class or Main Activity.

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

# Advanced Android Push Notification Options

Apart from Title and Message, you have the below mentioned options to add to your Android push notification. Please note that each of these are optional.

## **Image URL**

If an image link is specified, a large image is added to your push notification.

* Recommended resolution: 600×300
* Max size: 40 kb
* Supported file formats are .jpg and .png

## **Large Icon URL**

If a large icon link is specified, the large icon will be appended to the push notification. Based on the device, the large icon will be displayed either far left or far right.

* Max resolution – 72×72
* Max size – 1 kb

## **Deep Link/External URL**

Deep link helps you open a particular activity in your app on click of the notification. If left empty, the notification on click will open the launcher activity of the app. If you wish to use external URLs, then please whitelist the IPs or provide http/https before the URL so that they can be handled properly by the SDK.

## **Action Buttons**

You can add upto 3 call to action buttons for every push notification. For every action button, you have the following options:


* Title: contains the Call to Action text (mandatory)
* Deep link: the deep link that should open on click of that button
* Action ID: a user defined string (applicable to apps custom handling their android push notifications: This string will be available as an extra on the notification click intent for you to identify the action button clicked) This is a mandatory field.
* Icon Resource Identifier: A drawable icon in your app’s resources folder to display the icon along with the notification for Android devices below OS version Nougat. Android Nougat does not display icons by default to give more room for buttons.

If the user clicks on the main body of the notification, the app will open and the notification will disappear. If the user clicks on one of the action buttons, then by default android will not remove the notification from the tray. We provide two user options for this.

The first option is to handle closing the notification yourself (applies to apps that are custom handling the push notification). To accomplish this, the click intent has the notification Id in its extras. So to close, please add the following code in the activity that would get called.


```java JAVA
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
```kotlin Kotlin
intent.extras?.apply {
            getString("actionId")?.let {
                Log.d("ACTION_ID", it)
                val autoCancel = getBoolean("autoCancel", true)
                val notificationId = getInt("notificationId", -1)

                if (autoCancel && notificationId > -1) {
                    val notifyMgr: NotificationManager =
                        applicationContext.getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
                    notifyMgr.cancel(notificationId) // the bit that cancels the notification
                }
                Toast.makeText(baseContext, "Action ID is: $it", Toast.LENGTH_SHORT).show()
            }
        }
```

The second option is that CleverTap handles closing the notification. You will have to add an additional CleverTap IntentService to your Manifest.xml and the SDK will do it for you automatically.





























































































































































































































































<Image title="push1.png" alt={996} className="border" border={true} src="https://files.readme.io/2c29710-push1.png" />

## **Sound Files**

You can choose to have no notification sound, the default OS sound or use a custom sound. It has to be present in the resources folder of your app. Please see the [Android sound guide](doc:add-a-sound-file-to-your-android-app) to learn how to add a sound file to your android app. Android only supports .mp3, .ogg and .wav files.

## **Notification Tray Priority**


Relative priority for this notification in the device tray. Priority is an indication of how much of the user’s valuable attention should be consumed by this notification.

* MAXIMUM: Use for critical and urgent notifications that alert the user to a condition that is time-critical or needs to be resolved before they can continue with a particular task. A notification with priority set to the maximum will be a heads up notification, and will always be at the top in the notification tray.
* HIGH: Use primarily for important communication, such as message or chat events with content that is particularly interesting for the user. High-priority notifications trigger the heads-up notification display. A notification with priority set to high will be a heads up notification.
* DEFAULT: Use for all notifications that don’t fall into any of the other priorities. A notification with default priority will simply show up in the notification tray and its order in the notification tray is subject to the presence of other notifications.

> 📘 Note
>
> Notification tray priority is only applicable for Android 7.1 and below. For Android 8 and above, the notification tray priority is based on the notification channel.

## **Notification Delivery Priority**

You have two options for assigning delivery priority to downstream messages on Android: normal and high priority. 

Normal priority - This is the default priority for messages on Android. Normal priority messages are delivered immediately when the app is in the foreground. When the device is in Doze, the delivery may be delayed to conserve battery. For less time-sensitive messages, such as notifications of new email, keeping your UI in sync, or syncing app data in the background, choose normal delivery priority.

High priority - FCM attempts to deliver high priority messages immediately, allowing the FCM service to wake a sleeping device when necessary and to run some limited processing (including very limited network access). High priority messages generally should result in user interaction with your app or its notifications. If FCM detects a pattern in which they don't, your messages may be de-prioritized. 

# Setting Native Display in Android

We support Native Display for CleverTap Android SDK version 3.6.2 and above. Follow the steps to integrate Native Display with your App. 

## Step 1: Register the Native Display Listener


The Activity class must implement DisplayUnitListener on which you are going to show the NativeDisplayUnits. Register the activity to the Clevertap’s instance using the setDisplayUnitListener method.

```java JAVA
import com.clevertap.android.sdk.CleverTapAPI;
import com.clevertap.android.sdk.displayunits.DisplayUnitListener;
import com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit;
import java.util.ArrayList;
public class MainActivity extends AppCompatActivity implements DisplayUnitListener {
    @Override
    protected void onCreate(Bundle savedInstanceState){
        super.onCreate(savedInstanceState);
        CleverTapAPI.getDefaultInstance(this).setDisplayUnitListener(this);
    }
    @Override
    public void onDisplayUnitsLoaded(ArrayList<CleverTapDisplayUnit> units) {
        // you will get display units here
    }
}
```
```kotlin Kotlin
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import com.clevertap.android.sdk.CleverTapAPI
import com.clevertap.android.sdk.displayunits.DisplayUnitListener
import com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit
import java.util.ArrayList

class MainActivity: AppCompatActivity(), DisplayUnitListener {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val cleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(this@MainActivity)

        cleverTapDefaultInstance?.apply {
            setDisplayUnitListener(this@MainActivity)
        }
    }
    override fun onDisplayUnitsLoaded(units: ArrayList<CleverTapDisplayUnit>?) {
        // you will get display units here
    }
}
```

## Step 2: Get the NativeDisplayUnits

You can raise the event for the Native Display Campaign.

```java JAVA
CleverTapAPI.getDefaultInstance(this).pushEvent("Your Event Name");
```
```kotlin Kotlin
CleverTapAPI.getDefaultInstance(this).pushEvent("Your Event Name")
```

You receive the list of CleverTapDisplayUnits via the registered callback for all active Native Display campaigns. Implement your logic to create Native Display views using these CleverTapDisplayUnits.


```java JAVA
@Override
public void onDisplayUnitsLoaded(ArrayList<CleverTapDisplayUnit> units) {
    // you will get display units here
    // implement your logic to create your display views using these Display Units here
    for (int i = 0; i <units.size() ; i++) {
        CleverTapDisplayUnit unit = units.get(i);
        prepareDisplayView(unit);
    }
}
```
```kotlin Kotlin
override fun onDisplayUnitsLoaded(units:ArrayList<CleverTapDisplayUnit>) {
  // you will get display units here
  // implement your logic to create your display views using these Display Units here
  for (i in 0 until units.size())
  {
    val unit = units.get(i)
    prepareDisplayView(unit)
  }
}
```

Refer to our model classes CleverTapDisplayUnit and CleverTapDisplayUnitContent for creating the Native Display view.


```java CleverTapDisplayUnit
/**
 * This model class holds the data of an individual Display Unit.
 */
public class CleverTapDisplayUnit implements Parcelable {
    /**
     * Display unit identifier
     */
    private String unitID;
    /**
     * Display Type Could be (banner,carousel,custom key value etc.)
     */
    private CTDisplayUnitType type;
    /**
     * Background Color
     */
    private String bgColor;
    /**
     * List of Display Content Items
     */
    private ArrayList<CleverTapDisplayUnitContent> contents;
    /**
     * Custom Key Value Pairs
     */
    private HashMap<String, String> customExtras;
    /**
     * Getter for the unitId of the Display Unit
     *
     * @return String
     */
    public String getUnitID() {
        return unitID;
    }
    /**
     * Getter for the Key Value pairs of the Display Unit
     *
     * @return HashMap<String, String>
     */
    public HashMap<String, String> getCustomExtras() {
        return customExtras;
    }
    /**
     * Getter for the hex-value background color of the Display Unit e.g. #000000
     *
     * @return String
     */
    public String getBgColor() {
        return bgColor;
    }
    /**
     * Getter for the DisplayUnitType of the Display Unit, Refer{@link CTDisplayUnitType}
     *
     * @return CTDisplayUnitType
     */
    public CTDisplayUnitType getType() {
        return type;
    }
    /**
     * Getter for the list of Content Display Unit Items.
     *
     * @return ArrayList<CleverTapDisplayUnitContent>
     */
    public ArrayList<CleverTapDisplayUnitContent> getContents() {
        return contents;
    }
}
```
```java CleverTapDisplayUnitContent
/**
 * Content class for holding Display Unit Content Data
 */
public class CleverTapDisplayUnitContent implements Parcelable {
    private String title;
    private String titleColor;
    private String message;
    private String messageColor;
    private String media;
    private String contentType;
    private String posterUrl;
    private String actionUrl;
    private String icon;
    /**
     * Getter for the title section of the Display Unit Content
     * @return String
     */
    public String getTitle() {
        return title;
    }
    /**
     * Getter for the message section of the Display Unit Content
     * @return String
     */
    public String getMessage() {
        return message;
    }
    /**
     * Getter for the media URL of the Display Unit Content
     * @return String
     */
    public String getMedia() {
        return media;
    }
    /**
     * Getter for the action URL of the body of the Display Unit Content
     * @return String
     */
    public String getActionUrl() {
        return actionUrl;
    }
    /**
     * Getter for the URL as String for the icon in case of Icon Message template
     * @return String
     */
    public String getIcon() {
        return icon;
    }
    /**
     * Getter for the hex-code value of the title color e.g. #000000
     * @return String
     */
    public String getTitleColor() {
        return titleColor;
    }
    /**
     * Getter for the hex-code value of the message color e.g. #000000
     * @return String
     */
    public String getMessageColor() {
        return messageColor;
    }
    /**
     * Getter for the URL for the thumbnail of the video
     * @return String
     */
    public String getPosterUrl() {
        return posterUrl;
    }
    /**
     * Getter for the content type of the media(image/gif/audio/video etc.)
     *
     * Refer{@link #mediaIsImage()}, {@link #mediaIsGIF()},
     *      {@link #mediaIsAudio()} ,{@link #mediaIsVideo()}
     * @return String
     */
    public String getContentType() {
        return contentType;
    }
    /**
     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is an image.
     *
     * @return boolean - | true, if the media type is image
     *                   | false, if the media type is not an image
     */
    public boolean mediaIsImage() {
        return contentType != null && this.media != null && contentType.startsWith("image") && !contentType.equals("image/gif");
    }
    /**
     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is a GIF.
     *
     * @return boolean - | true, if the media type is GIF
     *                   | false, if the media type is not a GIF
     */
    public boolean mediaIsGIF() {
        return contentType != null && this.media != null && contentType.equals("image/gif");
    }
    /**
     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is a video.
     *
     * @return boolean - | true, if the media type is video
     *                   | false, if the media type is not a video
     */
    public boolean mediaIsVideo() {
        return contentType != null && this.media != null && contentType.startsWith("video");
    }
    /**
     * Method to check whether media in the {@link CleverTapDisplayUnitContent} object is an audio.
     *
     * @return boolean - | true, if the media type is audio
     *                   | false, if the media type is not an audio
     */
    public boolean mediaIsAudio() {
        return contentType != null && this.media != null && contentType.startsWith("audio");
    }
}
```


We have also provided a few utility methods to get Adunits corresponding to the last event raised.

```java Getting all the Display Units
/**
 * Getter for retrieving all the Display Units.
 *
 * @return ArrayList<CleverTapDisplayUnit> - could be null, if there is no Display Unit campaigns
 */
@Nullable
public ArrayList<CleverTapDisplayUnit> getAllDisplayUnits();
```
```java Getting Display Unit using unitID
/**
 * Getter for retrieving Display Unit using the unitID
 *
 * @param unitID - unitID of the Display Unit {@link CleverTapDisplayUnit#getUnitID()}
 * @return CleverTapDisplayUnit - could be null, if there is no Display Unit campaign with the identifier
 */
@Nullable
public CleverTapDisplayUnit getDisplayUnitForId(String unitID);
```

## Step 3: Raise Events for Tracking

You can raise the following events for tracking:

### Notification Viewed Event

You can raise Notification Viewed event using pushDisplayUnitViewedEventForID method, whenever the user sees the Native Display. You must provide the unitID of the CleverTapDisplayUnit corresponding to the Native Display.

```java JAVA
CleverTapAPI.getDefaultInstance(this).pushDisplayUnitViewedEventForID(adUnit.getUnitID());
```
```kotlin Kotlin
CleverTapAPI.getDefaultInstance(this).pushDisplayUnitViewedEventForID(adUnit.getUnitID())
```

### Notification Clicked Event

You can raise the Notification Clicked event using the pushDisplayUnitClickedEventForID method whenever the Native Display is clicked by the user.  You must provide the unitID of the CleverTapDisplayUnit corresponding to the Native Display.

```java JAVA
CleverTapAPI.getDefaultInstance(this).pushDisplayUnitClickedEventForID(adUnit.getUnitID();;
```
```kotlin Kotlin
CleverTapAPI.getDefaultInstance(this).pushDisplayUnitClickedEventForID(adUnit.getUnitID())
```

> 📘 Custom Handling
>
> The events that you raise are accepted automatically by the CleverTap dashboard.\
> For example, you have implemented the Native Display using RecyclerView and you raise a Notification Viewed event whenever the Native Display appears on the browser's viewport. If the users scroll through the app multiple times, then multiple Notification Viewed events are raised. 
>
> In this case, you might not want to raise multiple events. You can handle this scenario using the android framework callbacks.

## Step 4: Get Native Display Units using Send Test


In the Send Test flow, the CleverTapDisplayUnit is delivered via a Push notification. As soon we receive the notification, we parse the notification payload containing the CleverTapDisplayUnit and keep them in memory.

You can get the CleverTapDisplayUnit payload by either of the following methods:

### Method 1: By getting all the Display Units

```java JAVA
CleverTapAPI.getDefaultInstance(this).getAllDisplayUnits();
```
```kotlin Kotlin
CleverTapAPI.getDefaultInstance(this).getAllDisplayUnits()
```

OR

### Method 2: Register the DisplayUnitListener in the Application Class

You will receive the CleverTapDisplayUnit.

```java JAVA
import com.clevertap.android.sdk.CleverTapAPI;
import com.clevertap.android.sdk.displayunits.DisplayUnitListener;
import com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit;
import java.util.ArrayList;

public class MyApplication extends Application implements DisplayUnitListener {
    @Override
    public void onCreate(){
        super.onCreate();
        CleverTapAPI.getDefaultInstance(this).setDisplayUnitListener(this);
    }
    @Override
    public void onDisplayUnitsLoaded(ArrayList<CleverTapDisplayUnit> units) {
        // you will get send test display units here
    }
}
```
```kotlin Kotlin
import com.clevertap.android.sdk.CleverTapAPI
import com.clevertap.android.sdk.displayunits.DisplayUnitListener
import com.clevertap.android.sdk.displayunits.model.CleverTapDisplayUnit
import java.util.ArrayList

class MyApplication:Application(), DisplayUnitListener {
  fun onCreate() {
    super.onCreate()
    CleverTapAPI.getDefaultInstance(this).setDisplayUnitListener(this)
  }
  override fun onDisplayUnitsLoaded(units:ArrayList<CleverTapDisplayUnit>) {
    // you will get send test display units here
  }
}
```

## Changing Account Credentials

If you’d not want to insert your account credentials in your app’s AndroidManifest.xml, or would like to change your account ID programmatically, you need to create a custom Application class (if you don’t have one already) with the following content.


```java JAVA
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
```kotlin Kotlin
import android.app.Application
import com.clevertap.android.sdk.ActivityLifecycleCallback
import com.clevertap.android.sdk.CleverTapAPI

class MyApplication:Application() {
  override fun onCreate() {
    CleverTapAPI.changeCredentials("Your account ID here", "Your account token here")
    ActivityLifecycleCallback.register(this) // Must be called before super.onCreate()
    super.onCreate()
  }
}
```

If you’ve just created this subclass of Application, update your AndroidManifest.xml to specify your subclass.











































































































































If you have used this method, to set your CleverTap Account ID and Token, please do not add these values to your manifest file.

## Manually Updating User Location

The application is responsible for requesting the user’s permission to use location. If this permission is granted, location can be passed to CleverTap.

The App can pass the location to the SDK using setLocation.

```java JAVA
CleverTapAPI cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext());
cleverTapAPI.setLocation(location); //android.location.Location
```
```kotlin Kotlin
val cleverTapAPI = CleverTapAPI.getDefaultInstance(getApplicationContext())
cleverTapAPI.setLocation(location) //android.location.Location}
```

## Using Your Existing Activity Lifecycle Listener

If you’d like to use your own activity lifecycle listener, please update it to incorporate the following code.


```java JAVA
application.registerActivityLifecycleCallbacks(
        new android.app.Application.ActivityLifecycleCallbacks() {
        
            @Override
            public void onActivityCreated(Activity activity, Bundle bundle) {
                CleverTapAPI.setAppForeground(true);
                try {
                    CleverTapAPI.getDefaultInstance(application).pushNotificationClickedEvent(activity.getIntent().getExtras());
                } catch (Throwable t) {
                    // Ignore
                }
                try {
                    Intent intent = activity.getIntent();
                    Uri data = intent.getData();
                    CleverTapAPI.getDefaultInstance(application).pushDeepLink(data);
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
                    CleverTapAPI.getDefaultInstance(application).onActivityResumed(activity);
                } catch (Throwable t) {
                    // Ignore
                }
            }
            
            @Override
            public void onActivityPaused(Activity activity) {
                try {
                    CleverTapAPI.getDefaultInstance(application).onActivityPaused();
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
```kotlin Kotlin
registerActivityLifecycleCallbacks(object : android.app.Application.ActivityLifecycleCallbacks {
            override fun onActivityCreated(activity: Activity, savedInstanceState: Bundle?) {
                CleverTapAPI.setAppForeground(true)
                try
                {
                    CleverTapAPI.getDefaultInstance(applicationContext)?.pushNotificationClickedEvent(activity.intent.extras)
                }
                catch (t:Throwable) {
                    // Ignore
                }
                try
                {
                    val intent = activity.intent
                    val data = intent.data
                    CleverTapAPI.getDefaultInstance(applicationContext)?.pushDeepLink(data)
                }
                catch (t:Throwable) {
                    // Ignore
                }
            }
            override fun onActivityStarted(activity: Activity) {
            }
            override fun onActivityResumed(activity: Activity) {
                try
                {
                    CleverTapAPI.onActivityResumed(activity)
                }
                catch (t:Throwable) {
                    // Ignore
                }
            }
            override fun onActivityPaused(activity: Activity) {
                try
                {
                    CleverTapAPI.onActivityPaused()
                }
                catch (t:Throwable) {
                    // Ignore
                }
            }
            override fun onActivityStopped(activity: Activity) {
            }

            override fun onActivitySaveInstanceState(activity: Activity, outState: Bundle){
            }
            override fun onActivityDestroyed(activity: Activity) {
            }
        })
```