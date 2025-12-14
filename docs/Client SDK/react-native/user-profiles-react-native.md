---
title: React Native User Profiles
excerpt: Understand how to update user profiles with React Native.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# User Profiles

## Create a User profile when the User Logs in/Signs Up (On User Login)

CleverTap stores the user's demographic data (gender, age, location), app and website interaction events, campaign visits, and transaction history for every user.

A user profile is automatically created for every user launching your mobile application – whether logged in or not.

Initially, the user profile starts as *Anonymous*, meaning that the profile does not yet contain identifiable information about the user. You can enrich the profile with attributes such as name, age, and customer id.

CleverTap provides pre-defined profile properties such as name, phone, gender, age, and so on to represent well-known properties associated with a profile. It is strongly recommended to use these standard property names. A list of all pre-defined property names is available in the below code snippet. In addition, we also support arbitrary single and multi-value profile properties.

To send user profile information to CleverTap using our React Native SDK:

1. Create an object with the profile properties.
2. Call the SDK's `onUserLogin` method and pass the stringified object created in *step 1*.

```javascript
// Each of the following fields are optional.

var myStuff = ['bag', 'shoes'];

var props = {
  'Name': 'Jack Montana',                   // String
  'Identity': '61026032',                   // String or number
  'Email': 'jack@gmail.com',                // Email address of the user
  'Phone': '+14155551234',                  // Phone number (include country code, starting with +)
  'Gender': 'M',                            // Can be either M or F
  'DOB': new Date('2025-03-03T06:35:31'),   // 'DOB' is case-sensitive. Accepts a Date object (preferred) like new Date('2025-03-03T06:35:31') or a string in 'yyyy-mm-dd' format like '2025-03-03'.

  // Optional fields: Control whether the user receives email, push, or other notifications.
  'MSG-email': false,                       // Disable email notifications
  'MSG-push': true,                         // Enable push notifications
  'MSG-sms': false,                         // Disable SMS notifications
  'MSG-whatsapp': true,                     // Enable WhatsApp notifications

  'Stuff': myStuff                          // Array of strings for custom user properties
};

// Update user profile
CleverTap.onUserLogin(props);
```

## Update User Profile(Push Profile)

The `onUserLogin` method identifies the individual users on the device. However, you may need to add additional user properties to the user profile such as gender, date of birth, and so on. For this scenario, you may use 'profileSet' method . Below is an examples of updating these properties:

```javascript
// each of the below-mentioned fields are optional

var myStuff = ['bag','shoes']
var props = {
        'Name': 'Jack Montana',                    // String
        'Email': jack@gmail.com',                  // Email address of the user
        'Phone': '+14155551234',                   // Phone (with the country code, starting with +)
        'Gender': 'M',                             // Can be either M or F
        'DOB' : new Date('1992-12-22T06:35:31'),   // 'DOB' is case-sensitive. Accepts a Date object (preferred) like new Date('1992-12-22T06:35:31') or a string in 'yyyy-mm-dd' format like '1992-12-22'.

// optional fields. controls whether the user will be sent email, push, etc.
        'MSG-email': false,                        // Disable email notifications
        'MSG-push': true,                          // Enable push notifications
        'MSG-sms': false,                          // Disable SMS notifications
        'MSG-whatsapp': true,                      // Enable WhatsApp notifications
        'Stuff': myStuff                           //Array of Strings for user properties
}

CleverTap.profileSet(props)
```

## Set Multi Values For Key

```javascript
// To set a multi-value property
var myStuff = ['bag','shoes']
CleverTap.profileSetMultiValuesForKey(myStuff, 'My Stuff')
```

## Remove Multi Value For Key

```javascript
//To remove a value(s) from a multi-value property
 CleverTap.profileRemoveMultiValueForKey('bag', 'My Stuff');
// or
var myStuff = ['bag','shoes']
CleverTap.profileRemoveMultiValueForKey(myStuff, 'My Stuff');

//To remove the value of a property (scalar or multi-value)
CleverTap.profileRemoveValueForKey("My Stuff");
```

## Add Multi Value For Key

```javascript
// To add an additional value(s) to a multi-value property
CleverTap.profileAddMultiValueForKey('coat', 'My Stuff')
// or
var myStuff = ['bag','shoes']
CleverTap.profileAddMultiValuesForKey(myStuff, 'My Stuff')
```

## Get CleverTap Reference id

```javascript
CleverTap.getCleverTapID((err, res) => {
        console.log('CleverTapID', res, err)
    });
```

## Set Location to User Profile

```javascript
CleverTap.setLocation(34.15, -118.20)
```

## Increment a User Profile property

```text
// int values
CleverTap.profileIncrementValueForKey(10, 'score')

//double values
CleverTap.profileIncrementValueForKey(10.5, 'score')
```

## Decrement a User Profile property

```text
// int values
CleverTap.profileDecrementValueForKey(10, 'score')

//double values
CleverTap.profileDecrementValueForKey(10.5, 'score')
```
