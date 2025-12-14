---
title: Appsflyer
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

[Appsflyer](https://www.appsflyer.com/) is a mobile marketing analytics and attribution platform that powers predictable app growth and delivers an exceptional mobile experience. This document provides information about integrating Appsflyer and CleverTap. This integration helps track the following information from Appsflyer: 

* **Install events**: These events can be organic install events and inorganic install events.
* **Custom events**: These events can include any events (other than install events) that are provided by the attribution partner and tracked in the app. The **in-app events** are tracked for Appsflyer.

To learn more about these events and their default attribution settings in CleverTap dashboard, refer to [Types of Data](https://docs.clevertap.com/docs/attribution#types-of-data).

# Integrate Appsflyer

To enable Appsflyer integration with the CleverTap dashboard, proceed as follows:

1. [Add CleverTap Credentials to Appsflyer Dashboard](doc:appsflyer#add-clevertap-credentials-to-appsflyer-dashboard).
2. [Integrate Appsflyer](doc:appsflyer#integrate-appsflyer-1).
3. [Setup for Organic Install and Custom Events](doc:appsflyer#setup-for-organic-install-and-custom-events).

## Add CleverTap Credentials to Appsflyer Dashboard

To activate integration with CleverTap from the Appsflyer dashboard, follow the steps listed under [CleverTap integration with Appsflyer](https://support.appsflyer.com/hc/en-us/articles/360001299165-CleverTap-integration-with-AppsFlyer) in Appsflyer documentation.

<Image title="Add CleverTap Credentials to Appsflyer Dashboard" alt={2246} align="center" width="% " border={true} src="https://files.readme.io/9bcd108-AppsFlyer_Dashboard.png">
  Add CleverTap Credentials to Appsflyer Dashboard
</Image>

> 📘 Region
>
> In the above figure, enter the region of your CleverTap account. The following table helps identify the region of your account:

| Dashboard URL                                                                                        | Region |
| :--------------------------------------------------------------------------------------------------- | :----- |
| [https://eu1.dashboard.clevertap.com/login.html#/](https://eu1.dashboard.clevertap.com/login.html#/) | EU1    |
| [https://in1.dashboard.clevertap.com/login.html#/](https://in1.dashboard.clevertap.com/login.html#/) | IN1    |
| [https://us1.dashboard.clevertap.com/login.html#](https://us1.dashboard.clevertap.com/login.html#)   | US1    |
| [https://sg1.dashboard.clevertap.com/login.html#/](https://sg1.dashboard.clevertap.com/login.html#/) | SG1    |
| [https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html)   | ID     |
| [https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html)   | mec1   |

## Integrate AppsFlyer

The integration steps vary for every app. The steps for every app are listed in the sections to follow:

### For Android App

1. To track attribution and events data in your Android app, perform the steps listed in [Appsflyer Android SDK Integration Guide](https://dev.appsflyer.com/hc/docs/integrate-android-sdk).
2. In your Android app code, add the following code.

#### *For SDK version 4.2.0 and Above*

##### Set the Customer ID

The `getCleverTapAttributionIdentifier` method is deprecated for CleverTap Android SDKs version 4.2.0 and above. Use the new `getCleverTapID` method to get the CleverTap ID on the `OnInitCleverTapIDListener` to set `CustomerUserId` method of AppsFlyer.

```java
cleverTapInstance.getCleverTapID(new OnInitCleverTapIDListener() {
   @Override
   public void onInitCleverTapID(final String cleverTapID) {
       // Callback on main thread
       appsFlyerLib.setCustomerUserId(cleverTapID);
   }   
});
```
```kotlin
cleverTapInstance?.getCleverTapID {
  // Callback on main thread
  appsFlyerLib.setCustomerUserId(it)
}
```

##### Set Additional User Data

Let's say the customer is already using `setCustomerUserId` to send the identity of the user to AppsFlyer. So now, if you want to send the CleverTap ID, you can use the `setAdditionaldata` method instead of the `setCustomerUserId`.

> 📘 Note
>
> The `setAdditionaldata` method always takes priority over `setCustomerUserId`.

```java
cleverTapInstance.getCleverTapID(new OnInitCleverTapIDListener() {
   @Override
   public void onInitCleverTapID(final String CTID) {
       // Callback on main thread
       Map<String, Object> customData = new HashMap<>();
       customData.put("CleverTapID", CTID); // Add the CleverTap ID with the key
       
       AppsFlyerLib.getInstance().setAdditionalData(customData); // Pass the map to AppsFlyer
   }
});
```
```kotlin
cleverTapInstance.getCleverTapID { CTID ->
    // Callback on main thread
    val customData = mutableMapOf("CleverTapID" to CTID) // Create the map with the CleverTap ID
    AppsFlyerLib.getInstance().setAdditionalData(customData) // Pass the map to AppsFlyer
}
```

#### For SDK Version 4.1.1 and Below

```java
String attributionID = cleverTapInstance.getCleverTapAttributionIdentifier();
appsFlyerLib.setCustomerUserId(attributionID);
```
```kotlin
appsFlyerLib.setCustomerUserId(cleverTapInstance?.cleverTapAttributionIdentifier)
```

### For iOS App

To integrate Appsflyer with CleverTap for an iOS app, add the following code to your iOS app code:

```objectivec
[CleverTap autoIntegrate];
[[AppsFlyerTracker sharedTracker] setCustomerUserID:[[CleverTap sharedInstance] profileGetCleverTapAttributionIdentifier]];
```

### For React-Native

To integrate Appsflyer with CleverTap for the react native app, add the following code to your React-Native app code:

```javascript
CleverTap.profileGetCleverTapAttributionIdentifier((err, res) => { 

const userId = res;

appsFlyer.setCustomerUserId(userId, (response) => {   

//.. });

});
```

### For Unity

To integrate Appsflyer with CleverTap for the unity app, add the following code to your Unity app code:

```csharp
string CleverTapID = CleverTapBinding.ProfileGetCleverTapID();
AppsFlyer.setCustomerUserId(CleverTapID);
```

### For Cordova

Add the following code to your Cordova app code:

```javascript
CleverTap.getCleverTapID(function(clevertapId) {
    window.plugins.appsFlyer.setAppUserId(clevertapId);
});
```

### For Flutter

To integrate Appsflyer with CleverTap for the Flutter app, add the following code to your Flutter app code:

##### Set the Customer ID

The `getCleverTapAttributionIdentifier` method is deprecated for CleverTap Android SDKs version 4.2.0 and above. Use the new `getCleverTapID` method to get the CleverTap ID on the `OnInitCleverTapIDListener` to set `CustomerUserId` method of AppsFlyer.

```d Dart
CleverTapPlugin.getCleverTapID().then((clevertapId) {
  appsFlyerSdk.setCustomerUserId(clevertapId);
});
```

##### Set Additional User Data

Let's say the customer is already using `setCustomerUserId` to send the identity of the user to AppsFlyer. So now, if you want to send the CleverTap ID, you can use the `setAdditionaldata` method instead of the `setCustomerUserId`.

> 📘 Note
>
> The `setAdditionaldata` method always takes priority over `setCustomerUserId`.

```c Dart
CleverTapPlugin.getCleverTapID().then((clevertapId) {
    final customData = {'CleverTapID': clevertapId}; // Create a map with the CleverTap ID
    appsFlyerSdk.setAdditionalData(customData); // Pass the map to setAdditionalData
});
```

### Delay SDK Initialization for customerUserID

To set up delay SDK initialization for `customerUserID` in Android and iOS apps, we strongly recommend setting the customer user ID early in the app's flow. For more information about the setup, refer to [Delay SDK initialization until CUID is set](https://support.appsflyer.com/hc/en-us/articles/207032016-Customer-User-ID-field-CUID-#android-sdk-developer-instructions) section.

## Setup for Organic Install and Custom Events

After successful integration, Appsflyer starts pushing inorganic install events data to CleverTap. To receive in-app events (custom events) in CleverTap from Appsflyer, proceed as follows: 

1. Navigate to *Settings* > *Partners* and scroll down to the *Attribution partners* section.

<Image title="CleverTap Attribution Partners List" alt={2868} align="center" border={true} src="https://files.readme.io/f56bdb4-Navigate_to_Partners_page.png">
  CleverTap Attribution Partners
</Image>

2. Click the ![](https://files.readme.io/1e348ca-Edit_icon.png) icon against the *Appsflyer* partner.\
   On clicking, *Attribution partner - Appsflyer* popup appears on the right side of the screen.

<Image title="Attribution Partner - AppsFlyer" alt={2384} align="center" border={true} src="https://files.readme.io/48681e7-Attribution_partner_-_AppsFlyer.png">
  Attribution Partner - Appsflyer
</Image>

3. Select the required option from the options below. Selecting any of these options indicates that the CleverTap accepts event data when shared by the partner.
   * Custom events
   * Organic install events

> 🚧 Duplication of Events
>
> If you select *Organic install events* for more than one attribution partner, a warning for duplication of events displays, as shown in the following figure. We strongly recommend tracking the organic install events from only one attribution partner.

<Image title="Duplication of Events" alt={1122} align="center" width="70% " border={true} src="https://files.readme.io/72b3f33-Duplication_of_events.png">
  Duplication of Events
</Image>

4. Click **Save**. On clicking, the following message displays at the top of the screen: *Changes saved. Admins are notified of this via email*. 

# Viewing Data in Dashboard

You can now view the event data on the CleverTap dashboard. To do so, proceed as follows:

1. From the CleverTap dashboard, go to *Analytics > Events*.
2. Apply the required filters for the selected event. The filters vary depending on the type of event.

* **For Install Events**: All the install events are tracked under the *UTM Visited* event.

<Image title="Install Events Filter" alt={1688} align="center" border={true} src="https://files.readme.io/9878672-Events_Filters_AF.png">
  Install Events Filter
</Image>

* **For Custom Events**: In-app revenue events received from Appsflyer are prefixed with **AF** in CleverTap.
