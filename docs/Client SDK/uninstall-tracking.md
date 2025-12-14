---
title: Uninstall Tracking
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
## Overview

There may be multiple reasons why a user might uninstall an application, such as poor user experience, technical faults, or app performance. Tracking app uninstalls can provide powerful insights into user churn and help devise ways to increase retention. 

# Track Uninstalls

CleverTap tracks app uninstalls in three ways:

* Silent Push Notification - By sending a silent push notification daily
* Real-time - As soon as a user uninstalls the app. This real-time tracking is available only for Android devices.
* Push Notification Campaign - When a push notification cannot be delivered due to an invalid/expired token.

## Silent Push Notifications (iOS & Android)

 A silent notification is a simple, yet effective, mechanism to check token validity. An invalid token usually indicates that the app has been uninstalled. CleverTap uses silent push notifications to track application uninstalls for both, *Android* and *iOS* applications. A silent push notification contains an empty message that is sent to the user's device via Firebase Cloud Messaging (FCM) server for Android devices or Apple Push Notification service (APNs) for iOS devices. 

The [Uninstall](https://docs.clevertap.com/docs/uninstalls-view) board helps you monitor your app uninstalls and take timely action. 

### Enable Silent Push Notifications

Enable Silent Push notifications in CleverTap by performing the following steps:

1. From the CleverTap dashboard, select *Settings* > *Uninstall Tracking*.
2. Toggle to enable *Silent token-based push notification*.

<Image title="uninstall_tracking_silent_push.png" alt={800} className="border" border={true} src="https://files.readme.io/1f8a490-uninstall_tracking_silent_push.png" />

## Real-Time Uninstall Tracking (Android)

Check that the Android app is integrated with *Firebase Analytics* for real-time uninstall tracking. 

When a user uninstalls an app from their Android device, the Firebase event called  `app_remove`   tracks this app uninstall. You can push this event to CleverTap using the Firebase cloud functions.

## Implement Real-Time Uninstall Tracking

Real-time uninstall tracking is a four-step process that consists of the following:

1. [Set up a common identifier in your app](https://developer.clevertap.com/docs/uninstall-tracking#section-set-up-a-common-identifier)
2. [Set the `app_remove` event as a conversion event in Firebase](https://developer.clevertap.com/docs/uninstall-tracking#section-set-up-conversion-event-using-firebase)
3. [Use the Firebase cloud function to send uninstall information to CleverTap](https://developer.clevertap.com/docs/uninstall-tracking#section-create-a-cloud-function)
4. [Enable the real-time settings in CleverTap](https://developer.clevertap.com/docs/uninstall-tracking#section-enable-real-time-uninstall-tracking-in-clever-tap)

## Set Up a Common Identifier

Add the following code to your app to set up a common identifier between Firebase and CleverTap. 

```java
private FirebaseAnalytics mFirebaseAnalytics;
mFirebaseAnalytics = FirebaseAnalytics.getInstance(this);
mFirebaseAnalytics.setUserProperty("ct_objectId",Objects.requireNonNull(CleverTapAPI.getDefaultInstance(this)).getCleverTapID());
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

![1143](https://files.readme.io/1ef2441-2020-09-28_14-58-47_app_remove.png "2020-09-28_14-58-47_app_remove.png")

## Create a Cloud Function

After the conversion is set up, use the [Cloud Function for Firebase](https://firebase.google.com/docs/functions/get-started) to create a function and send the `app_remove` data to CleverTap. 

To create and publish a cloud function using Node JS, perform the following steps:

1. Open a terminal.
2. Set up Node.js and the Firebase CLI. 
3. Run `npm install -g firebase-tools`.
4. To initialize Firebase SDK for Cloud Functions, run `firebase login`.
5. From your Firebase project directory, run `firebase init functions`.
6. Select *Javascript* as a language option.
7. Open `index.js` and add the following code:

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

9. Open `package.json` and add the following code:

```json
{
  "name": "functions",
  "description": "Cloud Functions for Firebase",
  "scripts": {
    "lint": "eslint .",
    "serve": "firebase emulators:start --only functions",
    "shell": "firebase functions:shell",
    "start": "npm run shell",
    "deploy": "firebase deploy --only functions",
    "logs": "firebase functions:log"
  },
  "engines": {
    "node": "10"
  },
  "dependencies": {
    "firebase-admin": "^8.6.0",
    "firebase-functions": "^3.3.0",
    "web-push": "^3.4.4",
    "request": "^2.88.2",
    "requestretry": "^4.1.1"
  },
  "devDependencies": {
    "eslint": "^5.12.0",
    "eslint-plugin-promise": "^4.0.1",
    "firebase-functions-test": "^0.1.6",
    "web-push": "^3.4.4",
    "request": "^2.88.2",
    "requestretry": "^4.1.1"
  },
  "private": true
}
```

## Enable Real-Time Uninstall Tracking in CleverTap

Once you have configured the conversion set up and created the cloud function, enable real-time uninstall tracking in CleverTap by performing the following steps:

1. From the CleverTap dashboard, select *Settings* > *Uninstall Tracking*.
2. Select the toggle for *Android (Real-time tracking)*.

![1486](https://files.readme.io/b919669-2020-09-28_15-35-19_inabling_tracking_in_CT.png "2020-09-28_15-35-19_inabling tracking in CT.png")

# FAQs and Troubleshooting

### Q1. Can I run triggered campaigns on the *App uninstalled* event?

Yes. You can set up live triggered campaigns for the *App Uninstalled* event. Only those users who are tracked via the real-time uninstall will qualify for the campaign. 

### Q2. How do I target users who have uninstalled the app in a campaign?

Real-time uninstalls can be targeted in a campaign by using the event property filter *source = REALTIME\_FIREBASE*.

![1752](https://files.readme.io/db9dacb-Screenshot_2021-06-10_at_12.18.48_PM.png "Screenshot 2021-06-10 at 12.18.48 PM.png")

### Q3. Can I turn off uninstall tracking for push notification sent via a Campaign or a Journey?

No. This is tracked by default. For more information, see [Tracking Uninstalls Effectively](https://clevertap.com/blog/track-app-uninstalls-effectively).
