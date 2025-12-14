---
title: Cordova User Profiles
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
# User Profiles

## Update User Profile (Push Profile)

```javascript
// each of the below-mentioned fields are optional

var myStuff = {'bag','shoes'}
var props = {
        'Name': 'Jack Montana',                    // String
        'Email': jack@gmail.com',                  // Email address of the user
        'Phone': '+14155551234',                   // Phone (with the country code, starting with +)
        'Gender': 'M',                             // Can be either M or F
        'DOB' : new Date('1992-12-22T06:35:31'),   // Date of Birth. Set the Date object to the appropriate value first

// optional fields. controls whether the user will be sent email, push, etc.
        'MSG-email': false,                        // Disable email notifications
        'MSG-push': true,                          // Enable push notifications
        'MSG-sms': false,                          // Disable SMS notifications
        'MSG-whatsapp': true,                      // Enable WhatsApp notifications
        'Stuff': myStuff                           //Array of Strings for user properties
}

Clevertap.profileSet(props)
```

## Set Multi Values for Key

```javascript
// To set a multi-value property
var myStuff = {'bag','shoes'}
Clevertap.profileSetMultiValues('My Stuff', myStuff)
```

## Remove Multi Value for Key

```javascript
//To remove a value(s) from a multi-value property
Clevertap.profileRemoveMultiValue('My Stuff','bag');
// or
var myStuff = {'bag','shoes'}
Clevertap.profileRemoveMultiValues( 'My Stuff', myStuff);

//To remove the value of a property (scalar or multi-value)
CleverTap.profileRemoveValueForKey("My Stuff");
```

## Add Multi Value for Key

```javascript
// To add an additional value(s) to a multi-value property
CleverTap.profileAddMultiValue('My Stuff', 'coat')
// or
var myStuff = {'bag','shoes'}
CleverTap.profileAddMultiValues('My Stuff', myStuff)
```

## Create a User Profile (On User Login)

```javascript
// each of the below-mentioned fields are optional

var myStuff = {'bag','shoes'}
var props = {
        'Name': 'Jack Montana',                    // String
        'Identity': '61026032',                    // String or number
        'Email': 'jack@gmail.com',                 // Email address of the user
        'Phone': '+14155551234',                   // Phone (with the country code, starting with +)
        'Gender': 'M',                             // Can be either M or F
        'DOB' : new Date('1992-12-22T06:35:31'),   // Date of Birth. Set the Date object to the appropriate value first

// optional fields. controls whether the user will be sent email, push, etc.
        'MSG-email': false,                        // Disable email notifications
        'MSG-push': true,                          // Enable push notifications
        'MSG-sms': false,                          // Disable SMS notifications
        'MSG-whatsapp': true,                      // Enable WhatsApp notifications
        'Stuff': myStuff                           //Array of Strings for user properties
}

CleverTap.onUserLogin(props)
```

## Get CleverTap Reference id

```javascript
CleverTap.getCleverTapID(val => log("getCleverTapID is " + val))]
```

## Set Location to User Profile

```javascript
CleverTap.setLocation(34.15, -118.20)
```

## Increment a User Profile property

```javascript
// int values
CleverTap.profileIncrementValueBy('score',10)

//double values
CleverTap.profileIncrementValueBy('score',10.5)
```

## Decrement a User Profile property

```javascript
// int values
CleverTap.profileDecrementValueBy('score',10)

//double values
CleverTap.profileDecrementValueBy('score',10.5)
```