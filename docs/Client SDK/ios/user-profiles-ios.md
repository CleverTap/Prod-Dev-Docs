---
title: iOS User Profiles
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

CleverTap stores the user's demographic data (gender, age, location), app and website interaction events, campaign visits, and transaction history to give you a complete picture of every user.

A user profile is automatically created for every user launching your mobile application – whether logged in or not.

Initially, the user profile starts as "Anonymous," meaning that the profile does not yet contain identifiable information about the user. You can choose to enrich the profile with attributes such as name, age, and customer id.

CleverTap provides pre-defined profile properties such as name, phone, gender, age, and so on to represent well-known properties associated with a profile. It is strongly recommended to use these standard property names. A list of all pre-defined property names is available [here](doc:concepts-user-profiles#manually-updating-predefined-user-profile-properties). In addition, we also support arbitrary single and multi-value profile properties.

## Create a User profile when user logs in (On User Login method)

```swift
let profile: Dictionary<String, AnyObject> = [
    //Update pre-defined profile properties
    "Name": "Jack Montana" as AnyObject,
    "Email": "jack.montana@gmail.com" as AnyObject,
  	"Identity": 61026032 as AnyObject,
    //Update custom profile properties
    "Plan type": "Silver" as AnyObject,
    "Favorite Food": "Pizza" as AnyObject,
]

CleverTap.sharedInstance()?.onUserLogin(profile)
```
```objectivec
NSDictionary *profile = @{
                              //Update pre-defined profile properties
                              @"Name": @"Jack Montana",
                              @"Email": @"jack.montana@gmail.com",
  							  @"Identity": @61026032,
                              //Update custom profile properties
                              @"Plan Type": @"Silver",
                              @"Favorite Food": @"Pizza"
                              };
    
[[CleverTap sharedInstance] onUserLogin:profile];
```

## Updating User Profile(Push Profile method)

Examples of updating profile properties for a project written in Swift and Objective-C are documented as follows:

```swift Predefined Swift
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
let dob = NSDateComponents()
dob.day = 24
dob.month = 5
dob.year = 1992
let d = NSCalendar.currentCalendar().dateFromComponents(dob)
let profile: Dictionary<String, AnyObject> = [
    "Name": "Jack Montana" as AnyObject,                 // String
    "Email": "jack@gmail.com" as AnyObject,              // Email address of the user
    "Phone": "+14155551234" as AnyObject,                // Phone (with the country code, starting with +)
    "Gender": "M" as AnyObject,                          // Can be either M or F
    "DOB": d! as AnyObject,                              // Date of Birth. An NSDate object
    "Photo": "www.foobar.com/image.jpeg" as AnyObject,   // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
    "MSG-email": false as AnyObject,                     // Disable email notifications
    "MSG-push": true as AnyObject,                       // Enable push notifications
    "MSG-sms": false as AnyObject,                       // Disable SMS notifications
    "MSG-dndPhone": true as AnyObject,                   // Opt out phone number from SMS notifications                                        
    "MSG-dndEmail": true as AnyObject,                   // Opt out email from email notifications                                                 
]

CleverTap.sharedInstance()?.profilePush(profile)
```
```swift Arbitrary Single Value Swift
let profile: Dictionary<String, AnyObject> = [
    "Customer Type": "Silver" as AnyObject,
    "Prefered Language": "English" as AnyObject
]

CleverTap.sharedInstance()?.profilePush(profile)

/**
 * Data types:
 * The value of a property can be of type NSDate, a Number, a String, or a Bool.
 */
```
```swift Arbitrary Multi Value Swift
// To set a multi-value property
CleverTap.sharedInstance()?.profileSetMultiValues(["bag", "shoes"], forKey: "myStuff")

// To add an additional value(s) to a multi-value property
CleverTap.sharedInstance()?.profileAddMultiValue("coat", forKey: "myStuff")
// or
CleverTap.sharedInstance()?.profileAddMultiValues(["socks", "scarf"], forKey: "myStuff")

//To remove a value(s) from a multi-value property
CleverTap.sharedInstance()?.profileRemoveMultiValue("bag", forKey: "myStuff")
CleverTap.sharedInstance()?.profileRemoveMultiValues(["shoes", "coat"], forKey: "myStuff")

//To remove the value of a property (scalar or multi-value)
CleverTap.sharedInstance()?.profileRemoveValueForKey("myStuff")
```
```objectivec Predefined Obj-C
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
NSDateComponents *dob = [[NSDateComponents alloc] init];
dob.day = 24;
dob.month = 5;
dob.year = 1992;
NSDate *d = [[NSCalendar currentCalendar] dateFromComponents:dob];
NSDictionary *profile = @{
    @"Name": @"Jack Montana",               // String
    @"Email": @"jack@gmail.com",            // Email address of the user
    @"Phone": @"+14155551234",              // Phone (with the country code, starting with +)
    @"Gender": @"M",                        // Can be either M or F
    @"Employed": @"Y",                      // Can be either Y or N
    @"Education": @"Graduate",              // Can be either Graduate, College or School
    @"Married": @"Y",                       // Can be either Y or N
    @"DOB": d,                              // Date of Birth. An NSDate object
    @"Tz": @"Asia/Kolkata",                 //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                            //or a custom ID such as "GMT-8:00"
    @"Photo": @"www.foobar.com/image.jpeg", // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
    @"MSG-email": @NO,                      // Disable email notifications
    @"MSG-push": @YES,                      // Enable push notifications
    @"MSG-sms": @NO                         // Disable SMS notifications
    @"MSG-dndPhone": @YES,                  // Opt out phone number from SMS                                                    notifications
    @"MSG-dndEmail": @YES,                  // Opt out email from email                                                    notifications
};

[[CleverTap sharedInstance] profilePush:profile];
```
```objectivec Arbitrary Single Value Obj-C
NSDictionary *profile = @{
    @"Customer Type": @"Silver",
    @"Prefered Language": @"English",
};

[[CleverTap sharedInstance] profilePush:profile];

/**
 * Data types:
 * The value of a property can be of type NSDate, a NSNumber, a NSString, or a BOOL.
 */
```
```objectivec Arbitrary Multi Value Obj-C
// To set a multi-value property
[[CleverTap sharedInstance] profileSetMultiValues:@[@"bag", @"shoes"] forKey:@"myStuff"];

// To add an additional value(s) to a multi-value property
[[CleverTap sharedInstance] profileAddMultiValue:@"coat" forKey:@"myStuff"];
// or
[[CleverTap sharedInstance] profileAddMultiValues:@[@"socks", @"scarf"] forKey:@"myStuff"];

//To remove a value(s) from a multi-value property
[[CleverTap sharedInstance] profileRemoveMultiValue:@"bag" forKey:@"myStuff"];
[[CleverTap sharedInstance] profileRemoveMultiValues:@[@"shoes", @"coat"] forKey:@"myStuff"];

//To remove the value of a property (scalar or multi-value)
[[CleverTap sharedInstance] profileRemoveValueForKey:@"myStuff"];
```

> 📘 Age Property Behavior in SDKs
>
> The _Age_ property cannot be set manually via the API. It is automatically calculated from the Date of Birth (DOB) and stored by the system.

CleverTap provides easy ways to enrich the user profile with data from sources, such as Facebook. You can also store custom attributes in a user profile. These attributes can later be used to segment users.

## Increment/Decrement Operator in iOS

Increment or decrement a user profile property by using the `profileIncrementValue` or `profileDecrementValue` methods for Clevertap iOS SDK 3.10.0 version and above.

> 📘 Data Type of User Properties
>
> The applicable user properties are of type Integer, Float, or Double. The value can be zero or greater than zero.

```swift
// Integer properties
CleverTap.sharedInstance()?.profileIncrementValue(by: 15, forKey: "score")         CleverTap.sharedInstance()?.profileDecrementValue(by: 15, forKey: "score")

// Float properties
CleverTap.sharedInstance()?.profileIncrementValue(by: 10.5, forKey: "cost")         CleverTap.sharedInstance()?.profileDecrementValue(by: 10.5, forKey: "cost")
```
```objectivec
#import <CleverTapSDK/CleverTap.h>

// Integer properties
[[CleverTap sharedInstance] profileIncrementValueBy: @3 forKey: @"score"];
[[CleverTap sharedInstance] profileDecrementValueBy: @3 forKey: @"score"];

// Float properties
[[CleverTap sharedInstance] profileIncrementValueBy: @10.5 forKey: @"cost"];
[[CleverTap sharedInstance] profileDecrementValueBy: @10.5 forKey: @"cost"];
```

## DND

You can opt out a user by either phone or email.

### Phone

You can opt-out a user or a phone number:

* To opt out a specific user when multiple users share the same phone number, you can disable SMS for a specific user. Set the MSG-sms flag to false, and the specified user stops receiving SMS; however, all other users sharing the number continue to receive messages if they have not opted out.
* To opt-out a phone number and all the users associated with it, set the MSG-dndPhone flag to _true_.

Refer to [User Profile API Endpoints](doc:user-profile-object) for complete user profile documentation.

### Email

You can opt-out a user or an email address:

* To opt out a specific user when multiple users share the same email address, you can disable email for a specific user. Set the MSG-email flag to false, and the specified user stops receiving messages; however, all other users sharing the email address continue to receive messages if they have not opted out.
* To opt-out an email address and all the users associated with it, set the MSG-dndEmail flag to _true_.

Refer to [User Profile API Endpoints](doc:user-profile-object) for complete user profile documentation.

## Custom Identity Management

Starting from CleverTap iOS SDK v4.0.1, you can enable custom identity management. The `onUserLogin()` method can identify users in advance (only for new app installs) whenever a user logs in to the app. You can use any of the following identifiers or a combination of them:

* Phone
* Email
* Identity  
  This process varies for default(shared) CleverTap instances and additional Instances.

### Default(shared) CleverTap instance

The CleverTap iOS SDK provides the default instance to call CleverTap methods. The `autoIntegrate()` method initializes this instance as follows:

1. Navigate to the app's `Info.plist` file in your project navigator.
2. Create a key called _CleverTapIdentifiers_ with the type `Array`.
3. Add the identifier keys for the user.

### Additional Instances

Additional instances pointing to a different CleverTap account can be created and used to call CleverTap methods. These can be initialized by a `CleverTapInstanceConfig` object and calling the `setIdentityKeys` property to an array of identifiers.

```swift
let ctConfig = CleverTapInstanceConfig.init(accountId: "ACCOUNT_ID", accountToken: "ACCOUNT_TOKEN")
ctConfig.identityKeys = ["Phone", "Email"]
let additionalInstance = CleverTap.instance(with: ctConfig)
```
```objectivec
#import <CleverTapSDK/CleverTap.h>
#import <CleverTapSDK/CleverTapInstanceConfig.h>

CleverTapInstanceConfig *ctConfig = [[CleverTapInstanceConfig alloc]initWithAccountId:@"ACCOUNT_ID" accountToken:@"ACCOUNT_TOKEN"];
ctConfig.identityKeys = @[@"Phone", @"Email"];
CleverTap *additionalCleverTapInstance = [CleverTap instanceWithConfig:ctConfig];
```

> 📘 Additional Notes
>
> * For existing app installs/legacy users, only the current/existing identifiers(Identity/Email) are utilized.
>
> * We strongly recommend not changing the identifiers for new app installs after the identifiers are set (via an instance config). This may result in an error message, and the original identifier keys will be used.
