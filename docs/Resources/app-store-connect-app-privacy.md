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
[block:api-header]
{
  "title": "Overview"
}
[/block]
Starting December 8, 2020, Apple requires [App Privacy Details](https://developer.apple.com/app-store/app-privacy-details/) for all new apps and app updates. Refer to [Manage App Privacy](https://help.apple.com/app-store-connect/#/dev1b4647c5b) to understand Apple’s privacy rules and app integrations with multiple third-party SDK(s) to make the correct decision for your app. 
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "This document is not legal advice. It is provided for your information and convenience. You must work closely with legal and other professional advisors to determine exactly how Apple policies apply to you."
}
[/block]
#Data types
CleverTap collects the following data for your app:
* **Collected** - CleverTap SDK requires this data. It is collected by default.
* **Not collected **-  CleverTap SDK does not require this data.
* **May be, By Default: Not collected** -  Not required by CleverTap SDK. However, you can configure to send this data to CleverTap for additional tracking. 

#Data type

The following table should help you to answer the privacy declaration. 

[block:parameters]
{
  "data": {
    "h-0": "Data Type",
    "h-1": "Sent To CleverTap?",
    "h-2": "Description",
    "0-0": "**Contact information**\n\n* Name\n\n* Email address\n\n* Phone number \n\n* Physical address\n\n* Other user contact info ",
    "0-1": "May be, By Default: Not collected",
    "0-2": "If you have configured to send this data, then declare accordingly.",
    "1-0": "**Health and fitness**\n\nHealth\n\nFitness",
    "1-1": "May be, By Default: Not collected",
    "1-2": "If you have configured to send this data, then declare accordingly.",
    "2-0": "**Financial info**\n\nPayment info\n\nCredit info\n\nOther financial info",
    "2-1": "May be, By Default: Not collected",
    "2-2": "If you have configured to send this data, then declare accordingly.",
    "3-0": "**Location info**\n\nPrecise location\n\nCoarse location",
    "3-1": "May be, By Default: Not collected",
    "3-2": "If you have configured to send this data, then declare accordingly. \nThis data requires additional integration :\n* [Precise Location Integration](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/3.7.0)\n* [Coarse Location integration](https://developer.clevertap.com/docs/sdk-changes-for-gdpr-compliance#section-device-network-information-reporting-in-i-os).",
    "4-0": "**Sensitive info** ",
    "4-1": "May be, By Default: Not collected",
    "4-2": "If you have configured to send this data, then declare accordingly.",
    "5-0": "**Contacts** ",
    "5-1": "May be, By Default: Not collected",
    "5-2": "If you have configured to send this data, then declare accordingly.",
    "6-0": "**User content**\n\nEmails or text messages\n\nPhotos or videos\n\nAudio data\n\nGameplay content\n\nCustomer support\n\nOther user content",
    "6-1": "May be, By Default: Not collected",
    "6-2": "If you have configured to send this data, then declare accordingly.",
    "7-0": "**Browsing history** ",
    "7-1": "Not collected",
    "7-2": "If you have configured to send this data, then declare accordingly.",
    "8-0": "**Search history** ",
    "8-1": "Not collected",
    "8-2": "If you have configured to send this data, then declare accordingly.",
    "9-0": "**Identifiers**\n\nUser ID\n\nDevice ID",
    "9-1": "May be, By Default: Not collected",
    "9-2": "Latest CleverTap iOS SDK does not capture IDFA or other device IDs such as IMEI. The IDFV is used to create CleverTap ID which is disclosed under Other Data.",
    "10-0": "**Purchases**\n\nPurchase history",
    "10-1": "May be, By Default: Not collected",
    "10-2": "If you have configured to send this data, then declare accordingly.",
    "11-0": "**Usage data**\n\nProduct interaction\n\nAdvertising data\n\nOther usage data (for example, Wi-Fi)",
    "11-1": "Required, Collected\n\nProduct interaction => Yes\nAdvertising data => No\nOther usage data => No",
    "11-2": "* Product interactions - App launches are measured, as well as any other user interaction configured by the app developer.\n* Advertising & Other usage data -   If you have configured to send this data, then declare accordingly.",
    "12-0": "**Diagnostics data**\n\nCrash data\n\nPerformance data\n\nOther diagnostics data",
    "12-1": "Not collected\n\nCrash data ==> No\nPerformance data ==> No\nOther diagnostics data => No",
    "12-2": "For Crash & other diagnostics data. If you have configured to send this data, then declare accordingly.",
    "13-0": "**Other Data** ",
    "13-1": "Required, Collected",
    "13-2": "Technical device information (for example, OS, device type, language, Wi-Fi status, screen size,  etc.) or network information (for example, IP address, HTTP user agent) is received by CleverTap.\nIDFV is collected."
  },
  "cols": 3,
  "rows": 14
}
[/block]
#Data use

Understand how each data type is used by you and your third-party partners. For example, collecting an email address and using it to authenticate the user and personalize the user’s experience within your app would include App Functionality and Product Personalization. For more information, see [Apple Data Type Usage](https://developer.apple.com/app-store/app-privacy-details/#data-type-usage)
[block:parameters]
{
  "data": {
    "h-0": "Purpose",
    "h-1": "Selection based on how data from CleverTap will be used",
    "0-0": "Third-Party Advertising",
    "0-1": "Select if you share the data type with a third party for the purpose of displaying third-party advertisements in your app.",
    "1-0": "Developer’s Advertising or Marketing",
    "1-1": "Select if you share the data type with a third party for displaying first-party advertisements in your app, sending marketing communications directly to your users, or sharing data with entities who will display your advertisements.",
    "2-0": "Analytics",
    "2-1": "Select if you are using data to evaluate user behavior, which includes understanding the effectiveness of existing product features, plan new features, or measure audience size or characteristics.",
    "3-0": "Product Personalization",
    "3-1": "Customizing what the user sees, such as a list of recommended products, posts, or suggestions. Select if you plan to use CleverTap Engagement features such as Product Experiences, In Apps, App Inbox, and so on to personalize the product",
    "4-0": "App Functionality",
    "4-1": "Authenticate the user, enable features, prevent fraud, implement security measures, ensure server up-time, minimize app crashes, improve scalability and performance, or perform customer support. Select if applicable.",
    "5-0": "Other Purposes",
    "5-1": "Any other purposes not listed. Select if applicable"
  },
  "cols": 2,
  "rows": 6
}
[/block]
#Data linked to the user
Data types collected through CleverTap are typically connected to identifiers when configured.  Therefore, app developers must select 'Yes' when asked if the data is linked to an identity.
The associations that the customer is capable of making (for example, to a customer account or an identifiable individual) is unique to the customers and at their own discretion.

#Tracking
You’ll need to understand whether you and/or your third-party partners use data from your app to track users and, if so, which data is used for this purpose.  CleverTap iOS SDK does not track data by default and it is unique to the customers and at their own discretion.


#Privacy links
Please refer to CleverTap privacy policy here: https://clevertap.com/privacy-policy/