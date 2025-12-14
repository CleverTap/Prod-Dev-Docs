---
title: Flutter - Adjust
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
You can track mobile app installations with CleverTap via Adjust. 

You can retrieve the install data using the Adjust SDK in your app after the app is installed from an Adjust media source. You can then configure the app to send the installation source, medium, and campaign to CleverTap. 

The install attribution data appears on the specified Campaign Dashboard after integrating Adjust and CleverTap.

#Instruction

You can register a callback to notify tracker attribution changes. This information is asynchronous because of the different sources considered for attribution.
[block:callout]
{
  "type": "info",
  "body": "Follow the data attribution policies applicable to Adjust.",
  "title": "Note"
}
[/block]
With the config instance, before starting the SDK, add the attribution callback:

[block:code]
{
  "codes": [
    {
      "code": "AdjustConfig adjustConfig = new AdjustConfig(yourAppToken, environment);\nconfig.attributionCallback = (AdjustAttribution attributionChangedData) {\n  cleverTap.pushInstallReferrer(attributionChangedData.network, \n  attributionChangedData.trackerName, \n  attributionChangedData.campaign);\n};\nAdjust.start(adjustConfig);",
      "language": "javascript"
    }
  ]
}
[/block]
The `cleverTap.pushInstallReferrer` method receives attribution parameters from Adjust.

#Finding Users

To find those users that were acquired via a particular app install campaign:

  1. Go to CleverTap Dashboard -> Find users.
  2. Select App Launched as the “DID” event.
  3. Select First Time as “Yes” as shown below.
  4. Select UTM Source and set the appropriate value.
 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2ec5d91-Find_people.png",
        "Find people.png",
        2578,
        1642,
        "#f8f9fa"
      ]
    }
  ]
}
[/block]