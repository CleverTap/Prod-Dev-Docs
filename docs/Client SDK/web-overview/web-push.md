---
title: Web Push
excerpt: Learn about the basic configurations for Web Push channel
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

Web push notifications are browser-based messages that allow you to reach users directly on their devices, even when they are not actively browsing your website. Using CleverTap’s advanced segmentation capabilities and powerful infrastructure, you can send time-sensitive, relevant, and personalized push messages at scale. 

The latest Voluntary Application Server Identification (VAPID) protocol for Web Push enables secure communication between your server and CleverTap, by proving the server’s identity with VAPID keys.

# Supported Browsers and Devices

Web push notifications are supported on the following browsers: 

| Browser         | Windows | Android | iOS (iPhone, iPad) | macOS |
| --------------- | ------- | ------- | ------------------ | ----- |
| Google Chrome   | Yes     | Yes     | Yes                | Yes   |
| Mozilla Firefox | Yes     | Yes     | Yes                | Yes   |
| Safari          | N/A     | N/A     | Yes                | Yes   |

Following are the supported browser versions:

| Browser         | Version number                         |
| :-------------- | :------------------------------------- |
| Google Chrome   | v42 and above (desktop and android)    |
| Mozilla Firefox | v44 and above (desktop), v48 (android) |
| Safari          | v16 and above (macOS), v16.4 (iOS)     |

> 📘 Note
> 
> - **HTTPS Requirement:** Web push notifications function only on sites served over HTTPS.
> 
> - **Incognito/Private Browsing:** Web push does not operate in Incognito or Private Browsing mode.
> 
> - **In-App Browsers:** Browsers embedded within apps (such as Facebook, Instagram, etc.) do not support web push notifications.
> 
> - **Android WebViews:** Web push is not supported within Android WebViews.

# Features Availability across Browsers

Note the following features available across Chrome, Firefox, and Safari browsers:

[block:parameters]
{
  "data": {
    "h-0": "Feature",
    "h-1": "Chrome",
    "h-2": "Firefox",
    "h-3": "Safari",
    "0-0": "CTA",
    "0-1": "v53 and above (desktop, android)",
    "0-2": "No ",
    "0-3": "No",
    "1-0": "Icon",
    "1-1": "v33 and above (desktop),  \nv42 and above (android)",
    "1-2": "v26 and above (desktop, android)",
    "1-3": "No",
    "2-0": "Image",
    "2-1": "v56 and above (desktop, android)",
    "2-2": "No",
    "2-3": "No",
    "3-0": "Requires Interaction",
    "3-1": "v47 and above (desktop, android)",
    "3-2": "v117 and above (desktop, android)",
    "3-3": "No"
  },
  "cols": 4,
  "rows": 4,
  "align": [
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


For more information on the additional features, refer to [Notifications API](https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API).

# Web Push Configuration

Following are the four major steps involved in the Web Push configuration process:

1. [Generating VAPID keys](doc:web-push#generating-vapid-keys)
2. [Uploading VAPID Keys in CleverTap](doc:web-push#uploading-vapid-keys-in-clevertap)
3. [Adding the Service Worker file ](doc:web-push#adding-the-service-worker-file)
4. [Requesting User Permissions](doc:web-push#requesting-user-permissions)

> 🚧 Note
> 
> If you are still using Chrome FCM or Safari APNS, refer to [Web Push (Legacy)](doc:web-push-legacy) for detailed steps. We recommend migrating to the latest protocols for enhanced functionality and support.

## Generating VAPID keys

Generating VAPID keys is a one-time setup process. You can follow Legacy or VAPID protocol for generating VAPID keys.

> ❗️ Version requirements for VAPID-based Web Push notifications
> 
> This feature is supported only in CleverTap Web SDK versions 1.1.0 and above. To check your Web SDK version, use the `clevertap.getSDKVersion` method.

### Using npm package web-push

The VAPID keys can be generated using the npm package named web-push (recommended). 

For example, to add VAPID keys using the npm package, refer to `web-push generate-vapid-keys`:

```
Public Key:
BCCW5bb7HgyST570k0LwvSLoUH6GkNMLYmGek5eXA-DZMn8J5Rt9EfdszmMqg60tpcYuP9jmXq3xDPAdAebb538
Private Key:
dZq613Sfk_®_JCzqIFWhDtW6mabuR11VtEoBRXiSYA
```

### Using Firebase Console

The public and private key pair can be generated from Firebase with the following steps: 

1. In the Firebase console, navigate to your _Project_ > _Project Settings_ > _Cloud Messaging_ > _Web Configuration_.
2. Click **Generate key pair**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/cd27210-New-New_API_Firebase.png",
        "Generate Key Pair",
        2880
      ],
      "align": "center",
      "border": true,
      "caption": "Generating VAPID Keys"
    }
  ]
}
[/block]


## Uploading VAPID Keys in CleverTap

For all the browsers, user needs to enable the radio button to actually enable web push for that browser. To add the VAPID keys in the CleverTap dashboard, follow the steps below:

#### Chrome

1. In the CleverTap dashboard, navigate to _Settings_ > _Engage_ > _Channels_ > _Web Push_. 
2. Under the _Chrome Web Push_ section, add your generated public and private key pairs.
3. Click **Save**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9f34fdb7cfb7a2eb0788928c29e8c89303c12d65a11d8badb43c892ecf201bdd-image.png",
        null,
        "Add Public and Private Key Pairs"
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true,
      "caption": "Add Public and Private Key Pairs for Chrome"
    }
  ]
}
[/block]


