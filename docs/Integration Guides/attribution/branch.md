---
title: Branch
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
You can use Branch to send install events for your app to CleverTap.

Branch also sends campaign ​and channel​ analytics tags that are attached to the link that drove the events. This will allow you to analyze which campaigns and channels are helping you acquire users in CleverTap. Along with the events, Branch will also send your CleverTap attribution ID, allowing your Branch event data to fit directly in with your CleverTap user identifiers.

Currently, this integration is not self-serve. You must contact your Branch contact for setup. 

# Step 1: Add CleverTap Credentials to Branch Dashboard

In your Branch dashboard, navigate to “Account Settings” and save your app ID.

Go to your CleverTap dashboard and navigate to “Settings” and look for your “Account ID”, “Account token”, and “Passcode”. Provide the value of these three fields to your Branch contact.

# Step 2: Integration

To complete this integration, you need to add a couple of lines of code to your Android and iOS projects.

**Android**\
In your Android app, before ​you ​initialize ​in ​your Application ​onCreate() ​or ​Deep ​Link ​Activity’s ​onCreate() add the following code snippet.

```java
Branch branch = Branch.getInstance();
branch.setRequestMetadata("$clevertap_attribution_id",
cleverTapInstance.getCleverTapAttributionIdentifier());
...
Branch.initSession(...);
```

**iOS**\
In your iOS app, inside didFinishLaunchingWithOptions add the following code snippet.

```objectivec
Branch *branch = [Branch getInstance];
[CleverTap autoIntegrate];
[[Branch getInstance] setRequestMetadataKey:@"$clevertap_attribution_id"
value:[[CleverTap sharedInstance] profileGetCleverTapAttributionIdentifier]];
```
```swift
CleverTap.autoIntegrate()
if let branch = Branch.getInstance() {
branch.setRequestMetadataKey("$clevertap_attribution_id",
value:CleverTap.sharedInstance()?profileGetCleverTapAttributionIdentifier() as
NSObject!);
}
```

# Step 3: View Data in the CleverTap Dashboard

After you integrate, Branch will now push data in your CleverTap dashboard. You can view it under event UTM Visited filtered by event property, UTM\_source, UTM\_medium or UTM\_campaign.
