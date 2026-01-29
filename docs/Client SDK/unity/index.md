---
title: Unity SDK
excerpt: Learn how to install and integrate your app with Unity SDK.
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

CleverTap provides a [Unity Plugin](https://github.com/CleverTap/clevertap-unity-sdk) that enables app developers to track, segment, and engage their users.

# Prerequisites

Ensure you have the following prerequisites to start the Unity integration:

| Platform | Prerequisite                                                                                                                                            |
| :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------ |
| All      | CleverTap Account                                                                                                                                       |
| iOS      | Apple Xcode                                                                                                                                             |
| Android  | Optional: Android Studio                                                                                                                                |
| Unity    | Ensure you have the latest supported LTS version of Unity, v2022.2.13f1 or higher, from the [Unity website](https://unity.com/releases/editor/archive). |

# Supported Versions

The CleverTap Unity SDK from 2.4.0 and above supports Unity LTS versions 2023.2.x and 2022.3.x.

> 🚧 Need Help with Unity Version Support?
>
> If you're unsure which Unity version is supported or are using a version earlier than v2022.2.13f1, refer to the [CleverTap Developer Documentation](doc:getting-started) or reach out to our Support team via the Support section on your CleverTap dashboard.

# Unity SDK Versions and Size

Refer to the following table for Unity SDK versions and Unity SDK code sizes:

| SDK                 | Version | Code Size |
| :------------------ | :------ | :-------- |
| CleverTap Unity SDK | 5.5.0   | 2.9 MB    |

# Integrate the Unity SDK

Refer to our quick start guide to get started with the Unity integration:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Steps
      </th>

      <th>
        Procedure
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        **Step 1**
      </td>

      <td>
        [Install the SDK](doc:unity-sdk-quick-start-guide-sdk-v300#install)
      </td>
    </tr>

    <tr>
      <td>
        **Step 2**
      </td>

      <td>
        <ul>
          <li>[iOS Specific Instructions](doc:unity-sdk-quick-start-guide-sdk-v300#ios-specific-instructions)</li>
          <li>[Android Specific Instructions](doc:unity-sdk-quick-start-guide-sdk-v300#android-specific-instructions)</li>
        </ul>
      </td>
    </tr>
  </tbody>
</Table>

# Unity SDK Resources

The following table lists all the Unity SDK resources:

| Resources                                                                                    | Description                                                                                                      |
| :------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------- |
| [Unity GitHub Repository](https://github.com/CleverTap/clevertap-unity-sdk)                  | Provides all the resources required to integrate CleverTap Unity SDK.                                            |
| [Sample Application](https://github.com/CleverTap/clevertap-unity-sdk/tree/master/CTExample) | Provides a sample application to demonstrate the integration of our Unity SDK.                                   |
| [Unity Advanced Features](https://developer.clevertap.com/docs/unity-advanced-features)      | Provides all the advanced features related to Unity.                                                             |
| [Go Live Checklist ](https://developer.clevertap.com/docs/go-live-checklist)                 | Provides a list of actions to be performed before you launch your application.                                   |
| [Changelog](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/CHANGELOG.md)       | Provides details about the most recent updates to our SDKs, including bug fixes, new features, and enhancements. |

# Additional Reference

Use the following resources to implement and troubleshoot the CleverTap Unity SDK. These references cover core integration areas like data tracking, push notifications, In-App messaging, and common troubleshooting scenarios to help maximise your Unity integration.

<HTMLBlock>{`
<div class="two-col-container">
  <div class="link-content">
    <h4 class="custom-heading">Unity Data Tracking</h4>
    <p><a class="links" href="https://developer.clevertap.com/docs/unity-user-profiles" target="_blank" rel="noopener noreferrer">Unity User Profile</a></p>
    <p><a class="links" href="https://developer.clevertap.com/docs/unity-user-events" target="_blank" rel="noopener noreferrer">Unity User Events</a></p>
  </div>
  <div class="link-content">
    <h4 class="custom-heading">Unity Push</h4>
    <p><a class="links" href="https://developer.clevertap.com/docs/unity-push" target="_blank" rel="noopener noreferrer">Unity Push Notifications</a></p>
  </div>
  <div class="link-content">
    <h4 class="custom-heading">Unity SDK Features</h4>
    <p><a class="links" href="https://developer.clevertap.com/docs/unity-inapp" target="_blank" rel="noopener noreferrer">Unity In-App Notification</a></p>
    <p><a class="links" href="https://developer.clevertap.com/docs/unity-app-inbox" target="_blank" rel="noopener noreferrer">Unity App Inbox</a></p>
    <p><a class="links" href="https://developer.clevertap.com/docs/unity-advanced-features" target="_blank" rel="noopener noreferrer">Unity Advanced Features</a></p>    
    <p><a class="links" href="https://developer.clevertap.com/docs/unity-remote-config-sdk-v300" target="_blank" rel="noopener noreferrer">Unity Product Experiences</a></p>    
    <p><a class="links" href="https://developer.clevertap.com/docs/unity-multi-environment-credential-support" target="_blank" rel="noopener noreferrer">Unity Multi-Environment Credential Supports</a></p> 
  </div>
  <div class="link-content">
    <h4 class="custom-heading">Troubleshooting & FAQs</h4>
    <p><a class="links" href="https://developer.clevertap.com/docs/unity-troubleshooting-and-faqs">Unity Troubleshooting and FAQs</a></p>
  </div>
</div>
`}</HTMLBlock>
