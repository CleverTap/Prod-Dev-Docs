---
title: Find Your GCM Sender ID & GCM API Server Key
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
Google Cloud Messaging is a service that allows you to send notifications to your applications and receive information from them. Your GCM Sender ID helps authenticate against CleverTap, allowing us to send push notifications on your behalf.

> 🚧 Firebase Cloud Messaging (FCM) is the new version of GCM.
>
> Firebase Cloud Messaging (FCM) is the new version of GCM. It inherits the reliable and scalable GCM infrastructure, plus new features! If you are integrating messaging in a new app, start with FCM. GCM users are strongly recommended to upgrade to FCM, in order to benefit from new FCM features today and in the future.

# Step 1: Find Your GCM Sender ID

Login to [Google Developer Console](https://console.cloud.google.com/) and go to your Dashboard.

<Image title="gcm1.png" alt={768} className="border" border={true} src="https://files.readme.io/a4bbad5-gcm1.png" />

If you have multiple projects on GDC, identify the applications whose GCM Sender ID you want to use. 

Click on the project, the project number there is your GCM Sender ID.

<Image title="gcm2.png" alt={328} className="border" border={true} src="https://files.readme.io/f895cb0-gcm2.png" />

# Step 2: Generate Your GCM Server API Key

Login to [Google Developer Console](https://console.cloud.google.com/) and navigate to Enable and Manage APIs section.

![768](https://files.readme.io/c3bf83e-gcm3.png "gcm3.png")

Select Cloud Messaging for Android under Mobile APIs.

<Image title="gcm4.png" alt={768} className="border" border={true} src="https://files.readme.io/03e4ec0-gcm4.png" />

First, enable the API. Then go to the Credentials section and Add a new API Key.

<Image title="gcm5.png" alt={768} className="border" border={true} src="https://files.readme.io/c68f5a9-gcm5.png" />

Create a new Server Key.

![768](https://files.readme.io/6cb7a85-gcm6.png "gcm6.png")

Name the key and whitelist provided IP Addresses, after you create the key here, it can take up to 10 mins for the settings to take effect.

![493](https://files.readme.io/fe38869-gcm7.png "gcm7.png")

Here is your API key! You’re now good to go.
