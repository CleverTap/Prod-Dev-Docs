---
title: Improved InApp Notifications SDK Update
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
[block:api-header]
{
  "title": "Overview"
}
[/block]
In version 3.3.0 and above, of our [Android](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/3.3.0) and [iOS](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/3.3.0) SDKs, we released a major update that enables you to use our **new and improved In-app** Notifications. 

These now support GIF, Audio & Video in certain templates, cleaner and crisp views and A/B testing.
[block:api-header]
{
  "title": "Android"
}
[/block]
In version 3.3.0 of our [Android](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/3.3.0) SDK, we released a major update that enables you to use our new and improved InApp Notifications. These now support GIF, Audio & Video in certain templates along with cleaner and crisp views. 

To support Audio & Video in In-app templates, our SDK uses [ExoPlayer](https://github.com/google/ExoPlayer)

Please add the following dependencies in your app's **build.gradle** file.

[block:callout]
{
  "type": "warning",
  "title": "Exoplayer Dependency",
  "body": "If Exoplayer is not added, audio and video In-apps will **NOT** render on the devices"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "implementation 'com.google.android.exoplayer:exoplayer:2.8.4'\nimplementation 'com.google.android.exoplayer:exoplayer-hls:2.8.4'\nimplementation 'com.google.android.exoplayer:exoplayer-ui:2.8.4'\n",
      "language": "java"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "danger",
  "title": "Android Issue: Oreo Landscape",
  "body": "Due to an [Android Oreo bug](https://issuetracker.google.com/issues/68454482), the new InApp notifications will change their orientation when the phone's orientation is changed from portrait to landscape. \nHence, if your app is targeting to Android API 26, you will face this issue when our SDK renders the InApp notifications. It is hereby suggested that you target your app to Android API level 27 or above."
}
[/block]