---
title: User Profiles
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
# Overview

After you integrate our SDK, CleverTap creates a user profile for each person who launches your app or visits your website.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7ddbb47-7.png",
        "User Profile View on CleverTap Dashboard",
        2880
      ],
      "align": "center",
      "border": true,
      "caption": "User Profile Details"
    }
  ]
}
[/block]


A CleverTap user profile has a set of default fields, such as email, phone number, and language. You can also enrich the default user profile by adding custom fields specific to your business. 

For example, if you offer a subscription service in your app, you can create a custom profile field to track the type of plan the user purchased. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/218e3dd-8.png",
        "User Profile Properties",
        1298
      ],
      "align": "center",
      "border": true,
      "caption": "User Profile Properties"
    }
  ]
}
[/block]


Adding more information to a CleverTap user profile provides the following benefits:

- Create a user segment for people with a specific profile property you define and then build a campaign to engage with that segment. 
- Personalize your campaign messaging with information. 
- [Personalize your app](doc:app-personalization) based on information from that person's CleverTap user profile.

# User Profile Data Model

A CleverTap user profile consists of three things:

- Identifiers: Each user profile is given a unique CleverTap ID. You can also add other identifiers to recognize the user, including email, phone number, Facebook ID, or your custom identifier.
- Properties: Information stored about the user. For example, this might include age, gender, device, and location. You can also enrich the default user profile by adding custom fields specific to your business. 
- Events: This is a log of actions taken by a user in your app. For example, this might include a product viewed, a video watched, or an item added to the cart.

## User Profile Types

The CleverTap user profile type changes automatically depending on the information set in them. A user profile can only belong to one type.

- **Anonymous**: The user profile is termed Anonymous when the user launches any application or visits any website but has not logged in or registered yet. Here, the profile does not contain uniquely-identifiable information such as email/user ID. However, when the user launches the CleverTap-integrated app or website for the first time, CleverTap assigns a unique ID called CleverTap ID to help identify and track anonymous users.

- **Addressable**: User profiles are reachable via email or push notifications.

- **Customer**: The user will be marked as a Customer when you record a purchase via the Charged event.

# Unified User Profiles

CleverTap automatically unifies User Profiles across multiple devices when a user signs in to multiple devices using the same identity. This helps you discover how the same user interacts with your app or website.

This also helps you get a more accurate user count, as it avoids double-counting the same user on multiple devices.

# Maintaining Multiple User Profiles on the Same Device

If multiple users on the same device use your app, you can use the `onUserLogin` method to assign them a unique profile to track them separately. When initially installed on a device, your app is assigned an _anonymous_ profile. You can use the `profile.push` method to add more user properties to the identified profiles. The first time you identify a user on the device (whether via `onUserLogin` or `profilepush`/`profilePush`), the device's anonymous history will be associated with the newly identified user.

If you want to maintain a separate profile for each user, you can use `onUserLogin` to switch between subsequent separate identified users of your app on the same device. A good time to do this is immediately after a different user has logged into your app and you have access to identifying information about that user.

To initiate the switch, call `onUserLogin` with the same form of properties dictionary as you would when doing a profile push. Please note that switching from one identified user to another is a costly operation in that the current session for the previous user is automatically closed and data relating to the old user is removed, and a new session is started for the new user, and data for that user refreshed via a network call to CleverTap. In addition, any global frequency caps are reset as part of the switch.

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
profileUpdate.put("MSG-push", true);          // Indicates that the user has enabled push notification from the application. Works in combination with the DDND Flag. For detailed information, refer to communication preferences described later in this document.
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
```javascript React Native
// each of the below-mentioned fields are optional

var myStuff = ['bag','shoes']
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

CleverTap.onUserLogin(JSON.stringify(props))
```
```javascript Flutter/Dart
// each of the below-mentioned fields are optional

var myStuff = ["bag","shoes"];
var props = {
        'Name': 'Jack Montana',                    // String
        'Identity': '61026032',                    // String or number
        'Email': 'jack@gmail.com',                 // Email address of the user
        'Phone': '+14155551234',                   // Phone (with the country code, starting with +)
        'Gender': 'M',                             // Can be either M or F
        'DOB' : CleverTapPlugin.getCleverTapDate(new DateTime.now()),   // Date of Birth. Set the Date object to the appropriate value first

// optional fields. controls whether the user will be sent email, push, etc.
        'MSG-email': false,                        // Disable email notifications
        'MSG-push': true,                          // Enable push notifications
        'MSG-sms': false,                          // Disable SMS notifications
        'MSG-whatsapp': true,                      // Enable WhatsApp notifications
        'Stuff': myStuff                           //Array of Strings for user properties
};

CleverTapPlugin.onUserLogin(props);
```

