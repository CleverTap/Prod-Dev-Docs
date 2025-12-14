---
title: Cordova App Inbox
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
# App Inbox
The CleverTap React Native SDK allows you to create App Inbox notifications for your users. You can use the App Inbox provided by CleverTap or create your own. You can design App Inbox notifications right from the dashboard.

## JavaScript
Initializing the Inbox provides a callback to two methods `onCleverTapInboxDidInitialize` and `onCleverTapInboxMessagesDidUpdate`.
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.initializeInbox()",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "document.addEventListener('onCleverTapInboxDidInitialize', () => {\n            \nCleverTap.showInbox({'tabs': ['Offers', 'Promotions'],\n        'navBarTitle': 'My App Inbox',\n        'navBarTitleColor': '#FF0000',\n        'navBarColor': '#FFFFFF',\n        'inboxBackgroundColor': '#AED6F1',\n        'backButtonColor': '#00FF00',\n        'unselectedTabColor': '#0000FF',\n        'selectedTabColor': '#FF0000',\n        'selectedTabIndicatorColor': '#000000',\n        'noMessageText': 'No message(s)',\n        'noMessageTextColor': '#FF0000'})   \n        }\n    )",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
When a new inbox message arrives, the SDK provides the `inboxMessagesDidUpdate()` callback.
[block:code]
{
  "codes": [
    {
      "code": "document.addEventListener('onCleverTapInboxMessagesDidUpdate', () => {\n            log(\"Inbox updated\")\n        }\n    )",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
### Creating your own App Inbox
You can choose to create your own App Inbox by calling the following APIs:

#### Initialize App Inbox
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.initializeInbox()",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
#### Show App Inbox
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.showInbox({'tabs': ['Offers', 'Promotions'],\n        'navBarTitle': 'My App Inbox',\n        'navBarTitleColor': '#FF0000',\n        'navBarColor': '#FFFFFF',\n        'inboxBackgroundColor': '#AED6F1',\n        'backButtonColor': '#00FF00',\n        'unselectedTabColor': '#0000FF',\n        'selectedTabColor': '#FF0000',\n        'selectedTabIndicatorColor': '#000000',\n        'noMessageText': 'No message(s)',\n        'noMessageTextColor': '#FF0000'})   \n        }\n    )",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
#### Get Total Inbox Message Count
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.getInboxMessageCount(val => log(\"Inbox read message count\" + val))",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
#### Get Total Inbox Unread Count
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.getInboxMessageUnreadCount(val => log(\"Inbox unread message count\" + val))\n",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
#### Get All Inbox Messages
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.getAllInboxMessages(val => log(\"Inbox messages are \" + val))\n",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
#### Get All Inbox Unread Messages
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.getUnreadInboxMessages(val => log(\"Unread Inbox messages are \" + val))",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
#### Get Inbox Message for the given message-id
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.getInboxMessageForId(\"1642753141_1642755745\", val => log(\"Inbox message is \" + JSON.stringify(val)))",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
#### Delete Message for the given message-id
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.deleteInboxMessageForId(\"messageId\")",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
#### Mark Message as Read for the given message-id
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.markReadInboxMessageForId(\"messageId\")",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
#### Raise Notification Viewed event for Inbox Message
Message-id must be a String.
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.pushInboxNotificationViewedEventForId(\"messageId\")",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]
#### Raise Notification Clicked event for Inbox Message
Message-id must be a String.
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.pushInboxNotificationClickedEventForId(\"messageId\")",
      "language": "javascript",
      "name": "JavaScript"
    }
  ]
}
[/block]