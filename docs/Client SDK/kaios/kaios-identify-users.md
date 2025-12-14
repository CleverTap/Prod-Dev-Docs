---
title: KaiOS Identify Users
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