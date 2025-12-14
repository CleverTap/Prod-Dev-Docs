---
title: App Store Connect - App Privacy
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Overview

Starting December 8, 2020, Apple requires [App Privacy Details](https://developer.apple.com/app-store/app-privacy-details/) for all new apps and app updates. Refer to [Manage App Privacy](https://help.apple.com/app-store-connect/#/dev1b4647c5b) to understand Apple’s privacy rules and app integrations with multiple third-party SDK(s) to make the correct decision for your app. 

> 📘 Note
>
> This document is not legal advice. It is provided for your information and convenience. You must work closely with legal and other professional advisors to determine exactly how Apple policies apply to you.

# Data types

CleverTap collects the following data for your app:

* **Collected** - CleverTap SDK requires this data. It is collected by default.
* **Not collected** -  CleverTap SDK does not require this data.
* **May be, By Default: Not collected** -  Not required by CleverTap SDK. However, you can configure to send this data to CleverTap for additional tracking. 

# Data type

The following table should help you to answer the privacy declaration. 

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        <p>

        Data Type

        </p>
      </th>

      <th>
        <p>

        Sent To CleverTap?

        </p>
      </th>

      <th>
        <p>

        Description

        </p>
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        <p><strong>Contact information</strong></p><ul><li><p>Name</p></li><li><p>Email address</p></li><li><p>Phone number </p></li><li><p>Physical address</p></li><li><p>Other user contact info </p></li></ul>
      </td>

      <td>
        <p>May be, By Default: Not collected</p>
      </td>

      <td>
        <p>If you have configured to send this data, then declare accordingly.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>Health and fitness</strong></p><p>Health</p><p>Fitness</p>
      </td>

      <td>
        <p>May be, By Default: Not collected</p>
      </td>

      <td>
        <p>If you have configured to send this data, then declare accordingly.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>Financial info</strong></p><p>Payment info</p><p>Credit info</p><p>Other financial info</p>
      </td>

      <td>
        <p>May be, By Default: Not collected</p>
      </td>

      <td>
        <p>If you have configured to send this data, then declare accordingly.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>Location info</strong></p><p>Precise location</p><p>Coarse location</p>
      </td>

      <td>
        <p>May be, By Default: Not collected</p>
      </td>

      <td>
        <p>If you have configured to send this data, then declare accordingly.<br />This data requires additional integration :</p><ul><li><a href="https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/3.7.0">Precise Location Integration</a></li><li><a href="https://developer.clevertap.com/docs/sdk-changes-for-gdpr-compliance#section-device-network-information-reporting-in-i-os">Coarse Location integration</a>.</li></ul>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>Sensitive info</strong> </p>
      </td>

      <td>
        <p>May be, By Default: Not collected</p>
      </td>

      <td>
        <p>If you have configured to send this data, then declare accordingly.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>Contacts</strong> </p>
      </td>

      <td>
        <p>May be, By Default: Not collected</p>
      </td>

      <td>
        <p>If you have configured to send this data, then declare accordingly.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>User content</strong></p><p>Emails or text messages</p><p>Photos or videos</p><p>Audio data</p><p>Gameplay content</p><p>Customer support</p><p>Other user content</p>
      </td>

      <td>
        <p>May be, By Default: Not collected</p>
      </td>

      <td>
        <p>If you have configured to send this data, then declare accordingly.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>Browsing history</strong> </p>
      </td>

      <td>
        <p>Not collected</p>
      </td>

      <td>
        <p>If you have configured to send this data, then declare accordingly.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>Search history</strong> </p>
      </td>

      <td>
        <p>Not collected</p>
      </td>

      <td>
        <p>If you have configured to send this data, then declare accordingly.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>Identifiers</strong></p><p>User ID</p><p>Device ID</p>
      </td>

      <td>
        <p>May be, By Default: Not collected</p>
      </td>

      <td>
        <p>Latest CleverTap iOS SDK does not capture IDFA or other device IDs such as IMEI. The IDFV is used to create CleverTap ID which is disclosed under Other Data.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>Purchases</strong></p><p>Purchase history</p>
      </td>

      <td>
        <p>May be, By Default: Not collected</p>
      </td>

      <td>
        <p>If you have configured to send this data, then declare accordingly.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><strong>Usage data</strong></p><p>Product interaction</p><p>Advertising data</p><p>Other usage data (for example, Wi-Fi)</p>
      </td>

      <td>
        <p>Required, Collected</p><p>Product interaction => Yes<br />Advertising data => No<br />Other usage data => No</p>
      </td>

      <td>
        <ul><li>Product interactions - App launches are measured, as well as any other user interaction configured by the app developer.</li><li>Advertising &#x26; Other usage data -   If you have configured to send this data, then declare accordingly.</li></ul>
      </td>
    </tr>

    <tr>
      <td>
        * \*Diagnostics data\*\*Crash dataPerformance dataOther diagnostics data
      </td>

      <td>
        Not collectedCrash data ==> No\
        Performance data ==> No\
        Other diagnostics data => No
      </td>

      <td>
        For Crash & other diagnostics data. If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Other Data** 
      </td>

      <td>
        Required, Collected
      </td>

      <td>
        Technical device information (for example, OS, device type, language, Wi-Fi status, screen size,  etc.) or network information (for example, IP address, HTTP user agent) is received by CleverTap.\
        IDFV is collected.
      </td>
    </tr>
  </tbody>
</Table>

# Data use

Understand how each data type is used by you and your third-party partners. For example, collecting an email address and using it to authenticate the user and personalize the user’s experience within your app would include App Functionality and Product Personalization. For more information, see [Apple Data Type Usage](https://developer.apple.com/app-store/app-privacy-details/#data-type-usage)

| Purpose                              | Selection based on how data from CleverTap will be used                                                                                                                                                                                      |
| :----------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Third-Party Advertising              | Select if you share the data type with a third party for the purpose of displaying third-party advertisements in your app.                                                                                                                   |
| Developer’s Advertising or Marketing | Select if you share the data type with a third party for displaying first-party advertisements in your app, sending marketing communications directly to your users, or sharing data with entities who will display your advertisements.     |
| Analytics                            | Select if you are using data to evaluate user behavior, which includes understanding the effectiveness of existing product features, plan new features, or measure audience size or characteristics.                                         |
| Product Personalization              | Customizing what the user sees, such as a list of recommended products, posts, or suggestions. Select if you plan to use CleverTap Engagement features such as Product Experiences, In Apps, App Inbox, and so on to personalize the product |
| App Functionality                    | Authenticate the user, enable features, prevent fraud, implement security measures, ensure server up-time, minimize app crashes, improve scalability and performance, or perform customer support. Select if applicable.                     |
| Other Purposes                       | Any other purposes not listed. Select if applicable                                                                                                                                                                                          |

# Data linked to the user

Data types collected through CleverTap are typically connected to identifiers when configured.  Therefore, app developers must select 'Yes' when asked if the data is linked to an identity.\
The associations that the customer is capable of making (for example, to a customer account or an identifiable individual) is unique to the customers and at their own discretion.

# Tracking

You’ll need to understand whether you and/or your third-party partners use data from your app to track users and, if so, which data is used for this purpose.  CleverTap iOS SDK does not track data by default and it is unique to the customers and at their own discretion.

# Privacy links

Please refer to CleverTap privacy policy here: [https://clevertap.com/privacy-policy/](https://clevertap.com/privacy-policy/)
