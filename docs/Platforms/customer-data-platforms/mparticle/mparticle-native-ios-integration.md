---
title: mParticle Native iOS Integration
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

You can integrate mParticle iOS SDK with CleverTap to control the flow of information from the app SDK. You can send the following information to the CleverTap dashboard:

- Push User Profiles
- Push Events
- Push Notifications

# Prerequisites

The following are the prerequisites for performing this integration:

- A CleverTap account
- An mParticle account
- A functional iOS app

# Integration

For CleverTap mParticle iOS integration, the following are the major steps:

1. [Add Event Output](doc:mparticle-native-ios-integration#add-event-output).
2. [Connect the Input Feed and Output Event on the mParticle dashboard](doc:mparticle-native-ios-integration#connect-input-feed-and-output-event-on-mparticle-dashboard).
3. [Install SDK in the Native application](doc:mparticle-native-ios-integration#install-sdk-in-native-application).
4. [Initialize the mParticle client](doc:mparticle-native-ios-integration#initialize-mparticle-client).

## Add Event Output

To add an event output:

1. Navigate to _Setup_ > _Output_ from the mParticle dashboard.
2. Click **Add Event** Output_\_ and select \_CleverTap_ from the dropdown list.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a0640c0-Select_CleverTap_from_dropdown.png",
        "Select Event Output on mParticle Dashboard",
        2478
      ],
      "align": "center",
      "border": true,
      "caption": "Select Event Output on mParticle Dashboard"
    }
  ]
}
[/block]



After selecting, _CleverTap Default Group_ is added under the _Events_ tab of the _Outputs_ page.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b79bdb4-CleverTap_is_added_to_Events_tab.png",
        "Add CleverTap Event on mParticle Dashboard",
        2478
      ],
      "align": "center",
      "border": true,
      "caption": "Add CleverTap Event on mParticle Dashboard"
    }
  ]
}
[/block]



3. Click the ![Add Event Configuration](https://files.readme.io/dfea20a-Plus_Icon.png) icon to add event configuration. The _Output: Event Configuration_ popup opens.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/33f6567-Output_Event_Configuration_popup.png",
        "Add Event Configuration Details",
        1116
      ],
      "align": "center",
      "border": true,
      "caption": "Add Event Configuration Details"
    }
  ]
}
[/block]



4. Enter the following details and click **Save** to save the details:

[block:parameters]
{
  "data": {
    "h-0": "Field",
    "h-1": "Description",
    "0-0": "Configuration Name",
    "0-1": "Enter the name to identify your configuration uniquely.",
    "1-0": "Use same settings for Development & Production",
    "1-1": "Select this option if you want to apply the same configuration settings for your production and development projects.  \n~ OR ~  \nSelect PROD or DEV to apply these configuration settings to the respective project.",
    "2-0": "Account ID, Account Passcode, and Account Token",
    "2-1": "Enter these details to authenticate your CleverTap account. You can obtain these details by navigating to the _Settings_ > _Project_ page from the CleverTap dashboard.",
    "3-0": "Region",
    "3-1": "Enter the region of your CleverTap account."
  },
  "cols": 2,
  "rows": 4,
  "align": [
    "left",
    "left"
  ]
}
[/block]

The _Output Configuration Saved_ message is displayed at the top of the _Output_ page.

## Connect Input Feed and Output Event on mParticle Dashboard

To connect input feed and output event on the mParticle dashboard:

1. Navigate to the _Connections_ > _Connect_ page from the mParticle dashboard.
2. Select _iOS_ from the _Available Inputs_. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a33ae40-Select_iOS_from_Available_Inputs.png",
        "Select iOS from Available Inputs",
        2878
      ],
      "align": "center",
      "border": true,
      "caption": "Select iOS Input on mParticle Dashboard"
    }
  ]
}
[/block]



3. Click **Connect Output**. The _Connect Output_ popup opens.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/60e1909-Connect_Output_popup.png",
        "Connect Output",
        2480
      ],
      "align": "center",
      "border": true,
      "caption": "Connect Output"
    }
  ]
}
[/block]



4. Select _CleverTap_ and then select the _ Configuration Name_ that you want to connect.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e15bc8d-Select_Configuration_Name.png",
        "Select Configuration Name",
        1120
      ],
      "align": "center",
      "sizing": "smart",
      "border": true,
      "caption": "Select Configuration Name"
    }
  ]
}
[/block]



5. Toggle the _Connection Status_ to forward data to the CleverTap dashboard.
6. Select _Customer ID_ from the _User ID_ dropdown list.
7. (Optional) Select the Min Platform Version.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ba6a55f-Configuration_Details_-_2.png",
        "Add Connection Details",
        1114
      ],
      "align": "center",
      "sizing": "smart",
      "border": true,
      "caption": "Add Connection Details"
    }
  ]
}
[/block]



8. Click **Add Connection** and click **Done**.
9. (Optional) Set up the forwarding rule for your output. This allows you to control the data you want to forward to the CleverTap dashboard.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ce37eb0-Forwarding_Rules_for_Output_-_iOS.png",
        "Forwarding Rules for Output",
        1870
      ],
      "align": "center",
      "border": true,
      "caption": "Set Up Forwarding Rule"
    }
  ]
}
[/block]



10. (Optional) Enter the percentage of the user base for which you want to event data to help control the cost.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3719f37-User_Sampling_-_iOS.png",
        "Add User Sampling Percentage",
        1940
      ],
      "align": "center",
      "border": true,
      "caption": "Add User Sampling Percentage"
    }
  ]
}
[/block]



11. Click **Save**.

## Install SDK in Native Application

To install iOS SDK within the application, add the following code to your profile:

```shell
pod 'mParticle-CleverTap'
```



## Initialize mParticle Client

To initialize the mParticle client, refer to [Initialize SDK](https://docs.mparticle.com/developers/quickstart/ios/create-input/#12-initialize-the-sdk) under mParticle documentation.

# Push User Information

After installing an app on a device, your user is assigned an anonymous profile. The first time when the application identifies the user on the device, the anonymous history on the device is associated with the newly identified user. To learn more about how user information is pushed to the mParticle dashboard, refer to [Track Users](https://docs.mparticle.com/developers/quickstart/ios/track-users/) under mParticle documentation.

# Push Events

To learn how to push event data whenever you want to capture any user action in your application, refer to [Track Events](https://docs.mparticle.com/developers/quickstart/ios/track-events/) under mParticle documentation.

# Integrate Push Notification in iOS

For more information about integrating Push Notification in iOS, refer to [iOS Push Notifications](doc:push-notifications-ios).

# Integrate In-App in iOS

For more information about integrating In-App in iOS, refer to [iOS In-App Notification](doc:in-app-notification-ios).

# Integrate App Inbox in iOS

For more information about integrating App Inbox in iOS, refer to [iOS App Inbox](doc:app-inbox-ios).

# Advanced Features

To add advanced features to your iOS app, refer to [iOS Advanced Features](doc:advanced-options-ios).