---
title: iOS App Inbox
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
# Overview

App Inbox is a messaging channel that provides the ability to deliver rich, individually customized content to your users. Messages sent to App Inbox are saved on the user's device.

App Inbox provides the ability to send permanent content directly to your app from the CleverTap dashboard. Moreover, the inbox messages target individual segments such as other messaging channels. Your inbox can look different from another user's inbox; the possibilities are endless.

# App Inbox

* The CleverTap SDK 3.4.0 and above, allows you to create App Inbox notifications for your users. 
* You can use the App Inbox provided by CleverTap or create your own
* You can design App Inbox notifications right from the dashboard

Note that it is not applicable for tvOS and watchOS apps.

## Step 1: Integrating the CleverTap App Inbox

* You have to import the CleverTap Inbox Header
* Initialize the CleverTap App Inbox Method

```swift Swift
import CleverTapSDK

//Initialize the CleverTap App Inbox
CleverTap.sharedInstance()?.initializeInbox(callback: ({ (success) in
        let messageCount = CleverTap.sharedInstance()?.getInboxMessageCount()
        let unreadCount = CleverTap.sharedInstance()?.getInboxMessageUnreadCount()
        print("Inbox Message:\(String(describing: messageCount))/\(String(describing: unreadCount)) unread")
 }))
```
```objectivec Objective C
#import <CleverTapSDK/CleverTap+Inbox.h> 

//Initialize the CleverTap App Inbox
    [[CleverTap sharedInstance] initializeInboxWithCallback:^(BOOL success) {
        int messageCount = (int)[[CleverTap sharedInstance] getInboxMessageCount];
        int unreadCount = (int)[[CleverTap sharedInstance] getInboxMessageUnreadCount];
        NSLog(@"Inbox Message: %d/%d", messageCount, unreadCount);
    }];
```

## Step 2: Configure App Inbox Styling

* By default, App Inbox styling will be CleverTap's default colors and font
* You can choose to change the styling as shown below

```swift
// config the style of App Inbox Controller
    let style = CleverTapInboxStyleConfig.init()
    style.title = "App Inbox"
    style.backgroundColor = UIColor.blue()
    style.messageTags = ["tag1", "tag2"]
    style.navigationBarTintColor = UIColor.blue()
    style.navigationTintColor = UIColor.blue()
    style.tabUnSelectedTextColor = UIColor.blue()
    style.tabSelectedTextColor = UIColor.blue()
    style.tabSelectedBgColor = UIColor.blue()
    style.firstTabTitle = "My First Tab"
    
    if let inboxController = CleverTap.sharedInstance()?.newInboxViewController(with: style, andDelegate: self) {
        let navigationController = UINavigationController.init(rootViewController: inboxController)
        self.present(navigationController, animated: true, completion: nil)
  }
```
```objectivec
// config the style of App Inbox Controller
CleverTapInboxStyleConfig *style = [[CleverTapInboxStyleConfig alloc] init];
style.backgroundColor = [UIColor blueColor];
style.messageTags = @[@"tag1", @"tag2"];
style.navigationBarTintColor = [UIColor blueColor];
style.navigationTintColor = [UIColor blueColor];
style.tabUnSelectedTextColor = [UIColor blueColor];
style.tabSelectedTextColor = [UIColor blueColor];
style.tabSelectedBgColor = [UIColor blueColor];
style.firstTabTitle = @"My First Tab";

CleverTapInboxViewController *inboxController = [[CleverTap sharedInstance] 
newInboxViewControllerWithConfig:style andDelegate:self];
    if (inboxController) {
        UINavigationController *navigationController = [[UINavigationController alloc] initWithRootViewController:inboxController];
        [self presentViewController:navigationController animated:YES completion:nil];
    }
```

## Step 3: Creating your own App Inbox

* You can choose to create your own App Inbox by calling the following APIs:

```swift Public APIs: Swift
//Initialize App Inbox
func initializeInbox(callback: CleverTapInboxSuccessBlock) {
}

//Get Inbox Message Count
func getInboxMessageCount() -> Int {
}

//Get Inbox Unread Count
func getInboxMessageUnreadCount() -> Int {
}

//Get All messages 
func getAllInboxMessages() -> [CleverTapInboxMessage] {
}

//Get only Unread messages
func getUnreadInboxMessages() -> [CleverTapInboxMessage] {
}

//Get message object belonging to the given message id only. Message id must be a String
func getInboxMessageForID(messageId: String?) -> CleverTapInboxMessage? {
}

//Delete message from the Inbox. Message must be a String
func deleteInboxMessage(_ message: CleverTapInboxMessage) {
}

//Mark Message as Read
func markReadInboxMessage(_ message: CleverTapInboxMessage) {
}

//Callback on Inbox Message update/delete/read (any activity)
func registerInboxUpdatedBlock(_ block: CleverTapInboxUpdatedBlock) {
}

//Mark Message as Read. Message id must be a String
func markReadInboxMessageForID(messageId: String) {
}

//Delete message from the Inbox. Message id must be a String
func deleteInboxMessageForID(messageId: String) {
}

//Mark Messages as Read. Message ids must be an array of strings
func markReadInboxMessagesForIDs(messageIds: [String]) {
}

//Delete messages from the Inbox. Message ids must be an array of strings
func deleteInboxMessagesForIDs(messageIds: [String]) {
}

```
```objectivec Public APIs: Objective-C
//Initialize App Inbox
- (void)initializeInboxWithCallback:(CleverTapInboxSuccessBlock _Nonnull)callback;

//Get Inbox Message Count
- (NSUInteger)getInboxMessageCount;

//Get Inbox Unread Count
- (NSUInteger)getInboxMessageUnreadCount;

//Get All messages 
- (NSArray<CleverTapInboxMessage *> * _Nonnull )getAllInboxMessages;

//Get only Unread messages
- (NSArray<CleverTapInboxMessage *> * _Nonnull )getUnreadInboxMessages;

//Get message object belonging to the given message id only. Message id must be a String
- (CleverTapInboxMessage * _Nullable )getInboxMessageForId:(NSString *)messageId;

//Delete message from the Inbox
- (void)deleteInboxMessage:(CleverTapInboxMessage * _Nonnull )message;

//Mark Message as Read
- (void)markReadInboxMessage:(CleverTapInboxMessage * _Nonnull) message;

//Callback on Inbox Message update/delete/read (any activity)
- (void)registerInboxUpdatedBlock:(CleverTapInboxUpdatedBlock _Nonnull)block;

//Mark Message as Read. Message id must be a String
- (void)markReadInboxMessageForID:(NSString * _Nonnull)messageId;

//Delete message from the Inbox. Message id must be a String
- (void)deleteInboxMessageForID:(NSString * _Nonnull)messageId;

//Mark Messages as Read. Message ids must be an array of strings
- (void)markReadInboxMessagesForIDs:(NsArray<NSString *> *_Nonnull)messageIds;

//Delete messages from the Inbox. Message ids must be an array of strings
- (void)deleteInboxMessagesForIDs:(NsArray<NSString *> *_Nonnull)messageIds;

```

## Raise Events for Tracking

You can raise the following events for tracking:

### Notification Viewed Event

You can raise the Notification Viewed event in the App Inbox when the user opens up the App inbox dialog to view the notifications.

```swift
//Record Notification Viewed for App Inbox.
func recordInboxNotificationViewedEventForID(messageId: String) {
}
```
```objectivec
//Record Notification Viewed for App Inbox.
- (void)recordInboxNotificationViewedEventForID:(NSString * _Nonnull)messageId;
```

### Notification Clicked Event

You can raise the \_Notification Clicked \_event in the App Inbox using the `recordInboxNotificationClickedEventForID` method whenever the user clicks the App Inbox.

```swift
//Record Notification Clicked for App Inbox.
func recordInboxNotificationClickedEventForID(messageId: String) {
}
```
```objectivec
// Record Notification Clicked for App Inbox.
- (void)recordInboxNotificationClickedEventForID:(NSString * _Nonnull)messageId;
```

### App Inbox Item and Button Click Callbacks:

Let's understand the types of buttons first that App Inbox supports:

