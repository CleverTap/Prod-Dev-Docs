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
# Identify Users

CleverTap automatically creates a user profile for every person visiting your website. At first, the user profile starts out as anonymous which means the user profile does not contain any identifiable information. 

1. Enrich the user profile with information, such as the user’s name or email, by calling `clevertap.onuserlogin`.

Here is an example showing how to add a name and an email to a user’s profile:

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

2. In addition to our JavaScript library, we also provide a [profile API](https://developer.clevertap.com/docs/user-profile-object) to update user profiles from your server.
