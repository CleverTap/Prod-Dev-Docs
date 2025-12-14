---
title: User Profiles
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
# Overview

After you integrate our SDK, we will create a user profile for each person who launches your app or visits your website.

<Image title="profile2.png" alt={1408} className="border" border={true} src="https://files.readme.io/9098120-profile2.png" />

A CleverTap user profile has a set of default fields, such as email, phone number, and language. You can also extend the default user profile by adding custom fields that are specific to your business. 

For example, if you offer a subscription service in your app, you can create a custom profile field to track what type of plan the user purchased. 

<Image title="up2.png" alt={577} className="border" border={true} src="https://files.readme.io/76d6886-up2.png" />

The benefit of adding more information to a CleverTap user profile is the ability to create a user segment for people that have a specific profile property you define, and then build a campaign to engage with that segment. The second benefit of adding more information to a CleverTap user profile is the ability to personalize your campaign messaging with information. The third benefit is to [personalize your app](doc:app-personalization) based on information from that person's CleverTap user profile.

# User Profile Data Model

A CleverTap user profile consists of three things:

* Identifiers: Each user profile is given a unique CleverTap id. You can also add other identifiers to recognize the user including email, phone number, Facebook ID, Google Plus ID, or your own custom identifier.
* Properties: This is information stored about the user. For example, this might include age, gender, device, and location. You can also extend the default user profile by adding custom fields that are specific to your business. 
* Events: This is a log of actions taken by a user in your app. For example, this might include a product viewed, a video watched, or an item added to cart.

## User Profile Types

The CleverTap user profile type changes automatically depending on the information set in them. A user profile can only belong to one type.

**Anonymous**\
Anonymous profiles do not yet contain uniquely identifiable information about the user.

**Addressable**\
Addressable user profiles are reachable either via email or push-notifications.

**Customer**\
When you record a purchase via the Charged event, that user will be marked as a Customer.

# Updating the User Profile

## Updating User Profile via Facebook

If the user signs up using FB connect, you can update the User Profile with name, gender etc. using the following code snippet.

```java Android
GraphRequest.newGraphPathRequest(accessToken,
        "/me?fields=id,name,email,birthday,gender,education,work",
        new GraphRequest.Callback() {
            @Override
            public void onCompleted(GraphResponse response) {
                if (response != null && response.getJSONObject() != null) {
                    cleverTap.profile.pushFacebookUser(response.getJSONObject());
                }
            }
        }).executeAsync();
```
```objectivec iOS (Objective-C)
[[[FBSDKGraphRequest alloc] initWithGraphPath:@"me?fields=id,name,email,birthday,gender,education,work"
                                   parameters:nil]
 startWithCompletionHandler:^(FBSDKGraphRequestConnection *connection, id result, NSError *error) {
     if (!error) {
         [[CleverTap push] graphUser:result];
     }
 }];
```
```swift iOS (Swift)
FBSDKGraphRequest(graphPath: "me?fields=id,name,email,birthday,gender,education,work", parameters: nil).startWithCompletionHandler { (conn, result, error) in
    if (error == nil) {
        CleverTap.sharedInstance()?.profilePushGraphUser(result)
    }
}
```
```javascript Web
FB.api('/me', {fields: 'id,name,email,birthday,gender,education,work'}, function(userData) {
    clevertap.profile.push({"Facebook": userData});
});
```
```csharp WinRT
CleverTapInstance.Profile.PushGraphUser(facebookUserJsonString);
```
```csharp Silverlight
CleverTapInstance.Profile.PushGraphUser(facebookUserJsonString);
```

The above code snippet will automatically update the user’s name and gender from Facebook. For a richer user profile, you’ll have to ask for the following permissions explicitly.

* email – updates the user’s email address
* user\_birthday – used to show the age-group for demographic profile

> 🚧 Note: Recommended permissions are email and user\_birthday.

## Updating User Profiles via Google Plus

If the user signs up using Google Plus, you can push a Person object of the user to update their User Profile.

