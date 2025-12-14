---
title: KaiOS SDK QuickStart Guide
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
# Installation

You can install KaiOS in two ways:

- Copy clevertap.js/clevertap.min.js directly into your FireFox app js directory and load in a script tag in your index.html:

```javascript
<script type="text/javascript" src="js/libs/clevertap.js" defer></script>
```

- Install via npm `npm install clevertap-fxos --save` and require/import the module:

```javascript
var clevertap = require('clevertap-fxos');
```

# Generate VAPID keys

To get started, you must generate VAPID keys using one of the following methods:

- Using npm package name web-push (recommended).
- You can also use VAPID keys generated for Chrome web push in a Firebase Cloud Messaging (FCM) account. For more information, refer to [VAPID Protocol](https://developer.clevertap.com/docs/web-push#generating-vapid-keys). 

> 📘 VAPID Keys Valid for Chrome, Firefox, and KaiOS Web Push
> 
> The same VAPID keys are valid for Chrome, Firefox, and as well as KaiOS push.

# Paste VAPID Keys in CleverTap

After you get the VAPID keys, perform the following steps: 

1. In the CleverTap dashboard, navigate to _Settings_ > _Engage_ > _Channels_ > _Web Push_. 
2. Under the _KaiOS Web Push_ section, add your generated VAPID public and private key pairs. 
3. Click **Save**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/fb9f420-Screenshot_2021-05-07_at_10.03.46_AM.png",
        "Add Push Endpoint and VAPID Public/Private Key Pairs",
        1472
      ],
      "align": "center",
      "border": true,
      "caption": "Add Push Endpoint and VAPID Public/Private Key Pairs"
    }
  ]
}
[/block]


# Add Mandatory Permissions

Add the following permissions to the app-manifest:

```json
"systemXHR": {
    "description": "Required to load remote content"
  },
  "serviceworker": {
    "description": "Needed for associating service worker"
  },
  "desktop-notification": {
    "description": "Required for displaying alerts when app is on background"
  },
  "push": {
    "description": "To receive notifications"
  }
```

# Initialize the CleverTap SDK

To initialize the CleverTap SDK, use the following which is required to pass your CleverTap account ID:

```javascript
clevertap.setLogLevel(clevertap.logLevels.DEBUG); // optional

clevertap.setAppVersion("1.0.0"); // optional

clevertap.init("<YOUR_ACCOUNT_ID>","in1"); // required, pass your CleverTap Account ID
```

> 📘 Regions
> 
> The value for a region is based on the data center selected for the project with which the SDK is being integrated. 
> 
> The values are as follows:
> 
> - India: `in1`
> - Singapore: `sg1`
> - South Korea: `sk1`
> - U.S.: `us1`
> - Indonesia: `aps3`
> - Middle East (UAE): `mec1`