> 🚧 Using onUserLogin For Android
> 
> Do not call `onUserLogin` in the `onCreate()` Android LifeCycle method. Switching from one identified user to another is a costly operation.  
> In that operation:
> 
> - The current session for the previous user is automatically closed.
> - Data relating to the old user is removed.
> - A new session is started for the new user.
> - The data for that user is refreshed via a network call to CleverTap.
> 
> In addition, any global frequency caps are reset as part of the switch, along with the device token transfer.

# Updating the User Profile

The `Onuserlogin` method identifies the individual users on the device. However, you may need to add additional user properties such as gender, date of birth, etc. You can update these profiles with the `clevertap.profile.push` method.

## Manually Updating Predefined User Profile Properties

CleverTap predefines specific profile property names that are common to most businesses. It is **strongly recommended** to use these standard property names. A list of all predefined property names is mentioned below:

- Name
- Email
- Identity
- Phone
- Gender
- DOB
- Age
- Timezone
- MSG-email
- MSG-push
- MSG-sms 
- MSG-whatsapp 

> 📘 Note
> 
> User profile properties are not case-sensitive.

User Profile Property Name is used for personalizing communication (push messages, email, sms) with the user.

User Profile Property Identity is used to identify a user. In-depth information on Identity is documented [here](https://docs.clevertap.com/docs/how-profiles-merge).

User Profile Properties such as Gender, Employed, Education, Married, DOB, and Age form the user's demographic profile (aka attributes). This data can be used to understand users performing certain events' demographic break-ups. It can also be used along with location and Events to segment and message users.

> 📘 DOB Record
> 
> In the user interface, a DOB record appears under the user profile property _Birthday_.

Profile properties such as `MSG-email`, `MSG-push`, `MSG-sms`, and `MSG-whatsapp` are utilized to configure the Do-Not-Disturb status for users. By default, the DND status is disabled for `MSG-email`, `MSG-push`, and `MSG-sms` as their values are set to TRUE. However, `MSG-whatsapp` has a default value of FALSE, indicating that the DND status is initially enabled for WhatsApp notifications. To disable the DND status for `MSG-whatsapp` update its property value to TRUE.

Example: To disable push notifications for a user, set MSG-push to false.

The communication preferences can be visible under a user’s profile and are analyzed using the following flags:

- MSG-push: This is the application-level flag for push notifications that work in combination with the device-level flag i.e., DDND Flag. This device-level flag is updated whenever the user navigates to the Settings of their phone to subscribe or unsubscribe from the push notifications. Consider the following example to understand how DDND works in combination with the MSG-push flag:

  | Scenario | MSG-push | DDND  | Push Campaign Delivered |
  | :------- | :------- | :---- | :---------------------- |
  | 1        | True     | False | Yes                     |
  | 2        | True     | True  | No                      |

  - **Scenario 1**  
    MSG-push = True: Indicates that the user has subscribed for push notifications from the application.  
    DDND = False: Indicates that the user has not disabled Push Notification for the application at the device level.  
    Therefore, the push notification is delivered to the user. If the user now unsubscribes from the settings, then the Channel Unsubscribe event is raised on the user’s profile and will now not receive push notifications.
  - **Scenario 2**  
    MSG-PUSH = True: Indicates that the user has subscribed for push notifications from the application.  
    DDND = True: Indicates that the user has disabled Push Notification for the application at the device level.  
    Therefore, the push notification is not delivered to the user. If the user now subscribes from the settings, then the Channel Subscribe event is raised on the user’s profile and will start receiving push notifications.
- MSG-sms: When set to true, it indicates that the user has subscribed to SMS notifications. When set to false, it indicates that the user does not want to receive SMS notifications.
- MSG-email: When set to true, it indicates that the user has subscribed to Email notifications. When set to false, it indicates that the user does not want to receive email notifications.
- MSG-whatsapp: When set to true, it indicates that the user has subscribed to WhatsApp notifications. When set to false, it indicates that the user does not want to receive WhatsApp notifications.

> 📘 MSG-email Exception
> 
> All flags are on the device level except for MSG-email.

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
   "MSG-sms": true,                          // Enable sms notifications
   "MSG-whatsapp": true,                    // Enable whatsapp notifications
 }
});
```
```javascript React Native
// each of the below-mentioned fields are optional

