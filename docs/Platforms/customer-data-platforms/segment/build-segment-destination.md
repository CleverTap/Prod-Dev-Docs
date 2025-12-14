---
title: Segment (Action)
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

The Destination Action framework lets you see and control how Segment sends the event data it receives from your sources to action-based destinations. In other words, it helps to create the following:

- Subscriptions, a set of conditions that act as triggers to send data to the destination 
- Data mappings to format that data for the destination platform

After setting up the destination (action), Segment monitors the data that matches the conditions you create (called "triggers") for the subscription. When the conditions are met, explicit mapping transforms the incoming data into an output format that your destination can use.

# Advantages of CleverTap (Actions) Destination

The following are the advantages of setting up CleverTap (Actions) Destination:

- Upload User Profile
- Delete User Profile

# Set up CleverTap (Actions) Destination

To set up CleverTap (Actions) Destination:

1. From the Segment web app, click **Catalog** and then click **Destinations**.
2. Select _Destinations Actions_ under Categories from the left navigation.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ea1882a-Navigation.png",
        "Set Up CleverTap (Actions) Destination",
        2886
      ],
      "align": "center",
      "border": true,
      "caption": "Set Up CleverTap (Actions) Destination"
    }
  ]
}
[/block]



3. Click _CleverTap (Actions)_.
4. Click **Configure CleverTap (Actions)**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7d7b183-Click_Configure_CleverTap_Actions.png",
        "Configure CleverTap (Actions)",
        5752
      ],
      "align": "center",
      "border": true,
      "caption": "Configure CleverTap (Actions)"
    }
  ]
}
[/block]



5. Select an existing _Source_ to connect to CleverTap (Actions).
6. Enter the following project details to authorize the connection: 
   - Project ID
   - Region

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a171d1f-CT_Project_Details.png",
        "CleverTap Project Details",
        1774
      ],
      "align": "center",
      "border": true,
      "caption": "CleverTap Project Details"
    }
  ]
}
[/block]



 These details are obtained by navigating to the _Settings_ > _Project_ page of the CleverTap dashboard. Check the URL of your CleverTap account and then refer to the following table to identify the region for your account:

| CleverTap Dashboard URL                             | Region            |
| :-------------------------------------------------- | :---------------- |
| <https://eu1.dashboard.clevertap.com/login.html#/>  | EU                |
| <https://in1.dashboard.clevertap.com/login.html#/>  | India             |
| <https://us1.dashboard.clevertap.com/login.html#/>  | US                |
| <https://sg1.dashboard.clevertap.com/login.html#/>  | Singapore         |
| <https://aps3.dashboard.clevertap.com/login.html#/> | Indonesia         |
| <https://mec1.dashboard.clevertap.com/login.html#/> | Middle East (UAE) |

- Passcode: For more information about passcodes, refer to [Authentication](doc:authentication).

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/cd4a171-CT_Passcode.png",
        "CleverTap Passcode",
        2852
      ],
      "align": "center",
      "border": true,
      "caption": "CleverTap Passcode"
    }
  ]
}
[/block]



7. Select _Quick Setup_ to start with pre-populated subscriptions, or _Customized Setup_ to configure each action from scratch. 
8. Click **Configure Actions**.

# Available Presets

CleverTap (Actions) has the following presets:

| Preset Name | Trigger                    | Default Action                              |
| :---------- | :------------------------- | :------------------------------------------ |
| User Upload | Event type = "identify"    | Uploads User Profile to CleverTap dashboard |
| User Delete | Event type = "delete user" | Deletes user profile                        |

# Available Actions

Combine supported triggers with the following CleverTap-supported actions:

## User Upload

Create a profile on the CleverTap dashboard or update it if it exists.

| <p>Field</p>             | <p>Type</p>   | <p>Description</p>                                                                                                                                                                                                                                                                                                                                          |
| :----------------------- | :------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Identity</p>          | <p>string</p> | <ul><li>This field is mandatory.</li><li>It is used to recognize a unique user. </li><li>It can be the user’s email, a phone number, or any other identifier that you are using to tag your users.</li></ul>                                                                                                                                                |
| <p>Created At</p>        | <p>ts</p>     | <ul><li>This field is optional.</li><li>It denotes the date and time when the user profile was created.</li><li>Defaults to the current timestamp, if omitted.</li><li>For more information about converting this timestamp to Unix timestamps, refer to <a href="doc:build-segment-destination#conversion-timestamps">Conversion Timestamps</a>.</li></ul> |
| <p>Person Attributes</p> | <p>object</p> | <ul><li>This field is mandatory.</li><li>It consists of user profile properties. </li><li>It is passed as key/value pairs.</li><li>properties[“Phone”] must be formatted as +[country code][phone number].</li></ul>                                                                                                                                        |

## Delete User

Deletes the user profile on the CleverTap dashboard.

| <p>Field</p>    | <p>Type</p>   | <p>Description</p>                                                                                                                                                                                           |
| :-------------- | :------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Identity</p> | <p>string</p> | <ul><li>This field is mandatory.</li><li>It is used to recognize a unique user. </li><li>It can be the user’s email, a phone number, or any other identifier that you are using to tag your users.</li></ul> |

# Conversion Timestamps

When mapping actions, you will see a _Convert Timestamps_ setting. When enabled, it converts attributes containing ISO-8601 timestamps to Unix timestamps.

For example, if you send an event with a timestamp of 2006-01-02T18:04:07Z. It is converted to 1136253847. If the timestamp is not in ISO-8601 format, it is not converted. This avoids converting attributes such as phone numbers or IDs.