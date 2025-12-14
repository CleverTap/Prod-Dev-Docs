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

<Image title="FCM_Project_Settings.png" alt={1281} align="center" border={true} src="https://files.readme.io/1a609d8-FCM_Project_Settings.png">
  Navigate to Firebase Project Settings Page
</Image>

3. Go to the *Cloud Messaging* tab to copy the *sender ID* and the *API Key*. 

<Image title="FCM_Server_Key.png" alt="Firebase Project Settings Page" align="center" border={true} src="https://files.readme.io/e724e93-FCM_Server_Key.png">
  Copy Sender ID and API Key
</Image>

4. Log in to the CleverTap dashboard and navigate to *Settings* > *Channels* > *Mobile Push*.
5. Click **FCM credentials** and enter the  *FCM Sender ID* and the *Legacy Server API Key* copied in *step 2*.

<Image alt="Configure FCM for Push Campaigns" align="center" width="80% " border={true} src="https://files.readme.io/940111a-Configure_FCM_for_Mobile_Push_Campaigns.png">
  Configure FCM for Push Campaigns
</Image>

# Boost Throughput of Personalized Push Campaigns

> 📘 Optional for Setting up Mobile Push
>
> This is an optional step for setting up Push campaigns. This step becomes mandatory if you want to increase the throughput of your personalized Mobile Push campaigns.

To increase/boost the throughput of personalized Mobile Push campaigns: 

1. Log in to the CleverTap dashboard and navigate to *Settings* > *Channels* > *Mobile Push*.
2. Click **FCM credentials** and enter the *FCM Database URL*. You can obtain the FCM Database URL by navigating to the *Service Accounts* tab of the *Project Settings* page of the Firebase dashboard. 

<Image alt="Obtain Database URL" align="center" border={true} src="https://files.readme.io/9b9075f-Service_Accounts.png">
  Obtain Database URL
</Image>

Alternatively, you can also upload the FCM credential field by clicking **Upload FCM Credential File** from the *Settings* > *Channels* > *Mobile Push* page of the CleverTap dashboard. You can obtain this file by clicking Generate new private key from the *Service Accounts* tab of the *Project Settings* page of the Firebase dashboard.

> 📘 Prerequisite
>
> To improve the delivery rate of personalized Push campaigns, ensure that Firebase Cloud Messaging API is enabled. To do so:
>
> 1. Log in to the *Firebase Developer Console* and click **API & Services**.
>
> ![](https://files.readme.io/b4d5fd4-image.png)
>
> 2. Click **Cloud Messaging API** link from the left menu.
> 3. Click **ENABLE** if it is not already enabled.
>
> ![](https://files.readme.io/ca7a60d-image.png)
