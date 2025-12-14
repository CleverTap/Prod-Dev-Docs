---
title: September 2023
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
We are excited to share the September SDK changelog for our product!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# September 29

## [iOS 5.2.1](https://github.com/CleverTap/clevertap-signedcall-ios-sdk/releases/tag/0.0.5)

### Adds

- **Added** support to enable `NSFileProtectionComplete` to secure the App’s document directory.
- **Added** support for Integration Debugger to show errors and events on the dashboard when `debugLevel` is set to 3.
- **Added** support to send `locale` language and country data using `NSLocale`.
- **Added** a public API `setLocale` to set custom locale for LP Parity.

### Changes

- **Updated** logic to retrieve country code using `NSLocale` for iOS 16 and above. The `CTCarrier` is now deprecated for iOS 16 and above. For more information, refer to the [Apple documentation](https://developer.apple.com/documentation/coretelephony/ctcarrier).
- **Updated** logic to not send carrier name above iOS 16 in `CTCarrier` field.

### Fixes

- **Fixed** an iOS 17/Xcode 15 crash related to alert In-Apps.
- **Fixed** a failing `test_clevertap_instance_nscoding` test case.

# September 26

## [iOS Geofence 1.0.4](https://github.com/CleverTap/clevertap-geofence-ios/releases/tag/1.0.4)

### Adds

- **Added** support for the latest [CleverTap iOS SDK](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/5.2.0) via Swift Package Manager.

### Updates

- **Updated** [example applications](https://github.com/CleverTap/clevertap-geofence-ios/tree/master/Examples).

# September 13

## [Web 1.6.6](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.6.6)

### Fixes

- **Fixed** a bug related to Interstitial and Image only campaign coexistence.
- **Fixed** a bug where dismiss spam control did not work for campaigns created with delay.