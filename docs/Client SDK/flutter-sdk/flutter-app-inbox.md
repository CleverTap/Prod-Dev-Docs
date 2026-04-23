---
title: Flutter App Inbox
excerpt: Learn how to handle app inbox notifications in Flutter.
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
      title: Flutter Advanced Features
      url: https://developer.clevertap.com/docs/flutter-advanced-features
    - type: link
      title: Flutter Resources
      url: https://developer.clevertap.com/docs/flutter-resources
---
# Overview

App Inbox is a messaging channel that provides the ability to deliver rich, individually customized content to your users. Messages sent to App Inbox are saved on the user's device.

App Inbox provides the ability to send permanent content directly to your app from the CleverTap dashboard. Moreover, the inbox messages target individual segments such as other messaging channels. Your inbox can look different from another user's inbox; the possibilities are endless.

# App Inbox

* The CleverTap Flutter SDK allows you to create App Inbox notifications for your users.
* You can use the App Inbox provided by CleverTap or create your own
* You can design App Inbox notifications right from the dashboard

## Android

On the Android side, add the following inbox dependencies in your app's `build.gradle`:

> 📘 Migrating from ExoPlayer to AndroidX Media3
>
> [ExoPlayer](https://github.com/google/ExoPlayer) is deprecated as of CleverTap Android SDK v8.1.0 and support will be removed in v9.0.0. Migrate to [AndroidX Media3](https://developer.android.com/media/media3/exoplayer/migration-guide) before upgrading to v9.0.0. CleverTap Flutter SDK v2.5.0 and above supports AndroidX Media3. Update the following dependencies:

```groovy Dependencies with AndroidX Media3
implementation 'com.google.firebase:firebase-messaging:21.0.0'
implementation 'androidx.core:core:1.3.0'
implementation 'androidx.fragment:fragment:1.3.6'
        
//MANDATORY for App Inbox
implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'
        
//For CleverTap Android SDK v3.6.4 and above, add the following line of code
implementation 'com.android.installreferrer:installreferrer:2.2'

//Optional AndroidX Media3 Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies

implementation "androidx.media3:media3-exoplayer:1.1.1"
implementation "androidx.media3:media3-exoplayer-hls:1.1.1"
implementation "androidx.media3:media3-ui:1.1.1"
```
```groovy Dependencies with Exoplayer Libraries
implementation 'com.google.firebase:firebase-messaging:21.0.0'
implementation 'androidx.core:core:1.3.0'
implementation 'androidx.fragment:fragment:1.3.6'
        
//MANDATORY for App Inbox
implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'
        
//For CleverTap Android SDK v3.6.4 and above, add the following line of code
implementation 'com.android.installreferrer:installreferrer:2.2'
        
//Optional ExoPlayer Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped without these dependencies

implementation "com.google.android.exoplayer:exoplayer:2.19.1"
implementation "com.google.android.exoplayer:exoplayer-hls:2.19.1"
implementation "com.google.android.exoplayer:exoplayer-ui:2.19.1"
```

## Flutter

Initializing the Inbox provides a callback to two methods `inboxDidInitialize()` and `inboxMessagesDidUpdate()`

```javascript Dart
CleverTapPlugin.initializeInbox();
```

Configure styling and show the inbox by:

1. Customize the config object and call the Inbox in the `inboxDidInitialize()` method
2. Call this method on the button click which opens the CleverTap Inbox for your App

```javascript Dart
_clevertapPlugin.setCleverTapInboxDidInitializeHandler(inboxDidInitialize);

void inboxDidInitialize() {
    this.setState(() {
      print("inboxDidInitialize called");
      var styleConfig = {
          'noMessageTextColor': '#ff6600',
          'noMessageText': 'No message(s) to show.',
          'navBarTitle': 'App Inbox'
      };
CleverTapPlugin.showInbox(styleConfig);
    });
  }
```

When a new inbox message arrives, the SDK provides the `inboxMessagesDidUpdate()` callback

```javascript Dart
_clevertapPlugin
        .setCleverTapInboxMessagesDidUpdateHandler(inboxMessagesDidUpdate);
        
void inboxMessagesDidUpdate() {
    this.setState(() async {
      print("inboxMessagesDidUpdate called");
    });
  }
```

### App Inbox Item and Button Click Callbacks

Let's understand the types of buttons that App Inbox supports:

* **URL button**: fires the deeplink with the associated URL.
* **Copy button**: copies the associated text to the clipboard
* **KV button**: contains the custom key-value pair for custom handling

The CleverTap Flutter SDK v1.5.2 and above supports `inboxNotificationMessageClicked` callback on the click of an App Inbox item, such as text or media.

From the CleverTap Flutter SDK v1.5.6 onwards and below v1.6.0, the `inboxNotificationMessageClicked` callback supports the button click besides the item click. Two new arguments `contentPageIndex` and `buttonIndex` are added to the callback's payload.

To use this, add a callback for `inboxNotificationMessageClicked` in the following way:

```javascript Dart
_clevertapPlugin.setCleverTapInboxNotificationMessageClickedHandler(
        inboxNotificationMessageClicked);

void inboxNotificationMessageClicked(
      Map<String, dynamic>? data, int contentPageIndex, int buttonIndex) {
    this.setState(() {
      print(
          "inboxNotificationMessageClicked called = InboxItemClicked at page-index $contentPageIndex with button-index $buttonIndex");

      var inboxMessageClicked = data?["msg"];
      if (inboxMessageClicked == null) {
        return;
      }

      //The contentPageIndex corresponds to the page index of the content, which ranges from 0 to the total number of pages for carousel templates. For non-carousel templates, the value is always 0, as they only have one page of content.
      var messageContentObject = inboxMessageClicked["content"][contentPageIndex];

      //The buttonIndex corresponds to the CTA button clicked (0, 1, or 2). A value of -1 indicates the app inbox body/message clicked.
      if (buttonIndex != -1) {
        //button is clicked
        var buttonObject = messageContentObject["action"]["links"][buttonIndex];
        var buttonType = buttonObject?["type"];
        print("type of button clicked: $buttonType");
      } else {
        //Item's body is clicked
        print("type/template of App Inbox item: ${inboxMessageClicked["type"]}");
      }
    });
  }
```

> 📘 Parameters received in `inboxNotificationMessageClicked` callback
>
> * The `data` parameter represents the entire _App Inbox Item_ object.
> * The `contentPageIndex` parameter corresponds to the page index of the content, which ranges from 0 to n-1 (where n is the total number of pages for carousel templates). The non-carousel templates always have a value of 0, because they have only one page of content.
> * The `buttonIndex` parameter corresponds to the App Inbox button (0, 1, or 2), that is clicked. A value of -1 in `buttonIndex` field indicates the entire _App Inbox Item_ is clicked.

The CleverTap Flutter SDK v1.1.1 and above supports an exclusive `inboxNotificationButtonClicked` callback on the click of KV type of buttons. It returns a Map of Key-Value pairs. To use this, add a callback for `inboxNotificationButtonClicked` in following way:

```javascript Dart
_clevertapPlugin.setCleverTapInboxNotificationButtonClickedHandler(
        inboxNotificationButtonClicked);

void inboxNotificationButtonClicked(Map<String, dynamic>? map) {
    this.setState(() {
      print("inboxNotificationButtonClicked called = ${map.toString()}");
    });
  }
```

### Creating your own App Inbox

You can choose to create your own App Inbox by calling the following APIs

#### Initialize App Inbox

```javascript Dart
CleverTapPlugin.initializeInbox();
```

#### Show App Inbox

```javascript Dart
var styleConfig = {
  'noMessageTextColor': '#ff6600',
  'noMessageText': 'No message(s) to show.',
  'navBarTitle': 'App Inbox'
};
CleverTapPlugin.showInbox(styleConfig);
```

#### Get Total Inbox Message Count

```javascript Dart
int total = await CleverTapPlugin.getInboxMessageCount();
print("Total count = " + total.toString());
```

#### Get Total Inbox Unread Count

```javascript Dart
int unread = await CleverTapPlugin.getInboxMessageUnreadCount();
print("Unread count = " + unread.toString());
```

#### Get All Inbox Messages

```javascript Dart
List messages = await CleverTapPlugin.getAllInboxMessages();
```

#### Get All Inbox Unread Messages

```javascript Dart
List messages = await CleverTapPlugin.getUnreadInboxMessages();
```

#### Get Inbox Message for the given message-id

```javascript Dart
var messageForId = await CleverTapPlugin.getInboxMessageForId(messageId);
```

#### Delete Message for the given message-id

```javascript Dart
await CleverTapPlugin.deleteInboxMessageForId(messageId);
```

#### Mark Message as Read for the given message-id

```javascript Dart
var messageList = await CleverTapPlugin.getUnreadInboxMessages();
    if (messageList == null || messageList.length == 0) return;
    Map<dynamic, dynamic> itemFirst = messageList[0];
    if (Platform.isAndroid) {
      await CleverTapPlugin.markReadInboxMessageForId(itemFirst["id"]);
    } else if (Platform.isIOS) {
      await CleverTapPlugin.markReadInboxMessageForId(itemFirst["_id"]);
    }
```

#### Raise Notification Viewed event for Inbox Message. Message-id should be a String

```javascript Dart
await CleverTapPlugin.pushInboxNotificationViewedEventForId(messageId);
```

#### Raise Notification Clicked event for Inbox Message. Message-id should be a String

```javascript Dart
await CleverTapPlugin.pushInboxNotificationClickedEventForId(messageId);
```
