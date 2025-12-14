---
title: Branch_new_method_CustomerUserId_WIP
excerpt: ''
deprecated: false
hidden: true
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

## **Android**
In your Android app, before ​you ​initialize ​in ​your Application ​`onCreate()` ​or ​Deep ​Link ​Activity’s ​`onCreate()` add the following code snippet.

### SDK version 4.2.0 and above

The `getCleverTapAttributionIdentifier` method is deprecated for all CleverTap Android SDKs version 4.2.0 and above. Use the new `getCleverTapID` method to get the CleverTap ID on the `OnInitCleverTapIDListener` callback and set the `RequestMetadata` of Branch.
[block:code]
{
  "codes": [
    {
      "code": "cleverTapInstance.getCleverTapID(new OnInitCleverTapIDListener() {\n   @Override\n   public void onInitCleverTapID(final String cleverTapID) {\n   // Callback on main thread\n   Branch branch = Branch.getInstance();\n   branch.setRequestMetadata(\"$clevertap_attribution_id\",\n   cleverTapID);\n   ...\n   Branch.initSession(...);\n   }  \n});",
      "language": "java"
    },
    {
      "code": "cleverTapInstance?.getCleverTapID {\n// Callback on main thread\n      Branch.getInstance()?.setRequestMetadata(\"$clevertap_attribution_id\",\nit)\n...\nBranch.initSession(...)\n\n}\n\n\n",
      "language": "kotlin"
    }
  ]
}
[/block]
### SDK version 4.1.1 and below
[block:code]
{
  "codes": [
    {
      "code": "Branch branch = Branch.getInstance();\nbranch.setRequestMetadata(\"$clevertap_attribution_id\",\ncleverTapInstance.getCleverTapAttributionIdentifier());\n...\nBranch.initSession(...);\n",
      "language": "java"
    },
    {
      "code": "Branch.getInstance()?.setRequestMetadata(\"$clevertap_attribution_id\",\ncleverTapInstance?.cleverTapAttributionIdentifier);\n...\nBranch.initSession(...);\n\n",
      "language": "kotlin"
    }
  ]
}
[/block]
**iOS**
In your iOS app, inside didFinishLaunchingWithOptions add the following code snippet.
[block:code]
{
  "codes": [
    {
      "code": "Branch *branch = [Branch getInstance];\n[CleverTap autoIntegrate];\n[[Branch getInstance] setRequestMetadataKey:@\"$clevertap_attribution_id\"\nvalue:[[CleverTap sharedInstance] profileGetCleverTapAttributionIdentifier]];",
      "language": "objectivec"
    },
    {
      "code": "CleverTap.autoIntegrate()\nif let branch = Branch.getInstance() {\nbranch.setRequestMetadataKey(\"$clevertap_attribution_id\",\nvalue:CleverTap.sharedInstance()?profileGetCleverTapAttributionIdentifier() as\nNSObject!);\n}",
      "language": "swift"
    }
  ]
}
[/block]
# Step 3: View Data in the CleverTap Dashboard
After you integrate, Branch will now push data in your CleverTap dashboard. You can view it under event UTM Visited filtered by event property, UTM_source, UTM_medium or UTM_campaign.