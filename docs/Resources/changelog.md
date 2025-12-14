---
title: Changelog
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
We regularly update the CleverTap API and SDKs with new features, bug fixes, and performance improvements. 

If you want to receive email updates with the latest changes to the CleverTap API and SDKs, please fill out [this form](https://goo.gl/forms/5VpOnTsAlmAfQ2Lw2).

# 2017
**October 13, 2017: Updates to iOS SDK**
* Fixed App Extension deployment target.

**October 8, 2017: Updates to Android SDK**
* Fixed deep link handling issue for devices below Android N.

**September 21, 2017: Updates to Android SDK**
* Added Android O support for push notifications. 
* Improved debugging mechanism. 
* Added various performance enhancements.

**September 19, 2017: Updates to iOS SDK.** 
* Added support for iOS 11 and Xcode 9. 
* Updated debugging mechanism. 
* Added support for handling foreground notifications.

**June 30, 2017: Updates to Android SDK**
* Added ability to play custom sounds, display call-to-action buttons, and set priority for push notifications.

**June 29, 2017: Updates to iOS SDK**
* Added API to record screen views, tvOS support, and modulemap for Swift import.
2016

**September 23, 2016: Updates to iOS SDK**
* Added support for iOS 10 and Xcode 8 support. 
* Added support for rich push notifications, App Extensions and watchOS apps.

**August 31, 2016: Updates to Android SDK**
* Added FCM support. 
* Updated GCM handling.

**August 17, 2016: Updates to iOS SDK**
* Fixed Xcode 8 to Xcode 7 archiving issue.

**July 20, 2016 Updates to iOS SDK**
* Patched NSDate handling in the onUserLogin method.

**July 18, 2016:  Updates to Android & iOS SDKs**
* Added getLocation API, which lets your app get a user’s location. 
* Added onUserLogin API, which lets your app support multiple user profiles per device.

**June 28, 2016: Updates to iOS SDK**
* Fixed issue with handling of deep links embedded in push notifications on app launch.

**June 24, 2016: Updates to Android & iOS SDKs**
* Fixed an issue that caused failed push notifications when sending non-primitive values for profile or event properties.

**June 15, 2016: Updates to Android SDK**
* Updated to support the new Google Play privacy policies.

**June 6, 2016: Updates to Android SDK**
  * Fixed an in-app notification blacklist bug.
  
**May 8, 2016: Updates to Android & iOS SDKs**
  * Added dashboard analytics support for in-app notification button clicks.

**April 20, 2016: Updates to Android SDK**
* Added multi-value profile property handling improvements.

**April 6, 2016: Updates to Android SDK**
* Improved uninstall tracking support.

**April 5, 2016 Updates to iOS SDK**
  * Added support for Segment bundled integration. 
  * Removed support for Segment webhook integration. 
  * Fixed thread deadlock issue.
 
**March 14, 2016: Updates to Android & iOS SDKs**
  * Added support for migrating from Parse push notifications.
  * Added multi-value user profile properties.  To support multi-value user profile properties for Android, CleverTap.profile.getProperty(key) now returns an object instead of a string.
  * Added frequency capping for in-app notification displays.
  
**February 08, 2016: Updates to iOS SDK**
  * Fixed UIViewController exception issue.
  * February 02, 2016: Updates to iOS SDK
  * Fixed CTTelephonyNetworkInfo issue.

**January 09, 2016: Updates to Android & iOS SDKs**
  * Added an updateLocation method for Android and a setLocation method for iOS, which let your app send user’s geo data to CleverTap for segmentation. 
  * Added support for Segment webhook integration.