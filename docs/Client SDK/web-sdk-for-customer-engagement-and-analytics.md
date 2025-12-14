---
title: Web SDK for Customer Engagement and Analytics
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
## Overview

CleverTap's Web SDK brings together real-time user insights, an advanced segmentation engine, and easy-to-use marketing tools in one mobile marketing platform — giving your team the power to create amazing experiences that deepen customer relationships. Our intelligent mobile marketing platform provides the insights you need to keep users engaged and drive long-term retention and growth. 

CleverTap's Web SDK prevents ad blocker blocked requests, supports Angular and ReactJS frameworks via NPM, as well as single page application (SPA) support. Before following the steps in this section, be sure to complete the [Web SDK Quick Start Guide](doc:web-quickstart-guide), which shows how to setup CleverTap’s JavaScript library on your website, track events, create user profiles, and engage users by showing them a notification.

# Installation

CleverTap Web SDK is available as an *npm package* or as a script to add to your website manually.

## Use a Package Manager

`npm install clevertap-web-sdk --save`

or

`yarn add clevertap-web-sdk`

## Add the Script Manually

```html
<script type="text/javascript">
     var clevertap = {event:[ ], profile:[ ], account:[ ], onUserLogin:[ ], notifications:[ ], privacy:[ ]};
 // replace with the CLEVERTAP_ACCOUNT_ID with the actual ACCOUNT ID value from your Dashboard > Settings page
clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
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

# Basic Initialization

## Add your CleverTap account credentials

Perform the following only in case you are using a package manager.

```javascript
import clevertap from 'clevertap-web-sdk'
clevertap.privacy.push({optOut: false}) // Set the flag to true, if the user of the device opts out of sharing their data
clevertap.privacy.push({useIP: false})  // Set the flag to true, if the user agrees to share their IP data
clevertap.init('ACCOUNT_ID', 'REGION', 'TARGET_DOMAIN') // Replace with values applicable to you. 
```

Values:\
`ACCOUNT_ID` (mandatory): You can get this value from the Projects page on the CleverTap dashboard.\
`REGION` (optional): This will be the same as the region of the CleverTap dashboard. Possible values: (in1/us1/sg1).\
`TARGET_DOMAIN` (optional): domain of the proxy server.

For SPAs, you need to also set the following:

```javascript
clevertap.spa = true
```

## Event Push

Events track what individual actions users perform in your app or website. Some examples of events include a user launching an app, viewing a product, listening to a song, sharing a photo, making a purchase, or favoriting an item.

```javascript
// event without properties
clevertap.event.push("Product viewed");

// event with properties
clevertap.event.push("Product viewed", {
    "Product name": "Casio Chronograph Watch",
    "Category": "Mens Accessories",
    "Price": 59.99,
    "Date": new Date()
});
```

## Profile Push

After you integrate our SDK, we will create a user profile for each person who launches your app or visits your website.

```javascript
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
clevertap.profile.push({
 "Site": {
   "Name": "Jack Montana",                  // String
   "Identity": 61026032,                    // String or number
   "Email": "jack@gmail.com",               // Email address of the user
   "Phone": "+14155551234",                 // Phone (with the country code)
   "Gender": "M",                           // Can be either M or F
   "DOB": new Date(), // Date of Birth. Javascript Date object
   "Photo": 'www.foobar.com/image.jpeg',    // URL to the Image

// optional fields. controls whether the user will be sent an email, push etc.
   "MSG-email": false,                      // Disable email notifications
   "MSG-push": true,                        // Enable push notifications
   "MSG-sms": true                          // Enable sms notifications
   "MSG-whatsapp": true,                    // Enable whatsapp notifications
 }
})
```

## Debugging

This section is applicable for all browsers such as Chrome, Firefox, and Safari. Error messages and warnings are logged to the JS console of the browser.

For verbose logging, enable verbose logging of all communication with the CleverTap servers by setting the `WZRK_D` variable in sessionStorage. In the developer console of your browser type, `sessionStorage['WZRK_D'] = '';`

Alternatively, you can also set the log levels after calling `clevertap.init()` in the following way:

```javascript
clevertap.setLogLevel(LOG_LEVEL)
// Here Log Levels is an integer that can be any of the folowing: 
//  0: disable all logs
//  1: display only errors
//  2: display errors and info
//  3: display all logs
```

## Example Usage

* A [react application](https://github.com/CleverTap/clevertap-web-sdk/tree/master/example-apps/react) showing the integration of our SDK in a create react app project.
* An [angular application](https://github.com/CleverTap/clevertap-web-sdk/tree/master/example-apps/angular) showing the integration of our SDK in an Angular CLI generated project.

## Change Log

Refer to the [CleverTap Web SDK Change Log](https://github.com/CleverTap/clevertap-web-sdk/blob/master/CHANGELOG.md).

## License

CleverTap Web SDK is released under the MIT license. Refer to [LICENSE](https://github.com/CleverTap/clevertap-web-sdk/blob/master/LICENSE) for details.
