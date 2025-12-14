---
title: Uninstall Tracking
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
# Overview

There may be multiple reasons why a user might uninstall an application, such as poor user experience, technical faults, or app performance. Tracking app uninstalls can provide powerful insights into user churn and help devise ways to increase retention. 

# Track Uninstalls

CleverTap tracks app uninstalls in three ways:

* Silent Push Notification - By sending a silent push notification daily
* Real-time - As soon as a user uninstalls the app. This real-time tracking is available only for Android devices.
* Push Notification Campaign - When a push notification cannot be delivered due to an invalid/expired token.

> 🚧 Temporary Spike in Uninstall Tracking Numbers
>
> CleverTap has upgraded the uninstall tracking system to provide deeper insights into user behavior, now including tracking for unsubscribed devices. Starting February 22, 2024, this enhancement may lead to a temporary spike in uninstall numbers. If you have any queries, contact your Customer Success Manager.

## Silent Push Notifications (iOS & Android)

 A silent notification is a simple yet effective mechanism to check token validity. An invalid token usually indicates that the app has been uninstalled. CleverTap uses silent push notifications to track application uninstalls for both, *Android* and *iOS* applications. A silent push notification contains an empty message that is sent to the user's device via the Firebase Cloud Messaging (FCM) server for Android devices or Apple Push Notification service (APNs) for iOS devices. 