```java Android
@Override
public void onConnected(Bundle bundle) {
  Person currentPerson = Plus.PeopleApi.getCurrentPerson(mGoogleApiClient);
  cleverTap.profile.pushGooglePlusPerson(currentPerson);
}

// Recommended scopes for Google Plus are Plus.SCOPE_PLUS_LOGIN, and Plus.SCOPE_PLUS_PROFILE
```
```objectivec iOS (Objective-C)
GPPSignIn *signIn = [GPPSignIn sharedInstance];
GTLPlusPerson *p = signIn.googlePlusUser;
[[CleverTap sharedInstance] profilePushGooglePlusUser:p];

// Recommended scopes for Google Plus are kGTLAuthScopePlusMe, kGTLAuthScopePlusLogin, kGTLAuthScopePlusUserinfoEmail, and kGTLAuthScopePlusUserinfoProfile
```
```swift iOS (Swift)
let signIn = GPPSignIn.sharedInstance()
let p = signIn?.googlePlusUser
CleverTap.sharedInstance()?.profilePushGooglePlusUser(p!)

// Recommended scopes for Google Plus are kGTLAuthScopePlusMe, kGTLAuthScopePlusLogin, kGTLAuthScopePlusUserinfoEmail, and kGTLAuthScopePlusUserinfoProfile
```
```javascript Web
gapi.client.plus.people.get({
    'userId': 'me'}).execute(function(resp) {
    clevertap.profile.push({"Google Plus":resp});
});

// Recommended scopes for Google Plus are plus.login, plus.me, and E-mail
```
```csharp WinRT
CleverTapInstance.Profile.PushGooglePlusPerson(googlePlusUserJsonString);

// Recommended scopes for Google Plus are Plus.SCOPE_PLUS_LOGIN, and Plus.SCOPE_PLUS_PROFILE
```
```csharp Silverlight
CleverTapInstance.Profile.PushGooglePlusPerson(googlePlusUserJsonString);

// Recommended scopes for Google Plus are Plus.SCOPE_PLUS_LOGIN, and Plus.SCOPE_PLUS_PROFILE
```

## Manually Updating Predefined User Profile Properties

CleverTap predefines certain profile property names that are common to most businesses. It is **strongly recommended** to use these standard property names. A list of all predefined property names is mentioned below:\
Name

* Identity
* Phone
* Gender
* Employed
* Education
* Married
* DOB
* Age
* Tz
* MSG-email
* MSG-push
* MSG-sms 

User Profile Property Name is used for personalising communication (push messages, email, sms) with the user.

User Profile Property Identity is used to identify a user. In-depth information on Identity is documented here

User Profile Properties such as Gender, Employed, Education, Married, DOB and Age form the demographic profile (aka attributes) of the user. This data can be used to understand the demographic break-up of users performing certain Events. It can also be used along with location and Events to segment and message users.

Profile properties MSG-email, MSG-push, and MSG-sms are used to set the Do-Not-Disturb status for the user. Unless these are explicitly set to false, they are always true

Example: To disable push notifications for a user, set MSG-push to false

Any or all properties can be updated as shown in the code example below.