* **URL button**: fires the deeplink with the associated URL.
* **Copy button**: copies the associated text to the clipboard
* **KV button**: contains the custom kev-value pair for custom handling

The CleverTap iOS SDK supports `messageDidSelect` callback on the click of an App Inbox item, such as text or media. It also supports the button click besides the item click.

The callback returns CTInboxMessage object, index and buttonIndex parameters. To get the callback on the click of the App Inbox Notification, check that your class implements the CleverTapInboxViewControllerDelegate and use the following method to handle user-selected actions from InApp notifications:

```swift
func messageDidSelect(_ message: CleverTapInboxMessage, at index: Int32, withButtonIndex buttonIndex: Int32) {
    // This is called when an inbox message is clicked(tapped or call to action)
  ...
  }
```
```objectivec
-(void)messageDidSelect:(CleverTapInboxMessage *)message atIndex:(int)index withButtonIndex:(int)buttonIndex {
      // This is called when an inbox message is clicked(tapped or call to action)
    CleverTapLogDebug(_config.logLevel, @"%@: inbox message clicked: %@", self, message);
    CleverTapLogDebug(_config.logLevel, @"%@: InboxItemClicked at page-index: %@ with button-index: %@", self, index, buttonIndex);  
    [self recordInboxMessageStateEvent:YES forMessage:message andQueryParameters:nil];
    
    CleverTapInboxMessageContent *content = (CleverTapInboxMessageContent*)message.content[index];
    NSURL *ctaURL;
    // no button index, means use the on message click url if any
    if (buttonIndex < 0) {
        if (content.actionHasUrl) {
            if (content.actionUrl && content.actionUrl.length > 0) {
                ctaURL = [NSURL URLWithString:content.actionUrl];
            }
        }
    }
    // button index so find the corresponding action link if any
    else {
        if (content.actionHasLinks) {
            NSDictionary *customExtras = [content customDataForLinkAtIndex:buttonIndex];
            if (customExtras && customExtras.count > 0) return;
            NSString *linkUrl = [content urlForLinkAtIndex:buttonIndex];
            if (linkUrl && linkUrl.length > 0) {
                ctaURL = [NSURL URLWithString:linkUrl];
            }
        }
    }
}
```

> 📘 Payload sent via`CleverTap.CleverTapInboxMessageTapped`
>
> The `message` parameter represents the entire App Inbox Item object.\
> The `contentPageIndex` parameter corresponds to the page index of the content, which ranges from 0 to n-1 (where n is the total number of pages for carousel templates). The non-carousel templates always have a value of 0, because they have only one page of content.\
> The `buttonIndex` parameter corresponds to the App Inbox button (0, 1, or 2), that is clicked. A value of -1 in `buttonIndex` field indicates the entire App Inbox Item is clicked.

iOS SDK v3.7.1 and above supports callback on the click of App Inbox Notification Buttons by returning a map of Key-Value pairs. Use the following method:

```swift
func messageButtonTapped(withCustomExtras customExtras: [AnyHashable : Any]?) {
        print("App Inbox Button Tapped with custom extras: ", customExtras ?? "");
    }
```
```objectivec
- (void)messageButtonTappedWithCustomExtras:(NSDictionary *)customExtras {
          NSLog(@"App Inbox Button Tapped with custom extras::%@", customExtras);
  
}
```

# Dismiss App Inbox

Use following method to dismiss the App Inbox:

```swift
CleverTap.sharedInstance()?.dismissAppInbox()

```
```objectivec
[[CleverTap sharedInstance] dismissAppInbox];

```

# Custom Handling Deeplink

Deeplinks navigate the user to a particular part of the application. Whenever you click a deep link, the following `OpenURL` method of the application is invoked:

`application:openURL:sourceApplication:annotation:`

To close the App Inbox when the user is navigated to a different page inside the app (Internal deep link), you must call the `dismissAppInbox` method inside the `openURL` method:

```swift
if url {
  CleverTap.sharedInstance()?.dismissAppInbox()
}
```
```objectivec
if (url) 
{
[[CleverTap sharedInstance] dismissAppInbox];      
}
```
