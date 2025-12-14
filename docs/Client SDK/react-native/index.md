---
title: React Native
excerpt: Learn how to install and integrate your app with React Native SDK.
deprecated: false
hidden: false
link:
  new_tab: false
metadata:
  title: ''
  description: ''
  robots: index
---
# Overview

React Native (RN) is a popular JavaScript-based mobile app framework. The framework lets you create applications for various platforms using the same codebase as native apps for iOS and Android. CleverTap supports integrating your app with React Native and making the most of both platforms.

Before starting, refer to [React Native Quick Start Guide](doc:react-native-quick-start-guide) for more details.

# Prerequisites

Ensure you have the following prerequisites to integrate CleverTap React Native SDK with your app:

* CleverTap account
* React Native Integrated Development Environment (IDE) such as VS Code, Atom, WebStorm
* Android Studio IDE
* Apple Xcode IDE

# Supported Versions

* For [Android 15](https://developer.android.com/about/versions/15/summary), we recommend using the CleverTap React Native SDK v3.3.0 or higher.
* For [Android 14](https://developer.android.com/about/versions/14/summary), we recommend using the CleverTap React Native SDK v3.2.0 or below.

The following table outlines the minimum React Native versions required and the compatibility of each CleverTap SDK version with the new architecture:

| CleverTap React Native SDK Version | React Native Version with Old Architecture             | React Native Version with New Architecture           |
| :--------------------------------- | :----------------------------------------------------- | :--------------------------------------------------- |
| 3.0.0 or higher                    | The supported React Native version is 0.65 or higher   | The supported React Native version is 0.71 or higher |
| 2.2.1 or below                     | The supported React Native version is 0.49.5 or higher | Does not support                                     |

# React Native SDK Size

For React Native SDK versions and React Native SDK code size, refer to the following table:

| SDK          | Version | Code Size |
| :----------- | :------ | :-------- |
| React Native | 3.7.0   | 3.8 MB    |

# Integrate the React Native SDK

Refer to our quick start guide to get started with the React Native integration:


| Steps      | Procedure                                                                                                                |
| :--------- | :----------------------------------------------------------------------------------------------------------------------- |
| **Step 1** | [Android Integration](https://developer.clevertap.com/docs/react-native-quick-start-guide#android-integration)           |
| **Step 2** | [iOS Integration](https://developer.clevertap.com/docs/react-native-quick-start-guide#ios-integration)                   |
| **Step 3** | [Initialize CleverTap SDK](https://developer.clevertap.com/docs/react-native-quick-start-guide#initialize-clevertap-sdk) |

# React Native SDK Resources

The following table lists all the React Native SDK resources:

| Resources                                                                                     | Description                                                                                                      |
| :-------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------- |
| [Sample Application](https://github.com/CleverTap/clevertap-react-native/tree/master/Example) | Provides a sample application to demonstrate the integration of our React Native SDK.                            |
| [Advanced Features](https://developer.clevertap.com/docs/react-native-advanced-features)      | Provides all the advanced features related to React Native SDK.                                                  |
| [Go Live Checklist ](https://developer.clevertap.com/docs/go-live-checklist)                  | Provides a list of actions to be performed before you launch your application.                                   |
| [Changelog](https://github.com/CleverTap/clevertap-react-native/blob/master/CHANGELOG.md)     | Provides details about the most recent updates to our SDKs, including bug fixes, new features, and enhancements. |

# Additional Reference

Refer to the following for additional React Native SDK references:

<HTMLBlock>{`
<div class="two-col-container">
  <div class="link-content">
    <h4 class="custom-heading">React Native Data Tracking</h4>
    <p><a class="links" href="https://developer.clevertap.com/docs/user-profiles-react-native" target="_blank" rel="noopener noreferrer">React Native User Profile</a></p>
    <p><a class="links" href="https://developer.clevertap.com/docs/user-events-react-native" target="_blank" rel="noopener noreferrer">React Native User Events</a></p>
  </div>
  <div class="link-content">
    <h4 class="custom-heading">React Native Push</h4>
    <p><a class="links" href="https://developer.clevertap.com/docs/react-native-push-notification" target="_blank" rel="noopener noreferrer">React Native Push Notifications</a></p>
  </div>
  <div class="link-content">
    <h4 class="custom-heading">React Native SDK Features</h4>
    <p><a class="links" href="https://developer.clevertap.com/docs/react-native-in-app" target="_blank" rel="noopener noreferrer">React Native In-App Notification</a></p>
    <p><a class="links" href="https://developer.clevertap.com/docs/react-native-app-inbox" target="_blank" rel="noopener noreferrer">React Native App Inbox</a></p>
  </div> 
</div>
`}</HTMLBlock>

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