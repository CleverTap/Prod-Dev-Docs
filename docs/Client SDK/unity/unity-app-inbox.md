---
title: Unity App Inbox
excerpt: >-
  Learn how to use App Inbox to deliver persistent, personalized messages within
  your app using CleverTap’s Unity and Unity NativeSDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# App Inbox

App Inbox lets you deliver persistent, rich messages to users directly within your app. These messages are stored locally on the device and remain accessible until deleted or expired.

With CleverTap’s Unity SDK, you can:

* Use the default App Inbox provided by CleverTap, or build a custom interface.
* Design and manage App Inbox notifications directly from the dashboard.

## Android Setup

To enable App Inbox functionality in your Android app, add the following dependencies to your `build.gradle` file:

```groovy
// Mandatory for App Inbox support
implementation 'androidx.appcompat:appcompat:1.3.1'
implementation 'androidx.recyclerview:recyclerview:1.2.1'
implementation 'androidx.viewpager:viewpager:1.0.0'
implementation 'com.google.android.material:material:1.4.0'
implementation 'com.github.bumptech.glide:glide:4.12.0'

// Optional – for Audio/Video messages via ExoPlayer
implementation "com.google.android.exoplayer:exoplayer:2.19.1"
implementation "com.google.android.exoplayer:exoplayer-hls:2.19.1"
implementation "com.google.android.exoplayer:exoplayer-ui:2.19.1"
```

> 📘 Note
>
> Ensure these dependencies are added to the module-level `build.gradle` file, not the project-level one.

## Initialize App Inbox

Before displaying or interacting with App Inbox, initialize it using:

```csharp
CleverTap.InitializeInbox();
```

This triggers the following callback methods:

* `inboxDidInitialize()` – Triggers when the inbox is initialized.
* `inboxMessagesDidUpdate()` – Triggers when new App Inbox messages are available.

You can use these callbacks to configure styling and display the inbox.

### Sample Implementation

You can customize the App Inbox UI using the following sample style configuration options:

```csharp
void CleverTapInboxDidInitializeCallback()
{
    Dictionary<string, object> styleConfig = new Dictionary<string, object>();
    styleConfig.Add("navBarTitle", "My App Inbox");
    styleConfig.Add("navBarTitleColor", "#FF0000");
    styleConfig.Add("navBarColor", "#FFFFFF");
    styleConfig.Add("inboxBackgroundColor", "#AED6F1");
    styleConfig.Add("backButtonColor", "#00FF00");
    styleConfig.Add("unselectedTabColor", "#0000FF");
    styleConfig.Add("selectedTabColor", "#FF0000");
    styleConfig.Add("noMessageText", "No message(s)");
    styleConfig.Add("noMessageTextColor", "#FF0000");

    string jsonStr = JsonConvert.SerializeObject(styleConfig, Formatting.Indented);
    CleverTap.ShowAppInbox(jsonStr);
}
```

> 📘 Custom Parser Usage
>
> This example uses [Newtonsoft’s JSON parser](https://www.newtonsoft.com/json) to serialize the `Dictionary` object into a string, which is then passed to `ShowAppInbox()`.

When new messages arrive, the SDK invokes:

```csharp
void CleverTapInboxMessagesDidUpdateCallback()
{
    Debug.Log("Unity received inbox messages updated");
}
```

To handle inbox item clicks, use this callback to redirect users to relevant screens when a specific message is tapped.

```csharp
void CleverTapInboxItemClicked(string message)
{
    Debug.Log("Unity received inbox message clicked: " + (!String.IsNullOrEmpty(message) ? message : "NULL"));
}
```

## Create a Custom App Inbox

Use the following APIs to build and control a custom inbox layout and behavior in your Unity app.

### Initialize App Inbox

Initializes the inbox. Must be called before showing or fetching inbox messages.

```csharp
CleverTap.InitializeInbox();
```

### Show App Inbox (Default UI)

Displays the default CleverTap Inbox.

```csharp
CleverTap.ShowAppInbox("");
```

### Show App Inbox (Custom UI)

Displays a customized App Inbox using specified style parameters.

```csharp
Dictionary<string, object> styleConfig = new Dictionary<string, object>();
// (same configuration as earlier)
string jsonStr = JsonConvert.SerializeObject(styleConfig, Formatting.Indented);
CleverTap.ShowAppInbox(jsonStr);
```

### Dismiss App Inbox

Closes the App Inbox view. Call this method when the user taps the button or a close icon in your custom UI.

```csharp
CleverTap.DismissAppInbox();
```

### Get Total Inbox Message Count

Returns the total number of App Inbox messages (read and unread).

```csharp
CleverTap.GetInboxMessageCount();
```

### Get Total Inbox Unread Count

Returns the count of unread App Inbox messages.

```csharp
CleverTap.GetInboxMessageUnreadCount();
```

### Get All Inbox Messages

Retrieves all stored inbox messages.

```csharp
CleverTap.GetAllInboxMessages();
```

### Get All Unread Inbox Messages

Returns only unread App Inbox messages stored locally on the device.

```csharp
CleverTap.GetInboxMessageUnread();
```

### Get Inbox Message by ID

Fetch a specific App Inbox message by its ID.

```csharp
CleverTap.GetInboxMessageForId("messageId");
```

### Delete Inbox Message by ID

Deletes a single inbox message using its message ID.

```csharp
CleverTap.DeleteInboxMessageForID("messageId");
```

### Delete Multiple Inbox Messages

Deletes multiple messages by providing an array of message IDs.

> 📘 iOS-Only Support
>
> This method is available only on iOS.

```csharp
string[] ids = {"id1", "id2", "id3"};
CleverTap.DeleteInboxMessagesForIDs(ids);
```

### Mark Inbox Message as Read

Marks a specific message as read.

```csharp
CleverTap.MarkReadInboxMessageForID("messageId");
```

### Record Notification Viewed Event

Logs a `Notification Viewed` event for a message.

```csharp
CleverTap.RecordInboxNotificationViewedEventForID("messageId");
```

### Record Notification Clicked Event

Logs a `Notification Clicked` event for a message.

```csharp
CleverTap.RecordInboxNotificationClickedEventForID("messageId");
```
