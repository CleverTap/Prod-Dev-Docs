---
title: Web Quick Start Guide
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
CleverTap provides a JavaScript library to track and engage users on your website. 

This guide shows you how to set up CleverTap’s JavaScript library on your website, track events, and create user profiles.

# Add the CleverTap JavaScript Library to Your Website

To get started, use the following steps:

1. Copy and paste the code snippet below inside the <head></head> section of your website.

2. Replace CLEVERTAP\_ACCOUNT\_ID with your CleverTap Account ID. You can find your CleverTap Account ID on the *Settings* page of the CleverTap dashboard. 

```javascript
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
		 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';
		 var s = document.getElementsByTagName('script')[0];
		 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

# Add Information to a User’s Profile

CleverTap automatically creates a user profile for every person visiting your website. At first, the user profile starts out as anonymous which means the user profile does not contain any identifiable information. 

1. Enrich the user profile with information, such as the user’s name or email, by calling clevertap.profile.push.

Here is an example showing how to add a name and an email to a user’s profile:

```javascript
clevertap.profile.push({
 "Site": {
   "Name": "Jack Montana", // User's name
   "Email": "jackmontana@example.com"
 }
});
```

2. In addition to our JavaScript library, we also provide an [API](doc:api-overview) to update user profiles from your server.

# Track User Events

CleverTap provides the ability to track custom events that are specific to your website. For example, if you are an e-commerce company, you most likely want to track what products were viewed. 

1. Track custom events by calling clevertap.event.push("EVENT\_NAME").

Here is an example to track that a product was viewed:

```javascript
clevertap.event.push("Product Viewed");
```

2. In addition to tracking an event, you can add any key/value pairs as properties on the event which lets you create more specific segments and targeted campaigns. 

```javascript
clevertap.event.push("Product Viewed", {
  "Product name":"Casio Chronograph Watch",
  "Category":"Mens Accessories",
  "Price":59.99,
});
```

Below are a few more things to consider:

* The CleverTap JavaScript library does not have any external dependencies, such as third-party libraries like jQuery.
* CleverTap’s JavaScript library works well within [Google Tag Manager](https://developers.google.com/tag-manager/quickstart).
* We recommend recording an event only after it has passed all of your validations successfully. For example, record an *Added to Cart* event only on successful addition to cart.
* Error messages are logged to the browser’s console. Verbose logging can be enabled by setting the WZRK\_D variable in the browser’s sessionStorage by running the command sessionStorage['WZRK_D'] = " "; in the console.

# Next Steps

By completing this guide, you tracked user events and created a user profile. You now have integrated CleverTap into your website.

If you have not integrated our mobile SDKs, our [iOS Quick Start Guide](doc:ios-quickstart-guide) and [Android Quick Start Guide](doc:android-quickstart-guide) are useful pages to explore next. If you have already integrated them, check out our [API Overview page](doc:api-overview) to get started on sending events and enriching user profiles from your server.
