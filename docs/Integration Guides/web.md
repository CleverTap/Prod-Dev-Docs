---
title: Web
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
This guide will show you how to send web pop-ups and push notifications.

If you haven’t already completed the [web quickstart guide](doc:web-quickstart-guide), please complete that first before following this guide. The quickstart guide will show you how to setup CleverTap’s JavaScript library on your website, track events, create user profiles, and engage users by showing them a notification.

# Web Pop-Ups

A CleverTap web pop-up is a modal alert-type message view displayed by CleverTap in your desktop or mobile website.

![445](https://files.readme.io/7cb150e-image1.png "image1.png")

CleverTap web pop-ups come in two types:

* Simple Web Pop-up Notifications. You can use these templates to create your notifications including a box notification, a banner notification, and interstitial notification.
* Exit Intent Notifications. These are displayed just before the user exits your site. These are available for desktop only. 

User clicks on a web notification can be silent, open a url or invoke a javascript function. Custom key value pairs can also be passed to the invoked javascript function. You may also include a pre-set or custom image to be displayed in the notification.

> 🚧 Click Tracking
>
> Currently, we don’t support tracking clicks on custom web pop-ups.

# **Defining a Custom Rendering Listener**

Using this custom callback, you will be able to control the look, feel and location of the web pop-up which you create in your CleverTap Dashboard.

You need to explicitly call clevertap.raiseNotificationViewed(); & clevertap.raiseNotificationClicked(); to ensure that notification views and clicks are tracked in your CleverTap Dashboard.

To define the callback and raise the clicked and viewed events, you need to add the following snippet to the embed code.

```javascript
clevertap.notificationCallback = function(msg){
      //raise the notification viewed and clicked events in the callback
      clevertap.raiseNotificationViewed();
      console.log(JSON.stringify(msg));//your custom rendering implementation here
      var $button = jQuery("<button></button>");//element on whose click you want to raise the notification clicked event
      $button.click(function(){
         clevertap.raiseNotificationClicked();
      });
};
```

The msg will be in the format below. msgId contains the campaign id and the date stamp so you can programmatically decide whether to show the notification or not. kv contains the custom key value pairs.

```json
{
    "msgContent": {
        "title": "hello title!",
        "description": “hello message!"
    },
    "msgId": "1439796272_20160219",
    "kv": {
        "key1":"value1",
        "key2":"value2"
    }
}
```

# Web Push

Web push notifications are currently supported for Chrome version 50 for Desktop and Android and work only on HTTPS sites (and on localhost).

**Add the CleverTap JavaScript Library**\
Integrate the CleverTap JavaScript library by following the [Web Quickstart Guide](doc:web-quickstart-guide).

**Integrate GCM and CleverTap**\
Get your GCM Sender ID and Server Key. In the CleverTap Dashboard, go to Settings → Integrate Web Push and add your Server Key.

# **Add the Service Worker File**

Push notifications on Chrome make use of a special script called a service worker that listens for web pushes. Since the execution of these scripts is managed by the browser in the background, users can receive push notifications from your website even when they’re not on your site. To get started, download the service worker file we have provided and copy it in your document root of your website, such that it’s publicly accessible.

Download the service worker file [from here](http://static.clevertap.com/docs/clevertap_sw.js). As mentioned above, you’ll have to host this file in your document root.

# **Specify the GCM Sender ID in manifest.json File**

In order to register the service worker and subscribe for push notifications, you need to specify your GCM Sender ID inside a website manifest file.

If your website already has a manifest, simply add the following property to it. Your GCM Sender ID is your GCM Project Number (12 digit number) and not your API Key.

```json
{
  "gcm_sender_id": "YOUR_GCM_SENDER_ID"
}
```

If you don’t have a manifest file yet, create a new file called manifest.json and add it in the document root of your website. Ensure that you specify the "name" and "gcm\_sender\_id" parameters, where "name" is the name of your brand (e.g. “Acme Fashion”).

Here is a sample manifest.json file.

```json
{
  "name": "Acme Fashion | Latest in fashion world",
  "gcm_sender_id": "1234567890" // replace with actual value
}
```

Then, in all your HTML files, include the below snippet in the header.

```html
<head>
  <link rel="manifest" href="/manifest.json">
</head>
```

# **Requesting User Permissions**

Now that you have a service worker and manifest set up, it’s time to start asking users if they want to subscribe to Web pushes. For flexibility, CleverTap provides you with the ability to ask for notifications based on context and user actions.

As a general rule, we highly recommend asking for permission from users only after they’ve spent some time on your website, and completed an action of significance. Do not ask new users to subscribe for notifications as soon as they land on your site.

For example, after a user has completed a transaction, you may call the function below.

```javascript
clevertap.notifications.push({
    "titleText":'Would you like to receive Push Notifications?',
    "bodyText":'We promise to only send you relevant content and give you updates on your transactions',
    "okButtonText":'Sign me up!',
    "rejectButtonText":'No thanks',
    "okButtonColor":'#f28046'});
```

The general format for creating the popup is below.

```javascript
clevertap.notifications.push({
    "titleText":popupTitleText,
    "bodyText":popupBodyText,
    "okButtonText":okButtonText,
    "rejectButtonText":rejectButtonText,
    "okButtonColor":okButtonColorInHex});
```

If a user blocks the native chrome permissions popup, the user is lost forever (unless the user clears their browser settings). However, if you are using the Clever popup, you get the advantage of asking the same user again for permission after a defined period.

**Permission Frequency**\
To ensure that users are not asked for permission too frequently, our API ensures that calling clevertap.notifications.push does nothing if the user has rejected the dialog less than one week ago. Once a week has passed by, calling the function will display the dialog as expected. This eliminates the need for you to implement a separate check before calling the function. 

To set a separate frequency, send "askAgainTimeInSeconds":time\_in\_seconds along with the above object. 

To skip the Clever popup, send "skipDialog":true along with the above object.

**Migrating from Other Services**\
If you are migrating from a different web push notifications provider, you may be concerned about asking subscribed users to enable notifications a second time.

Note that, if a user has already enabled notifications on your host domain, calling clevertap.notifications.push will not display the popup for that particular user, and the registration will automatically be migrated to our new implementation.

# End User Session

When a user logs out of your site, you can end the CleverTap session by calling the clevertap.logout(); method.

# Debugging

Error messages and warnings are logged to the JS console of the browser.

Verbose logging: enable verbose logging of all communication with the CleverTap servers by setting the WZRK\_D variable in sessionStorage. In the developer console of your browser type: sessionStorage['WZRK_D'] = '';