```java Android
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Name", "Jack Montana");                  // String
profileUpdate.put("Identity", 61026032);                    // String or number
profileUpdate.put("Email", "jack@gmail.com");               // Email address of the user
profileUpdate.put("Phone", "+14155551234");                 // Phone (with the country code, starting with +)
profileUpdate.put("Gender", "M");                           // Can be either M or F
profileUpdate.put("Employed", "Y");                         // Can be either Y or N
profileUpdate.put("Education", "Graduate");                 // Can be either Graduate, College or School
profileUpdate.put("Married", "Y");                          // Can be either Y or N
profileUpdate.put("DOB", new Date());                       // Date of Birth. Set the Date object to the appropriate value first
profileUpdate.put("Age", 28);                               // Not required if DOB is set
profileUpdate.put("Tz", "Asia/Kolkata");                    //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                                            //or a custom ID such as "GMT-8:00"
profileUpdate.put("Photo", "www.foobar.com/image.jpeg");    // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.put("MSG-email", false);                      // Disable email notifications
profileUpdate.put("MSG-push", true);                        // Enable push notifications
profileUpdate.put("MSG-sms", false);                        // Disable SMS notifications

ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
profileUpdate.put("MyStuff", stuff);                        //ArrayList of Strings

String[] otherStuff = {"Jeans","Perfume"};
profileUpdate.put("MyStuff", otherStuff);                   //String Array

cleverTap.profile.push(profileUpdate);
```
```objectivec iOS (Objective-C)
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
NSDateComponents *dob = [[NSDateComponents alloc] init];
dob.day = 24;
dob.month = 5;
dob.year = 1992;
NSDate *d = [[NSCalendar currentCalendar] dateFromComponents:dob];
NSDictionary *profile = @{
    @"Name": @"Jack Montana",               // String
    @"Identity": @61026032,                 // String or number
    @"Email": @"jack@gmail.com",            // Email address of the user
    @"Phone": @"+14155551234",              // Phone (with the country code, starting with +)
    @"Gender": @"M",                        // Can be either M or F
    @"Employed": @"Y",                      // Can be either Y or N
    @"Education": @"Graduate",              // Can be either Graduate, College or School
    @"Married": @"Y",                       // Can be either Y or N
    @"DOB": d,                              // Date of Birth. An NSDate object
    @"Age": @28,                            // Not required if DOB is set
    @"Tz": @"Asia/Kolkata",                 //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                            //or a custom ID such as "GMT-8:00"
    @"Photo": @"www.foobar.com/image.jpeg", // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
    @"MSG-email": @NO,                      // Disable email notifications
    @"MSG-push": @YES,                      // Enable push notifications
    @"MSG-sms": @NO                         // Disable SMS notifications
};

[[CleverTap sharedInstance] profilePush:profile];
```
```swift iOS (Swift)
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
let dob = NSDateComponents()
dob.day = 24
dob.month = 5
dob.year = 1992
let d = NSCalendar.currentCalendar().dateFromComponents(dob)
let profile: Dictionary<String, AnyObject> = [
    "Name": "Jack Montana",                 // String
    "Identity": 61026032,                   // String or number
    "Email": "jack@gmail.com",              // Email address of the user
    "Phone": "+14155551234",                // Phone (with the country code, starting with +)
    "Gender": "M",                          // Can be either M or F
    "Employed": "Y",                        // Can be either Y or N
    "Education": "Graduate",                // Can be either School, College or Graduate
    "Married": "Y",                         // Can be either Y or N
    "DOB": d!,                              // Date of Birth. An NSDate object
    "Age": 28,                              // Not required if DOB is set
    "Tz":"Asia/Kolkata",                    //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                            //or a custom ID such as "GMT-8:00"
    "Photo": "www.foobar.com/image.jpeg",   // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
    "MSG-email": false,                     // Disable email notifications
    "MSG-push": true,                       // Enable push notifications
    "MSG-sms": false                        // Disable SMS notifications
]

CleverTap.sharedInstance()?.profilePush(profile)
```
```javascript Web
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
clevertap.profile.push({
 "Site": {
   "Name": "Jack Montana",                  // String
   "Identity": 61026032,                    // String or number
   "Email": "jack@gmail.com",               // Email address of the user
   "Phone": "+14155551234",                 // Phone (with the country code)
   "Gender": "M",                           // Can be either M or F
   "Employed": "Y",                         // Can be either Y or N
   "Education": "Graduate",                 // Can be either School, College or Graduate
   "Married": "Y",                          // Can be either Y or N
   "DOB": new Date(), // Date of Birth. Javascript Date object
   "Age": 28,                               // Not required if DOB is set
   "Tz": "Asia/Kolkata",                    //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                            //or a custom ID such as "GMT-8:00"
   "Photo": 'www.foobar.com/image.jpeg',    // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
   "MSG-email": false,                      // Disable email notifications
   "MSG-push": true,                        // Enable push notifications
   "MSG-sms": true                          // Enable sms notifications
 }
});
```
```csharp WinRT
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
Dictionary<string, object> profileUpdate = new Dictionary<string, object>();
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Name, "Jack Montana");                  //String
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Identity, 61026032);                    // String or number
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Email, "jack@gmail.com");              //Email address of the user
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Phone, 4155551234);                     //Phone (without the country code)
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Gender, "M");                           // Can be either M or F
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Employed, "Y");                         // Can be either Y or N
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Education, "Graduate");                 // Can be either School, College or Graduate
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Married, "Y");                          // Can be either Y or N
profileUpdate.Add(CleverTapSDK.CleverTapProfile.DOB, new DateTime());                   // Date of Birth. Set the DateTime object to the appropriate value first
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Age, 28);                               // Not required if DOB is set
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Tz, "Asia/Kolkata");                    //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                                                                        //or a custom ID such as "GMT-8:00"
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Photo, "www.foobar.com/image.jpeg");    // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-email, false);                      // Disable email notifications
profileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-push, true);                        // Enable push notifications
profileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-sms, false);                        // Disable SMS notifications
CleverTapInstance.Profile.Push(profileUpdate);
```
```csharp Silverlight
// each of the below mentioned fields are optional
// if set, these populate demographic information in the Dashboard
Dictionary<string, object> profileUpdate = new Dictionary<string, object>();
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Name, "Jack Montana");                  // String
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Identity, 61026032);                    // String or number
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Email, "jack@gmail.com");              // Email address of the user
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Phone, 4155551234);                     // Phone (without the country code)
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Gender, "M");                           // Can be either M or F
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Employed, "Y");                         // Can be either Y or N
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Education, "Graduate");                 // Can be either School, College or Graduate
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Married, "Y");                          // Can be either Y or N
profileUpdate.Add(CleverTapSDK.CleverTapProfile.DOB, new DateTime());                   // Date of Birth. Set the DateTime object to the appropriate value first
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Age, 28);                               // Not required if DOB is set
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Tz, "Asia/Kolkata");                    //an abbreviation such as "PST", a full name such as "America/Los_Angeles", 
                                                                                        //or a custom ID such as "GMT-8:00"
profileUpdate.Add(CleverTapSDK.CleverTapProfile.Photo, "www.foodbar.com/image.jpeg");   // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-email, false);                      // Disable email notifications
profileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-push, true);                        // Enable push notifications
profileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-sms, false);                        // Disable SMS notifications
CleverTapInstance.Profile.Push(profileUpdate);
```

