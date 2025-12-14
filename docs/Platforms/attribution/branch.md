---
title: Branch
excerpt: Attribution Platforms
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

[Branch](https://branch.io/) is a mobile measurement and deep linking platform that helps acquire and engage users efficiently across all devices, channels, and platforms. This document provides information about integrating Branch and CleverTap. This integration helps track the following information from Branch: 

* **Install events**: These events can be organic install events and inorganic install events. 
* **Custom Events**: These events can include any events (other than install events) that are provided by the attribution partner or tracked in the app. The following events are tracked for Branch: Lifecycle events, Commerce events, and Content events. For more information about each event, refer to [Branch documentation](https://help.branch.io/developers-hub/docs/tracking-commerce-content-lifecycle-and-custom-events).

To learn more about these events and their default attribution settings in the CleverTap dashboard, refer to [Types of Data](https://docs.clevertap.com/docs/attribution#types-of-data).

# Integrating Branch

To enable Branch integration with the CleverTap dashboard, proceed as follows:

1. [Add CleverTap Credentials to Branch Dashboard](doc:branch#add-clevertap-credentials-to-branch-dashboard).
2. [Integrate Branch](doc:branch#integrate-branch).
3. [Setup for Organic Install and Custom Events](doc:branch#setup-for-organic-install-and-custom-events).

## Add CleverTap Credentials to Branch Dashboard

To activate integration with CleverTap from the Branch dashboard, follow the steps listed under [CleverTap integration with Branch](https://help.branch.io/partners-portal/docs/clevertap-data#2-connect-clevertap-in-branch) section of Branch documentation.

<Image title="Add CleverTap Credentials to Branch Dashboard" alt={1310} align="center" border={true} src="https://files.readme.io/09d6e51-Branch_Dashboard.png">
  Add CleverTap Credentials to Branch Dashboard
</Image>

> 📘 Region
>
> In the above figure, enter the region of your CleverTap account. The following table helps identify the region of your account:

| Dashboard URL                                                                                        | Region    |
| :--------------------------------------------------------------------------------------------------- | :-------- |
| [https://eu1.dashboard.clevertap.com/login.html#/](https://eu1.dashboard.clevertap.com/login.html#/) | Europe    |
| [https://in1.dashboard.clevertap.com/login.html#/](https://in1.dashboard.clevertap.com/login.html#/) | India     |
| [https://us1.dashboard.clevertap.com/login.html#](https://us1.dashboard.clevertap.com/login.html#)   | US        |
| [https://sg1.dashboard.clevertap.com/login.html#/](https://sg1.dashboard.clevertap.com/login.html#/) | Singapore |
| [https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html)   | aps3      |
| [https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html)   | mec1      |

## Integrate Branch

The integration steps vary for Android and iOS. The steps for both are listed below.

### **For Android App**

1. To track attribution and events data in your Android app, perform the steps listed in [Branch Android SDK Integration Guide](https://help.branch.io/developers-hub/docs/android-basic-integration).
2. Add the following code to your Android app code before ​you ​initialize ​`onCreate()` or Deep ​Link ​Activity’s `onCreate()` in ​your application:

*For SDK version 4.2.0 and above*\
The `getCleverTapAttributionIdentifier` method is deprecated for CleverTap Android SDKs version 4.2.0 and above. Use the new `getCleverTapID` method to get the CleverTap ID on the `OnInitCleverTapIDListener` to set `CustomerUserId` method of Branch.

```java
cleverTapInstance.getCleverTapID(new OnInitCleverTapIDListener() {
   @Override
   public void onInitCleverTapID(final String cleverTapID) {
   // Callback on main thread
   Branch branch = Branch.getInstance();
   branch.setRequestMetadata("$clevertap_attribution_id",
   cleverTapID);
   ...
   Branch.initSession(...);
   }  
});
```
```kotlin
cleverTapInstance?.getCleverTapID {
// Callback on main thread
      Branch.getInstance()?.setRequestMetadata("$clevertap_attribution_id",
it)
...
Branch.initSession(...)

}
```

### SDK version 4.1.1 and below

```java
Branch branch = Branch.getInstance();
branch.setRequestMetadata("$clevertap_attribution_id",
cleverTapInstance.getCleverTapAttributionIdentifier());
...
Branch.initSession(...);
```
```kotlin
Branch.getInstance()?.setRequestMetadata("$clevertap_attribution_id",
cleverTapInstance?.cleverTapAttributionIdentifier);
...
Branch.initSession(...);
```

### **For iOS App**

To integrate Branch with CleverTap for an iOS app:

1. To track attribution and events data in your iOS app, perform the steps listed in [Branch iOS SDK Integration Guide](https://help.branch.io/developers-hub/docs/ios-basic-integration).
2. Add the following code inside `didFinishLaunchingWithOptions` of  iOS app code:

```objectivec
Branch *branch = [Branch getInstance];
[CleverTap autoIntegrate];
[[Branch getInstance] setRequestMetadataKey:@"$clevertap_attribution_id"
value:[[CleverTap sharedInstance] profileGetCleverTapAttributionIdentifier]];
```
```swift
CleverTap.autoIntegrate()
if let branch = Branch.getInstance() {
branch.setRequestMetadataKey("$clevertap_attribution_id",
value:CleverTap.sharedInstance()?profileGetCleverTapAttributionIdentifier() as
NSObject!);
}
```

## Setup for Organic Install and Custom Events

After successful integration, the Branch starts pushing inorganic install events data to CleverTap. To receive additional events, proceed as follows:

1. Navigate to *Settings* > *Partners* and scroll down to the *Attribution partners* section. 

<Image alt="CleverTap Attribution Partners" align="center" src="https://files.readme.io/65f3ee649982e8d2962865337cea2deb72133a68a4bd09d755c5db59f0287b5b-Amplitude_Partner_Page.png">
  CleverTap Attribution Partners
</Image>

2. The *Attribution partner - Branch* popup appears on the right side of the screen.

<Image title="Attribution Partner - Branch" alt={2370} align="center" border={true} src="https://files.readme.io/92ad667-Attribution_partner_-_Branch.png">
  Attribution Partner - Branch
</Image>

3. Select *Custom events* to accept this event data when shared by the partner.
4. Click **Save**. On clicking, the following message displays at the top of the screen: *Changes saved. Admins are notified of this via email*. 

# Viewing Data in Dashboard

You can now view the event data on the CleverTap dashboard. To do so, proceed as follows:

1. From the CleverTap dashboard, navigate to *Analytics > Events*.
2. Apply the required filters for the selected event. The filters vary depending on the type of event.

* **For Install Events**: All the install events are tracked under the *UTM Visited* event.

<Image title="Filter Install Events" alt={1688} align="center" border={true} src="https://files.readme.io/29e275d-Events_Filters_BR.png">
  Filter Install Events
</Image>

* **For Custom Events**: Lifecycle events, Commerce events, Content events, and Custom events received from Branch are prefixed with **BR** in CleverTap.
