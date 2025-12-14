---
title: Find Your GCM Sender ID & GCM API Server Key
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
Google Cloud Messaging is a service that allows you to send notifications to your applications and receive information from them. Your GCM Sender ID helps authenticate against CleverTap, allowing us to send push notifications on your behalf.
[block:callout]
{
  "type": "warning",
  "body": "Firebase Cloud Messaging (FCM) is the new version of GCM. It inherits the reliable and scalable GCM infrastructure, plus new features! If you are integrating messaging in a new app, start with FCM. GCM users are strongly recommended to upgrade to FCM, in order to benefit from new FCM features today and in the future.",
  "title": "Firebase Cloud Messaging (FCM) is the new version of GCM."
}
[/block]
# Step 1: Find Your GCM Sender ID 

Login to [Google Developer Console](https://console.cloud.google.com/) and go to your Dashboard.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a4bbad5-gcm1.png",
        "gcm1.png",
        768,
        403,
        "#e3eaf6"
      ],
      "border": true
    }
  ]
}
[/block]
If you have multiple projects on GDC, identify the applications whose GCM Sender ID you want to use. 

Click on the project, the project number there is your GCM Sender ID.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f895cb0-gcm2.png",
        "gcm2.png",
        328,
        179,
        "#faf1ef"
      ],
      "border": true
    }
  ]
}
[/block]
# Step 2: Generate Your GCM Server API Key

Login to [Google Developer Console](https://console.cloud.google.com/) and navigate to Enable and Manage APIs section.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c3bf83e-gcm3.png",
        "gcm3.png",
        768,
        402,
        "#e4eaf5"
      ]
    }
  ]
}
[/block]
Select Cloud Messaging for Android under Mobile APIs.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/03e4ec0-gcm4.png",
        "gcm4.png",
        768,
        430,
        "#eef2f9"
      ],
      "border": true
    }
  ]
}
[/block]
First, enable the API. Then go to the Credentials section and Add a new API Key.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c68f5a9-gcm5.png",
        "gcm5.png",
        768,
        403,
        "#f0f4fa"
      ],
      "border": true
    }
  ]
}
[/block]
Create a new Server Key.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6cb7a85-gcm6.png",
        "gcm6.png",
        768,
        403,
        "#f6f8fb"
      ]
    }
  ]
}
[/block]
Name the key and whitelist provided IP Addresses, after you create the key here, it can take up to 10 mins for the settings to take effect.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/fe38869-gcm7.png",
        "gcm7.png",
        493,
        176,
        "#f9f5f7"
      ]
    }
  ]
}
[/block]
Here is your API key! You’re now good to go.