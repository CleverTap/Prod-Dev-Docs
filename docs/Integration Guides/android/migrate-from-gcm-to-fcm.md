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

<Image title="Screen Shot 2018-09-02 at 10.18.46 PM.png" alt={1095} className="border" border={true} src="https://files.readme.io/ed0b289-Screen_Shot_2018-09-02_at_10.18.46_PM.png" />

In the dropdown box, select your GCM project from the list of existing Google Cloud projects.

<Image title="image2.png" alt={559} className="border" border={true} src="https://files.readme.io/35aa641-image2.png" />

Then click Add Firebase.

<Image title="Screen Shot 2018-09-02 at 10.17.47 PM.png" alt={516} className="border" border={true} src="https://files.readme.io/5dd7c06-Screen_Shot_2018-09-02_at_10.17.47_PM.png" />

In the Firebase welcome screen, select Add Firebase to your Android App. 

<Image title="image3.png" alt={841} className="border" border={true} src="https://files.readme.io/91dbb84-image3.png" />

Provide your package name and SHA-1, and select Add App. Download the google-services.json file for your Firebase app. Select Continue, and follow the detailed instructions for adding the Google Services plugin in Android Studio.

<Image title="image4.png" alt={888} className="border" border={true} src="https://files.readme.io/3d0820f-image4.png" />

## Step 2: Register CleverTap’s Firebase Services in Your App

Add the code below to your app's AndroidManifest.xml file.

```xml
<service
    android:name="com.clevertap.android.sdk.FcmTokenListenerService">
    <intent-filter>
        <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
    </intent-filter>
</service>

<service
    android:name="com.clevertap.android.sdk.FcmMessageListenerService">
    <intent-filter>
        <action android:name="com.google.firebase.MESSAGING_EVENT"/>
    </intent-filter>
</service>
```

## Step 3: Add Your FCM Sender ID & Server API Key to the CleverTap Dashboard

Login to the [Firebase console](https://console.firebase.google.com/), and go to your app's Dashboard. Click on the gear icon and then click on Project settings.

<Image title="image5.png" alt={696} className="border" border={true} src="https://files.readme.io/140ed9d-image5.png" />

Go to the Cloud Messaging Section section, and you will have see your sender ID and API Key.

<Image title="image11.png" alt={486} className="border" border={true} src="https://files.readme.io/26a7e00-image11.png" />

Login to the CleverTap dashboard, and go to the Settings page. 

<Image title="1,20.png" alt={1088} className="border" border={true} src="https://files.readme.io/b6c510a-120.png" />

Click the Push settings button. 

<Image title="21.png" alt={834} className="border" border={true} src="https://files.readme.io/c1ddfe9-21.png" />

Add your FCM sender ID and server API key here, and then click Save.

<Image title="22.png" alt={1916} className="border" border={true} src="https://files.readme.io/9fc19d2-22.png" />
