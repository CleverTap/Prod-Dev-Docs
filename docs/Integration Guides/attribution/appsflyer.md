---
title: AppsFlyer
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
CleverTap helps to track your app installs via AppsFlyer. This integration requires changes to both your Android and iOS app for a successful integration:

# Step 1: Add CleverTap Credentials to AppsFlyer Dashboard

On your AppsFlyer dashboard, select *Integrated Partners* and search for CleverTap. 

Enter your *Account Details* such as CleverTap *Account ID,* *Account Passcode*, and *Account Token*(available in the CleverTap Settings dashboard). 

![642](https://files.readme.io/02e5c32-Screen-Shot-2017-04-07-at-3.26.40-PM.png "Screen-Shot-2017-04-07-at-3.26.40-PM.png")

Check Enable and Save. All AppsFlyer parameters will now be sent to CleverTap.

# Step 2: Integration

**Android App**\
In your Android app, open the file where you have added your AppsFlyer integration. Add the following code below it.

If you haven't already integrated AppsFlyer, you will first need to follow [this guide](https://support.appsflyer.com/hc/en-us/articles/207032126-AppsFlyer-SDK-Integration-Android).

```java
String attributionID = cleverTapInstance.getCleverTapAttributionIdentifier();
appsFlyerLib.setCustomerUserId(attributionID);
```

**iOS App**\
In your iOS app code, add the following code:

```objectivec
[CleverTap autoIntegrate];
[[AppsFlyerTracker sharedTracker] setCustomerUserID:[[CleverTap sharedInstance] profileGetCleverTapAttributionIdentifier]];
```

**React-Native App**\
In your React-Native App, add the following code:

```javascript
CleverTap.profileGetCleverTapAttributionIdentifier((err, res) => { 

const userId = res;

appsFlyer.setCustomerUserId(userId, (response) => {   

//.. });

});
```

# Step 3: View Data in Dashboard

After you integrate, AppsFlyer will push data into your CleverTap dashboard. You can view it under event UTM Visited filtered by event property, UTM\_source, UTM\_medium or UTM\_campaign.

> 📘 Note:
>
> CleverTap accepts only non-organic installs from Appsflyer.
