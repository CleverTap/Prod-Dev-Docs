---
title: Web SDK Quick Start Guide
excerpt: Learn how to quickly integrate and start using the CleverTap Web SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: link
      title: Web Pop-ups
      url: https://developer.clevertap.com/docs/web-popup
    - type: link
      title: Web Push
      url: https://developer.clevertap.com/docs/web-push
---
# Overview

CleverTap provides a JavaScript library to track and engage users on your website. This section shows how to set up CleverTap’s JavaScript library on your website, track events, and create user profiles.

# Integrate SDK

There are two ways to integrate CleverTap Web SDK:

## Option A: Add the CleverTap JavaScript Library Manually to Your Website

To get started, use the following steps:

1. Copy and paste the code snippet below inside the <head></head> section of your website.
2. Replace CLEVERTAP_ACCOUNT_ID with your CleverTap Account ID. You can find your CleverTap Account ID on the _Settings_ page of the CleverTap dashboard.
3. Setting the region is optional. You can get the region code from the CleverTap Dashboard. For more information, refer to [Region Codes](https://developer.clevertap.com/docs/idc). If no region is provided, the default region _eu1_, is used.
4. Setting the _Target Domain_ is optional. If no target domain is provided, the default target domain, _clevertap-prod.com_, is used.

<br />

<Callout icon="📘" theme="info">
  ### Note

  If you are upgrading from Legacy SDK, we strongly recommend testing for event/user properties instrumentation with all integrated platforms like Website, App WebViews, and TVs.
</Callout>

```javascript
<script type="text/javascript">
     var clevertap = {event:[], profile:[], account:[], onUserLogin:[], notifications:[], privacy:[]};
 // replace with the CLEVERTAP_ACCOUNT_ID with the actual ACCOUNT ID value from your Dashboard -> Settings page
clevertap.account.push({ "id": "CLEVERTAP_ACCOUNT_ID" }, "Region", "Target_Domain");
clevertap.privacy.push({optOut: false}); //set the flag to true, if the user of the device opts out of sharing their data
clevertap.privacy.push({useIP: false}); //set the flag to true, if the user agrees to share their IP data
 (function () {
		 var wzrk = document.createElement('script');
		 wzrk.type = 'text/javascript';
		 wzrk.async = true;
		 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
		 var s = document.getElementsByTagName('script')[0];
		 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

## Option B - Using the Package Manager (Recommended)

CleverTap Web SDK is available as an [npm](https://github.com/CleverTap/clevertap-web-sdk/?tab=readme-ov-file#-installation) package for adding to your website.
Use the following npm command for the node package manager:

```javascript
npm install clevertap-web-sdk --save
```

You can also use the yarn package manager to integrate the Web SDK:

```javascript
yarn add clevertap-web-sdk
```

## Add Region Code

Refer to the following table to identify the region for your account:

| CleverTap Dashboard URL                                                                                   | Region            |
| :-------------------------------------------------------------------------------------------------------- | :---------------- |
| [https://eu1.dashboard.clevertap.com/login.html#/](https://eu1.dashboard.clevertap.com/login.html#/)      | EU                |
| [https://in1.dashboard.clevertap.com/login.html#/](https://in1.dashboard.clevertap.com/login.html#/)      | India             |
| [https://us1.dashboard.clevertap.com/login.html#/](https://us1.dashboard.clevertap.com/login.html#/)      | US                |
| [https://sg1.dashboard.clevertap.com/login.html#/](https://sg1.dashboard.clevertap.com/login.html#/)      | Singapore         |
| \[[https://aps3.dashboard.clevertap.com/login.html#/](https://aps3.dashboard.clevertap.com/login.html#/)] | Indonesia         |
| \[[https://mec1.dashboard.clevertap.com/login.html#/](https://mec1.dashboard.clevertap.com/login.html#/)] | Middle East (UAE) |

Refer the following table for `region` key values:

| Region                  | Value                                 |
| :---------------------- | :------------------------------------ |
| India                   | `in1`                                 |
| Singapore               | `sg1`                                 |
| United States (U.S)     | `us1`                                 |
| Indonesia               | `aps3`                                |
| Middle East (UAE)       | `mec1`                                |
| Europe (default region) | You can skip adding the `region` key. |

To enable the CleverTap region, add a `region` element to the `clevertap` object as shown in the following code (second line):

```javascript India
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'in1', account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript Singapore
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'sg1', account:[], onUserLogin:[], notifications:[],privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript United States
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'us1', account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript Indonesia
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'aps3', account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript Middle East (UAE)
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'mec1', account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript Europe (default region)
<script type="text/javascript">
     var clevertap = {event:[], profile:[], account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

To know how to add region code in Web SDK, refer to [Region Codes](doc:idc#web).

## Migrating from Legacy Web SDK to the Latest Web SDK

The legacy Web SDK represented below is deprecated, and no fixes will be released for this version. All upcoming features will be supported only on the [latest Web SDK](doc:web-quickstart-guide#option-a-add-the-clevertap-javascript-library-manually-to-your-website). We recommend switching to the latest Web SDK for all the latest features and fixes.

### Option A: Migrate to the Latest SDK by Manually Adding the CleverTap JavaScript Library to Your Website

To add the CleverTap JavaScript library manually to your website:

1. Copy and paste the code snippet below inside the `<head> `section of your website.
2. Replace `CLEVERTAPACCOUNT_ID` with your CleverTap Account ID. You can find your CleverTap Account ID on the _Settings_ page of the CleverTap dashboard.
3. Replace the existing code snippet with the new code snippet mentioned below.

**Existing Code Snippet**

```javascript
<script type="text/javascript">
     var clevertap = {event:[], profile:[], account:[], onUserLogin:[], notifications:[], privacy:[]};
 // replace with the CLEVERTAP_ACCOUNT_ID with the actual ACCOUNT ID value from your Dashboard -> Settings page
clevertap.account.push({ "id": "CLEVERTAP_ACCOUNT_ID" }, "Region");
clevertap.privacy.push({optOut: false}); //set the flag to true, if the user of the device opts out of sharing their data
clevertap.privacy.push({useIP: false}); //set the flag to true, if the user agrees to share their IP data
 (function () {
         var wzrk = document.createElement('script');
         wzrk.type = 'text/javascript';
         wzrk.async = true;
         wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';
        var s = document.getElementsByTagName('script')[0];
         s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

**New Code Snippet**

```javascript
<script type="text/javascript">
     var clevertap = {event:[], profile:[], account:[], onUserLogin:[], notifications:[], privacy:[]};
 // replace with the CLEVERTAP_ACCOUNT_ID with the actual ACCOUNT ID value from your Dashboard -> Settings page
clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"},"Region");
clevertap.privacy.push({optOut: false}); //set the flag to true, if the user of the device opts out of sharing their data
clevertap.privacy.push({useIP: false}); //set the flag to true, if the user agrees to share their IP data
 (function () {
         var wzrk = document.createElement('script');
         wzrk.type = 'text/javascript';
         wzrk.async = true;
         wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';        
         var s = document.getElementsByTagName('script')[0];
         s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

### Option B: Migrate to the Latest SDK Using the Package Manager

CleverTap Web SDK is available as an _npm_ package for adding to your website. Use the following _npm_ command for the node package manager:

```javascript
npm install clevertap-web-sdk --save
```

Please feel free to reach out to our customer support in case you face any issues.

<Callout icon="📘" theme="info">
  ### Endpoint Change

  The CleverTap Web SDK endpoint is now also configured with domains and sub-domains of `clevertap-prod.com`.
</Callout>

## Add Region Code

To know how to add region code in Web SDK, refer to [Region Codes](doc:idc#web).

# Single Page Applications

SPA (Single Page Application) is a web app implementation that loads only a single web document and updates its body content via JavaScript APIs when dynamic content is to be shown. It therefore allows users to access websites without loading whole new pages from the server, which can result in performance gains and a more dynamic experience.

For SPAs, it is recommended as a best practice to set the following:

```javascript
clevertap.spa = true
```

<Callout icon="📘" theme="info">
  ### Progressive Web Applications

  - In the case of a PWA (Progressive Web Application), all the functionalities similar to the standard website are available.
  - SPA popup dismissal and automatic page view tracking are available from Web SDK v2.6.1 onwards.
</Callout>

# Identify Users

CleverTap automatically creates a user profile for each person visiting your website. At first, the user profile starts out as anonymous which means the user profile does not contain any identifiable information.

1. Enrich the user profile with information, such as the user’s name or email, by calling `clevertap.onuserlogin`.<br />Here is an example showing how to add a name and an email to a user’s profile:

```javascript
// with the exception of one of Identity, Email, or FBID
// each of the following fields is optional

clevertap.onUserLogin.push({
 "Site": {
   "Name": "Jack Montana",            // String
   "Identity": 61026032,              // String or number
   "Email": "jack@gmail.com",         // Email address of the user
   "Phone": "+14155551234",           // Phone (with the country code)
   "Gender": "M",                     // Can be either M or F
   "DOB": new Date(),                 // Date of Birth. Date object
// optional fields. controls whether the user will be sent email, push etc.
   "MSG-email": false,                // Disable email notifications
   "MSG-push": true,                  // Enable push notifications
   "MSG-sms": true,                   // Enable sms notifications
   "MSG-whatsapp": true,              // Enable WhatsApp notifications
 }
})
```

2. When you use `profile.push`, all the details sent are added to the current profile. Whereas, using `onuserlogin` assigns request details to the current profile if the user is unidentified. If the current profile is identified and belongs to the same user, the details are appended to the user's profile. If the current profile is for a different user, a new profile is created for the current user with the properties sent in the request.

   ```javascript
   clevertap.profile.push({
    "Site": {
      "Customer Type": "Silver",
      "Prefered Language": "English"
    }
   });

   /**
    * Data types
    * Event property keys must be Strings and property values must, with certain specific exceptions,
    * be scalar values, i.e. String, Boolean, Integer, or Float, or a Date object.
    *
    * Date object
    * When a property value is of type Date, the date and time are both recorded to the second.
    */
   ```
3. In addition to the JavaScript library, we also provide a [Profile API](https://developer.clevertap.com/docs/user-profile-object) to update user profiles from your server.

# Track User Events

CleverTap provides the ability to track custom events that are specific to your website. For example, if you are an e-commerce company, you most likely want to track what products were viewed.

1. Track custom events by calling `clevertap.event.push("EVENT_NAME")`.<br />Here is an example to track that a product was viewed:

```javascript
clevertap.event.push("Product Viewed");
```

2. In addition to tracking an event, you can add any key/value pairs as properties on the event which lets you create more specific segments and targeted campaigns.

```javascript
clevertap.event.push("Product Viewed", {
  "Product name":"Casio Chronograph Watch",
  "Category":"Mens Accessories",
  "Price":59.99,
  "Date": new Date()
});
```

Below are a few more things to consider:

* The CleverTap JavaScript library does not have any external dependencies, such as third-party libraries like jQuery.
* CleverTap’s JavaScript library works well within [Google Tag Manager](https://developers.google.com/tag-manager/quickstart).
* We recommend recording an event only after it has passed all of your validations successfully. For example, record an _Added to Cart_ event only on successful addition to the cart.

# Debugging

This section is applicable to all browsers such as Chrome, Firefox, and Safari. Error messages and warnings are logged to the JS console of the browser.

For verbose logging, enable verbose logging of all communication with CleverTap servers by setting the the`WZRK_D` variable in `sessionStorage`. Enter `sessionStorage['WZRK_D'] = '';` in the developer console of your browser.

Alternatively, you can also set the log levels after calling the `clevertap.init()` method in the following way:

```javascript
clevertap.init('ACCOUNT_ID', 'REGION', 'TARGET_DOMAIN') // Replace with values applicable to you
clevertap.setLogLevel(LOG_LEVEL)
// Here LOG_LEVEL is an integer that can be any of the following: 
//  0: disable all logs
//  1: display only errors
//  2: display errors and info
//  3: display all logs
```

# Offline Mode

The offline mode allows setting CleverTap SDK to offline state. The `setOffline` method determines the SDK's online state. By default, the offline state of the SDK is set to false.<br />However, once the `setoffline` method is passed with `true` as described below, the CleverTap SDK goes offline. All the events are recorded and queued locally, but they are not sent to the CleverTap server.

If the `setoffline` method is passed with `false` as described below, the CleverTap SDK is set to online mode, and the queued events are sent to the CleverTap server immediately.

```javascript
clevertap.setOffline(true) // sets the sdk in offline mode.Events will be queued locally and             will be fired only when offline mode is set to false
clevertap.setOffline(false) // disables the offline mode. Events will now get fired immediately
```
