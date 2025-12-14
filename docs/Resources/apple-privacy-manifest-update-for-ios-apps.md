---
title: Apple Privacy Manifest Update for iOS Apps
excerpt: >-
  This document explains the Apple Privacy Manifest Updates and CleverTap's
  compliance with these updates to ensure clarity for CleverTap iOS SDK users.
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

During Worldwide Developers Conference (WWDC23), Apple introduced new privacy manifests for SDKs to enhance transparency and privacy protection for users. These manifests provide insights into how third-party SDKs utilize data, ensuring developers can secure software dependencies effectively. 

Additionally, Apple introduced new requirements for APIs, necessitating reasons for their usage of In-App updates. This means it is essential to provide an approved reason in the app's Privacy Manifest that explains how your app utilizes the APIs. Failure to comply with these requirements results in a notice prompting you to update the Privacy Manifest accordingly. This update ensures transparency and compliance with Apple's privacy standards.

CleverTap recommends you include an approved reason in the app's Privacy Manifest that accurately explains how your app uses the concerned APIs. For more information, refer to the [Apple](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api)  documentation.

> 📘 Important
> 
> Starting May 1st, 2024, apps incorporating third-party SDKs, used on the App Store must integrate versions of the SDKs that include privacy manifests.

# CleverTap's Compliance with Apple's Privacy Manifest

This section describes the Privacy Manifest and how CleverTap adheres to the new guidelines.

## Privacy Manifest

The [Privacy Manifest](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_data_use_in_privacy_manifests) file outlines an iOS app's data collection practices, such as personal information, browsing history, location, or how the app uses the data, and whether this data is shared with third parties. It empowers the app users with essential insights into their privacy. CleverTap SDKs and associated modules above core version v6.2.1 align with Apple's guidelines.

The following data is declared in CleverTap's Privacy Manifest:

[block:parameters]
{
  "data": {
    "h-0": "NSPrivacyCollected  \nDataType",
    "h-1": " NSPrivacyCollected  \nDataTypePurposes",
    "h-2": "Modules with NSPrivacyCollected  \nDataTypeLinked",
    "h-3": "Modules with NSPrivacyCollected  \nDataTypeTracking",
    "0-0": "`UserID`",
    "0-1": "Product personalization, App functionality",
    "0-2": "CleverTap-iOS-SDK",
    "0-3": "\\- ",
    "1-0": "`Device ID`",
    "1-1": "Product personalization, App functionality",
    "1-2": "CleverTap-iOS-SDK",
    "1-3": "\\-",
    "2-0": "`Product interaction`",
    "2-1": "Analytics, Product personalization",
    "2-2": "CleverTap-iOS-SDK",
    "2-3": "\\-",
    "3-0": "`Precise location`",
    "3-1": "Analytics, Product personalization, App functionality",
    "3-2": "CleverTapLocation, clevertap-geofence-ios",
    "3-3": "\\- "
  },
  "cols": 4,
  "rows": 4,
  "align": [
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


# FAQs

## Q. What should you do?

A. First, ensure you are using the latest CleverTap iOS SDK. Then, evaluate which APIs you are using and add them to your application's privacy manifest file accordingly. For more information, refer to Apple documentation: [Create a privacy manifest file](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files#4284009) .

## Q. When should apps be updated?

A. Apple has initiated warnings for app submissions lacking Privacy Manifests. Although Apple's documentation lacks a specific date, it mentions:

_Starting in spring 2024, if your new app or app update submission adds a third-party SDK that is commonly used in apps on the App Store, you will need to include the privacy manifest for the SDK._

## Q. Which third-party SDKs are affected by this requirement?

A. The following third-party SDKs are affected by this requirement.

- `CleverTap-iOS-SDK`
- `CTNotificationContent`
- `CTNotificationService`
- `CleverTap-Geofence-SDK`

We recommend updating these SDKs with app updates uploaded after May 1, 2024, to prevent potential rejections

## Q. How can this update in Privacy Manifest impact the existing app builds?

A. This update affects App versions released on or after May 1, 2024. Users on versions published before this date, including new installs and reinstalls, remain unaffected.

## Q. What are the consequences of not including Privacy Manifests for the affected SDKs?

A. Failure to include Privacy Manifests for third-party SDKs commonly used in apps on the App Store may result in Apple potentially rejecting app submissions. Therefore, developers must ensure compliance with this requirement to avoid any disruptions to their app releases.

For further assistance, refer to the official [Apple](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api) documentation or contact [CleverTap support](https://docs.clevertap.com/docs/support-ticket#raise-a-support-ticket).