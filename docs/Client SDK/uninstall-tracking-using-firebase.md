---
title: Uninstall Tracking using Firebase
excerpt: Learn how to set up uninstall tracking using Firebase-based implementation.
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

There may be multiple reasons why a user might uninstall an application, such as poor user experience, technical faults, or app performance. Tracking app uninstalls can provide powerful insights into user churn and help devise ways to increase retention.

<Callout icon="📘" theme="info">
  **For Projects Using Google Analytics (GA4) or Cloud Functions v2**

  If your project uses Google Analytics (GA4) Key Events or Cloud Functions v2, this implementation does not apply. Recent updates from Google to Analytics, Events, and Cloud Functions require a revised configuration for real-time uninstall tracking.

  For more information about implementation, refer to [Uninstall Tracking Using Google Analytics(G4)](doc:uninstall-tracking-google-analytics#/) .

  For more information about supported regions and versions, refer to [Cloud Function Locations](https://firebase.google.com/docs/functions/locations#tier_1_pricing.).
</Callout>

# Track Uninstalls

CleverTap tracks app uninstalls in three ways:

* _Silent Push Notification_: Detects uninstalls by sending a silent push notification daily.
* _Real-time_: Detects the uninstall event as soon as a user removes the app. This method is available only for Android devices.
* _Push Notification Campaign_: Detects uninstalls when a push notification cannot be delivered due to an invalid or expired device token. For more information, refer to [Push Campaign](https://staging.docs.user.clevertap.net/docs/create-message-push).

<Callout icon="🚧" theme="warn">
  **Temporary Spike in Uninstall Tracking Numbers**

  CleverTap has upgraded the uninstall tracking system to provide deeper insights into user behavior, now including tracking for unsubscribed devices. Starting February 22, 2024, this enhancement may lead to a temporary spike in uninstall numbers. If you have any queries, contact your Customer Success Manager.
</Callout>

## Silent Push Notifications (For iOS & Android)

A silent notification is an effective mechanism to check token validity. An invalid token usually indicates that the app has been uninstalled. CleverTap uses silent push notifications to track application uninstalls for both, _Android_ and _iOS_ applications. A silent push notification contains an empty message that is sent to the user's device via Firebase Cloud Messaging (FCM) server for Android devices or Apple Push Notification service (APNs) for iOS devices.

The [Uninstall](https://docs.clevertap.com/docs/uninstalls-view) board helps you monitor your app uninstalls and take timely action.

<Callout icon="🚧" theme="warn">
  **Change in App Uninstall Tracking for Android Devices**

  Starting May 15, 2024, FCM is implementing changes regarding [stale tokens](https://firebase.google.com/docs/cloud-messaging/manage-tokens#stale-and-expired-tokens) for devices not connected to FCM in over 270 days. Such tokens are considered expired and invalid. Consequently, any push notification request sent to stale tokens will fail and return a 404 error (Unregistered).

  **Whom does this change impact?**

  The change solely affects customers who rely on the Silent Push Notification method for uninstall tracking.

  **Impact of this implementation**

  With this change, FCM will return a 404 error (Unregistered) from inactive apps after 270 days. Thus, CleverTap will stop marking such devices as uninstalled because this may incorrectly identify inactive users as churned, thereby inflating uninstall numbers.

  To address this, CleverTap strongly recommends implementing CleverTap's Real-Time Uninstall Tracking capability, which leverages Firebase Analytics. If you fail to implement Real-Time Uninstall Tracking by May 15, 2024, Uninstall Tracking through Silent Push Notifications will be discontinued. However, the _Push Unregistered_ event will be raised whenever a silent push notification is sent. You can view the Push Unregistered event count from the CleverTap dashboard by applying a filter where source = `CT_Push`.

  If you have already implemented Real-Time Uninstall Tracking, this change will not affect you. For any queries or further assistance, raise a support ticket from the CleverTap Dashboard or contact your Customer Success Manager.
</Callout>

### Enable Silent Push Notifications

Enable Silent Push notifications in CleverTap by performing the following steps:

1. From the CleverTap dashboard, go to _Settings_ > _Uninstall Tracking_.
2. Toggle ON the _Silent token-based push notification_ option.

<Image align="center" alt={800} border={true} caption="Enable Silent Push Notifications" title="Enable Silent Push Notifications" src="https://files.readme.io/1f8a490-uninstall_tracking_silent_push.png" />

## Real-Time Uninstall Tracking (For Android)

Verify that the Android app is integrated with _Firebase Analytics_ for real-time uninstall tracking. Add Firebase Analytics dependency in the app SDK.

When a user uninstalls the app from an Android device, Firebase logs the `app_remove` event. You can send this event to CleverTap using Firebase Cloud Functions.

<Callout icon="📘" theme="info">
  **Uninstall Tracking Process in Google Analytics**

  Google has introduced changes to Google Analytics (G4), Firebase Events, and Cloud Functions that impact real-time uninstall tracking.

  If you are setting up real-time uninstall tracking in v2 or updating an existing integration, refer to [Uninstall Tracking Using Google Analytics](doc:uninstall-tracking-using-google-analytics). This document includes the updated GA4 Key Event flow, revised Cloud Function script, region settings, and Node.js version requirements.
</Callout>

## Implement Real-Time Uninstall Tracking

Real-time uninstall tracking is a four-step process :

1. [Set up a common identifier in your app](https://developer.clevertap.com/docs/uninstall-tracking#section-set-up-a-common-identifier).
2. [Set the `app_remove` event as a conversion event in Firebase](https://developer.clevertap.com/docs/uninstall-tracking#section-set-up-conversion-event-using-firebase).
3. [Use the Firebase cloud function to send uninstall information to CleverTap](https://developer.clevertap.com/docs/uninstall-tracking#section-create-a-cloud-function).
4. [Enable the real-time settings in CleverTap](https://developer.clevertap.com/docs/uninstall-tracking#section-enable-real-time-uninstall-tracking-in-clever-tap).

## Set Up Common Identifier in App

Add the following code to your app to set up a common identifier between Firebase and CleverTap.

```java
private FirebaseAnalytics mFirebaseAnalytics;
mFirebaseAnalytics = FirebaseAnalytics.getInstance(this);
mFirebaseAnalytics.setUserProperty("ct_objectId",Objects.requireNonNull(CleverTapAPI.getDefaultInstance(this)).getCleverTapID());
```
```kotlin
val defaultInstance = CleverTapAPI.getDefaultInstance(this)
defaultInstance?.let { ins ->
    Log.i(TAG, "setting object id to firebase : ${ins.cleverTapID}")
    FirebaseAnalytics.getInstance(this).setUserProperty("ct_objectId", ins.cleverTapID)
} ?: run {
    Log.e(TAG, "Uninstall tracking not setup cause of non initialised instance")
}
```

## Set Up Conversion Event Using Firebase

To enable real-time uninstall tracking, ensure that the `app_remove` event is marked as a conversion event in the Firebase dashboard.

The `app_remove` event is automatically logged by Firebase Analytics. It is triggered when an Android app is uninstalled from a device, regardless of the installation source. This event is available only for Android devices.

<Callout icon="🚧" theme="warn">
  **Firebase Blaze Plan Required**

  To allow Firebase Cloud Functions to call a third-party HTTP endpoint (such as the CleverTap API upload endpoint), your Firebase project must be on the Blaze plan. For more information about plans and pricing, refer to [Firebase Pricing](https://firebase.google.com/pricing).

  Additionally, ensure that the [API region](https://developer.clevertap.com/docs/idc#section-configuration-steps-to-enable-clever-tap-region) is configured correctly.
</Callout>

To set up the conversion event, perform the following steps:

1. Select the Firebase project that is integrated with the Android app.
2. From the Firebase dashboard, select _Analytics_ > _Events_.
3. Toggle ON the _Mark as conversion_ option for the _app_remove_ event.

<Image align="center" alt={1143} border={true} caption="Enable _Mark as conversion_ option  for app_remove Event" title="Enable the _Mark as conversion_ Toggle" src="https://files.readme.io/1ef2441-2020-09-28_14-58-47_app_remove.png" />

## Create Cloud Function

After the conversion is set up, use the [Cloud Function for Firebase](https://firebase.google.com/docs/functions/get-started) to create a function and send the `app_remove` data to CleverTap.

To create and publish a cloud function using Node.js, perform the following steps:

1. Open a terminal and set up Node.js and the Firebase CLI.
2. Run `npm install -g firebase-tools`.
3. To initialize Firebase SDK for Cloud Functions, run the `firebase login`.
4. From your Firebase project directory, run `firebase init functions`.
5. Select the option to use an existing project.
6. Open `index.js` and add the following code:

```javascript
'use strict';

const functions = require('firebase-functions');
const admin = require('firebase-admin');
const https = require('https');
var request = require('requestretry');


admin.initializeApp();

exports.sendAndroidUninstallToCleverTap = functions.analytics.event('app_remove').onLog((event) => {

    //console.log("Event is: " + JSON.stringify(event));

    function myRetryStrategy(err, response, body, options) {
        // retry the request if we had an error or if the response was a 'Bad Gateway'
        return !!err || response.statusCode === 503;
    }

    var clevertapId = event.user.userProperties.ct_objectId.value;
    // This is where the CleverTap ID of the user who uninstalled the app is passed as an identifier in the API call.
    const data = JSON.stringify({
       "d": [{
            "objectId": clevertapId,
            "type": "event",
            "evtName" : "App Uninstalled",
            "evtData": {
            }
        }]
    });

    request({
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'X-CleverTap-Account-Id': '<CT_ACCOUNT_ID>',
            'X-CleverTap-Passcode': '<CT_ACCOUNT_PASSCODE>'
        },
        body: data,
        url: 'https://in1.api.clevertap.com/firebase/upload',//Add your account region in the url

        // The below parameters are specific to request-retry
        maxAttempts: 5, // (default) try 5 times
        retryDelay: 2000, // (default) wait for 2s before trying again
        retryStrategy: myRetryStrategy // (default) retry on 5xx or network errors
    }, function (err, response, body) {
        // this callback will only be called when the request succeeded or after maxAttempts or on error
        if (response && response.statusCode === 200) {
            console.log("Response Body: " + JSON.stringify(body));
            console.log('The number of request attempts: ' + response.attempts);
            return 0;
        }else{
            console.log("err: " + err + " ,response: " + JSON.stringify(response) + " , body: " + JSON.stringify(body));
            return 1;
        }
    });


});


// // Create and Deploy Your First Cloud Functions
// // https://firebase.google.com/docs/functions/write-firebase-functions
//
// exports.helloWorld = functions.https.onRequest((request, response) => {
//  response.send("Hello from Firebase!");
// });
```

8. Replace `<CT_ACCOUNT_ID>` and `<CT_ACCOUNT_PASSCODE>` with your CleverTap ID and Passcode in the `index.js` file. You can find these values in your dashboard from _Settings_ > _Project_.
9. Open `package.json` and add the following code:

```json
{
  "name": "functions",
  "description": "Cloud Functions for Firebase",
  "scripts": {
    "serve": "firebase emulators:start --only functions",
    "shell": "firebase functions:shell",
    "start": "npm run shell",
    "deploy": "firebase deploy --only functions",
    "logs": "firebase functions:log"
  },
  "engines": {
    "node": "20"
  },
  "main": "index.js",
  "dependencies": {
    "firebase-admin": "^11.8.0",
    "firebase-functions": "^4.3.1",
    "web-push": "^3.4.4",
    "request": "^2.88.2",
    "requestretry": "^4.1.1"
  },
  "devDependencies": {
    "firebase-functions-test": "^3.1.0"
  },
  "private": true
}
```

10. Run the `firebase deploy --only functions` command to deploy the cloud function for uninstall event listening.

## Enable Real-Time Uninstall Tracking in CleverTap

After you have configured the conversion setup and created the cloud function, enable real-time uninstall tracking in CleverTap by performing the following steps:

1. From the CleverTap dashboard, select _Settings_ > _Uninstall Tracking_.
2. Toggle ON the _Android (Real-time tracking)_ option.

<Image align="center" alt={1486} border={true} caption="Select Android (Real-time tracking)" title="Select Android (Real-time tracking)" src="https://files.readme.io/b919669-2020-09-28_15-35-19_inabling_tracking_in_CT.png" />

When you enable Silent Push notifications, CleverTap starts collecting a silent push token for each device. This token allows CleverTap to deliver background notifications that are not visible to the user. After the setting is turned on, devices become eligible to receive silent pushes from CleverTap.

# FAQs

### Can I run triggered campaigns on the _App Uninstalled_ event?

Yes. You can set up live-triggered campaigns for the _App Uninstalled_ event. Only those users who are tracked via the real-time uninstall will qualify for the campaign.

### How do I target users who have uninstalled the app in a campaign?

Real-time uninstalls can be targeted in a campaign by using the event property filter _source = REALTIME_FIREBASE_.

<Image align="center" alt={1752} border={false} caption="App Uninstalled Event" title="App Uninstalled Event" src="https://files.readme.io/db9dacb-Screenshot_2021-06-10_at_12.18.48_PM.png" />

### Can I turn off uninstall tracking for push notifications sent via a Campaign or a Journey?

No. This is tracked by default. For more information, refer to [Tracking Uninstalls Effectively](https://clevertap.com/blog/track-app-uninstalls-effectively).
