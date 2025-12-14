---
title: LG webOS TV Integration with Web SDK
excerpt: Learn how to integrate CleverTap Web SDK for apps on LG webOS TV
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

CleverTap Web SDK integration with LG webOS TV enables you to improve user engagement and personalize the experience for your TVOS viewers. You can gain insights into user behavior, preferences, and interactions with your TV app.

This guide walks you through the step-by-step process of installing and integrating CleverTap's Web SDK into your LG webOS application.

# Steps to Integrate CleverTap Web SDK for LG webOS TV

To integrate CleverTap Web SDK with LG webOS TV:

1. Install LG webOS Command Line Interface (CLI). For more information, refer to the [LG CLI Installation](https://webostv.developer.lge.com/develop/tools/cli-installation) document.
2. Add the extension for [webOS TV](https://marketplace.visualstudio.com/items?itemName=webOSTVSDK.webostv) in Visual Studio Code from the VSCode Extension Marketplace.
3. Install the LG Simulator. For more information, refer to the [LG Simulator Installation](https://webostv.developer.lge.com/develop/tools/simulator-installation) document. 
4. Create the sample application as follows: 
   - Using VSCode: [App Development](https://webostv.developer.lge.com/develop/tools/vsce-dev-guide#app-development)
   - Using CLI: [App Development ](https://webostv.developer.lge.com/develop/tools/cli-dev-guide)
5. In the sample application, update the `index.html`file as required. Add the CleverTap script in the `index.html` file.
6. Open the `simulator.app` file inside the **Simulator** folder, as mentioned in Step 2.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d5be1b0-WebOS.png",
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

7. In **Simulator**, go to **File** > **Launch App** and select the folder of sampleApp, which has `.ipk` file

## Initialize CleverTap SDK

To initialize CleverTap, add the CleverTap script in the `index.html` file.

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

To push the event along with properties to CleverTap, add the following code snippet:

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