## Manually Updating Single-Value User Profile Properties

CleverTap supports arbitrary (foo = bar) scalar-value (aka single-value) profile properties to be set against the User Profile as shown below.

```java Android
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Customer Type", "Silver");
profileUpdate.put("Prefered Language", "English");

cleverTap.profile.push(profileUpdate);

/**
 * Data types
 * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 */
```
```objectivec iOS (Objective-C)
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
```swift iOS (Swift)
let profile: Dictionary<String, AnyObject> = [
    "Customer Type": "Silver",
    "Prefered Language": "English"
]

CleverTap.sharedInstance()?.profilePush(profile)

/**
 * Data types:
 * The value of a property can be of type NSDate, a Number, a String, or a Bool.
 */
```
```javascript Web
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
```csharp WinRT
Dictionary<string, object> profileUpdate = new Dictionary<string, object>();
profileUpdate.Add("Customer Type", "Silver");
profileUpdate.Add("Prefered Language", "English");

CleverTapInstance.Profile.Push(profileUpdate);

/**
 * Data types
 * The value of a property can be either a DateTime, an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 *
 * Date object
 * When you pass the value of the property as DateTime, the date and time are both recorded to the second.
 */
```
```csharp Silverlight
Dictionary<string, object> profileUpdate = new Dictionary<string, object>();
profileUpdate.Add("Customer Type", "Silver");
profileUpdate.Add("Prefered Language", "English");

CleverTapInstance.Profile.Push(profileUpdate);

/**
 * Data types
 * The value of a property can be either a DateTime, an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 *
 * Date object
 * When you pass the value of the property as DateTime, the date and time are both recorded to the second.
 */
```

