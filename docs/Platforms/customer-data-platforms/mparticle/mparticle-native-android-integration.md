---
title: mParticle Native Android Integration
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

You can integrate mParticle Android SDK with CleverTap to control the flow of information from the app SDK. You can send the following information to the CleverTap dashboard:

- Push User Profiles
- Push Events
- Integrate Push Notifications

# Prerequisites

The following are the prerequisites for performing this integration:

- A CleverTap account
- An mParticle account
- A functional Android app

# Integration

For CleverTap mParticle Android integration, the following are the major steps:

1. [Add Event Output](doc:mparticle-native-android-integration#add-event-output).
2. [Connect the Input Feed and Output Event on the mParticle dashboard](doc:mparticle-native-android-integration#connect-input-feed-and-output-event-on-mparticle-dashboard).
3. [Install SDK in the Native application](doc:mparticle-native-android-integration#install-sdk-in-native-application).
4. [Initialize the mParticle client](doc:mparticle-native-android-integration#initialize-mparticle-client).

## Add Event Output

To add an event output:

1. Navigate to Setup > Output from the mParticle dashboard.
2. Click **Add Event Output** and select _CleverTap_ from the dropdown list.

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
2. Select _Android_ from the _Available Inputs_. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2841994-Select_Android_from_Available_Inputs.png",
        "Select Android from Available Inputs",
        2878
      ],
      "align": "center",
      "border": true,
      "caption": "Select Android Input on mParticle Dashboard"
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
        "https://files.readme.io/5b454c3-Forwarding_Rules_for_Output.png",
        "Set Up Forwarding Rule for Output",
        1864
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
        "https://files.readme.io/0c37d0e-User_Sampling.png",
        "Add User Sampling Percentage",
        1980
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

To install Android SDK within the application, add the following dependency under dependencies within `app/build.gradle` file:

```groovy
implementation 'com.mparticle:android-clevertap-kit:5.41.1'
```

## Initialize mParticle Client

To initialize the mParticle client:

1. Refer to [Initialize SDK](https://docs.mparticle.com/developers/quickstart/android/create-input/#13-initialize-the-sdk) under mParticle documentation for initializing the SDK.
2. Call `ActivityLifecycleCallback.register(this);` before `super.onCreate()` in your custom application class.

# Push User Information

After installing an app on a device, your user is assigned an anonymous profile. The first time when the application identifies the user on the device, the anonymous history on the device is associated with the newly identified user. To learn more about how user information is pushed to the mParticle dashboard, refer to [Track Users](https://docs.mparticle.com/developers/quickstart/android/track-users/) under mParticle documentation.

# Push Events

To learn how to push event data whenever you want to capture any user action in your application, refer to [Track Events](https://docs.mparticle.com/developers/quickstart/android/track-events/) under mParticle documentation.

# Integrate Push Notification in Android

For more information about integrating Push Notification in Android, refer to [Android Push Notification](doc:android-push).

# Integrate In-App in Android

For more information about integrating In-App in Android, refer to [Android In-App](doc:android-in-app-notifications).

# Integrate App Inbox in Android

For more information about integrating App Inbox in Android, refer to [Android App Inbox](doc:android-app-inbox).

# Advanced Features

To add advanced features to your Android app, refer to [Android Advanced Settings](doc:android-advanced-features).