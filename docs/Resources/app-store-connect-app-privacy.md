---
title: App Store Connect - App Privacy
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
        Data Type
      </th>

      <th>
        Sent To CleverTap?
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        **Contact information**

        * Name

        * Email address

        * Phone number 

        * Physical address

        * Other user contact info 
      </td>

      <td>
        May be, By Default: Not collected
      </td>

      <td>
        If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Health and fitness**

        Health

        Fitness
      </td>

      <td>
        May be, By Default: Not collected
      </td>

      <td>
        If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Financial info**

        Payment info

        Credit info

        Other financial info
      </td>

      <td>
        May be, By Default: Not collected
      </td>

      <td>
        If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Location info**

        Precise location

        Coarse location
      </td>

      <td>
        May be, By Default: Not collected
      </td>

      <td>
        If you have configured to send this data, then declare accordingly.\
        This data requires additional integration :

        * [Precise Location Integration](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/3.7.0)
        * [Coarse Location integration](https://developer.clevertap.com/docs/sdk-changes-for-gdpr-compliance#section-device-network-information-reporting-in-i-os).
      </td>
    </tr>

    <tr>
      <td>
        **Sensitive info** 
      </td>

      <td>
        May be, By Default: Not collected
      </td>

      <td>
        If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Contacts** 
      </td>

      <td>
        May be, By Default: Not collected
      </td>

      <td>
        If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **User content**

        Emails or text messages

        Photos or videos

        Audio data

        Gameplay content

        Customer support

        Other user content
      </td>

      <td>
        May be, By Default: Not collected
      </td>

      <td>
        If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Browsing history** 
      </td>

      <td>
        Not collected
      </td>

      <td>
        If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Search history** 
      </td>

      <td>
        Not collected
      </td>

      <td>
        If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Identifiers**

        User ID

        Device ID
      </td>

      <td>
        May be, By Default: Not collected
      </td>

      <td>
        Latest CleverTap iOS SDK does not capture IDFA or other device IDs such as IMEI. The IDFV is used to create CleverTap ID which is disclosed under Other Data.
      </td>
    </tr>

    <tr>
      <td>
        **Purchases**

        Purchase history
      </td>

      <td>
        May be, By Default: Not collected
      </td>

      <td>
        If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Usage data**

        Product interaction

        Advertising data

        Other usage data (for example, Wi-Fi)
      </td>

      <td>
        Required, Collected

        Product interaction => Yes\
        Advertising data => No\
        Other usage data => No
      </td>

      <td>
        * Product interactions - App launches are measured, as well as any other user interaction configured by the app developer.
        * Advertising & Other usage data -   If you have configured to send this data, then declare accordingly.
      </td>
    </tr>

    <tr>
      <td>
        **Diagnostics data**

        Crash data

        Performance data

        Other diagnostics data
      </td>

      <td>
        Not collected

        Crash data ==> No\
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

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Purpose
      </th>

      <th>
        Selection based on how data from CleverTap will be used
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Third-Party Advertising
      </td>

      <td>
        Select if you share the data type with a third party for the purpose of displaying third-party advertisements in your app.
      </td>
    </tr>

    <tr>
      <td>
        Developer’s Advertising or Marketing
      </td>

      <td>
        Select if you share the data type with a third party for displaying first-party advertisements in your app, sending marketing communications directly to your users, or sharing data with entities who will display your advertisements.
      </td>
    </tr>

    <tr>
      <td>
        Analytics
      </td>

      <td>
        Select if you are using data to evaluate user behavior, which includes understanding the effectiveness of existing product features, plan new features, or measure audience size or characteristics.
      </td>
    </tr>

    <tr>
      <td>
        Product Personalization
      </td>

      <td>
        Customizing what the user sees, such as a list of recommended products, posts, or suggestions. Select if you plan to use CleverTap Engagement features such as Product Experiences, In Apps, App Inbox, and so on to personalize the product
      </td>
    </tr>

    <tr>
      <td>
        App Functionality
      </td>

      <td>
        Authenticate the user, enable features, prevent fraud, implement security measures, ensure server up-time, minimize app crashes, improve scalability and performance, or perform customer support. Select if applicable.
      </td>
    </tr>

    <tr>
      <td>
        Other Purposes
      </td>

      <td>
        Any other purposes not listed. Select if applicable
      </td>
    </tr>
  </tbody>
</Table>

# Data linked to the user

Data types collected through CleverTap are typically connected to identifiers when configured.  Therefore, app developers must select 'Yes' when asked if the data is linked to an identity.\
The associations that the customer is capable of making (for example, to a customer account or an identifiable individual) is unique to the customers and at their own discretion.

# Tracking

You’ll need to understand whether you and/or your third-party partners use data from your app to track users and, if so, which data is used for this purpose.  CleverTap iOS SDK does not track data by default and it is unique to the customers and at their own discretion.

# Privacy links

Please refer to CleverTap privacy policy here: [https://clevertap.com/privacy-policy/](https://clevertap.com/privacy-policy/)