## Manually Updating Multi-Value User Profile Properties

CleverTap supports arbitrary multi-value profile properties to be set against the User Profile as shown below.

> 🚧 Multi-value profile properties are only supported from Android and iOS SDKs. Also, note that only the first property value of a multi-value user property is available when personalising notification content when creating campaigns.

```java Android
// To set a multi-value property
ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
cleverTap.profile.setMultiValuesForKey("mystuff", stuff);

// To add an additional value(s) to a multi-value property
cleverTap.profile.addMultiValueForKey("mystuff", "coat");
// or
ArrayList<String> newStuff = new ArrayList<String>();
newStuff.add("socks");
newStuff.add("scarf");
cleverTap.profile.addMultiValuesForKey("mystuff", newStuff);


//To remove a value(s) from a multi-value property
cleverTap.profile.removeMultiValueForKey("mystuff", "bag");
// or
ArrayList<String> oldStuff = new ArrayList<String>();
oldStuff.add("shoes");
oldStuff.add("coat");
cleverTap.profile.removeMultiValuesForKey("mystuff", oldStuff);

//To remove the value of a property (scalar or multi-value)
cleverTap.profile.removeValueForKey("mystuff");
```
```objectivec iOS (Objective-C)
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
```swift iOS (Swift)
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

# Identifying a User

Use the Identity field of a User Profile to set it to your customer ID, email or any other ID that uniquely identifies your user. You can also use the user’s Facebook ID (FBID) or Google Plus ID (GPID) as the Identity. CleverTap uses Identity, Email, FBID, GPID or Email2 to merge events across different devices for the same user. A User Profile can have more than one Identity value associated with it.

Specifying the same Identity attribute on different devices for the same user will then automatically associate the user’s events on those devices with the consolidated CleverTap User Profile for that user.

On your website, one or more Identity values (Identity, Email, FBID, GPID or Email2) are linked to the same User Profile if they are sent within a minute. Otherwise, CleverTap assumes a different user has signed in on your site and creates a different user profile with the new Identity value(s).

On your mobile app, one or more Identity values (Identity, Email, FBID, GPID or Email2) are always linked to the User Profile associated with the mobile device, and are considered the same user.

## Accessing the CleverTap ID in Your App

CleverTap assigns each User Profile a unique identifier by default. This identifier is suitable for use in your application as a user identifier, particularly in the case where your application does not otherwise create a user identifier.

```java Android
CleverTapAPI.getInstance(getApplicationContext()).getCleverTapID();
```
```objectivec iOS (Objective-C)
[[CleverTap sharedInstance] profileGetCleverTapID];
```
```swift iOS (Swift)
CleverTap.sharedInstance()?.profileGetCleverTapID()
```

# User Location Handling

The Android and iOS SDK’s provide convenience methods to access the device location. You can then set the location returned from these methods (or from your own location handling) on the User Profile to enable, among other things, finer-grained location based segmentation in CleverTap.

