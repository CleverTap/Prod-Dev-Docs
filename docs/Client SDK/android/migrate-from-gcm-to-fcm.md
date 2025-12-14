---
title: Migrate from GCM to FCM
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
# Overview

As of April 10, 2018, [Google has deprecated GCM](https://developers.google.com/cloud-messaging/faq). The GCM server and client APIs are deprecated, and will be removed by April 11, 2019. The client SDKs and GCM tokens will continue to work indefinitely. However, you will not be able to target the latest version of Google Play Services in your Android app unless you migrate to FCM.

If you are using GCM, it is recommended that you migrate your app from GCM to FCM.

# GCM to FCM Migration Steps

## Step 1: Import GCM Project into FCM

In the [Firebase console](https://console.firebase.google.com/), click Add Project. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ed0b289-Screen_Shot_2018-09-02_at_10.18.46_PM.png",
        "Screen Shot 2018-09-02 at 10.18.46 PM.png",
        1095,
        603,
        "#cfdfe3"
      ],
      "border": true
    }
  ]
}
[/block]
In the dropdown box, select your GCM project from the list of existing Google Cloud projects.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/35aa641-image2.png",
        "image2.png",
        559,
        626,
        "#ebedee"
      ],
      "border": true
    }
  ]
}
[/block]
Then click Add Firebase.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/5dd7c06-Screen_Shot_2018-09-02_at_10.17.47_PM.png",
        "Screen Shot 2018-09-02 at 10.17.47 PM.png",
        516,
        133,
        "#e6eaf0"
      ],
      "border": true
    }
  ]
}
[/block]
In the Firebase welcome screen, select Add Firebase to your Android App. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/91dbb84-image3.png",
        "image3.png",
        841,
        460,
        "#1e97d2"
      ],
      "border": true
    }
  ]
}
[/block]
Provide your package name and SHA-1, and select Add App. Download the google-services.json file for your Firebase app. Select Continue, and follow the detailed instructions for adding the Google Services plugin in Android Studio.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3d0820f-image4.png",
        "image4.png",
        888,
        785,
        "#f5f7f8"
      ],
      "border": true
    }
  ]
}
[/block]
## Step 2: Register CleverTap’s Firebase Services in Your App

Add the code below to your app's AndroidManifest.xml file.
[block:code]
{
  "codes": [
    {
      "code": "<service\n    android:name=\"com.clevertap.android.sdk.FcmTokenListenerService\">\n    <intent-filter>\n        <action android:name=\"com.google.firebase.INSTANCE_ID_EVENT\"/>\n    </intent-filter>\n</service>\n\n<service\n    android:name=\"com.clevertap.android.sdk.FcmMessageListenerService\">\n    <intent-filter>\n        <action android:name=\"com.google.firebase.MESSAGING_EVENT\"/>\n    </intent-filter>\n</service>",
      "language": "xml"
    }
  ]
}
[/block]
## Step 3: Add Your FCM Sender ID & Server API Key to the CleverTap Dashboard

Login to the [Firebase console](https://console.firebase.google.com/), and go to your app's Dashboard. Click on the gear icon and then click on Project settings.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/140ed9d-image5.png",
        "image5.png",
        696,
        515,
        "#2576a2"
      ],
      "border": true
    }
  ]
}
[/block]
Go to the Cloud Messaging Section section, and you will have see your sender ID and API Key.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/26a7e00-image11.png",
        "image11.png",
        486,
        430,
        "#6d8191"
      ],
      "border": true
    }
  ]
}
[/block]
Login to the CleverTap dashboard, and go to the Settings page. 


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b6c510a-120.png",
        "1,20.png",
        1088,
        840,
        "#efeff0"
      ],
      "border": true
    }
  ]
}
[/block]
Click the Push settings button. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c1ddfe9-21.png",
        "21.png",
        834,
        1412,
        "#f1f3f5"
      ],
      "border": true
    }
  ]
}
[/block]
Add your FCM sender ID and server API key here, and then click Save.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9fc19d2-22.png",
        "22.png",
        1916,
        1032,
        "#fafafb"
      ],
      "border": true
    }
  ]
}
[/block]