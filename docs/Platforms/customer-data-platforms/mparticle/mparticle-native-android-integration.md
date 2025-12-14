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

* Push User Profiles
* Push Events
* Integrate Push Notifications

# Prerequisites

The following are the prerequisites for performing this integration:

* A CleverTap account
* An mParticle account
* A functional Android app

# Integration

For CleverTap mParticle Android integration, the following are the major steps:

1. [Add Event Output](doc:mparticle-native-android-integration#add-event-output).
2. [Connect the Input Feed and Output Event on the mParticle dashboard](doc:mparticle-native-android-integration#connect-input-feed-and-output-event-on-mparticle-dashboard).
3. [Install SDK in the Native application](doc:mparticle-native-android-integration#install-sdk-in-native-application).
4. [Initialize the mParticle client](doc:mparticle-native-android-integration#initialize-mparticle-client).

## Add Event Output

To add an event output:

1. Navigate to Setup > Output from the mParticle dashboard.
2. Click **Add Event Output** and select *CleverTap* from the dropdown list.

<Image title="Select Event Output on mParticle Dashboard" alt={2478} align="center" border={true} src="https://files.readme.io/a0640c0-Select_CleverTap_from_dropdown.png">
  Select Event Output on mParticle Dashboard
</Image>

After selecting, *CleverTap Default Group* is added under the *Events* tab of the *Outputs* page.

<Image title="Add CleverTap Event on mParticle Dashboard" alt={2478} align="center" border={true} src="https://files.readme.io/b79bdb4-CleverTap_is_added_to_Events_tab.png">
  Add CleverTap Event on mParticle Dashboard
</Image>

3. Click the ![Add Event Configuration](https://files.readme.io/dfea20a-Plus_Icon.png) icon to add event configuration. The *Output: Event Configuration* popup opens.

<Image title="Add Event Configuration Details" alt={1116} align="center" border={true} src="https://files.readme.io/33f6567-Output_Event_Configuration_popup.png">
  Add Event Configuration Details
</Image>

4. Enter the following details and click **Save** to save the details:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Field
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Configuration Name
      </td>

      <td>
        Enter the name to identify your configuration uniquely.
      </td>
    </tr>

    <tr>
      <td>
        Use same settings for Development & Production
      </td>

      <td>
        Select this option if you want to apply the same configuration settings for your production and development projects.\
        \~ OR \~\
        Select PROD or DEV to apply these configuration settings to the respective project.
      </td>
    </tr>

    <tr>
      <td>
        Account ID, Account Passcode, and Account Token
      </td>

      <td>
        Enter these details to authenticate your CleverTap account. You can obtain these details by navigating to the *Settings* > *Project* page from the CleverTap dashboard.
      </td>
    </tr>

    <tr>
      <td>
        Region
      </td>

      <td>
        Enter the region of your CleverTap account.
      </td>
    </tr>
  </tbody>
</Table>

The *Output Configuration Saved* message is displayed at the top of the *Output* page.

## Connect Input Feed and Output Event on mParticle Dashboard

To connect input feed and output event on the mParticle dashboard:

1. Navigate to the *Connections* > *Connect* page from the mParticle dashboard.
2. Select *Android* from the *Available Inputs*. 

<Image title="Select Android from Available Inputs" alt={2878} align="center" border={true} src="https://files.readme.io/2841994-Select_Android_from_Available_Inputs.png">
  Select Android Input on mParticle Dashboard
</Image>

3. Click **Connect Output**. The *Connect Output* popup opens.

<Image title="Connect Output" alt={2480} align="center" border={true} src="https://files.readme.io/60e1909-Connect_Output_popup.png">
  Connect Output
</Image>

4. Select *CleverTap* and then select the *Configuration Name* that you want to connect.

<Image title="Select Configuration Name" alt={1120} align="center" width="smart" border={true} src="https://files.readme.io/e15bc8d-Select_Configuration_Name.png">
  Select Configuration Name
</Image>

5. Toggle the *Connection Status* to forward data to the CleverTap dashboard.
6. Select *Customer ID* from the *User ID* dropdown list.
7. (Optional) Select the Min Platform Version.

<Image title="Add Connection Details" alt={1114} align="center" width="smart" border={true} src="https://files.readme.io/ba6a55f-Configuration_Details_-_2.png">
  Add Connection Details
</Image>

8. Click **Add Connection** and click **Done**.
9. (Optional) Set up the forwarding rule for your output. This allows you to control the data you want to forward to the CleverTap dashboard.

<Image title="Set Up Forwarding Rule for Output" alt={1864} align="center" border={true} src="https://files.readme.io/5b454c3-Forwarding_Rules_for_Output.png">
  Set Up Forwarding Rule
</Image>

10. (Optional) Enter the percentage of the user base for which you want to event data to help control the cost.

<Image title="Add User Sampling Percentage" alt={1980} align="center" border={true} src="https://files.readme.io/0c37d0e-User_Sampling.png">
  Add User Sampling Percentage
</Image>

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
