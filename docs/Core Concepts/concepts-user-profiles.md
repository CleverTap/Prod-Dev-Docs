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

<Image title="7.png" alt={2880} className="border" border={true} src="https://files.readme.io/7ddbb47-7.png" />

A CleverTap user profile has a set of default fields, such as email, phone number, and language. You can also extend the default user profile by adding custom fields that are specific to your business. 

For example, if you offer a subscription service in your app, you can create a custom profile field to track what type of plan the user purchased. 

<Image title="8.png" alt={1298} className="border" border={true} src="https://files.readme.io/218e3dd-8.png" />

The benefit of adding more information to a CleverTap user profile is creating a user segment for people with a specific profile property you define and then building a campaign to engage with that segment. The second benefit of adding more information to a CleverTap user profile is personalizing your campaign messaging with information. The third benefit is to [personalize your app](doc:app-personalization) based on information from that person's CleverTap user profile.

# User Profile Data Model

A CleverTap user profile consists of three things:

* Identifiers: Each user profile is given a unique CleverTap id. You can also add other identifiers to recognize the user, including email, phone number, Facebook ID, or your own custom identifier.
* Properties: This is information stored about the user. For example, this might include age, gender, device, and location. You can also extend the default user profile by adding custom fields that are specific to your business. 
* Events: This is a log of actions taken by a user in your app. For example, this might include a product viewed, a video watched, or an item added to the cart.

## User Profile Types

The CleverTap user profile type changes automatically depending on the information set in them. A user profile can only belong to one type.

**Anonymous**\
Anonymous profiles do not yet contain uniquely identifiable information about the user.

**Addressable**\
Addressable user profiles are reachable either via email or push-notifications.

**Customer**\
When you record a purchase via the Charged event, that user will be marked as a Customer.

# Unified User Profiles

CleverTap automatically unifies User Profiles across multiple devices when a user signs in to multiple devices using the same Identity. This helps you discover how the same user interacts with your app or website.

This also helps you get a more accurate count of the user, as it avoids double-counting the same user on multiple devices.

# Maintaining Multiple User Profiles on the Same Device

If multiple users on the same device use your app, you can use the onUserLogin API to assign them each a unique profile to track them separately. When initially installed on a device, your app is assigned an *anonymous* profile. The first time you identify a user on the device (whether via onUserLogin or profile.push/profilePush), the device's anonymous history will be associated with the newly identified user.

If you want to maintain a separate profile for each user, you can use onUserLogin to switch between subsequent separate identified users of your app on the same device. A good time to do this is immediately after a different user has logged into your app, and you have access to identifying information about that user.

To initiate the switch, call onUserLogin with the same form of properties dictionary as you would when doing a profile push. Please note that switching from one identified user to another is a costly operation in that the current session for the previous user is automatically closed and data relating to the old user removed, and a new session is started for the new user and data for that user refreshed via a network call to CleverTap. In addition, any global frequency caps are reset as part of the switch.

```java Android
// Do not call onUserLogin directly in the onCreate() lifecycle method

// each of the below mentioned fields are optional
HashMap<String, Object> profileUpdate = new HashMap<String, Object>();
profileUpdate.put("Name", "Jack Montana");    // String
profileUpdate.put("Identity", 61026032);      // String or number
profileUpdate.put("Email", "jack@gmail.com"); // Email address of the user
profileUpdate.put("Phone", "+14155551234");   // Phone (with the country code, starting with +)
profileUpdate.put("Gender", "M");             // Can be either M or F
profileUpdate.put("DOB", new Date());         // Date of Birth. Set the Date object to the appropriate value first

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.put("MSG-email", false);        // Disable email notifications
profileUpdate.put("MSG-push", true);          // Enable push notifications
profileUpdate.put("MSG-sms", false);          // Disable SMS notifications
profileUpdate.put("MSG-whatsapp", true);      // Enable WhatsApp notifications

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
// with the exception of one of Identity, Email, or FBID 
NSDictionary *profile = @{
    @"Name": @"Jack Montana",       // String
    @"Identity": @61026032,         // String or number
    @"Email": @"jack@gmail.com",    // Email address of the user
    @"Phone": @"+14155551234",      // Phone (with the country code, starting with +)
    @"Gender": @"M",                // Can be either M or F                 

// optional fields. controls whether the user will be sent email, push etc.
    @"MSG-email": @NO,              // Disable email notifications
    @"MSG-push": @YES,              // Enable push notifications
    @"MSG-sms": @NO                 // Disable SMS notifications
    @"MSG-whatsapp": @YES,          // Enable WhatsApp notifications
};

[[CleverTap sharedInstance] onUserLogin:profile];
```
```swift iOS (Swift)
// each of the below mentioned fields are optional
// with the exception of one of Identity, Email, or FBID
let profile: Dictionary<String, AnyObject> = [
    "Name": "Jack Montana",       // String
    "Identity": 61026032,         // String or number
    "Email": "jack@gmail.com",    // Email address of the user
    "Phone": "+14155551234",      // Phone (with the country code, starting with +)
    "Gender": "M",                // Can be either M or F               

// optional fields. controls whether the user will be sent email, push etc.
    "MSG-email": false,           // Disable email notifications
    "MSG-push": true,             // Enable push notifications
    "MSG-sms": false              // Disable SMS notifications
    "MSG-whatsapp": true,         // Enable WhatsApp notifications
]

CleverTap.sharedInstance()?.onUserLogin(profile)
```
```javascript Web
// each of the below mentioned fields are optional
// with the exception of one of Identity, Email, or FBID
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
});
```

