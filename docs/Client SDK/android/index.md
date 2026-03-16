---
title: Android SDK
excerpt: Learn how to install and integrate your Android app.
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

This section shows how to track custom events, enrich user profiles, and send push notifications with the CleverTap SDK in your Android app. Before getting started, complete the [Android SDK Quick Start Guide](doc:android-quickstart-guide). The quick start guide shows how to install the CleverTap SDK, track your first user event, and refer to this information within the CleverTap dashboard.

# Prerequisites

Ensure you have the following prerequisites to start the Android integration:

* CleverTap account
* Android Studio

# Supported Versions

* For [Android 14](https://developer.android.com/about/versions/14/summary), we recommend using the CleverTap Android SDK v6.1.1 or higher.
* For [Android 15](https://developer.android.com/about/versions/15/summary), we recommend using the CleverTap Android SDK v7.2.0 or higher.
* For [Android 16](https://developer.android.com/about/versions/16/summary), we recommend using the CleverTap Android SDK v7.5.0 or higher.

# Android SDK Versions and Size

Refer to the following table for Android SDK code sizes. All the below SDKs are built on the Core Android SDK, which currently supports up to Android 15.

| SDK                                                                   | Version | Code Size |
| :-------------------------------------------------------------------- | :------ | :-------- |
| [Core Android](doc:android-quickstart-guide)                          | 8.0.0   | 1.61 MB   |
| [Geofence](doc:geofence-android)                                      | 1.4.0   | 114 KB    |
| [Huawei Mobile Services (HMS)](doc:clevertap-huawei-push-integration) | 1.5.1   | 55.4 KB   |
| [Push Templates](doc:push-templates-android)                          | 2.2.0   | 331 KB    |

# Integrate the Android SDK

Refer to our quick start guide to get started with the Android integration:

| Steps      | Procedure                                                                                                                                                                                     |
| :--------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Step 1** | [Install the SDK](https://developer.clevertap.com/docs/android-quickstart-guide#install-sdk)                                                                                                  |
| **Step 2** | [Add your CleverTap Credentials in the AndroidManifest.xml File](https://developer.clevertap.com/docs/android-quickstart-guide#add-your-clevertap-credentials-in-the-androidmanifestxml-file) |
| **Step 3** | [Access the CleverTap Instance](https://developer.clevertap.com/docs/android-quickstart-guide#access-clevertap-instance)                                                                      |
| **Step 4** | [Verify the Integration](https://developer.clevertap.com/docs/android-quickstart-guide#verify-integration)                                                                                    |

# Android SDK Resources

The following table lists all the Android SDK resources:

| Resources                                                                                   | Description                                                                                                      |
| :------------------------------------------------------------------------------------------ | :--------------------------------------------------------------------------------------------------------------- |
| [Android GitHub Repository](https://github.com/CleverTap/clevertap-android-sdk)             | Provides all the resources required to integrate CleverTap Android SDK.                                          |
| [Sample Application](https://github.com/CleverTap/clevertap-android-sdk/tree/master/sample) | Provides a sample application to demonstrate the integration of our Android SDK.                                 |
| [Android Advanced Features](https://developer.clevertap.com/docs/android-advanced-features) | Provides all the advanced features related to Android.                                                           |
| [Go Live Checklist ](https://developer.clevertap.com/docs/go-live-checklist)                | Provides a list of actions to be performed before you launch your application.                                   |
| [Changelog](https://github.com/CleverTap/clevertap-android-sdk/blob/master/CHANGELOG.md)    | Provides details about the most recent updates to our SDKs, including bug fixes, new features, and enhancements. |

# Additional Reference

Refer to the following for additional Android SDK references.

<details>
  <summary>Android Data Tracking</summary>

  * [Android User Profiles](doc:user-profiles-android)
  * [Android User Events](doc:user-events-android)
</details>

<details>
  <summary>Android Push</summary>

  * [Android Push](doc:android-push)
  * [CleverTap Huawei Push Integration](doc:clevertap-huawei-push-integration)
  * [CleverTap Baidu Push Integration](doc:baidu-push-notifications)
  * [CleverTap Xiaomi Push Integration](doc:xiaomi-push-notifications)
  * [Android Push Templates](doc:push-templates-android)
</details>

<details>
  <summary>Android SDK Features</summary>

  * [Android In-App](doc:android-in-app-notifications)
  * [Android App Inbox](doc:android-app-inbox)
  * [Android Native Display](doc:android-native-display)
  * [Android Product Experiences](doc:android-remote-config)
  * [Multi-Instance Support Android](doc:multi-instance-support-android)
  * [Geofence Android SDK](doc:geofence-android-sdk)
</details>

<HTMLBlock>{`
<style>
  .rm-Guides .content-body, .rm-Guides #content-head .col-xs-9 {
      max-width: 100%;
      width: 1000px;
  }
  .markdown-body summary {
  font-size: 20px;
  font-weight: bold; 
  text-transform: camelcase;
}
</style>
`}</HTMLBlock>