#### Firefox

1. In the CleverTap dashboard, navigate to _Settings_ > _Engage_ > _Channels_ > _Web Push_. 
2. Under the _Firefox Web Push_ section, add your generated VAPID public and private key pairs. 
3. Click **Save**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d09620c-25.png",
        "Add VAPID Public and Private Key Pairs",
        1448
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true,
      "caption": "Add Public and Private Key Pairs for Firefox"
    }
  ]
}
[/block]


#### Safari

1. In the CleverTap dashboard, navigate to _Settings_ > _Engage_ > _Channels_ > _Web Push_. 
2. Under the _Safari Web Push_ section, add your generated public and private key pairs.
3. Click **Save**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3f73fae16fd16cac26671c79b00dde290f507288f0c2726b6c6bec5f22d0353b-2025-01-23_21-15-19.png",
        null,
        "Add Public and Private Keys for Safari"
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true,
      "caption": "Add Public and Private Keys for Safari"
    }
  ]
}
[/block]


## Adding the Service Worker file

This section applies to Chrome, Firefox, and Safari browsers. Browser push notifications uses 'Service Worker', a special script that listens for web pushes. Since the browser manages the execution of these scripts in the background, users can receive push notifications from your website even when they are not on your site. 

To get started, follow the steps below:

1. Download the service worker file provided here - [clevertap_sw.js](https://static.clevertap.com/docs/clevertap_sw.js) . 
2. You must host this file in your document root. Keep the file name -  _clevertap_sw.js_ to ensure all the functionalities work as expected.
3. Copy it to your website's document root to make it publicly accessible.

### Integration with Existing Service Worker File

Import CleverTap's service worker at the start of your existing service worker. Add the following code snippet:

```javascript
importScripts('https://s3-eu-west-1.amazonaws.com/static.wizrocket.com/js/sw_webpush.js');// remove CleverTap server worker from your root folder
```

> 📘 Migrating Tokens with CleverTap
> 
> This script will also handle the migration of the tokens from your previous service provider. For more information about migration, refer to [Migrating from other services](https://developer.clevertap.com/docs/web-push#migrating-from-other-services).

## Requesting User Permissions

This section applies to all browsers, such as Chrome, Firefox, and Safari. Now that you have a service worker and [manifest set up](doc:web-push#creating-a-manifest-file), it is time to start asking users if they want to subscribe to web pushes. For flexibility, CleverTap provides you with the ability to ask for notifications based on context and user actions.

As a general rule, we highly recommend asking for permission from users only after they have spent some time on your website and completed action of significance. Do not ask new users to subscribe to notifications as soon as they land on your site.

For example, after a user has completed a transaction, you may call the function below:

```javascript
clevertap.notifications.push({
   	"titleText": "Would you like to receive Push Notifications?",
   	"bodyText": "We promise to only send you relevant content and give you updates on your transactions",
   	"okButtonText": "Sign me up!",
   	"rejectButtonText": "No thanks",
   	"okButtonColor":"#F28046",
   	"askAgainTimeInSeconds":5,
  	"serviceWorkerPath": "/foo/my_sw.js" // path to your custom service worker file
  });
```

If a user blocks the native Chrome, Firefox, and Safari permissions pop-up, the user is lost forever unless the user clears their browser settings; however, if you are using the CleverTap pop-up, you get the advantage of asking the same user again for permission after a defined period.

### Permission Frequency

To ensure that users are not asked for permission too frequently, our API ensures that calling `clevertap.notifications.push` does nothing if the user has rejected the dialog less than one week ago. Once a week has passed by, calling the function will display the dialog as expected. This eliminates the need for you to implement a separate check before calling the function. 

# Soft Prompt Setup

Before calling the Soft Prompt for Web Push, you must first enable it in the CleverTap dashboard. To enable, follow the steps below:

1. From the CleverTap dashboard, go to Settings > Channels > Web push. Click Soft Prompt.
2. Toggle the Card Popup radio button to enable the prompt.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/920f9f1cc3d31ec0773c3e6dbc06d744785e610e3acbe45a5cf90a0cc9bbf6ac-image.png",
        null,
        null
      ],
      "align": "center",
      "border": true,
      "caption": "Soft Prompt"
    }
  ]
}
[/block]


