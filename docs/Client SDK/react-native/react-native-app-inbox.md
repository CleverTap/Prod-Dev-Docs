---
title: React Native App Inbox
excerpt: Learn how to handle app inbox notifications in React Native.
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
      title: React Native Advanced Features
      url: https://developer.clevertap.com/docs/react-native-advanced-features
    - type: link
      title: React Native Resources
      url: https://developer.clevertap.com/docs/react-native-resources
---
# App Inbox

The CleverTap React Native SDK allows you to create App Inbox notifications for your users. You can use the App Inbox provided by CleverTap or create your own. You can also design App Inbox notifications right from the dashboard.

## Android

On the Android side, add the following inbox dependencies in your app's `build.gradle` file:

> 📘 Migrating from ExoPlayer to AndroidX Media3
>
> Starting from CleverTap React Native SDK v3.0.0, CleverTap now supports [AndroidX Media3](https://developer.android.com/media/media3/exoplayer/migration-guide), replacing the deprecated ExoPlayer libraries. CleverTap continues to support [ExoPlayer](https://github.com/google/ExoPlayer), but for a smoother migration to AndroidX Media3, update the following dependencies:

```groovy Dependencies with AndroidX Media3 Libraries
//MANDATORY for App Inbox

implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'

//OPTIONAL AndroidX Media3 Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped if you do not add these dependencies

implementation "androidx.media3:media3-exoplayer:1.1.1"
implementation "androidx.media3:media3-exoplayer-hls:1.1.1"
implementation "androidx.media3:media3-ui:1.1.1"
```
```groovy Dependencies with Exoplayer Libraries
//MANDATORY for App Inbox

implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'

//OPTIONAL ExoPlayer Libraries for Audio/Video Inbox Messages. Audio/Video messages will be dropped if you do not add these dependencies

implementation 'com.google.android.exoplayer:exoplayer:2.17.1'
implementation 'com.google.android.exoplayer:exoplayer-hls:2.17.1'
implementation 'com.google.android.exoplayer:exoplayer-ui:2.17.1'
```

## React-Native

Initializing the Inbox provides a callback to two methods `inboxDidInitialize()` and `inboxMessagesDidUpdate()`:

```javascript JavaScript
CleverTap.initializeInbox();
```

Configure styling and show the inbox by:

1. Customize the config object and call the Inbox in the `inboxDidInitialize()` method.
2. Call the following method on the button click which opens the CleverTap Inbox for your App:

```javascript JavaScript
CleverTap.addListener(CleverTap.CleverTapInboxDidInitialize, (event) => {
        _handleCleverTapInbox(CleverTap.CleverTapInboxDidInitialize, event);
    });

function _handleCleverTapInbox(eventName, event) {
    console.log('CleverTap Inbox Event - ', eventName, event);
    CleverTap.showInbox({
        'tabs': ['Offers', 'Promotions'],
        'navBarTitle': 'My App Inbox',
        'navBarTitleColor': '#FF0000',
        'navBarColor': '#FFFFFF',
        'inboxBackgroundColor': '#AED6F1',
        'backButtonColor': '#00FF00',
        'unselectedTabColor': '#0000FF',
        'selectedTabColor': '#FF0000',
        'selectedTabIndicatorColor': '#000000',
        'noMessageText': 'No message(s)',
        'noMessageTextColor': '#FF0000'
    });
}
```

When a new inbox message arrives, the SDK provides the `inboxMessagesDidUpdate()` callback:

```javascript JavaScript
CleverTap.addListener(CleverTap.CleverTapInboxMessagesDidUpdate, (event) => {
        _handleCleverTapInbox(CleverTap.CleverTapInboxMessagesDidUpdate, event);
    });

function _handleCleverTapInbox(eventName, event) {
    console.log('CleverTap Inbox Event - ', eventName, event);
}
```

#### App Inbox Item and Button Click Callbacks

Let's understand the types of buttons first that App Inbox supports:

* **URL button**: fires the deep link with the associated URL.
* **Copy button**: copies the associated text to the clipboard.
* **KV button**: contains the custom kev-value pair for custom handling.

The CleverTap React Native SDK v0.9.2 and above supports `CleverTap.CleverTapInboxMessageTapped` callback on the click of an App Inbox item, such as text or media.

From the CleverTap React Native SDK v0.9.6 onwards and below v1.0.0, the `CleverTap.CleverTapInboxMessageTapped` callback supports the button click besides the item click. Two new arguments  `contentPageIndex` and `buttonIndex`  are added to the payload.

To use this, add a callback for `CleverTap.CleverTapInboxMessageTapped` in the following way:

```javascript
CleverTap.addListener(CleverTap.CleverTapInboxMessageTapped, (event) => {
   
    //following contentPageIndex and buttonIndex fields are available from CleverTap React Native SDK v0.9.6 onwards and below v1.0.0 
    let contentPageIndex = event.contentPageIndex;
    let buttonIndex = event.buttonIndex;
    console.log(appInboxTag, 'InboxItemClicked at page-index ' + contentPageIndex + ' with button-index ' + buttonIndex) ;

    var data;
    if (Platform.OS === 'android') {
        //parses the stringified json to the json format
        data = JSON.parse(event.data); 
    } else if (Platform.OS === 'ios') {
         data = event.data; 
    }
    let inboxMessageClicked = data.msg;
    
    //The contentPageIndex corresponds to the page index of the content, which ranges from 0 to the total number of pages for carousel templates. For non-carousel templates, the value is always 0, as they only have one page of content.
    let messageContentObject = inboxMessageClicked.content[contentPageIndex];
    
    //The buttonIndex corresponds to the CTA button clicked (0, 1, or 2). A value of -1 indicates the app inbox body/message clicked.
    if (buttonIndex != -1) {
        //button is clicked
        let buttonObject = messageContentObject.action.links[buttonIndex];
        let buttonType = buttonObject.type;
        console.log("type of button clicked: " + buttonType);
    } else {
        //Item's body is clicked
        console.log(appInboxTag, 'type/template of App Inbox item: ' + inboxMessageClicked.type)
    }
});
```

> 📘 Payload sent via`CleverTap.CleverTapInboxMessageTapped`
>
> * The `data` field represents the entire App Inbox item object that is clicked.
> * The `contentPageIndex` field corresponds to the page index of the content, which ranges from 0 to n-1 (where n is the total number of pages for carousel templates). The non-carousel templates always have a value of 0, as they only have one page of content.
> * The `buttonIndex` field corresponds to the App Inbox button clicked (0, 1, or 2). A value of -1 in `buttonIndex` field indicates the App Inbox item is clicked.

The CleverTap React Native SDK v0.3.0 and above supports an exclusive `CleverTap.CleverTapInboxMessageButtonTapped` callback on the click of **KV** type of buttons. It returns a Map of Key-Value pairs. To use this, add a callback for `CleverTap.CleverTapInboxMessageButtonTapped` in following way:

```javascript
CleverTap.addListener(CleverTap.CleverTapInboxMessageButtonTapped, (event) => {/*consume the payload*/});
```

### Dismiss App Inbox

Use the following method to dismiss the App Inbox.

```javascript
CleverTap.dismissInbox();
```

### Creating your own App Inbox

You can create your own App Inbox by calling the following APIs, which allow you to initialize, display, and interact with inbox messages.

#### Initialize App Inbox

To initialize the App Inbox, call:

```javascript JavaScript
CleverTap.initializeInbox();
```

#### Show App Inbox

You can customize the App Inbox UI using the following method:

```javascript JavaScript
CleverTap.showInbox({
        'tabs': ['Offers', 'Promotions'],
        'navBarTitle': 'My App Inbox',
        'navBarTitleColor': '#FF0000',
        'navBarColor': '#FFFFFF',
        'inboxBackgroundColor': '#AED6F1',
        'backButtonColor': '#00FF00',
        'unselectedTabColor': '#0000FF',
        'selectedTabColor': '#FF0000',
        'selectedTabIndicatorColor': '#000000',
        'noMessageText': 'No message(s)',
        'noMessageTextColor': '#FF0000'
    });
```

The above parameters allow you to configure the Inbox appearance, such as:

* Tabs for categorizing messages (for example, Offers, Promotions)
* Navigation bar customization (title, colors)
* Inbox background and message display settings

#### Get Total Inbox Message Count

Retrieve the total number of messages in the inbox:

```javascript JavaScript
CleverTap.getInboxMessageCount((err, res) => {
        console.log('Total Messages: ', res, err);
    });
```

#### Get Total Inbox Unread Count

Retrieve the count of unread messages:

```javascript JavaScript
CleverTap.getInboxMessageUnreadCount((err, res) => {
        console.log('Unread Messages: ', res, err);
    });
```

#### Get All Inbox Messages

Retrieve all inbox messages:

```javascript JavaScript
CleverTap.getAllInboxMessages((err, res) => {
        console.log('All Inbox Messages: ', res, err);
    });
```

#### Get All Inbox Unread Messages

Retrieve only the unread messages:

```javascript JavaScript
CleverTap.getUnreadInboxMessages((err, res) => {
        console.log('Unread Inbox Messages: ', res, err);
    });
```

#### Get Inbox Message for the given message-id

Retrieve a message using its unique message ID:

```javascript JavaScript
CleverTap.getInboxMessageForId('Message Id', (err, res) => {
        console.log("marking message read = " + res);
    });
```

#### Delete Message for the given message-id

Remove a specific message using its message ID:

```javascript JavaScript
CleverTap.deleteInboxMessageForId('Message Id');// Message-id must be a String
```

#### Mark Message as Read for the given message-id

Mark a specific inbox message as read:

```javascript JavaScript
CleverTap.markReadInboxMessageForId('Message Id');// Message-id must be a String
```

#### Raise Notification Viewed event for Inbox Message

Track when a user views an inbox message:

```javascript JavaScript
CleverTap.pushInboxNotificationViewedEventForId('Message Id');// Message-id must be a String
```

#### Raise Notification Clicked event for Inbox Message

Track when a user clicks on an inbox message:

```javascript JavaScript
CleverTap.pushInboxNotificationClickedEventForId('Message Id');// Message-id must be a String
```

#### App Inbox Message Payload Structure

When implementing the App Inbox, messages received from the dashboard follow a standardized JSON payload structure. Following is an example of an App Inbox message payload:

```asp JavaScript
const message = {
  messageId: "1234567890", // Message-id must be a String
  campaignId: "987654321",
  title: "Exclusive Offer!",
  body: "Get 20% off on your next purchase. Limited time only!",
  imageUrl: "https://example.com/image.jpg",
  actionUrl: "https://example.com/deal",
  bgColor: "#FFFFFF",
  isRead: false,
  date: 1700000000000,
  expires: 1705000000000,
  tags: ["discount", "offer"],
  orientation: "portrait",
  customData: {
    discountCode: "SAVE20"
  },
  wzrkParams: {
    source: "promotion"
  }
};

console.log(message);
```

The following table describes the key attributes of an App Inbox message:  

| **Field**     | **Type**     | **Description**                                                                                               |
| ------------- | ------------ | ------------------------------------------------------------------------------------------------------------- |
| `messageId`   | `String`     | A unique identifier for the App Inbox message.                                                                |
| `campaignId`  | `String`     | The campaign ID is associated with the message.                                                               |
| `title`       | `String`     | The title of the message is usually used as a headline.                                                       |
| `body`        | `String`     | The main content or description of the message.                                                               |
| `imageUrl`    | `String`     | The URL of the image is displayed in the message.                                                             |
| `actionUrl`   | `String`     | URL associated with the message action (for example, opening a webpage or deep link).                         |
| `bgColor`     | `String`     | Background color of the message, allowing customization to match app themes.                                  |
| `isRead`      | `boolean`    | A flag indicating whether the user has read the message.                                                      |
| `date`        | `long`       | The timestamp when the message was created, in milliseconds. ($D\_EPOCH format).                              |
| `expires`     | `long`       | The timestamp when the message will expire and no longer be visible.                                          |
| `tags`        | `JSONArray`  | Tags used to categorize or filter messages.                                                                   |
| `orientation` | `String`     | Orientation of the message layout (for example, `portrait` or `landscape`).                                   |
| `customData`  | `JSONObject` | Any additional metadata sent with the message (for example, discount codes, deep links).                      |
| `wzrkParams`  | `JSONObject` | Additional parameters used for internal tracking within CleverTap (for example, campaign source, message ID). |
