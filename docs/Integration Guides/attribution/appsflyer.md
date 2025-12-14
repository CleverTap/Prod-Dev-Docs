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
CleverTap helps you to track your app installs via AppsFlyer. This integration requires changes to both your Android and iOS app for a successful integration:

#Step 1: Add CleverTap Credentials to AppsFlyer Dashboard

On your AppsFlyer dashboard, select “Integrated Partners” and search for CleverTap. 

Enter your Account Details such as CleverTap Account ID, Account passcode and Account Token (available in the CleverTap Settings Dashboard). 
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
In your Android app, open the file where you have added your AppsFlyer integration. Add the following code below it.

If you haven't already integrated AppsFlyer, you will first need to follow [this guide](https://support.appsflyer.com/hc/en-us/articles/207032126-AppsFlyer-SDK-Integration-Android).
[block:code]
{
  "codes": [
    {
      "code": "String attributionID = cleverTapInstance.getCleverTapAttributionIdentifier();\nappsFlyerLib.setCustomerUserId(attributionID);",
      "language": "java"
    }
  ]
}
[/block]
**iOS App**
In your iOS app code, add the following code.
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
#Step 3: View Data in Dashboard
After you integrate, CleverTap will now pull in data from AppsFlyer in your CleverTap dashboard. You can view it under event UTM Visited filtered by event property, UTM_source, UTM_medium or UTM_campaign.

**Notes**
AppsFlyer passes only Non-Organic Installs to the CleverTap Dashboard.