3. Configure the prompt options as needed (title, body text, appearance, and so on).
4. Save your changes.

Once enabled, you can use the SDK to call the Soft Prompt in your app. For more information, refer to [Soft Prompt](https://docs.clevertap.com/docs/setup-web-push#soft-prompt).

> 📘 Note
> 
> The setup step is required. If the Soft Prompt is not enabled in the dashboard, calling the prompt through the SDK will not display it.

# iOS/iPadOS Web Push Configurations

Once you have completed the above steps, proceed with the following configurations to enable web push notifications on Safari for iOS and iPadOS devices. 

> 📘 Note
> 
> For Shopify Plugin customers, this feature is currently not supported out of the box. Please follow the steps below to enable web push for iOS and iPadOS.

Following are the five major steps involved in the iOS/iPadOS Web Push configuration process:

1. [Creating a Manifest File](doc:web-push#creating-a-manifest-file)
2. [Linking the Manifest File](doc:web-push#linking-the-manifest-file)
3. [Adding a Service Worker](doc:web-push#adding-a-service-worker)
4. [Adding to Home Screen](doc:web-push#adding-to-home-screen)
5. [Displaying the Native Push Prompt](doc:web-push#Displaying-the-native-push-prompt)

## Creating a Manifest File

A web app manifest file is a JSON-formatted text file that provides metadata about your web app, including how it should appear once installed. It is essential for enabling web push notifications on iOS. Create a `manifest.json` file in your website’s root directory:

```json
{
  "name": "CleverTap PWA App",
  "short_name": "CTPWA",
  "display": "standalone",
  "theme_color": "#your_theme_color",
  "background_color": "#your_background_color",
  "icons": [{
    "src": "/icon-512x512.png",
    "sizes": "512x512",
    "type": "image/png"
  }]
}
```

## Linking the Manifest File

Add the following `<link>` tag to the `<head>` element of your website to reference the manifest file:

```html
<link rel="manifest" href="/manifest.json" />
```

## Adding a Service Worker

This process is the same as the standard web push notifications integration. For detailed steps, refer to [Service Worker Integration](doc:web-push#adding-the-service-worker-file).

## Adding to Home Screen

Unlike significant browsers such as Chrome and Firefox, Safari on iOS/iPadOS requires users to add your web app to their Home Screen before they can request push notification permissions.

The "Add to Home Screen" feature allows users to bookmark your website and display your app’s icon on their home screen.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0195a67b0808b3b6d390b3a42645bed0daa2d0671fa0a633ade241d34c9489c6-76538E59-B9C5-4F89-B9F6-50C5338C7A17_1_201_a.jpeg",
        null,
        "Add to Home Screen"
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true,
      "caption": "Add to Home Screen"
    }
  ]
}
[/block]


## Displaying the Native Push Prompt

Web apps added to the Home Screen require direct user interaction to request push notification permissions.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/48778db39d58dd75066fe0084272bdcec5f52b9ffad233428d8fa4cec8d6a851-F29AC9B4-B451-4165-9F4D-BB27B839665D_1_105_c.jpeg",
        null,
        "Native Prompt"
      ],
      "align": "center",
      "sizing": "40% ",
      "border": true,
      "caption": "Native Prompt"
    }
  ]
}
[/block]


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2d6b2aa03765171747933b2e2a837a09671f911ccaa2d4cb8c820e7566b5a952-Image.jpeg",
        null,
        "Push Notification Opt-In"
      ],
      "align": "center",
      "sizing": "40% ",
      "border": true,
      "caption": "Push Notification Opt-In"
    }
  ]
}
[/block]


