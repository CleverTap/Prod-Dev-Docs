---
title: Tune
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
CleverTap helps you to track your app installs via Tune. This integration requires changes to both the Tune Dashboard and your Android/iOS app for successful integration.

#Step 1: Add CleverTap Credentials to Tune Dashboard

Go to the Tune dashboard and click on Manage Partners.



[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8844c7d-tune.png",
        "tune.png",
        624,
        371,
        "#f0f2f2"
      ]
    }
  ]
}
[/block]
Search and enable CleverTap as an integrated partner. Click on the CleverTap Logo. Enter your credentials including your CleverTap Account ID and Token. 

Enable View-through Attribution in Attribution Setting. 

Enable Install Postback which allows tune to report data to CleverTap API. This is under Postbacks (Postbacks Templates) > Templates > Generic.

#Step 2: Integration

**Android App**

In your Android app, add the following code in your Application Class within the onCreate() method.

Follow the [Android Quick Start Guide](doc:android-quickstart-guide) to understand how to instantiate a CleverTap instance.
[block:code]
{
  "codes": [
    {
      "code": "String attributionId = cleverTapInstance.getCleverTapAttributionIdentifier();\nTune.init(this, \"your_advertiser_ID\", \"your_conversion_key\"); \nTune.getInstance().setUserId(attributionId);",
      "language": "java"
    }
  ]
}
[/block]
If you haven’t done the Tune integration for Android apps, follow [this guide](
https://developers.tune.com/sdk/android-quick-start/).

**iOS App**

In your AppDelegate.m file, add the following code.
[block:code]
{
  "codes": [
    {
      "code": "[Tune initializeWithTuneAdvertiserId:@\"your_advertiser_ID\" tuneConversionKey:@\"your_conversion_key\"];\n[Tune  setUserID:[[CleverTap sharedInstance] profileGetCleverTapAttributionIdentifier]];",
      "language": "objectivec"
    }
  ]
}
[/block]
If you haven’t done the Tune integration for iOS app, follow [this guide](https://developers.tune.com/sdk/ios-quick-start/).

# Step 3: View Data in the CleverTap Dashboard
After you integrate, Tune will now push data in your CleverTap dashboard. You can view it under event UTM Visited filtered by event property, UTM_source, UTM_medium or UTM_campaign.