> 🚧 Note : Using onUserLogin For Android
>
> Do not call onUserLogin in the **onCreate()** Android LifeCycle method.\
> Switching from one identified user to another is a costly operation in that the current session for the previous user is automatically closed and data relating to the old user removed, and a new session is started for the new user and data for that user refreshed via a network call to CleverTap. In addition, any global frequency caps are reset as part of the switch along with the device token transfer.

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
                    cleverTap.pushFacebookUser(response.getJSONObject());
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

## Manually Updating Predefined User Profile Properties

CleverTap predefines certain profile property names that are common to most businesses. It is **strongly recommended** to use these standard property names. A list of all predefined property names is mentioned below:

* Name
* Email
* Identity
* Phone
* Gender
* DOB
* MSG-email
* MSG-push
* MSG-sms 
* MSG-whatsapp 

User Profile Property Name is used for personalizing communication (push messages, email, sms) with the user.

User Profile Property Identity is used to identify a user. In-depth information on Identity is documented [here](https://docs.clevertap.com/docs/how-profiles-merge).

User Profile Properties such as Gender, Employed, Education, Married, DOB and Age form the demographic profile (aka attributes) of the user. This data can be used to understand the demographic break-up of users performing certain Events. It can also be used along with location and Events to segment and message users.

*Note*: In the user interface, a DOB record shows up under the user profile property *Birthday*.

Profile properties MSG-email, MSG-push, MSG-sms and MSG-whatsapp are used to set the Do-Not-Disturb status for the user. Unless these are explicitly set to false, they are always true

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
profileUpdate.put("DOB", new Date());                       // Date of Birth. Set the Date object to the appropriate value first
profileUpdate.put("Photo", "www.foobar.com/image.jpeg");    // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
profileUpdate.put("MSG-email", false);                      // Disable email notifications
profileUpdate.put("MSG-push", true);                        // Enable push notifications
profileUpdate.put("MSG-sms", false);                        // Disable SMS notifications
profileUpdate.put("MSG-whatsapp", true);                    // Enable WhatsApp notifications

ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
profileUpdate.put("MyStuff", stuff);                        //ArrayList of Strings

String[] otherStuff = {"Jeans","Perfume"};
profileUpdate.put("MyStuff", otherStuff);                   //String Array

cleverTap.pushProfile(profileUpdate);
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
    @"DOB": d,                              // Date of Birth. An NSDate object
    @"Photo": @"www.foobar.com/image.jpeg", // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
    @"MSG-email": @NO,                      // Disable email notifications
    @"MSG-push": @YES,                      // Enable push notifications
    @"MSG-sms": @NO                         // Disable SMS notifications
    @"MSG-whatsapp": @YES,                  // Enable WhatsApp notifications
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
    "DOB": d!,                              // Date of Birth. An NSDate object
    "Photo": "www.foobar.com/image.jpeg",   // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
    "MSG-email": false,                     // Disable email notifications
    "MSG-push": true,                       // Enable push notifications
    "MSG-sms": false                        // Disable SMS notifications
    "MSG-whatsapp": true,                   // Enable WhatsApp notifications
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
   "DOB": new Date(), // Date of Birth. Javascript Date object
   "Photo": 'www.foobar.com/image.jpeg',    // URL to the Image

// optional fields. controls whether the user will be sent email, push etc.
   "MSG-email": false,                      // Disable email notifications
   "MSG-push": true,                        // Enable push notifications
   "MSG-sms": true                          // Enable sms notifications
   "MSG-whatsapp": true,                    // Enable whatsapp notifications
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
profileUpdate.Add(CleverTapSDK.CleverTapProfile.DOB, new DateTime());                   // Date of Birth. Set the DateTime object to the appropriate value first
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
profileUpdate.Add(CleverTapSDK.CleverTapProfile.DOB, new DateTime());                   // Date of Birth. Set the DateTime object to the appropriate value first                               //or a custom ID such as "GMT-8:00"
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

cleverTap.pushProfile(profileUpdate);

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

> 🚧 Note that only the first property value of a multi-value user property is available when personalising notification content when creating campaigns.

```java Android
// To set a multi-value property
ArrayList<String> stuff = new ArrayList<String>();
stuff.add("bag");
stuff.add("shoes");
cleverTap.setMultiValuesForKey("mystuff", stuff);

// To add an additional value(s) to a multi-value property
 cleverTap.addMultiValueForKey("mystuff", "coat");
// or
ArrayList<String> newStuff = new ArrayList<String>();
newStuff.add("socks");
newStuff.add("scarf");
cleverTap.addMultiValuesForKey("mystuff", newStuff);


//To remove a value(s) from a multi-value property
cleverTap.removeMultiValueForKey("mystuff", "bag");
// or
ArrayList<String> oldStuff = new ArrayList<String>();
oldStuff.add("shoes");
oldStuff.add("coat");
cleverTap.removeMultiValuesForKey("mystuff", oldStuff);
//To remove the value of a property (scalar or multi-value)
cleverTap.removeValueForKey("mystuff");
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
```javascript Web
clevertap.profile.push({
 "Site": {
   "multi":['bag','shoes'],
 }
});
```

# Identifying a User

Use the Identity field of a User Profile to set it to your customer ID, email, or any other ID that uniquely identifies your user. You can also use the user’s Facebook ID (FBID) as the Identity. CleverTap uses Identity, Email, or FBID to merge events across different devices for the same user. A User Profile can have more than one Identity value associated with it.

Specifying the same Identity attribute on different devices for the same user will then automatically associate the user’s events on those devices with the consolidated CleverTap User Profile for that user.

You can pass identity via Profile Push or On-User Login on your mobile app or web browser.\
Using one of the above methods, you can link one or more Identity values (Identity, Email, FBID ) to a specific User Profile.

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