# Shopify Web Push Configurations for iOS/iPadOS

To configure Web Push for your Shopify website, follow these steps:

1. [Creating a Manifest File](https://developer.clevertap.com/docs/web-push#creating-a-manifest-file-1)
2. [Linking the Manifest File](https://developer.clevertap.com/docs/web-push#linking-the-manifest-file-1)

## Creating a Manifest File

You require a `manifest.json` file to enable Shopify web push configurations. Refer to the [manifest file](https://developer.clevertap.com/docs/web-push#creating-a-manifest-file) for iOS/iPadOS Web Push Configurations, and add it to your Shopify theme’s `assets` folder.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4e3618f16bdb2cb6b4ebba11141f66634b33b69a73fe9631b0823fc81cd119dc-shopify_theme.png",
        "",
        "Shopify Theme Folders"
      ],
      "align": "center",
      "border": true,
      "caption": "Shopify Theme Folders"
    }
  ]
}
[/block]


## Linking the Manifest File

Open the `theme.liquid` file in your Shopify folders. Add the following `<link>` tag to the `<head>` element to reference the manifest file:

```Text HTML
<link rel="manifest" href="{{ 'manifest.json' | asset_url }}">
```

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6adbdabdbb32a8a3c80340919982faa9430456ee2c784f8e236816b6e8f0e61e-theme.liquid_file.png",
        "",
        "Theme.Liquid File"
      ],
      "align": "center",
      "border": true,
      "caption": "Theme.Liquid File"
    }
  ]
}
[/block]


> 📘 Note
> 
> Beyond these two steps, no additional setup is required. Web Push prompts and permissions are handled during user interaction.
> 
> For more information about Shopify dashbaord, refer to [Shopify: Themes](https://shopify.dev/docs/storefronts/themes/tools/online-editor).

# Migrating from Other Services

This section applies to Chrome, Firefox, and Safari browsers. If you are migrating from a different web push notifications provider, you may be concerned about asking subscribed users to enable notifications a second time.

## Migrating Subscribed Users

The [parent service worker](https://developer.clevertap.com/docs/web#section-add-the-service-worker-file) will automatically migrate tokens from previous service providers and handle Web Push notifications.

**Case 1: Existing Users - Registered with the previous service provider**

If a user has already accepted receiving Web Push notifications on your domain through previous service provider, calling clevertap.notifications.push will not display the soft-prompt to that particular user and the user token is migrated automatically to CleverTap. 

**Case 2: Existing Users  - Not registered with the previous service provider**

The user token is not generated if a user has declined to receive Web Push notifications on your domain through a previous service provider. Therefore, calling clevertap.notifications.push will not display the soft-prompt to that particular user. 

**Case 3: New Users**

Calling clevertap.notifications.push will display the soft-prompt for that particular user. If the user accepts receiving Web Push notifications, the user token is generated, and this user is registered with Clevertap.