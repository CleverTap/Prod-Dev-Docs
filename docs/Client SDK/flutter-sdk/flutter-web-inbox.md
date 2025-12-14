---
title: Flutter Web Inbox
excerpt: Learn how to set up and handle web inbox notifications in Flutter.
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

The Web Inbox channel enables you to deliver personalized notifications to your website users in real time based on the actions they perform on your website. The Web Inbox window stacks these notifications; users can access them later.

# Set Up Web Inbox in Flutter

To set up Web Inbox in Flutter: 

1. Add the button/div with the `id` as configured on the CleverTap dashboard. Set the visibility of the element as hidden.
   ```d Dart
   <button id="Element ID" style="visibility: hidden;">Inbox</button>
   ```
2. Clear the Notifications Badge in the Web Inbox configuration Style section on the CleverTap dashboard.
3. Use the custom widget`NotificationButton` to pass the widget on which the inbox must be rendered.
   ```d Dart
   NotificationButton(child: Icon(Icons.notifications))
   ```

# Manage Web Inbox in Flutter

You can manage your Web Inbox by calling the following APIs:

## Get Total Inbox Message Count

Retrieves the total count of the web inbox messages.

```d Dart
int total = await CleverTapPlugin.getInboxMessageCount();
print("Total count = " + total.toString());
```

## Get Total Inbox Unread Count

Retrieves the total count of the unread web inbox messages.

```d Dart
int unread = await CleverTapPlugin.getInboxMessageUnreadCount();
print("Unread count = " + unread.toString());
```

## Get All Inbox Messages

Retrieves all the inbox message content. 

```d Dart
List messages = await CleverTapPlugin.getAllInboxMessages();
```

## Get All Unread Inbox Messages

Retrieves all the unread inbox messages. 

```d Dart
List messages = await CleverTapPlugin.getUnreadInboxMessages();
```

## Get an Inbox Message for the Given Message ID

Retrieves an inbox message based on the specified message ID. 

```d Dart
var messageForId = await CleverTapPlugin.getInboxMessageForId(messageId);				
```

## Delete an Inbox Message for the Given message ID

Deletes an inbox message based on the specified message ID.

```d Dart
await CleverTapPlugin.deleteInboxMessageForId(messageId);	
```

## Mark Message as Read for the Given Message ID

Marks an inbox message as read based on the specified message ID.

```d Dart
await CleverTapPlugin.markReadInboxMessageForId(messageId);	
```
