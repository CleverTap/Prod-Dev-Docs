---
title: Smart TV SDKs
excerpt: Learn about supported features by CleverTap for Smart TV SDKs.
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

Smart TVs have become an integral part of modern entertainment, offering a range of interactive features and services. Various applications are available for these platforms, each providing different functionalities. The CleverTap SDKs enable developers to incorporate functionalities such as analytics, push notifications, in-app pop-ups, and app inboxes into their Smart TV applications. This document provides the information about supported features across different Smart TV platforms, highlighting the capabilities and limitations of each platform.

# Supported Features for Smart TV SDKs

The following table describes the supported features for each Smart TV SDK:

| Feature x Platform               | Android TV / Google TV | Apple TV | Samsung Web OS TV (Tizen) | LG Web OS TV | Fire TV (Android) | Jio STB (Android) |
| :------------------------------- | :--------------------- | :------- | :------------------------ | :----------- | :---------------- | :---------------- |
| **Analytics**                    | Yes                    | Yes      | Yes (only 7.0)            | Yes          | Yes               | Yes               |
| **Push (OOTB)**                  | No                     | No       | No                        | No           | No                | No                |
| **Custom Push**                  | Yes                    | No       | No                        | No           | No                | No                |
| **In Apps/Web Pop-Ups**          | Yes                    | Yes      | Yes                       | Yes          | Yes               | Yes               |
| **App Inbox**                    | Yes                    | No       | Yes                       | Yes          | Yes               | Yes               |
| **Native Display**               | Yes                    | No       | Yes                       | Yes          | Yes               | Yes               |
| **Product Experiences**          | Yes                    | No       | Yes                       | Yes          | Yes               | Yes               |
| **Minimum SDK Version Required** | v5.0.0 +               | v4.2.0 + | v1.7.0 +                  | v1.7.0 +     | v5.0.0 +          | v5.0.0 +          |

**Yes**: Supported

**No**: Not Supported

# Smart TV Integration Guides

This section lists all the available Smart TV integration guides for various platforms:

## iOS

* [Apple TV](doc:apple-tv-integration-with-clevertap-ios-sdk)

## Android

* [Android TV](doc:android-tv-integration-with-android-sdk)
* [Fire TV](doc:fire-tv-integration-with-android-sdk)
* [Jio TV](doc:jio-tv-set-top-boxstb-integration-with-android-sdk)

## Web

* [Samsung Tizen TV](doc:samsung-tizen-tv-integration-with-web-sdk)
* [LG WebOS TV](doc:lg-webos-tv-integration-with-web-sdk)
