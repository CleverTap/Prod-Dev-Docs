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

The industry approach for tracking mobile app uninstalls is through silent push notifications sent daily. CleverTap supports the following daily and real-time uninstall tracking, which helps track or enhance a campaign and increase user retention rates. 

* Silent token-based push notification (Android)
* Silent token-based push notification (iOS)
* Real-time uninstall tracking (Android)

## Silent Token-Based Push Notification (Android)

For Android devices, uninstalls are detected by sending silent push notification APIs to the Firebase Cloud Messaging (FCM) server. Silent push notifications are notifications that aren’t rendered on the user’s device. The customer can send a silent push notification daily to all the devices to track uninstalls. The uninstall numbers available on the CleverTap dashboard are based on the feedback received from notification APIs. When uninstalls are detected, the application sends blank push notifications without a payload, and based on the API response from there, CleverTap understands that the application has been uninstalled.

## Silent Token-Based Push Notification (iOS)

For iOS devices, uninstalls are detected by sending silent push notification APIs to the Apple Push Notification (APN) server. Silent push notifications are notifications that aren’t rendered on the user’s device. Like Android devices, when uninstalls are detected, the application sends blank push notifications without a payload, and based on the API response from there, CleverTap understands that the application has been uninstalled.

## Real-Time Uninstall Tracking (Android)

Real-time uninstall tracking identifies churned users in real-time by raising events in CleverTap whenever users uninstall their app. By creating live-action campaigns with uninstall events, customers can, in real-time, re-engage these identified churned users and get them to reinstall the app in real-time. When a user uninstalls the customer’s app on their Android device, Firebase raises an  `app_remove.` system event. Use a Google cloud function to push Firebase's `app_remove` event to CleverTap; this can be done via a CleverTap API call . When this occurs, customers can create a live campaign with the goal of winning back churned users.

For example, if the users of a travel app just uninstalled the app, the customer can create a live user segment campaign to re-engage with users by offering an incentive (for example, a 15% off coupon) to reinstall the app. 

> 📘 "App uninstall event"
>
> Only the `app uninstalled` event tracked via the real-time uninstall method is available in trigger campaigns.

## Implement Real-Time Uninstall Tracking

Real-time uninstall tracking is a four-step process that consists of the following:

* Setting up a common identifier in the customer app
* Setting the `app_remove` event as a conversion event in Firebase
* Using the Google cloud function in Firebase to send uninstall notification to CleverTap via API
* Toggling ON the real-time settings in CleverTap

## Set Up a Common Identifier

Use the following code snippet to set up a common identifier between Firebase and CleverTap. When doing this, we recommend setting the CleverTap ID as a Firebase user property on every app launch.

```java
private FirebaseAnalytics mFirebaseAnalytics;
mFirebaseAnalytics = FirebaseAnalytics.getInstance(this);
mFirebaseAnalytics.setUserProperty("ct_objectId",Objects.requireNonNull(CleverTapAPI.getDefaultInstance(this)).getCleverTapID());
```

## Set Up Conversion Event Using Firebase

Real-time unistall set up requires conversion set up in Firebase, which is a Backend-as-a-Service (Baas) built on Google's infrastructure. Firebase is categorized as a NoSQL database program that stores data in JSON-like documents. Firebase analytics track an event called `app_remove`, which is one of the automatically-collected Firebase events. The `app_remove` is an Android-only event that is tracked when an application package is removed or uninstalled regardless of the installation source from an android device.

> 🚧 Firebase Account Required
>
> To set up real-time uninstall tracking, you must have a Firebase account with Blaze plan and CleverTap dashboard access.

To perform conversion set up, perform the following steps:

1. Select the Firebase project integrated with the Android app. 

> 📘 Note
>
> Update the billing plan to Blaze if required.

2. From the Firebase dashboard, select *Analytics* > *Events*.
3. Select the *Mark as conversion* toggle for *app\_remove*.

![1143](https://files.readme.io/1ef2441-2020-09-28_14-58-47_app_remove.png "2020-09-28_14-58-47_app_remove.png")

## Create a Cloud Function

After the conversion set up, use the Cloud Function for Firebase to create a function and send the `app_remove` data to CleverTap. 

> 📘 Google Cloud Platform
>
> Learn more about using [Cloud Function for Firebase](https://firebase.google.com/docs/functions/get-started).

To create and publish a cloud function using Node JS and perform the following steps:

1. Open a terminal.
2. Set up Node.js and the Firebase CLI. 
3. Run `npm install -g firebase-tools`.
4. To initialize Firebase SDK for Cloud Functions, run `firebase login`.
5. From your Firebase project directory, run `firebase init functions`.
6. Select Javascript as a language option.
7. Open `index.js` and add the following code snippet:

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

9. Open `package.json` and add the following code snippet:

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