```java Android
//requires Location Permission in AndroidManifest e.g. "android.permission.ACCESS_COARSE_LOCATION"
CleverTapAPI clevertap = CleverTapAPI.getInstance(getApplicationContext());
Location location = clevertap.getLocation();
// do something with location, optionally set on CleverTap for use in segmentation etc
clevertap.setLocation(location);
```
```objectivec iOS (Objective-C)
/*
Get the device location if available. Will prompt the user location permissions dialog.
 
Please be sure to include the NSLocationWhenInUseUsageDescription key in your Info.plist.
See https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26

Uses desired accuracy of kCLLocationAccuracyHundredMeters.

If you need background location updates or finer accuracy please implement your own location handling.  
 Please see https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManager_Class/index.html for more info.
 
Optional.  You can use location to pass it to CleverTap via the setLocation API
for, among other things, more fine-grained geo-targeting and segmentation purposes.
*/

[CleverTap getLocationWithSuccess:^(CLLocationCoordinate2D location) {
     //do something with location here, optionally set on CleverTap for use in segmentation etc
    [CleverTap setLocation:location];
}
andError:^(NSString *error) {
    NSLog(@"CleverTapLocation Error is %@", error);
}];
```
```swift iOS (Swift)
/*
Get the device location if available. Will prompt the user location permissions dialog.
 
Please be sure to include the NSLocationWhenInUseUsageDescription key in your Info.plist.
See https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26

Uses desired accuracy of kCLLocationAccuracyHundredMeters.

If you need background location updates or finer accuracy please implement your own location handling.  
 Please see https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManager_Class/index.html for more info.
 
Optional.  You can use location to pass it to CleverTap via the setLocation API
for, among other things, more fine-grained geo-targeting and segmentation purposes.
*/

CleverTap.getLocationWithSuccess({(location: CLLocationCoordinate2D) -> Void in
    // do something with location here, optionally set on CleverTap for use in segmentation etc
    CleverTap.setLocation(location)
}, andError: {(error: String?) -> Void in
    if let e = error {
        print(e)
    }
})
```

# Unified User Profiles

CleverTap automatically unifies User Profiles across multiple devices when a user signs into multiple devices using the same Identity. This helps you discover how the same user interacts with your app or website.

This also helps you get a more accurate count of the user, as it avoids double counting the same user on multiple devices.

# Maintaining Multiple User Profiles on the Same Device

If your app is used by multiple users on the same device, you can use the onUserLogin API to assign them each a unique profile to track them separately.

When initially installed on a device, your app is assigned an “anonymous” profile. The first time you identify a user on the device (whether via onUserLogin or profile.push/profilePush), the “anonymous” history on the device will be associated with the newly identified user.

If you wish to maintain a separate profile for each user, you can then use onUserLogin to switch between subsequent separate identified users of your app on the same device. A good time to do this is immediately after a different user has logged into your app and you have access to identifying information about that user.

If instead you wish to assign multiple Identity, Email, FBID and/or GPID values to the same user profile, push that profile information on the current user profile (via profile.push/profilePush) rather than onUserLogin.

To initiate the switch, call onUserLogin with the same form of properties dictionary as you would when doing a profile push, including a value for one of Identity, Email, FBID or GPID (if none of Identity, Email, FBID or GPID is included in the properties dictionary, all passed property values will be associated with the current user profile).

Please note that switching from one identified user to another is a costly operation in that the current session for the previous user is automatically closed and data relating to the old user removed, and a new session is started for the new user and data for that user refreshed via a network call to CleverTap. In addition, any global frequency caps are reset as part of the switch.

