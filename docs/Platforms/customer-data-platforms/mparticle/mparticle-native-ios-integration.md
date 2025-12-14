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

* Push User Profiles
* Push Events
* Push Notifications

# Prerequisites

The following are the prerequisites for performing this integration:

* A CleverTap account
* An mParticle account
* A functional iOS app

# Integration

For CleverTap mParticle iOS integration, the following are the major steps:

1. [Add Event Output](doc:mparticle-native-ios-integration#add-event-output).
2. [Connect the Input Feed and Output Event on the mParticle dashboard](doc:mparticle-native-ios-integration#connect-input-feed-and-output-event-on-mparticle-dashboard).
3. [Install SDK in the Native application](doc:mparticle-native-ios-integration#install-sdk-in-native-application).
4. [Initialize the mParticle client](doc:mparticle-native-ios-integration#initialize-mparticle-client).

## Add Event Output

To add an event output:

1. Navigate to *Setup* > *Output* from the mParticle dashboard.
2. Click **Add Event** Output *\_and select\_CleverTap* from the dropdown list.

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
2. Select *iOS* from the *Available Inputs*. 

<Image title="Select iOS from Available Inputs" alt={2878} align="center" border={true} src="https://files.readme.io/a33ae40-Select_iOS_from_Available_Inputs.png">
  Select iOS Input on mParticle Dashboard
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

<Image title="Forwarding Rules for Output" alt={1870} align="center" border={true} src="https://files.readme.io/ce37eb0-Forwarding_Rules_for_Output_-_iOS.png">
  Set Up Forwarding Rule
</Image>

10. (Optional) Enter the percentage of the user base for which you want to event data to help control the cost.

<Image title="Add User Sampling Percentage" alt={1940} align="center" border={true} src="https://files.readme.io/3719f37-User_Sampling_-_iOS.png">
  Add User Sampling Percentage
</Image>

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