var myStuff = {'bag','shoes'}
var props = {
        'Name': 'Jack Montana',                    // String
        'Identity': '61026032',                    // String or number
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

CleverTap.profileSet(JSON.stringify(props))
```
```javascript Flutter/Dart
// each of the below-mentioned fields are optional

var myStuff = ["bag","shoes"];
var props = {
        'Name': 'Jack Montana',                    // String
        'Identity': '61026032',                    // String or number
        'Email': jack@gmail.com',                  // Email address of the user
        'Phone': '+14155551234',                   // Phone (with the country code, starting with +)
        'Gender': 'M',                             // Can be either M or F
        'DOB' : CleverTapPlugin.getCleverTapDate(new DateTime.now()),   // Date of Birth. Set the Date object to the appropriate value first

// optional fields. controls whether the user will be sent email, push, etc.
        'MSG-email': false,                        // Disable email notifications
        'MSG-push': true,                          // Enable push notifications
        'MSG-sms': false,                          // Disable SMS notifications
        'MSG-whatsapp': true,                      // Enable WhatsApp notifications
        'Stuff': myStuff                           //Array of Strings for user properties
}

CleverTapPlugin.profileSet(props);
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
```javascript React Native
var props = {'Customer Type': 'Silver',
             'Prefered Language': 'English')

CleverTap.profileSet(JSON.stringify(props))

/**
 * Data types
 * The value of a property can be either a DateTime, an Integer, a Long, a Double,
 * a Float, a Character, a String, or a Boolean.
 *
 * Date object
 * When you pass the value of the property as DateTime, the date and time are both recorded to the second.
 */
```
```javascript Flutter/Dart
var props = {'Customer Type': 'Silver',
             'Prefered Language': 'English');

CleverTapPlugin.profileSet(props);

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

CleverTap supports arbitrary multi-value profile properties to be set against the User Profile, as shown below. The updated value appears on the _Find People_ page under the ‘User properties’ section on the CleverTap dashboard.

> 🚧 Note
> 
> Only the first property value of a multi-value user property is available when personalizing notification content for a campaign.

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
clevertap.setMultiValuesForKey(key, value) // Eg: setMultiValuesForKey(‘stuff’, [‘bag’, ‘shoes’])
  clevertap.addMultiValueForKey(key, value) // Eg: addMultiValueForKey(‘stuff’, ‘coat’)
  clevertap.addMultiValuesForKey(key, value)// Eg: addMultiValuesForKey(‘stuff’, [‘cap’, ‘belt’])
  clevertap.removeMultiValueForKey(key, value)// Eg: removeMultiValueForKey(‘stuff’, ‘belt’)
  clevertap.removeMultiValuesForKey(key, value)// Eg: removeMultiValuesForKey(‘stuff’, [‘bag’, ‘cap’])
  clevertap.removeValueForKey(key) // Eg: removeValueForKey(‘stuff’)
```
```javascript React Native
// To set a multi-value property
var myStuff = {'bag','shoes'}
CleverTap.profileSetMultiValuesForKey(myStuff, 'My Stuff')

// To add an additional value(s) to a multi-value property
CleverTap.profileAddMultiValueForKey('coat', 'My Stuff')
// or
var myStuff = {'bag','shoes'}
CleverTap.profileAddMultiValuesForKey(myStuff, 'My Stuff')


//To remove a value(s) from a multi-value property
 CleverTap.profileRemoveMultiValueForKey('bag', 'My Stuff');
// or
var myStuff = {'bag','shoes'}
CleverTap.profileRemoveMultiValueForKey(myStuff, 'My Stuff');

//To remove the value of a property (scalar or multi-value)
CleverTap.profileRemoveValueForKey("My Stuff");
```
```javascript Flutter/Dart
// To set a multi-value property
var myStuff = ["bag","shoes"];
CleverTapPlugin.profileSetMultiValues("My Stuff", myStuff);

// To add an additional value(s) to a multi-value property
CleverTapPlugin.profileAddMultiValue("My Stuff", "coat");
// or
var myStuff = ["bag","shoes"];
CleverTapPlugin.profileAddMultiValues("My Stuff", myStuff);


//To remove a value(s) from a multi-value property
CleverTapPlugin.profileRemoveMultiValue("My Stuff", "bag");
// or
var myStuff = ["bag","shoes"];
CleverTapPlugin.profileRemoveMultiValues("My Stuff", myStuff);

//To remove the value of a property (scalar or multi-value)
CleverTap.profileRemoveValueForKey("My Stuff");
```

## Increment or Decrement the User Profile Property

> 📘 Note
> 
> The applicable user properties are of type Integer, Float, or Double. The value can be zero or greater than zero.

CleverTap provides you with the following increment or decrement operations for user profile property:

### Increment/Decrement Operator in Android

Increment or decrement a user profile property by using the `incrementValue` or `decrementValue` methods for Clevertap Android SDK version 4.2.0 and above.

### Increment/Decrement Operator in iOS

Increment or decrement a user profile property by using the `profileIncrementValue` or `profileDecrementValue` methods for Clevertap iOS SDK 3.10.0 version and above.

```java
// int values
CleverTapSingleton.getDefaultInstance(ctx).incrementValue("score",10);
CleverTapSingleton.getDefaultInstance(ctx).decrementValue("score",10);

// double values
CleverTapSingleton.getDefaultInstance(ctx).incrementValue("score",5.5);
CleverTapSingleton.getDefaultInstance(ctx).decrementValue("score",5.5);
```
```kotlin
// int values
CleverTapAPI.getDefaultInstance(ctx)!!.incrementValue("score",10)
CleverTapAPI.getDefaultInstance(ctx)!!.decrementValue("score",10)

// double values
CleverTapAPI.getDefaultInstance(ctx)!!.incrementValue("score",5.5)
CleverTapAPI.getDefaultInstance(ctx)!!.decrementValue("score",5.5)
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
```swift
// Integer properties
CleverTap.sharedInstance()?.profileIncrementValue(by: 15, forKey: "score")         CleverTap.sharedInstance()?.profileDecrementValue(by: 15, forKey: "score")


// Float properties
CleverTap.sharedInstance()?.profileIncrementValue(by: 10.5, forKey: "cost")         CleverTap.sharedInstance()?.profileDecrementValue(by: 10.5, forKey: "cost")
```
```javascript React Native
// int values
CleverTap.profileIncrementValueForKey(10, 'score')
CleverTap.profileDecrementValueForKey(10, 'score')

//double values
CleverTap.profileIncrementValueForKey(10.5, 'score')
CleverTap.profileDecrementValueForKey(10.5, 'score')
```
```javascript Flutter/Dart
// int values
CleverTapPlugin.profileIncrementValue("score", 10);
CleverTapPlugin.profileDecrementValue("score", 10);

//double values
CleverTapPlugin.profileIncrementValue("score", 10.5);
CleverTapPlugin.profileDecrementValue("score", 10.5);
```

### Increment/Decrement Operator in Web

Increment or decrement a user profile property by using the `handleIncrementValue(propName, propValue)` or `handleDecrementValue(propName, propValue)` methods for Clevertap Web SDK 1.3.0 version and above.

```javascript
clevertap.handleIncrementValue('price', 10)
clevertap.handleDecrementValue('price', 10)
```

# User Profile Consideration

After you integrate CleverTap SDK, CleverTap creates a user profile for each person who launches your application or visits your website.  
The following are the essential CleverTap User Profile considerations:

- You can set a maximum number of 256 custom attribute keys. 

- Attribute keys must be of type _String_ and attribute values can be scalar values, that is, String, Boolean, Integer, Float, or a Date object, or [multi-values](https://developer.clevertap.com/docs/concepts-user-profiles#multi-value-property-constraints) (returned as a JSONArray or NSArray/Array).

- Attribute key names are limited to 120 characters in length.

- Scalar attribute values are limited to 512 characters in length.

## Multi-value Property Constraints

The multi-value property has the following constraints:

- Values must be unique for that attribute key.

- Values must be Strings and are limited to 100 items. The excess items are removed on a FIFO (first-in, first-out) basis.

- Values are limited to 512 characters in length.

- When setting a multi-value property, any existing value is overwritten. 
  - If the multi-value property does not exist, it will be created when adding items to a multi-value property.  
  - If the attribute key currently contains a scalar value, the key will be promoted to a multi-value property, with the current value and the new values added. 

- When removing the items from a multi-value property: 
  - If the attribute key has a scalar value, then the key will be promoted to a multi-value property with the current scalar value before the remove operation. 
  - If the attribute key does not have any scalar value, the key will not be promoted to a multi-value property and will be removed after the removal operation.

# Identifying a User

Use the Identity field of a User Profile to set it to your customer ID, email, or any other ID that uniquely identifies your user. You can also use the user’s Facebook ID (FBID) as the Identity. CleverTap uses Identity, Email, or FBID to merge events across different devices for the same user. A User Profile can have more than one Identity value associated with it.

Specifying the same Identity attribute on different devices for the same user will then automatically associate the user’s events on those devices with the consolidated CleverTap User Profile for that user.

You can pass identity via Profile Push or On-User Login on your mobile app or web browser.  
Using one of the above methods, you can link one or more Identity values (Identity, Email, FBID) to a specific user profile.

## Accessing the CleverTap ID in Your App

CleverTap assigns each User Profile a unique identifier by default. This identifier is suitable for use in your application as a user identifier, particularly when your application does not otherwise create a user identifier.

```java Android
CleverTapAPI.getInstance(getApplicationContext()).getCleverTapID();
```
```objectivec iOS (Objective-C)
[[CleverTap sharedInstance] profileGetCleverTapID];
```
```swift iOS (Swift)
CleverTap.sharedInstance()?.profileGetCleverTapID()
```
```javascript Web
clevertap.getCleverTapID()
```
```javascript React Native
CleverTap.getCleverTapID((err, res) => {
        console.log('CleverTapID', res, err)
    });
```
```javascript Flutter/Dart
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

# User Location Handling

The Android and iOS SDKs provide convenient methods to access the device location. You can set the location returned from the following methods (or from your location handling) on the User Profile to enable the location-based segmentation in CleverTap.

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
```javascript React Native
CleverTap.setLocation(34.15, -118.20)
```
```javascript Flutter/Dart
var lat = 19.07;
var long = 72.87;
CleverTapPlugin.setLocation(lat, long);
```
```javascript Web
clevertap.getLocation(lat, lng)     // if lat, lng not provided , take location from navigator.geolocation API
```

# Export User Profiles

For more information on exporting user profiles, refer to [Export User Profiles](https://docs.clevertap.com/docs/data-export-to-aws-s3#q-how-can-i-export-data-from-clevertap).