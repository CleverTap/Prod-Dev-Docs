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

```javascript JavaScript
CleverTap.initializeInbox()
```

```javascript JavaScript
document.addEventListener('onCleverTapInboxDidInitialize', () => {
            
CleverTap.showInbox({'tabs': ['Offers', 'Promotions'],
        'navBarTitle': 'My App Inbox',
        'navBarTitleColor': '#FF0000',
        'navBarColor': '#FFFFFF',
        'inboxBackgroundColor': '#AED6F1',
        'backButtonColor': '#00FF00',
        'unselectedTabColor': '#0000FF',
        'selectedTabColor': '#FF0000',
        'selectedTabIndicatorColor': '#000000',
        'noMessageText': 'No message(s)',
        'noMessageTextColor': '#FF0000'})   
        }
    )
```

When a new inbox message arrives, the SDK provides the `inboxMessagesDidUpdate()` callback.

```javascript JavaScript
document.addEventListener('onCleverTapInboxMessagesDidUpdate', () => {
            log("Inbox updated")
        }
    )
```

### Creating your own App Inbox

You can choose to create your own App Inbox by calling the following APIs:

#### Initialize App Inbox

```javascript JavaScript
CleverTap.initializeInbox()
```

#### Show App Inbox

```javascript JavaScript
CleverTap.showInbox({'tabs': ['Offers', 'Promotions'],
        'navBarTitle': 'My App Inbox',
        'navBarTitleColor': '#FF0000',
        'navBarColor': '#FFFFFF',
        'inboxBackgroundColor': '#AED6F1',
        'backButtonColor': '#00FF00',
        'unselectedTabColor': '#0000FF',
        'selectedTabColor': '#FF0000',
        'selectedTabIndicatorColor': '#000000',
        'noMessageText': 'No message(s)',
        'noMessageTextColor': '#FF0000'})   
        }
    )
```

#### Get Total Inbox Message Count

```javascript JavaScript
CleverTap.getInboxMessageCount(val => log("Inbox read message count" + val))
```

#### Get Total Inbox Unread Count

```javascript JavaScript
CleverTap.getInboxMessageUnreadCount(val => log("Inbox unread message count" + val))
```

#### Get All Inbox Messages

```javascript JavaScript
CleverTap.getAllInboxMessages(val => log("Inbox messages are " + val))
```

#### Get All Inbox Unread Messages

```javascript JavaScript
CleverTap.getUnreadInboxMessages(val => log("Unread Inbox messages are " + val))
```

#### Get Inbox Message for the given message-id

```javascript JavaScript
CleverTap.getInboxMessageForId("1642753141_1642755745", val => log("Inbox message is " + JSON.stringify(val)))
```

#### Delete Message for the given message-id

```javascript JavaScript
CleverTap.deleteInboxMessageForId("messageId")
```

#### Mark Message as Read for the given message-id

```javascript JavaScript
CleverTap.markReadInboxMessageForId("messageId")
```

#### Raise Notification Viewed event for Inbox Message

Message-id must be a String.

```javascript JavaScript
CleverTap.pushInboxNotificationViewedEventForId("messageId")
```

#### Raise Notification Clicked event for Inbox Message

Message-id must be a String.

```javascript JavaScript
CleverTap.pushInboxNotificationClickedEventForId("messageId")
```
