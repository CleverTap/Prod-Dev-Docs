---
title: Android App Inbox
excerpt: >-
  Learn how to send app inbox notifications and create your own app inbox
  notification.
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

App Inbox is a messaging channel that provides the ability to deliver rich, individually customized content to your users. Messages sent to App Inbox are saved on the user's device.

App Inbox allows you to send permanent content directly to your app from the CleverTap dashboard. Moreover, the inbox messages target individual segments just as other messaging channels. Your inbox can look different for each user; the possibilities are endless.

> 📘 SDK Version Compatibility
> 
> The CleverTap SDK version 3.4.0 and above allows your users to create App Inbox notifications.

# App Inbox

CleverTap provides the App Inbox out of the box to send messages directly to your app from the CleverTap dashboard.  Using the CleverTap App Inbox, you can design App Inbox notifications right from the dashboard. 

Perform the following steps to use the CleverTap App Inbox: 

1. Add the inbox dependencies in your app's `build.gradle` file.

> 📘 Migrating from ExoPlayer to AndroidX Media3
> 
> Starting from SDK v7.0.0 CleverTap now supports [AndroidX Media3](https://developer.android.com/media/media3/exoplayer/migration-guide), replacing the deprecated ExoPlayer libraries. CleverTap continues to support [ExoPlayer](https://github.com/google/ExoPlayer), but for a smoother migration to AndroidX Media3, update the following dependencies:

```groovy App Inbox with Exoplayer Libraries
//MANDATORY for App Inbox

implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'

//Optional ExoPlayer Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies

implementation "com.google.android.exoplayer:exoplayer:2.19.1"
implementation "com.google.android.exoplayer:exoplayer-hls:2.19.1"
implementation "com.google.android.exoplayer:exoplayer-ui:2.19.1"
```
```groovy App Inbox with Androidx Media3 Support
//MANDATORY for App Inbox

implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'

//Optional AndroidX Media3 Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies

implementation "androidx.media3:media3-exoplayer:1.1.1"
implementation "androidx.media3:media3-exoplayer-hls:1.1.1"
implementation "androidx.media3:media3-ui:1.1.1"
```

2. Initializing the inbox provides a callback to two methods, `inboxDidInitialize()` and `inboxMessagesDidUpdate()`.

```java Java
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

3. Configure styling and show the inbox by:

- Customizing the config object and calling the inbox in the `inboxDidInitialize()` method.
- Calling `showAppInbox()` method on the button click, which opens the CleverTap Inbox for your app.

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

## Dismiss App Inbox

Use the following method to dismiss the App Inbox.

```java
cleverTapDefaultInstance.dismissAppInbox();
```
```kotlin
cleverTapDefaultInstance?.dismissAppInbox()
```

## Create Your App Inbox

You can use the App Inbox provided by CleverTap or create your own App Inbox. Creating your own app inbox requires additional coding effort and debug skills.

Use the following APIs to create, read, and delete inbox messages in your own App Inbox:

```java Java
//This API is used to initialize App Inbox.
cleverTapDefaultInstance.initializeInbox();

//This API is used to get inbox message count.
cleverTapDefaultInstance.getInboxMessageCount();

//Use this API to get inbox unread count.
cleverTapDefaultInstance.getInboxMessageUnreadCount();

//Use this API to get all messages in your inbox.
cleverTapDefaultInstance.getAllInboxMessages();

//This API is used to get only unread messages in your inbox.
cleverTapDefaultInstance.getUnreadInboxMessages();

//Use this API to get message object belonging to the given message id only. The messageId should be a String.
cleverTapDefaultInstance.getInboxMessageForId(messageId);

//Use this API to delete message from the inbox. The messageId should be a String.
cleverTapDefaultInstance.deleteInboxMessage(messageId);

//This API is used to delete message from the inbox. The message should be object of CTInboxMessage.
cleverTapDefaultInstance.deleteInboxMessage(message);

//Use this API to mark message as read. The messageId should be a String.
cleverTapDefaultInstance.markReadInboxMessage(messageId);

//Use this API to mark message as read. The message should be object of CTInboxMessage.
cleverTapDefaultInstance.markReadInboxMessage(message);



//Callback on Inbox Message update/delete/read (any activity).
@Override
public void inboxMessagesDidUpdate() {    }
```
```kotlin Kotlin
cleverTapDefaultInstance?.apply {
        //This API is used to initialize App Inbox.
        initializeInbox()

        //This API is used to get inbox message count.
        val msgCount = inboxMessageCount

        //Use this API to get inbox unread count.
        val unreadCount = inboxMessageUnreadCount

        //Use this API to get all messages in your inbox.
        allInboxMessages?.forEach {
            println("inbox messages ID = ${it.messageId}")
        }

       //This API is used to get only unread messages in your inbox.
        unreadInboxMessages?.forEach {
            println("unread inbox messages ID = ${it.messageId}")
        }

        //Use this API to get message object belonging to the given message id only. The messageId should be a String.
        val inboxMessageForId = getInboxMessageForId(messageId)

        //Use this API to delete message from the inbox. The messageId should be a String.
        deleteInboxMessage(messageId)

        //This API is used to delete message from the inbox. The message should be object of CTInboxMessage.
        deleteInboxMessage(message)

        //Use this API to mark message as read. The messageId should be a String.
        markReadInboxMessage(messageId)

        //Use this API to mark message as read. The message should be object of CTInboxMessage.
        markReadInboxMessage(message)

        //Use this API to raise Notification Viewed event for Inbox Message. The messageId should be a String.
        pushInboxNotificationViewedEvent(messageId)

        //Use this API to raise Notification Clicked event for Inbox Message. The messageId should be a String.
        pushInboxNotificationClickedEvent(messageId)
    }

override fun inboxMessagesDidUpdate() {}
```

For more information on the App Inbox APIs, refer to our [example Android project](https://github.com/CleverTap/clevertap-android-sdk/blob/master/sample/src/main/java/com/clevertap/demo/ui/main/HomeScreenViewModel.kt#L196). For more information on our example Android project, refer to [sample App](doc:android-resources#sample-app-and-live-demo).

## Raise Events for Tracking

You can raise the following events for tracking:

1. [Notification Viewed](doc:android-app-inbox#notification-viewed)
2. [Notification Clicked](doc:android-app-inbox#notification-clicked)

### Notification Viewed

You can raise the Notification Viewed event whenever the user sees the inbox messages by using the `pushInboxNotificationViewedEvent` method. You must provide the id corresponding to the inbox message.

```java
//Raise Notification Viewed event for Inbox Message. Message id should be a String
cleverTapDefaultInstance.pushInboxNotificationViewedEvent(messageId);
```
```kotlin Kotlin
//Raise Notification Viewed event for Inbox Message. Message id should be a String.
cleverTapDefaultInstance.pushInboxNotificationViewedEvent(messageId)
```

For more details on the Notification Viewed API, refer to our [sample App](https://github.com/CleverTap/clevertap-android-sdk/blob/master/sample/src/main/java/com/clevertap/demo/ui/main/HomeScreenViewModel.kt#L248).

### Notification Clicked

You can raise the Notification Clicked event using the `pushInboxNotificationClickedEvent` method whenever the user clicks the inbox message. You must provide the id corresponding to the inbox message.

```java Java
//Raise Notification Clicked event for Inbox Message. Message id should be a String.
cleverTapDefaultInstance.pushInboxNotificationClickedEvent(messageId);
```
```kotlin Kotlin
//Raise Notification Clicked event for Inbox Message. Message id should be a String.
cleverTapDefaultInstance.pushInboxNotificationClickedEvent(messageId)
```

For more information on the Notification Clicked API, refer to our [sample App](https://github.com/CleverTap/clevertap-android-sdk/blob/master/sample/src/main/java/com/clevertap/demo/ui/main/HomeScreenViewModel.kt#L255).

### App Inbox Item and Button Click Callbacks

Let's understand the types of buttons first that App Inbox supports:

- URL button (fires the deeplink with the associated URL)
- Copy button (copies the associated text to the clipboard)
- KV button (contains the custom kev-value pair for custom handling)

The Android SDK v4.6.1 and above supports `onInboxItemClicked` callback on the click of an App Inbox item, such as text or media.

 The Android SDK v4.6.9 onwards and below v4.7.0, the `onInboxItemClicked` callback supports the button click besides the item click.

The callback returns `CTInboxMessage` object, `contentPageIndex` and `buttonIndex` parameters. To use this callback, check that your activity implements the `InboxMessageListener` and overrides the following method:

```java
@Override
public void onInboxItemClicked(CTInboxMessage message, int contentPageIndex, int buttonIndex){
    Log.i(TAG, "InboxItemClicked at page-index " + contentPageIndex + " with button-index " + buttonIndex);
    
    //The contentPageIndex corresponds to the page index of the content, which ranges from 0 to the total number of pages for carousel templates. For non-carousel templates, the value is always 0, as they only have one page of content.
    CTInboxMessageContent messageContentObject = message.getInboxMessageContents().get(contentPageIndex);
    
    //The buttonIndex corresponds to the CTA button clicked (0, 1, or 2). A value of -1 indicates the app inbox body/message clicked.
    if (buttonIndex != -1) {
        //button is clicked
        try {
            JSONObject buttonObject = (JSONObject) messageContentObject.getLinks().get(buttonIndex);
            String buttonType = buttonObject.getString("type");
            Log.i(TAG, "type of button clicked: " + buttonType);
        } catch (Throwable t) {
            t.printStackTrace();
        }
    } else {
        //item is clicked
        Log.i(TAG, "type/template of App Inbox item:" + message.type);
    }
}
```
```kotlin
override fun onInboxItemClicked(message: CTInboxMessage?, contentPageIndex: Int, buttonIndex: Int) {
    Log.i(TAG, "InboxItemClicked at page-index $contentPageIndex with button-index $buttonIndex")

    //The contentPageIndex corresponds to the page index of the content, which ranges from 0 to the total number of pages for carousel templates. For non-carousel templates, the value is always 0, as they only have one page of content.
    val messageContentObject = message?.inboxMessageContents?.get(contentPageIndex)
        
    //The buttonIndex corresponds to the CTA button clicked (0, 1, or 2). A value of -1 indicates the app inbox body/message clicked.
    if (buttonIndex != -1) {
        //button is clicked
        try {
             val buttonObject: JSONObject? = messageContentObject?.links?.get(buttonIndex) as JSONObject?
             val buttonType = buttonObject?.optString("type")
             Log.i(TAG, "type of button clicked: " + buttonType);
        } catch (t: Throwable) {
             t.printStackTrace();
        }
        } else {
            //item is clicked
            Log.i(TAG, "type/template of App Inbox item:" + message.type);
        }
 }
```

> 📘 Parameters received in `onInboxItemClicked` callback
> 
> The `message` parameter represents the entire _App Inbox Item_ object.  
> The `contentPageIndex` parameter corresponds to the page index of the content, which ranges from 0 to n-1 (where n is the total number of pages for carousel templates). The non-carousel templates always have a value of 0, because they have only one page of content.  
> The `buttonIndex` parameter corresponds to the App Inbox button (0, 1, or 2), that is clicked. A value of -1 in `buttonIndex` field indicates the entire _App Inbox Item_ is clicked.

Android SDK v3.6.1 and above supports an exclusive `onInboxButtonClick` callback on the click of **KV** type of buttons. It returns a Map of Key-Value pairs. To use this, make sure your activity implements the `InboxMessageButtonListener` and override the following method:

```java
@Override
public void onInboxButtonClick(HashMap<String, String> hashMap) {
    Log.i(TAG, "InboxButtonClick with payload:" + payload);
}
```
```kotlin
override fun onInboxButtonClick(payload: HashMap<String, String>?) {
    Log.i(TAG, "InboxButtonClick with payload: $payload")
}
```