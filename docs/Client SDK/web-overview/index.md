---
title: Web SDK
excerpt: Learn how to install and integrate your app with Web SDK.
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

This section demonstrates how to set up CleverTap’s JavaScript library on your website, track events, create user profiles, and engage users through web pop-ups and push notifications using the CleverTap web SDK. Before getting started, complete the integration steps mentioned in [Web SDK Quick Start Guide](doc:web-quickstart-guide).

# Prerequisites

Ensure you have the following prerequisites to start the Web integration:

* CleverTap account
* A functional website
* Web Integrated Development Environment (IDE) such as Visual Studio Code and WebStorm

# Supported Versions

The CleverTap Web SDK version 2.3.0 supports all web browsers with existing versions.

# Web SDK Versions and Size

Refer to the following table for Web SDK versions and Web SDK code size:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Version
      </th>

      <th>
        SDK
      </th>

      <th>
        Code Size
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        2.3.0
      </td>

      <td>
        Web
      </td>

      <td>
        Minified Version: Approximately 240.7 KB\
        Unpacked Version: 2.1 MB
      </td>
    </tr>
  </tbody>
</Table>

# Integrate the Web SDK

Refer to our quick start guide to get started with the Web integration:

| Steps      | Procedure                                                                                                                                                                                                   |
| :--------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Step 1** | [Option A: Add the CleverTap JavaScript Library Manually to Your Website](https://developer.clevertap.com/docs/web-quickstart-guide#option-a-add-the-clevertap-javascript-library-manually-to-your-website) |
|            | [Option B: Using the Package Manager](https://developer.clevertap.com/docs/web-quickstart-guide#option-b---using-the-package-manager)                                                                       |
| **Step 2** | [Add Region Code](https://developer.clevertap.com/docs/web-quickstart-guide#add-region-code)                                                                                                                |

# Web SDK Resources

The following table lists all the Web SDK resources:

| Resources                                                                                     | Description                                                                                                      |
| :-------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------- |
| [Sample Web Project](https://github.com/CleverTap/clevertap-web-sdk/tree/master/example-apps) | Provides a sample project to demonstrate the integration of our Web SDK.                                         |
| [Advanced Configurations](https://developer.clevertap.com/docs/advanced-configurations)       | Provides all the advanced GDPR configurations related to Web SDK.                                                |
| [Go Live Checklist ](https://developer.clevertap.com/docs/go-live-checklist)                  | Provides a list of actions to be performed before you launch your application.                                   |
| [Changelog](https://github.com/CleverTap/clevertap-web-sdk/blob/master/CHANGELOG.md)          | Provides details about the most recent updates to our SDKs, including bug fixes, new features, and enhancements. |

# Additional Reference

Following are the additional Web SDK references:

<HTMLBlock>{`
<div class="two-col-container">
  <div class="link-content">
    <h4 class="custom-heading">Web Data Tracking</h4>
    <p><a class="links" href="https://developer.clevertap.com/docs/web-quickstart-guide#identify-users" target="_blank" rel="noopener noreferrer">Web User Profile</a></p>
    <p><a class="links" href="https://developer.clevertap.com/docs/web-quickstart-guide#track-user-events" target="_blank" rel="noopener noreferrer">Web User Events</a></p>
  </div>
  <div class="link-content">
    <h4 class="custom-heading">Web Push</h4>
    <p><a class="links" href="https://developer.clevertap.com/docs/web-push" target="_blank" rel="noopener noreferrer">Web Push Notifications
      </a></p>
  </div>
  <div class="link-content">
    <h4 class="custom-heading">Web SDK Features</h4>
    <p><a class="links" href="https://developer.clevertap.com/docs/web-popup" target="_blank" rel="noopener noreferrer">Web Pop-Ups and Exit Intent</a></p>
    <p><a class="links" href="https://developer.clevertap.com/docs/web-native-display" target="_blank" rel="noopener noreferrer">Web Native Display
      </a></p>
    <p><a class="links" href="https://developer.clevertap.com/docs/web-inbox" target="_blank" rel="noopener noreferrer">Web Inbox
      </a></p>
  </div> 
</div>
`}</HTMLBlock>
