---
title: Samsung Tizen TV Integration with Web SDK
excerpt: Learn how to integrate the CleverTap Web SDK for apps on Samsung TV.
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

CleverTap Web SDK integration with Tizen OS for Samsung TV enables you to improve user engagement and personalize the experience for your TVOS viewers. You can gain insights into user behavior, preferences, and interactions with your app.

This guide walks you through the step-by-step process of integrating CleverTap's Web SDK into your Samsung Tizen TV application.

## Steps to Integrate CleverTap Web SDK with Samsung Tizen TV

To integrate CleverTap Web SDK with Samsung Tizen TV:

1. Download the **Tizen Studio DMG** file on your system. 
2. Open the installer and complete the installation.
3. Open the **Package Manager** console extracted from **Tizen Studio**.
4. From the **Extension SDK** tab, install **TV Extensions-7.0**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/068c0e9-TizenPackageManager.png",
        "",
        ""
      ],
      "align": "center",
      "sizing": "80% ",
      "border": true
    }
  ]
}
[/block]

5. Create the sample project and update the `index.html` file as required. Add the CleverTap script in the `index.html` file.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/08584b3-RunAsTizen.png",
        "",
        ""
      ],
      "align": "center",
      "sizing": "90% ",
      "border": true
    }
  ]
}
[/block]

6. Click **Run As** > **Tizen Web Simulator Application(Samsung TV)**.

## Initialise CleverTap

To initialize CleverTap, add the CleverTap script in  the `index.html` file.

```html
<script type="text/javascript">
     var clevertap = {event:[], profile:[], account:[], onUserLogin:[], notifications:[], privacy:[]};
 // replace with the CLEVERTAP_ACCOUNT_ID with the actual ACCOUNT ID value from your Dashboard -> Settings page
clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
clevertap.privacy.push({optOut: false}); //set the flag to true, if the user of the device opts out of sharing their data
clevertap.privacy.push({useIP: false}); //set the flag to true, if the user agrees to share their IP data
 (function () {
		 var wzrk = document.createElement('script');
		 wzrk.type = 'text/javascript';
		 wzrk.async = true;
		 wzrk.src = 'https://d2r1yp2w7bby2u.cloudfront.net/js/clevertap.min.js';
		 var s = document.getElementsByTagName('script')[0];
		 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

## Send User Profile

To send user profile details to CleverTap, add the following code snippet:

```javascript
clevertap.onUserLogin.push({
 "Site": {
   "Name": "Jack Montana",            // String
   "Identity": 61026032,              // String or number
   "Email": "jack@gmail.com",         // Email address of the user
 }
})
```

## Push Event

To push an event to CleverTap, add the following code snippet:

```javascript
clevertap.event.push("Product viewed");
```

## Push Event With Properties

To push event with properties to CleverTap, add the following code snippet:

```javascript
clevertap.event.push("Product viewed", {
    "Product name": "Casio Chronograph Watch",
    "Category": "Mens Accessories",
    "Price": 59.99,
    "Date": new Date()
});
```

## Add Profile Properties

To add the profile properties, add the following code snippet:

```javascript
cleverTapAPI.addMultiValueForKey("userTVCount","1")
```

## Remove Profile Properties

To remove the profile properties, add the following code snippet:

```javascript
cleverTapAPI.removeMultiValueForKey("userTVCount","1")
```

> 📘 Note
> 
> CleverTap Push Notifications do not work on Tizen as the Service worker runs only on an HTTPS or a localhost connection. It does not work on `file://`.