The [Uninstall](https://docs.clevertap.com/docs/uninstalls-view) board helps you monitor your app uninstalls and take timely action. 

> 🚧 Change in App Uninstall Tracking For Android Devices
>
> Starting May 15, 2024, FCM is implementing changes regarding [stale tokens](https://firebase.google.com/docs/cloud-messaging/manage-tokens#stale-and-expired-tokens) for devices not connected to FCM in over 270 days. Such tokens are considered expired and invalid. Consequently, any push notification request sent to stale tokens will fail and return a 404 error (Unregistered).
>
> **Whom does this change impact?**
>
> The change solely impacts customers relying on the Silent Push Notification method for uninstall tracking.
>
> **Impact of this implementation**
>
> With this change, FCM will return a 404 error (Unregistered) from inactive apps after 270 days. Thus, CleverTap will stop marking such devices as uninstalled because this may incorrectly identify inactive users as churned, thereby inflating uninstall numbers.
>
> To address this, CleverTap strongly recommends implementing CleverTap’s Real-Time Uninstall Tracking capability, which leverages Firebase Analytics. If you fail to implement Real-Time Uninstall Tracking by May 15, 2024, Uninstall Tracking through Silent Push Notifications will be discontinued. However, the Push Unregistered event will be raised whenever a silent push notification is sent. You can view the Push Unregistered event count from the CleverTap dashboard by applying a filter where source = `CT_Push`.
>
> If you have already implemented Real-Time Unistall Tracking, then this change does not impact you. For any queries or further assistance, raise a support ticket from the CleverTap Dashboard or contact your Customer Success Manager.

### Enable Silent Push Notifications

Enable Silent Push notifications in CleverTap by performing the following steps:

1. From the CleverTap dashboard, select *Settings* > *Uninstall Tracking*.
2. Toggle to enable *Silent token-based push notification*.

<Image title="Enable Silent Push Notifications" alt={800} align="center" border={true} src="https://files.readme.io/1f8a490-uninstall_tracking_silent_push.png">
  Enable Silent Push Notifications
</Image>

## Real-Time Uninstall Tracking (Android)

Check that the Android app is integrated with *Firebase Analytics* for real-time uninstall tracking. Add Firebase Analytics dependency in the app SDK. For more information, refer to[ Get Started with Google Analytics](https://firebase.google.com/docs/analytics/get-started?platform=android).

When a user uninstalls an app from their Android device, a Firebase event called  `app_remove`   tracks this app uninstall. You can push this event to CleverTap using the Firebase cloud functions.

## Implement Real-Time Uninstall Tracking

Real-time uninstall tracking is a four-step process :

1. [Set up a common identifier in your app](https://developer.clevertap.com/docs/uninstall-tracking#section-set-up-a-common-identifier)
2. [Set the `app_remove` event as a conversion event in Firebase](https://developer.clevertap.com/docs/uninstall-tracking#section-set-up-conversion-event-using-firebase)
3. [Use the Firebase cloud function to send uninstall information to CleverTap](https://developer.clevertap.com/docs/uninstall-tracking#section-create-a-cloud-function)
4. [Enable the real-time settings in CleverTap](https://developer.clevertap.com/docs/uninstall-tracking#section-enable-real-time-uninstall-tracking-in-clever-tap)

Discover the video tutorial for implementing Real-Time Uninstall Tracking in CleverTap.

<HTMLBlock>{`
<div
              style="
                position: relative;
                padding-bottom: 56.25%;
                height: 0;
                border-radius: 0;
                box-shadow: 0 15px 40px rgba(63,58,79,.3);
                overflow: hidden;
                min-width:320px"><iframe
              src="https://clevertap.portal.trainn.co/share/UaV9YLInv7BlxRMRs6wevA/embed?autoplay=false"
              frameborder="0"
              webkitallowfullscreen
              mozallowfullscreen
              allowfullscreen
              allow="autoplay; fullscreen"
              style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe></div>
`}</HTMLBlock>

## Set Up a Common Identifier

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

To set up real-time uninstall, check that a conversion event is set up on the Firebase dashboard.\
Firebase analytics track an event called `app_remove`, which is one of the automatically collected Firebase events. The `app_remove` event is an Android-only event that is tracked when an application package is removed or uninstalled from the device, irrespective of the installation source.

> 🚧 Firebase Blaze plan Account Required
>
> To allow Firebase cloud functions to call a third-party HTTP endpoint (CleverTap API upload endpoint), a Firebase account with a Blaze plan is required. 
>
> See [Firebase Pricing](https://firebase.google.com/pricing) for more information on pricing plans. Also, check that the [API region](https://developer.clevertap.com/docs/idc#section-configuration-steps-to-enable-clever-tap-region) is configured correctly.

To set up conversion, perform the following steps:

1. Select the Firebase project that is integrated with the Android app.
2. From the Firebase dashboard, select *Analytics* > *Events*.
3. Enable the *Mark as conversion* toggle for *app\_remove*.

<Image title="Enable the _Mark as conversion_ Toggle" alt={1143} align="center" border={true} src="https://files.readme.io/1ef2441-2020-09-28_14-58-47_app_remove.png">
  Enable the *Mark as conversion* Toggle
</Image>

## Create a Cloud Function

After the conversion is set up, use the [Cloud Function for Firebase](https://firebase.google.com/docs/functions/get-started) to create a function and send the `app_remove` data to CleverTap. 

To create and publish a cloud function using Node JS, perform the following steps:

1. Open a terminal.
2. Set up Node.js and the Firebase CLI. 
3. Run `npm install -g firebase-tools`.
4. To initialize Firebase SDK for Cloud Functions, run `firebase login`.
5. From your Firebase project directory, run `firebase init functions`.
6. Select the option to use an existing project. 
7. Open `index.js` and add the following code:

> 📘 Note
>
> Check that the region of your CleverTap account is included as a prefix in the Cloud Function URL. For example, if your acount is in the *in1* region, the endpoint must be `https://in1.api.clevertap.com/firebase/upload`. For more information, refer to our [Configuration Guide for CleverTap Regions](https://developer.clevertap.com/docs/idc).

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
        url: 'https://api.clevertap.com/firebase/upload',
//include dashboard region as a prefix in the URL <https://in1.api.clevertap.com/firebase/upload>

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

8. Replace `<CT_ACCOUNT_ID>` and `<CT_ACCOUNT_PASSCODE>` with your CleverTap ID and Passcode in the `index.js` file. You can find these values in your dashboard.
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
    "node": "18"
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

10. To deploy the cloud function for uninstall event listening, run the `firebase deploy --only functions` command.

> 📘 Note
>
> For more information about deploying the Firebase Cloud Functions, refer to [Firebase Cloud Functions](https://firebase.google.com/docs/functions/get-started#deploy-functions-to-a-production-environment) document.

## Enable Real-Time Uninstall Tracking in CleverTap

After you have configured the conversion set up and created the cloud function, enable real-time uninstall tracking in CleverTap by performing the following steps:

1. From the CleverTap dashboard, select *Settings* > *Uninstall Tracking*.
2. Select the toggle for *Android (Real-time tracking)*.

<Image title="Select Android (Real-time tracking)" alt={1486} align="center" border={true} src="https://files.readme.io/b919669-2020-09-28_15-35-19_inabling_tracking_in_CT.png">
  Select Android (Real-time tracking)
</Image>

# FAQs and Troubleshooting

### Q1. Can I run triggered campaigns on the App uninstalled event?

Yes. You can set up live triggered campaigns for the *App Uninstalled* event. Only those users who are tracked via the real-time uninstall will qualify for the campaign. 

### Q2. How do I target users who have uninstalled the app in a campaign?

Real-time uninstalls can be targeted in a campaign by using the event property filter *source = REALTIME\_FIREBASE*.

<Image title="App Uninstalled Event" alt={1752} align="center" src="https://files.readme.io/db9dacb-Screenshot_2021-06-10_at_12.18.48_PM.png">
  App Uninstalled Event
</Image>

### Q3. Can I turn off uninstall tracking for push notification sent via a Campaign or a Journey?

No. This is tracked by default. For more information, see [Tracking Uninstalls Effectively](https://clevertap.com/blog/track-app-uninstalls-effectively).
