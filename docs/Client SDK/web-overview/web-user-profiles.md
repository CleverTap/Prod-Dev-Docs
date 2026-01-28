---
title: Web User Profiles
excerpt: Learn how to install and integrate your Web app.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
CleverTap stores the user's demographic data (gender, age, location), app and website interactions, campaign visits, and transaction history to give you a complete picture of every user.

# Create a User Profile When User Logs in (On User Login)

A user profile is automatically created for every user launching your web application, whether logged in or not.

At first, the user profile starts as _Anonymous_ which means that the profile does not contain any identifiable information about the user yet.

CleverTap automatically creates a user profile for each person visiting your website. At first, the user profile starts out as anonymous, which means the user profile does not contain any identifiable information.

Enrich the user profile with information, such as the user’s name or email, by calling `clevertap.onUserlogin`.

You can also manage category-level subscriptions using the following keys:

* `category-unsubscribe`: to opt users out of specific email categories
* `category-resubscribe`: to opt users back into previously unsubscribed categories

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
   "category-resubscribe": {
        	"email": ["Newsletter"]
    },
      "category-unsubscribe": {
        	"email": ["Articles"]
     }
 }
})
```

For more information, refer to the [Quick Start Guide](https://developer.clevertap.com/docs/android-quickstart-guide#identify-users).

# Update the User Profile

The `Onuserlogin` method identifies the individual users on the device. However, you may need to add additional user properties to the user profile, such as gender, date of birth, etc.

When you use `profile.push`, all the details sent are added to the current profile. Whereas, using `onuserlogin` assigns request details to the current profile if the user is unidentified. If the current profile is identified and belongs to the same user, the details are appended to the user's profile. If the current profile is for a different user, a new profile is created for the current user with the properties sent in the request.

Below are examples of updating these types of properties:

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

> 📘 Note
>
> You do not require to pass _Age_ as a profile property in the API because it is automatically calculated and stored.

CleverTap provides easy ways to enrich the user profile with data from sources such as Facebook. You can also store custom attributes in a user profile, which can be used to segment users later.

## Set User Location

You can enrich a user profile with location information by using the `getLocation()` method. This sets the `$ct.location` property on the user profile.

```javascript
clevertap.getLocation(lat, lng)
```

### Parameters

| Name  | Type   | Description                        |
| ----- | ------ | ---------------------------------- |
| `lat` | number | Optional. Latitude (-90 to 90).    |
| `lng` | number | Optional. Longitude (-180 to 180). |

### Behavior

* If you provide `lat` and `lng`, the values are validated and used to update the user's location. It sets `$ct.location` and calls `#sendLocationData`.
* If you do not provide values, the method will attempt to use the browser's [Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API) to automatically fetch the user's location with  `navigator.geolocation`.
* Errors are logged in case of validation or geolocation failure.

### Example Usage

```javascript
// Set location manually
clevertap.getLocation(37.7749, -122.4194);

// Auto-detect location via browser
clevertap.getLocation();
```

> 📘 Note
>
> If the browser blocks access to geolocation or the user denies permission, automatic location detection will fail silently.

## Increment/Decrement Operator in Web

Increment or decrement a user profile property by using the `handleIncrementValue(propName, propValue)` or `handleDecrementValue(propName, propValue)` methods for Clevertap Web SDK 1.3.0 version and above.

> 📘 Note
>
> The applicable user properties are of type Integer, Float, or Double. The value can be zero or greater than zero.

```javascript
clevertap.handleIncrementValue('price', 10)
clevertap.handleDecrementValue('price', 10)
```

Refer to [User Profile API Endpoints](doc:user-profile-object) for complete user profile documentation.
