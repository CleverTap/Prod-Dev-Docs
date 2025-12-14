---
title: Configure Firebase Cloud Messaging (FCM)
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Configure FCM

Firebase Cloud Messaging is a service that allows you to send notifications to your applications and receive information from them. Your FCM Sender ID and API key authenticate against CleverTap, enabling you to send notifications to your users from CleverTap.

To configure FCM:

1. Log in to the [Firebase Developer Console](https://console.firebase.google.com/) and go to your Dashboard.
2. Click the **Gear** icon and access **Project settings**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1a609d8-FCM_Project_Settings.png",
        "FCM_Project_Settings.png",
        1281
      ],
      "align": "center",
      "border": true,
      "caption": "Navigate to Firebase Project Settings Page"
    }
  ]
}
[/block]

3. Go to the _Cloud Messaging _ tab to copy the _sender ID_ and the _API Key_. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e724e93-FCM_Server_Key.png",
        "FCM_Server_Key.png",
        "Firebase Project Settings Page"
      ],
      "align": "center",
      "border": true,
      "caption": "Copy Sender ID and API Key"
    }
  ]
}
[/block]

4. Log in to the CleverTap dashboard and navigate to _Settings_ > _Channels_ > _Mobile Push_.
5. Click **FCM credentials** and enter the  _FCM Sender ID_ and the _Legacy Server API Key_ copied in _step 2_.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/940111a-Configure_FCM_for_Mobile_Push_Campaigns.png",
        null,
        "Configure FCM for Push Campaigns"
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true,
      "caption": "Configure FCM for Push Campaigns"
    }
  ]
}
[/block]

# Boost Throughput of Personalized Push Campaigns

> 📘 Optional for Setting up Mobile Push
> 
> This is an optional step for setting up Push campaigns. This step becomes mandatory if you want to increase the throughput of your personalized Mobile Push campaigns.

To increase/boost the throughput of personalized Mobile Push campaigns: 

1. Log in to the CleverTap dashboard and navigate to _Settings_ > _Channels_ > _Mobile Push_.
2. Click **FCM credentials** and enter the _FCM Database URL_. You can obtain the FCM Database URL by navigating to the _Service Accounts_ tab of the _Project Settings_ page of the Firebase dashboard. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9b9075f-Service_Accounts.png",
        null,
        "Obtain Database URL"
      ],
      "align": "center",
      "border": true,
      "caption": "Obtain Database URL"
    }
  ]
}
[/block]

Alternatively, you can also upload the FCM credential field by clicking **Upload FCM Credential File** from the _Settings_ > _Channels_ > _Mobile Push_ page of the CleverTap dashboard. You can obtain this file by clicking Generate new private key from the _Service Accounts_ tab of the _Project Settings_ page of the Firebase dashboard.

> 📘 Prerequisite
> 
> To improve the delivery rate of personalized Push campaigns, ensure that Firebase Cloud Messaging API is enabled. To do so:
> 
> 1. Log in to the _ Firebase Developer Console_ and click **API & Services**.
> 
> ![](https://files.readme.io/b4d5fd4-image.png)
> 
> 2. Click **Cloud Messaging API** link from the left menu.
> 3. Click **ENABLE** if it is not already enabled.
> 
> ![](https://files.readme.io/ca7a60d-image.png)