---
title: KaiOS
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
[block:api-header]
{
  "title": "Overview"
}
[/block]
The KaiOS push notification section covers how to set up CleverTap’s KaiOS SDK on your app. Once KaiOS has been integrated, new web push campaigns can be created to engage users.

#Generate VAPID keys
To get started, you must generate VAPID keys using one of the following methods:
  * Using npm package name web-push (recommended).
  * You can also use VAPID keys generated for Chrome web push in a Firebase Cloud Messaging (FCM) account. For more information, refer to [VAPID Protocol](https://developer.clevertap.com/docs/web#section-vapid-protocol). 
[block:callout]
{
  "type": "info",
  "body": "The same VAPID keys are valid for Chrome, Firefox, and as well as KaiOS push.",
  "title": "VAPID Keys Valid for Chrome, Firefox, and KaiOS Web Push"
}
[/block]
#Paste VAPID Keys in CleverTap
After you get the VAPID keys, perform the following steps: 

1. In the CleverTap dashboard, navigate to *Settings* > *Engage* > *Channels* > *Web Push*. 
2. Under the *KaiOS Web Push* section, add your generated VAPID public and private key pairs. 
3. Click **Save**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/fb9f420-Screenshot_2021-05-07_at_10.03.46_AM.png",
        "Screenshot 2021-05-07 at 10.03.46 AM.png",
        1472,
        1010,
        "#f9f8fa"
      ],
      "border": true
    }
  ]
}
[/block]
#Add Mandatory Permissions
Add the following permissions to the app-manifest:

[block:code]
{
  "codes": [
    {
      "code": "\"systemXHR\": {\n    \"description\": \"Required to load remote content\"\n  },\n  \"serviceworker\": {\n    \"description\": \"Needed for associating service worker\"\n  },\n  \"desktop-notification\": {\n    \"description\": \"Required for displaying alerts when app is on background\"\n  },\n  \"push\": {\n    \"description\": \"To receive notifications\"\n  }",
      "language": "json"
    }
  ]
}
[/block]
#Initialize the CleverTap SDK 

To initialize the CleverTap SDK, use the following which is required to pass your CleverTap account ID:
  * clevertap.init("<Project ID>", "<Region>") 
  * Optional: clevertap.setLogLevel(clevertap.logLevels.DEBUG)
  * Optional: clevertap.setAppVersion("1.0.0")
[block:callout]
{
  "type": "info",
  "title": "Regions",
  "body": "The value for a region is based on the data center selected for the project with which the SDK is being integrated. \n\nThe values are as followed:\n* India: in1\n* Singapore: sg1\n* South Korea: sk1\n* U.S.: us1"
}
[/block]
#Identify Users

CleverTap automatically creates a user profile for every person visiting your website. At first, the user profile starts out as anonymous which means the user profile does not contain any identifiable information. 

1. Enrich the user profile with information, such as the user’s name or email, by calling `clevertap.onuserlogin`.

Here is an example showing how to add a name and an email to a user’s profile:
[block:code]
{
  "codes": [
    {
      "code": "// with the exception of one of Identity, Email, or FBID\n// each of the following fields is optional\n\nclevertap.onUserLogin.push({\n \"Site\": {\n   \"Name\": \"Jack Montana\",            // String\n   \"Identity\": 61026032,              // String or number\n   \"Email\": \"jack@gmail.com\",         // Email address of the user\n   \"Phone\": \"+14155551234\",           // Phone (with the country code)\n   \"Gender\": \"M\",                     // Can be either M or F\n   \"DOB\": new Date(),                 // Date of Birth. Date object\n// optional fields. controls whether the user will be sent email, push etc.\n   \"MSG-email\": false,                // Disable email notifications\n   \"MSG-push\": true,                  // Enable push notifications\n   \"MSG-sms\": true,                   // Enable sms notifications\n   \"MSG-whatsapp\": true,              // Enable WhatsApp notifications\n }\n})",
      "language": "javascript"
    }
  ]
}
[/block]
2. In addition to our JavaScript library, we also provide a [profile API](https://developer.clevertap.com/docs/user-profile-object) to update user profiles from your server.

#Track User Events

CleverTap provides the ability to track custom events that are specific to your website. For example, if you are an e-commerce company, you most likely want to track what products were viewed. 

1. Track custom events by calling clevertap.event.push("EVENT_NAME").

Here is an example how to track that a product was viewed:
[block:code]
{
  "codes": [
    {
      "code": "clevertap.event.push(\"Product Viewed\");",
      "language": "javascript"
    }
  ]
}
[/block]
2. In addition to tracking an event, you can add any key/value pairs as properties on the event which lets you create more specific segments and targeted campaigns. 
[block:code]
{
  "codes": [
    {
      "code": "clevertap.event.push(\"Product Viewed\", {\n  \"Product name\":\"Casio Chronograph Watch\",\n  \"Category\":\"Mens Accessories\",\n  \"Price\":59.99,\n});",
      "language": "javascript"
    }
  ]
}
[/block]
#Register for Push Notifications

Call the following method to register for push notifications: clevertap.registerCTNotifications(arg1) 
  * arg1 = Clevertap ServiceWorker path (if not provided, /serviceWorker.js in the root directory will be considered)

This registers the service worker and updates the push token for the profile.

#Un-Register for Push Notifications

Call the following method to un-register the service worker: 
clevertap.unregisterCTNotifications(arg1) 
  * arg1 = Clevertap ServiceWorker path (if not provided, /serviceWorker.js in the root directory will be considered)

#Add the Service Worker File
Push notifications on KaiOS use a special script called a service worker that listens for pushes. Since the execution of these scripts is managed by the app in the background, users can receive push notifications from your app even when they are not online. To get started, download the service worker file we have provided and copy it in the document root of your app in order to make it publicly accessible.

Download the service worker file [from this JavaScript code](https://github.com/CleverTap/clevertap-fxos/blob/master/ctServiceWorker.js). As mentioned above, you will have to host this file in your document root.

#Debugging

By default, CleverTap logs are set to CleverTap.Loglevels.INFO. During development, we recommend that you set the SDK to DEBUG mode in order to log warnings or other important messages to the Android logging system. This can be done by setting the debug level to CleverTap.Loglevels.DEBUG. If you want to disable CleverTap logs for the production environment, you can set the debug level to CleverTap.Loglevels.OFF.

[block:code]
{
  "codes": [
    {
      "code": "clevertap.setLogLevel(clevertap.logLevels.DEBUG); ",
      "language": "javascript"
    }
  ]
}
[/block]