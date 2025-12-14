---
title: AppsFlyer
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
CleverTap helps to track your app installs via AppsFlyer. This integration requires changes to both your Android and iOS app for successful integration:

#Step 1: Add CleverTap Credentials to AppsFlyer Dashboard

On your AppsFlyer dashboard, select *Integrated Partners* and search for CleverTap. 

Enter your *Account Details* such as CleverTap *Account ID,* *Account Passcode*, and *Account Token*(available in the CleverTap Settings dashboard). 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/02e5c32-Screen-Shot-2017-04-07-at-3.26.40-PM.png",
        "Screen-Shot-2017-04-07-at-3.26.40-PM.png",
        642,
        604,
        "#f5f2eb"
      ]
    }
  ]
}
[/block]
Check Enable and Save. All AppsFlyer parameters will now be sent to CleverTap.

#Step 2: Integration

**Android App**
If you haven't already integrated AppsFlyer, you will first need to follow [this guide](https://support.appsflyer.com/hc/en-us/articles/207032126-AppsFlyer-SDK-Integration-Android).

In your Android app, open the file where you have added your AppsFlyer integration. Add the following code below it.

## SDK version 4.2.0 and above

The `getCleverTapAttributionIdentifier` method is deprecated for CleverTap Android SDKs version 4.2.0 and above. Use the new `getCleverTapID` method to get the CleverTap ID on the `OnInitCleverTapIDListener` to set `CustomerUserId` method of AppsFlyer.
[block:code]
{
  "codes": [
    {
      "code": "cleverTapInstance.getCleverTapID(new OnInitCleverTapIDListener() {\n   @Override\n   public void onInitCleverTapID(final String cleverTapID) {\n       // Callback on main thread\n       appsFlyerLib.setCustomerUserId(cleverTapID);\n   }   \n});",
      "language": "java"
    },
    {
      "code": "cleverTapInstance?.getCleverTapID {\n  // Callback on main thread\n  appsFlyerLib.setCustomerUserId(it)\n}",
      "language": "kotlin"
    }
  ]
}
[/block]
## SDK version 4.1.1 and below
[block:code]
{
  "codes": [
    {
      "code": "String attributionID = cleverTapInstance.getCleverTapAttributionIdentifier();\nappsFlyerLib.setCustomerUserId(attributionID);",
      "language": "java"
    },
    {
      "code": "appsFlyerLib.setCustomerUserId(cleverTapInstance?.cleverTapAttributionIdentifier)\n\n  ",
      "language": "kotlin"
    }
  ]
}
[/block]
**iOS App**
In your iOS app code, add the following code:
[block:code]
{
  "codes": [
    {
      "code": "[CleverTap autoIntegrate];\n[[AppsFlyerTracker sharedTracker] setCustomerUserID:[[CleverTap sharedInstance] profileGetCleverTapAttributionIdentifier]];",
      "language": "objectivec"
    }
  ]
}
[/block]
**React-Native App**
In your React-Native App, add the following code:

[block:code]
{
  "codes": [
    {
      "code": "CleverTap.profileGetCleverTapAttributionIdentifier((err, res) => { \n\nconst userId = res;\n\nappsFlyer.setCustomerUserId(userId, (response) => {   \n\n//.. });\n\n});\n  \n  ",
      "language": "javascript"
    }
  ]
}
[/block]
#Step 3: View Data in Dashboard
After you integrate, AppsFlyer will push data into your CleverTap dashboard. You can view it under event UTM Visited filtered by event property, UTM_source, UTM_medium or UTM_campaign.
[block:callout]
{
  "type": "info",
  "body": "CleverTap accepts only non-organic installs from Appsflyer.",
  "title": "Note:"
}
[/block]