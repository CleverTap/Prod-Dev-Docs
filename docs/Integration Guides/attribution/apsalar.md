---
title: Apsalar
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
CleverTap helps you to track your app installs via Apsalar. This integration requires changes to both your Android and iOS app for a successful integration.

# Step 1: Integration

CleverTap assigns each User Profile a unique identifier by default. This identity value should be pushed as an event attribute along with the event that gets raised to Apsalar whenever an install is detected. The name of the event property needs to be ‘CleverTapId’.

If you do not already have an event that is raised on install, we recommend you use the event name as ‘CleverTapInstallEvent’

**Android App**

You will have to raise an event with Apsalar, which would look like the code below.

```java
String cleverTapId = CleverTapAPI.getInstance(getApplicationContext()).getCleverTapID();
Apsalar.event("CleverTapInstallEvent", "CleverTapId", cleverTapId);
```

**iOS App**

You will have to raise an event with Apsalar, which would look like the code below.

```objectivec
NSString *cleverTapId = [[CleverTap sharedInstance] profileGetCleverTapID];
[Apsalar eventWithArgs:@"CleverTapInstallEvent",
   @"CleverTapId", cleverTapId, nil];
```

# Step 2: Contact Apsalar

You will have to reach out to [support@apsalar.com](mailto:support@apsalar.com) with the following details:

* The event name you are using to detect installs.
* The name of the event attribute (‘CleverTapId’) that contains the CleverTap identity for that user
* Your CleverTap Account ID
* Your CleverTap Passcode
* Your CleverTap Token

# Step 3: View Data in Dashboard

After you integrate, CleverTap will now pull in data from Apsalar in your CleverTap dashboard. You can view it under the event UTM Visited filtered by event property, UTM\_source, UTM\_medium or UTM\_campaign.
