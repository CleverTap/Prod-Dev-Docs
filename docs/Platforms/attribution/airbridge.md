---
title: Airbridge
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
# Introduction

[Airbridge](https://www.airbridge.io/) is a unified measurement solution that features Mobile Measurement (MMP), Multi-Touch Attribution (MTA) & Incrementality measurement, and Marketing Mix Modeling (MMM) packed into a single unified dashboard. CleverTap's integration with Airbridge helps marketers to:

* Track *Install Events* and *In-App Events* and determine where a user was acquired from, for a customized onboarding experience, resulting in higher customer engagement and retention rate.
* Identify the acquisition channels that deliver the most engaged users and analyze the quality of acquisition for strategic business and investment decisions.

To learn more about these events and their default attribution settings in the CleverTap dashboard, refer to [Types of Data](https://docs.clevertap.com/docs/attribution#types-of-data).\
For any issues regarding this integration, contact [Airbridge Support Team](mailto:platform@ab180.co).

# Integrate Airbridge

To enable Airbridge integration with the CleverTap dashboard:

1. [Add CleverTap Credentials to Airbridge Dashboard](doc:airbridge#add-clevertap-credentials-to-airbridge-dashboard).
2. [Integrate Airbridge SDK](doc:airbridge#integrate-airbridge-sdk).
3. [Setup for Organic Install and Custom Events](doc:airbridge#setup-for-organic-install-and-custom-events).

## Add CleverTap Credentials to Airbridge Dashboard

To activate integration with CleverTap from the Airbridge dashboard:

1. Navigate to *Integrations* > *Third-party Integrations* from the Airbridge dashboard and select *CleverTap*.

<Image title="Add CleverTap to Airbridge Dashboard" alt={2788} align="center" border={true} src="https://files.readme.io/451168d-Third-party_Integrations_page_.png">
  Add CleverTap to Airbridge Dashboard
</Image>

2. Enter the CleverTap credentials to the respective fields and click **Submit**.

<Image title="Add CleverTap Project Details to Airbridge Dashboard" alt={2798} align="center" border={true} src="https://files.readme.io/2c4619d-Add_CleverTap_Project_Details_to_Airbridge_Dashboard.png">
  Enter CleverTap Credentials
</Image>

After submitting the credentials successfully, the attributed Install Events are automatically sent to CleverTap.

## Integrate Airbridge SDK

Airbridge SDK's implementation requires `CleverTap ID` configured as an Airbridge custom userId. 

> 🚧 Warning
>
> Events without the `clevertapId` parameter in the postback are not processed.

The integration steps vary for Android and iOS. The steps for both are listed below.

### **For Android App**

1. To track attribution and event data in your Android app, perform the steps listed in [Airbridge Android SDK Integration Guide](https://developers.airbridge.io/v1.1-en/docs/android-sdk).
2. Set the `CleverTap ID` value as the Airbridge Global Property. To do so, add the following code to your Android app code.

*For SDK version 4.2.0 and above*\
The `getCleverTapAttributionIdentifier` method is deprecated for CleverTap Android SDKs version 4.2.0 and above. Use the new `getCleverTapID` method to get the CleverTap ID on the `OnInitCleverTapIDListener` to set the Airbridge Global Property as shown in the following code block:

```java
cleverTapInstance.getCleverTapID(new OnInitCleverTapIDListener() {
   @Override
   public void onInitCleverTapID(final String cleverTapID) {
       // Callback on main thread
      Airbridge.setDeviceAlias("clevertap_device_id", cleverTapID);
      // Explicitly start tracking
      Airbridge.startTracking();
   }   
});
```
```kotlin
// Fetch CleverTapID from CleverTap SDK
    CleverTapAPI.getDefaultInstance(this)?.getCleverTapID { cleverTapID ->
        // Set device alias into Airbridge SDK
        Airbridge.setDeviceAlias("clevertap_device_id", cleverTapID);
        // Explicitly start tracking
        Airbridge.startTracking();
    }
}
```

*For SDK version 4.1.1 and below*\
To set the Airbridge Global Property, use the following code to get the CleverTap ID on the `OnInitCleverTapIDListener`:

```java
String attributionID = cleverTapInstance.getCleverTapAttributionIdentifier();
Airbridge.setDeviceAlias("clevertap_device_id", cleverTapID);
Airbridge.startTracking(); // Explicitly start tracking
```
```kotlin
cleverTapInstance?.getCleverTapID {
// Set device alias into Airbridge SDK
Airbridge.setDeviceAlias("clevertap_device_id", cleverTapID);
// Explicitly start tracking
Airbridge.startTracking();
}
```

### **For iOS App**

1. To track attribution and event data in your iOS app, perform the steps listed in [Airbridge iOS SDK Integration Guide](https://developers.airbridge.io/v1.1-en/docs/ios-sdk). 
2. Set the `CleverTap ID` value as the Airbridge Global Property. Add the below code to your iOS app code.

```objectivec
[CleverTap autoIntegrate];
NSString *cleverTapID = [CleverTap.sharedInstance profileGetCleverTapID];
    if (cleverTapID != nil) {
        [AirBridge setDeviceAliasWithKey:@"clevertap_device_id" value:cleverTapID];
    }

[AirBridge startTracking];
```
```swift
CleverTap.autoIntegrate()
    if let attributionId = CleverTap.sharedInstance()?.profileGetID() {
        AirBridge.setDeviceAlias(withKey:"clevertap_device_id", value:String(attributionId));
    }

    AirBridge.startTracking();
```

## Setup for Organic Install and Custom Events

To set up organic install and custom events:

1. Navigate to *Settings* > *Partners* > *Partner List*. Select *Adjust* from the list.

<Image alt="CleverTap Attribution Partners" align="center" border={true} src="https://files.readme.io/2714c13c38a56caa0f44e7d1c6dbe5ed8436ecf0f0cd99943eea233f48b51db9-Amplitude_Partner_Page.png">
  CleverTap Attribution Partners
</Image>

2. The *Attribution partner - Airbridge* popup appears on the right side of the screen.

<Image title="Attribution Partner - Airbridge" alt={2406} align="center" border={true} src="https://files.readme.io/cf4c5fe-Attribution_partner_-_Airbridge.png">
  Attribution Partner - Airbridge
</Image>

3. Select the required option from the following available options: 
   * Custom events
   * Organic install events\
     Selecting any of these options indicates that the CleverTap accepts event data when shared by the partner.

> 🚧 Duplication of Events
>
> We strongly recommend tracking the organic install events from only one attribution partner. If you select *Organic install events* for more than one attribution partner, a warning for duplication of events displays, as shown in the following figure.

<Image title="Duplication of Events" alt={2396} align="center" width="80%" border={true} src="https://files.readme.io/2d3f9c7-Duplication_of_events_.png">
  Duplication of Events
</Image>

4. Click **Save**. The following message displays at the top of the screen: *Changes saved. Admins are notified of this via email*. 

# Viewing Data on Dashboard

You can now view the event data on the CleverTap dashboard. To do so, proceed as follows:

1. From the CleverTap dashboard, navigate to *Analytics* > *Events*.
2. Apply the required filters for the selected event. The filters vary depending on the type of event.

* **For Install Events**: All the install events are tracked under the *UTM Visited* event.

<Image title="Apply Event Filters" alt={1734} align="center" border={true} src="https://files.readme.io/004fa22-View_Install_Events.png">
  Apply Event Filters
</Image>

* **For Custom Events**: Custom events received from Airbridge are prefixed with **AB** in CleverTap.
