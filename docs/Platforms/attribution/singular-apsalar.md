---
title: Singular (formerly known as Apsalar)
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

[Singular](https://www.singular.net/), a marketing analytics platform, empowers businesses to drive growth by delivering attribution, cost aggregation, marketing analytics, creative reporting, and workflow automation. This document provides information about integrating Singular and CleverTap. This integration helps track the following information from Singular: 

* **Install events**: These events can be organic install events and inorganic install events. 
* **Custom events**: These events can include any events (other than install events) that are provided by the attribution partner and tracked in the app. The following events are tracked for Singular **Revenue events** and **Fraud Data**.

To learn more about these events and their default attribution settings on the CleverTap dashboard, refer to [Types of Data](https://docs.clevertap.com/docs/attribution#types-of-data).

# Integrating Singular

To enable Singular integration with the CleverTap dashboard:

1. Add CleverTap Credentials to Singular Dashboard:\
   To activate integration with CleverTap from the Singular dashboard, follow the steps listed under [Partner Configuration](https://support.singular.net/hc/en-us/articles/360053018851-How-to-Configure-Partner-Settings-and-Postbacks#Configuration_Steps) in Singular documentation.
2. [Integrate Singular](doc:singular-apsalar#integrate-singular).
3. [Setup for Organic Install and Custom Events](doc:singular-apsalar#setup-for-organic-install-and-custom-events).

## Integrate Singular

Singular SDK's implementation requires `CleverTap ID` configured as a Singular custom userId. 

> 🚧 Warning
>
> Only the events with the `clevertapId` parameter in postback are processed.

The integration steps vary for Android and iOS. The steps for both are listed below.

### **For Android App**

1. To track attribution and events data in your Android app, perform the steps listed in [Singular Android SDK Integration Guide](https://support.singular.net/hc/en-us/articles/360038263692-Android-SDK-Tracking-Events-and-Revenue).
2. Set the value of `CleverTap ID` as the Singular Global Property. To do so, add the following code to your Android app code. For more information, [click here](https://support.singular.net/hc/en-us/articles/4406754979995--NEW-CleverTap-Attribution-Integration#:~:text=In%20your-,Android%20app%20code,-%2C%20add%20the%20following). 

#### *For SDK version 4.2.0 and above*

The `getCleverTapAttributionIdentifier` method is deprecated for CleverTap Android SDKs version 4.2.0 and above. Use the new `getCleverTapID` method to get the CleverTap ID on the `OnInitCleverTapIDListener` to set GlobalProperty of Singular.

```java
cleverTapInstance.getCleverTapID(new OnInitCleverTapIDListener() {
   @Override
   public void onInitCleverTapID(final String cleverTapID) {
       // Callback on main thread
      SingularSDK.setGlobalProperty("CLEVERTAPID", cleverTapID, true);
   }   
});
```

#### *For SDK version 4.1.1 and below*

```java
String attributionID = cleverTapInstance.getCleverTapAttributionIdentifier();
SingularSDK.setGlobalProperty("CLEVERTAPID", attributionID, true);
```

### **For iOS App**

To integrate Singular with CleverTap for an iOS app:

1. To track attribution and events data in your iOS app, perform the steps listed in [Singular iOS SDK Integration Guide](https://support.singular.net/hc/en-us/articles/360038341672-iOS-SDK-Tracking-Events-and-Revenue). 
2. Add the below code to your iOS app code. For more information about setting Singular Global Property, refer to [Singular documentation](https://support.singular.net/hc/en-us/articles/4406754979995--NEW-CleverTap-Attribution-Integration#:~:text=code%20to%20your-,iOS%20app,-%3A).

```objectivec
[CleverTap autoIntegrate];
[Singular setGlobalProperty:@"CLEVERTAPID" value:[[CleverTap sharedInstance] profileGetCleverTapAttributionIdentifier] overrideExisting:YES];
```

## Setup for Organic Install and Custom Events

After successful integration, Singular sends inorganic install events data to CleverTap. To receive additional events, proceed as follows: 

1. Navigate to *Settings* > *Partners* and scroll down to the *Attribution partners* section.

<Image alt="CleverTap Attribution Partners" align="center" border={true} src="https://files.readme.io/ee5bb5a28e1f8d9d2592c06647e9f4a3436566cfff523567c03ddae8c44b66da-Amplitude_Partner_Page.png">
  CleverTap Attribution Partners
</Image>

2. On clicking, *Attribution partner - Singular* popup appears on the right side of the screen.

<Image title="Attribution Partner - Singular" alt={2374} align="center" border={true} src="https://files.readme.io/7138ba6-Attribution_partner_-_Singular.png">
  Attribution Partner - Singular
</Image>

3. Select the required option from the following available options. Selecting any of these options indicates that the CleverTap accepts event data when shared by the partner.
   * Custom events
   * Organic install events

> 🚧 Duplication of Events
>
> If you select *Organic install events* for more than one attribution partner, a warning for duplication of events displays, as shown in the following figure. We strongly recommend tracking the organic install events from only one attribution partner.

<Image title="Duplication of Events" alt={1124} align="center" width="80%" border={true} src="https://files.readme.io/713fdc3-Duplication_of_events.png">
  Duplication of Events
</Image>

4. Click **Save**. On clicking, the following message displays at the top of the screen: *Changes saved. Admins are notified of this via email*. 

# Viewing Data in Dashboard

You can now view the event data in the CleverTap dashboard. To do so, proceed as follows:

1. From the CleverTap dashboard, navigate to *Analytics* > *Events*.
2. Apply the required filters for the selected event. The filters vary depending upon the type of event.

* **For Install Events**: All the install events are tracked under the *UTM Visited* event.

<Image title="Filter Install Events" alt={1686} align="center" border={true} src="https://files.readme.io/6ecb385-Screenshot_2021-11-30_at_10.48.21_PM.png">
  Filter Install Events
</Image>

* **For Custom Events**: Revenue and Custom events received from Singular are prefixed with **AP** in CleverTap.
