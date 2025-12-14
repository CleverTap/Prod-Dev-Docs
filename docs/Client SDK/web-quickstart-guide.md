---
title: Web SDK Quick Start Guide
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
CleverTap provides a JavaScript library to track and engage users on your website. This section shows how to set up CleverTap’s JavaScript library on your website, track events, and create user profiles.

#Add the CleverTap JavaScript Library to Your Website

To get started, use the following steps:

1. Copy and paste the code snippet below inside the <head></head> section of your website.

2. Replace CLEVERTAP_ACCOUNT_ID with your CleverTap Account ID. You can find your CleverTap Account ID on the *Settings* page of the CleverTap dashboard. 
[block:code]
{
  "codes": [
    {
      "code": "<script type=\"text/javascript\">\n     var clevertap = {event:[], profile:[], account:[], onUserLogin:[], notifications:[], privacy:[]};\n // replace with the CLEVERTAP_ACCOUNT_ID with the actual ACCOUNT ID value from your Dashboard -> Settings page\nclevertap.account.push({\"id\": \"CLEVERTAP_ACCOUNT_ID\"});\nclevertap.privacy.push({optOut: false}); //set the flag to true, if the user of the device opts out of sharing their data\nclevertap.privacy.push({useIP: false}); //set the flag to true, if the user agrees to share their IP data\n (function () {\n\t\t var wzrk = document.createElement('script');\n\t\t wzrk.type = 'text/javascript';\n\t\t wzrk.async = true;\n\t\t wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';\n\t\t var s = document.getElementsByTagName('script')[0];\n\t\t s.parentNode.insertBefore(wzrk, s);\n  })();\n</script>\n",
      "language": "javascript"
    }
  ]
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
2. In addition to our JavaScript library, we also provide a [Profile API](https://developer.clevertap.com/docs/user-profile-object) to update user profiles from your server.

#Track User Events

CleverTap provides the ability to track custom events that are specific to your website. For example, if you are an e-commerce company, you most likely want to track what products were viewed. 

1. Track custom events by calling clevertap.event.push("EVENT_NAME").

Here is an example to track that a product was viewed:
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
Below are a few more things to consider:

  * The CleverTap JavaScript library does not have any external dependencies, such as third-party libraries like jQuery.
  * CleverTap’s JavaScript library works well within [Google Tag Manager](https://developers.google.com/tag-manager/quickstart).
  * We recommend recording an event only after it has passed all of your validations successfully. For example, record an *Added to Cart* event only on successful addition to cart.
  * Error messages are logged to the browser’s console. Verbose logging can be enabled by setting the WZRK_D variable in the browser’s sessionStorage by running the command sessionStorage['WZRK_D'] = " "; in the console.

# Next Steps

By completing this guide, you tracked user events and created a user profile. You now have integrated CleverTap into your website.

If you have not integrated our mobile SDKs, our [iOS SDK Quick Start Guide](doc:ios-quickstart-guide) and [Android SDK Quick Start Guide](doc:android-quickstart-guide) are useful pages to explore next. If you have already integrated them, check out our [API Overview page](doc:api-overview) to get started on sending events and enriching user profiles from your server.