```java Android
// each of the below mentioned fields are optional
// with the exception of one of Identity, Email, FBID or GPID
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Name", "Jack Montana");    // String
profileUpdate.put("Identity", 61026032);      // String or number
profileUpdate.put("Email", "jack@gmail.com"); // Email address of the user
profileUpdate.put("Phone", "+14155551234");   // Phone (with the country code, starting with +)
profileUpdate.put("Gender", "M");             // Can be either M or F
profileUpdate.put("Employed", "Y");           // Can be either Y or N
profileUpdate.put("Education", "Graduate");   // Can be either Graduate, College or School
profileUpdate.put("Married", "Y");            // Can be either Y or N
profileUpdate.put("DOB", new Date());         // Date of Birth. Set the Date object to the appropriate value first
profileUpdate.put("Age", 28);                 // Not required if DOB is set

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.put("MSG-email", false);        // Disable email notifications
profileUpdate.put("MSG-push", true);          // Enable push notifications
profileUpdate.put("MSG-sms", false);          // Disable SMS notifications

ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
profileUpdate.put("MyStuff", stuff);                        //ArrayList of Strings

String[] otherStuff = {"Jeans","Perfume"};
profileUpdate.put("MyStuff", otherStuff);                   //String Array


CleverTapAPI.getInstance(getApplicationContext()).onUserLogin(profileUpdate);
```
```objectivec iOS (Objective-C)
// each of the below mentioned fields are optional 
// with the exception of one of Identity, Email, FBID or GPID
NSDictionary *profile = @{
    @"Name": @"Jack Montana",       // String
    @"Identity": @61026032,         // String or number
    @"Email": @"jack@gmail.com",    // Email address of the user
    @"Phone": @"+14155551234",      // Phone (with the country code, starting with +)
    @"Gender": @"M",                // Can be either M or F
    @"Employed": @"Y",              // Can be either Y or N
    @"Education": @"Graduate",      // Can be either Graduate, College or School
    @"Married": @"Y",               // Can be either Y or N
    @"Age": @28,                    

// optional fields. controls whether the user will be sent email, push etc.
    @"MSG-email": @NO,              // Disable email notifications
    @"MSG-push": @YES,              // Enable push notifications
    @"MSG-sms": @NO                 // Disable SMS notifications
};

[[CleverTap sharedInstance] onUserLogin:profile];
```
```swift iOS (Swift)
// each of the below mentioned fields are optional
// with the exception of one of Identity, Email, FBID or GPID
let profile: Dictionary<String, AnyObject> = [
    "Name": "Jack Montana",       // String
    "Identity": 61026032,         // String or number
    "Email": "jack@gmail.com",    // Email address of the user
    "Phone": "+14155551234",      // Phone (with the country code, starting with +)
    "Gender": "M",                // Can be either M or F
    "Employed": "Y",              // Can be either Y or N
    "Education": "Graduate",      // Can be either School, College or Graduate
    "Married": "Y",               // Can be either Y or N
    "Age": 28,                    

// optional fields. controls whether the user will be sent email, push etc.
    "MSG-email": false,           // Disable email notifications
    "MSG-push": true,             // Enable push notifications
    "MSG-sms": false              // Disable SMS notifications
]

CleverTap.sharedInstance()?.onUserLogin(profile)
```
```javascript Web
// each of the below mentioned fields are optional
// with the exception of one of Identity, Email, FBID or GPID
clevertap.onUserLogin.push({
 "Site": {
   "Name": "Jack Montana",            // String
   "Identity": 61026032,              // String or number
   "Email": "jack@gmail.com",         // Email address of the user
   "Phone": "+14155551234",           // Phone (with the country code)
   "Gender": "M",                     // Can be either M or F
   "Employed": "Y",                   // Can be either Y or N
   "Education": "Graduate",           // Can be either School, College or Graduate
   "Married": "Y",                    // Can be either Y or N
   "DOB": new Date(),                 // Date of Birth. Date object
   "Age": 28,                         // Not required if DOB is set

// optional fields. controls whether the user will be sent email, push etc.
   "MSG-email": false,                // Disable email notifications
   "MSG-push": true,                  // Enable push notifications
   "MSG-sms": true                    // Enable sms notifications
 }
});
```

# Platform Considerations

## General Constraints

* In a User Profile, you can set a maximum number of 256 custom attribute keys
* User Profile attribute keys must be of type String and attribute values can be scalar values, i.e. String, Boolean, Integer, Float or a Date object or multi-values (returned as a JSONArray or NSArray/Array).
* Attribute key names are limited to 120 characters in length.
* Scalar attribute values are limited to 512 characters in length.

## Multi-value Property Constraints

* Multi-values property values must be unique for that key.
* Multi-value property values must be Strings and are limited to 100 items. Excess items will be removed on a FIFO (first-in, first-out) basis.
* Multi-value property values are limited to 512 characters in length.
* When setting a multi-value property, any existing value will be overwritten.
* When adding item(s) to a multi-value property, if the property does not exist it will be created. If the key currently contains a scalar value, the key will be promoted to a multi-value property, with the current value cast to a string and the new value(s) added.
* When removing item(s) from a multi-value property, if the key currently contains a scalar value, prior to performing the remove operation, the key will be promoted to a multi-value property with the current value cast to a string. If the multi-value property is empty after the remove operation, the key will be removed.
