---
title: Flutter User Profiles
excerpt: Understand how to update user profiles in Flutter SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## User Profiles

CleverTap stores the user's demographic data (gender, age, location), app and website interaction events, campaign visits, and transaction history to give you a complete picture of every user.

A user profile is automatically created for every user launching your mobile application – whether logged in or not.

Initially, the user profile starts as "Anonymous," meaning that the profile does not yet contain identifiable information about the user. You can choose to enrich the profile with attributes such as name, age, and customer id.

CleverTap provides pre-defined profile properties such as name, phone, gender, age, and so on to represent well-known properties associated with a profile. It is strongly recommended to use these standard property names. A list of all pre-defined property names is available here. In addition, we also support arbitrary single and multi-value profile properties.

## Create a User profile when user logs in (On User Login method)

```javascript Dart
var stuff = ["bags", "shoes"];
var profile = {
    'Name': 'Captain America',
    'Identity': '100',
    'Email': 'captain@america.com',
    'Phone': '+14155551234',
    'stuff': stuff
};
CleverTapPlugin.onUserLogin(profile);
```

## Updating User Profile(Profile Set Method)

Examples of updating profile properties for a project written in Dart are documented as follows:

```javascript Dart
var stuff = ["bags", "shoes"];
var dob = '2012-04-22';
var profile = {
    'Name': 'John Wick',
    // Key always has to be "dob" and format should always be yyyy-MM-dd
    'dob': CleverTapPlugin.getCleverTapDate(DateTime.parse(dob)),
    'Email': 'john@gmail.com',
    'Phone': '+14155551234',
    'props': 'property1',
    'stuff': stuff
};
CleverTapPlugin.profileSet(profile);
```

### Set Multi Values For Key

```javascript Dart
var values = ["value1", "value2"];
CleverTapPlugin.profileSetMultiValues("props", values);
```

CleverTap provides easy ways to enrich the user profile with data from sources, such as Facebook. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

### Remove Multi Value For Key

```javascript Dart
var values = ["value1", "value2"];
CleverTapPlugin.profileRemoveMultiValues("props", values);
```

### Add Multi Value For Key

```javascript Dart
var values = ["value1", "value2"];
CleverTapPlugin.profileAddMultiValues("props", values);
```

### Increment/Decrement Operator in Flutter

Increment or decrement a user profile property by using the `profileIncrementValue` or `profileDecrementValue` methods.

```javascript Dart
CleverTapPlugin.profileIncrementValue("score", value);
```

Decrement Value For Key

```javascript Dart
CleverTapPlugin.profileDecrementValue("score", value);
```

### Get CleverTap Reference id

```javascript Dart
CleverTapPlugin.getCleverTapID().then((clevertapId) {
      setState((() {
        print("$clevertapId");
      }));
    }).catchError((error) {
      setState(() {
        print("$error");
      });
    });
```

### Set Location to User Profile

You can login custom location to CleverTap dashboard.

```javascript Dart
var lat = 19.07;
var long = 72.87;
CleverTapPlugin.setLocation(lat, long);
```

## DND

### Phone

You can opt-out of a user or a phone number. 

* To opt-out of a specific user when multiple users share the same phone number,  you can disable SMS for a particular user. Set the `MSG-sms` flag to false, and the specified user stops receiving SMS. However, all other users sharing the number continue to receive messages if they have not opted out.   
* To opt out of a phone number and all the associated users, set the `MSG-dndPhone` flag to true.

Refer to [User Profile API Endpoints](doc:user-profile-object) for complete user profile documentation.

### Email

You can opt out of a user or an email address.  

* To opt-out of a specific user when multiple users share the same email address,  you can disable email for a particular user. Set the `MSG-email` flag to false, and the specified user stops receiving email. However, all other users sharing the email address continue to receive messages if they have not opted out.  
* To opt-out of an email and all its associated users, set the `MSG-dndEmail` flag to true.

Refer to [User Profile API Endpoints](doc:user-profile-object) for complete user profile documentation.
