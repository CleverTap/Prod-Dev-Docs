---
title: Adjust
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

[Adjust](https://www.adjust.com/company/) is an analytics platform that provides marketers with attribution information to derive business intelligence and grow their mobile apps. This document explains how to integrate Adjust with CleverTap. By setting up this integration, you can track the following data from Adjust:

- **Install events**: Organic and inorganic installs.
- **Custom data**: Any In-App events or revenue tracked in Adjust, including **In-App revenue**, **In-App events**, and other custom events.

To learn more about these events and their default attribution settings in the CleverTap dashboard, refer to [Types of Data](https://docs.clevertap.com/docs/attribution#types-of-data).

# Integrate Adjust

To enable Adjust integration with the CleverTap dashboard, complete the following steps:

1. Add CleverTap credentials in the Adjust dashboard. Refer to [Setup CleverTap](https://help.adjust.com/en/integrated-partners/clevertap#basic-setup) in Adjust documentation.
2. [Configure CleverTap ID in Adjust SDK](doc:adjust#configure-clevertap-id-in-adjust-sdk).
3. [Set up for Organic Install and Custom Events](doc:adjust#set-up-for-organic-install-and-custom-events).

## Configure CleverTap ID in Adjust SDK

Adjust requires the `clevertapId` to be configured as a **global partner parameter**.

> ❗️ Warning
> 
> Events without the `clevertapId` parameter are not processed in CleverTap.

You must set up the parameter differently depending on your platform. Refer to the relevant guide below:

- [Android App](doc:adjust#android-app)
- [iOS App](doc:adjust#ios-app)
- [React Native App](doc:adjust#react-native-app)
- [Flutter App](doc:adjust#flutter-app)
- [Other Platforms](doc:adjust#other-platforms)

### Android App

To track attribution and event data in your Android app, first integrate the [Adjust Android SDK](https://help.adjust.com/en/article/get-started-android-sdk). Then, add the `clevertapId` as a [global partner parameter](https://dev.adjust.com/en/sdk/android/features/session-parameters/#global-callback-parameters).

#### For CleverTap SDK version 4.2.0 and above

The method `getCleverTapAttributionIdentifier` is deprecated. Use `getCleverTapID` with the `OnInitCleverTapIDListener` callback:

The `getCleverTapAttributionIdentifier` method is deprecated for CleverTap Android SDKs version 4.2.0 and above. Use the new `getCleverTapID` method to get the CleverTap ID on the `OnInitCleverTapIDListener` to set `CustomerUserId` method of Adjust.

```java
cleverTapInstance.getCleverTapID(new OnInitCleverTapIDListener() {
   @Override
   public void onInitCleverTapID(final String cleverTapID) {
       // Callback runs on main thread
       Adjust.addGlobalPartnerParameter("clevertapId", cleverTapID);
   }   
});
```

#### For CleverTap SDK version 4.1.1 and below

If you are using an older CleverTap SDK 4.1.1 and below, you can still retrieve the CleverTap attribution identifier and pass it to Adjust:

```java
String attributionID = cleverTapInstance.getCleverTapAttributionIdentifier();
Adjust.addGlobalPartnerParameter("clevertapId", attributionID);
```

### iOS App

To track attribution and event data in your iOS app, integrate the [Adjust iOS SDK](https://help.adjust.com/en/article/get-started-ios-sdk). Then, update your app delegate to include Adjust and set the `clevertapId`. For more details about partner parameters, refer to [Adjust iOS partner parameters](https://dev.adjust.com/en/sdk/ios/features/session-parameters/#session-partner-parameters).

Update the `application:didFinishLaunchingWithOptions:` method:

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    NSString *yourAppToken = @"YOUR_ADJUST_APP_TOKEN";
    NSString *environment = ADJEnvironmentProduction;

    ADJConfig *adjustConfig = [ADJConfig configWithAppToken:yourAppToken
                                                environment:environment];
    [adjustConfig setDelegate:self];
    [Adjust appDidLaunch:adjustConfig];

    return YES;
}
```

Add the CleverTap ID as a global partner parameter:

```objectivec
[CleverTap autoIntegrate];
[Adjust addGlobalPartnerParameter:@"clevertapId"
                            value:[[CleverTap sharedInstance] profileGetCleverTapAttributionIdentifier]];
```

### React Native App

If you are using React Native, initialize both Adjust and CleverTap SDKs. Then, pass the CleverTap ID to Adjust as a global partner parameter.

```javascript
CleverTap.getCleverTapID((err, res) => {
    if (!err) {
        Adjust.addGlobalPartnerParameter("clevertapId", res);
    }
});
```

### Flutter App

For Flutter apps, use the [Adjust Flutter SDK](https://pub.dev/packages/adjust_sdk/versions/5.1.1) with the [CleverTap Flutter SDK](https://pub.dev/packages/clevertap_plugin/versions/3.3.1). After both SDKs are initialized, set the `clevertapId`:

```dart Dart
import 'package:adjust_sdk/adjust.dart';
import 'package:clevertap_plugin/clevertap_plugin.dart';

void setCleverTapIdWithAdjust() async {
  String? cleverTapId = await CleverTapPlugin.getCleverTapID();
  if (cleverTapId != null) {
    Adjust.addGlobalPartnerParameter("clevertapId", cleverTapId);
  }
}
```

### Other Platforms

If you are integrating Adjust with CleverTap using Cordova or Unity, refer to Adjust’s migration guides for the correct partner parameter setup:

- [Cordova migration guide](https://dev.adjust.com/en/sdk/migration/cordova/v4-to-v5)
- [Unity migration guide](https://dev.adjust.com/en/sdk/migration/unity/v4-to-v5)

Both platforms also require using `addGlobalPartnerParameter` instead of `addSessionPartnerParameter`.

## Set up for Organic Install and Custom Events

After completing the SDK integration, Adjust automatically starts sending inorganic install data to CleverTap. To also receive organic installs, in-app revenue, and custom events:

1. In the CleverTap dashboard, go to _Settings_ > _Partners_ > _Integration_.
2. Select **Adjust**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/268770b656d3cdd31764759845c0c1335c1f23203667e0e03d57820cfdeb3988-2025-09-30_00-45-58_1.gif",
        "",
        "Attribution Partner-Adjust"
      ],
      "align": "center",
      "sizing": "85% ",
      "border": true,
      "caption": "Attribution Partner-Adjust"
    }
  ]
}
[/block]


3. Select the required option from the following available options. Selecting any of these options indicates that the CleverTap accepts event data when shared by the partner.
   - Custom events
   - Organic install events

> ⚠️ Duplication of Events
> 
> If you select _Organic install events_ for more than one attribution partner, a warning for duplication of events displays, as shown in the following image. We strongly recommend tracking the _Organic install events_ from only one attribution partner.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/922eff5483d7844ba15f0272171f00b0c7579adb2e1d5e568ca0fba3a42b2c91-image.png",
        null,
        "Duplication of Events"
      ],
      "align": "center",
      "sizing": "45% ",
      "border": true,
      "caption": "Duplication of Events"
    }
  ]
}
[/block]


4. Click **Save**. A confirmation message appears at the top of the screen: _Changes saved. Admins are notified by email_.

# Viewing Data in the Dashboard

Once the integration is complete, you can view Adjust event data in the CleverTap dashboard. To do so, proceed as follows:

1. From the CleverTap dashboard, go to _Analytics_ > _Events_.
2. Apply the required filters for the selected event. The filters vary depending on the type of event.
   - **For Install Events**: All the install events are tracked under the _UTM Visited_ event.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/191de94f2ccff378f411c9caebb4b3ae2f4e8d20f8f2bea987e1657f5b368fca-image.png",
        null,
        "Apply Event Filters"
      ],
      "align": "center",
      "border": true,
      "caption": "Apply Event Filters"
    }
  ]
}
[/block]


To further filter events by organic or inorganic events, you can use the event property `is_organic`, where `is_organic` = `1` indicates that the event is an inorganic install event and `is_organic` = `0` indicates that the event is an organic install event.

- **For Custom Events**: In-App revenue, Custom data, and In-App events received from Adjust are prefixed with **AD** in CleverTap.