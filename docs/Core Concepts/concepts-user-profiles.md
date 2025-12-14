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
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7ddbb47-7.png",
        "7.png",
        2880,
        1800,
        "#f9f9fa"
      ],
      "border": true
    }
  ]
}
[/block]
A CleverTap user profile has a set of default fields, such as email, phone number, and language. You can also extend the default user profile by adding custom fields that are specific to your business. 

For example, if you offer a subscription service in your app, you can create a custom profile field to track what type of plan the user purchased. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/218e3dd-8.png",
        "8.png",
        1298,
        770,
        "#fafafa"
      ],
      "border": true
    }
  ]
}
[/block]
The benefit of adding more information to a CleverTap user profile is creating a user segment for people with a specific profile property you define and then building a campaign to engage with that segment. The second benefit of adding more information to a CleverTap user profile is personalizing your campaign messaging with information. The third benefit is to [personalize your app](doc:app-personalization) based on information from that person's CleverTap user profile.

# User Profile Data Model

A CleverTap user profile consists of three things:
  * Identifiers: Each user profile is given a unique CleverTap id. You can also add other identifiers to recognize the user, including email, phone number, Facebook ID, or your own custom identifier.
  * Properties: This is information stored about the user. For example, this might include age, gender, device, and location. You can also extend the default user profile by adding custom fields that are specific to your business. 
  * Events: This is a log of actions taken by a user in your app. For example, this might include a product viewed, a video watched, or an item added to the cart.

## User Profile Types

The CleverTap user profile type changes automatically depending on the information set in them. A user profile can only belong to one type.

**Anonymous**
Anonymous profiles do not yet contain uniquely identifiable information about the user.

**Addressable**
Addressable user profiles are reachable either via email or push-notifications.

**Customer**
When you record a purchase via the Charged event, that user will be marked as a Customer.

# Unified User Profiles

CleverTap automatically unifies User Profiles across multiple devices when a user signs in to multiple devices using the same Identity. This helps you discover how the same user interacts with your app or website.

This also helps you get a more accurate count of the user, as it avoids double-counting the same user on multiple devices.

# Maintaining Multiple User Profiles on the Same Device

If multiple users on the same device use your app, you can use the onUserLogin API to assign them each a unique profile to track them separately. When initially installed on a device, your app is assigned an *anonymous* profile. The first time you identify a user on the device (whether via onUserLogin or profile.push/profilePush), the device's anonymous history will be associated with the newly identified user.

If you want to maintain a separate profile for each user, you can use onUserLogin to switch between subsequent separate identified users of your app on the same device. A good time to do this is immediately after a different user has logged into your app, and you have access to identifying information about that user.

To initiate the switch, call onUserLogin with the same form of properties dictionary as you would when doing a profile push. Please note that switching from one identified user to another is a costly operation in that the current session for the previous user is automatically closed and data relating to the old user removed, and a new session is started for the new user and data for that user refreshed via a network call to CleverTap. In addition, any global frequency caps are reset as part of the switch.
[block:code]
{
  "codes": [
    {
      "code": "// Do not call onUserLogin directly in the onCreate() lifecycle method\n\n// each of the below mentioned fields are optional\nHashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Name\", \"Jack Montana\");    // String\nprofileUpdate.put(\"Identity\", 61026032);      // String or number\nprofileUpdate.put(\"Email\", \"jack@gmail.com\"); // Email address of the user\nprofileUpdate.put(\"Phone\", \"+14155551234\");   // Phone (with the country code, starting with +)\nprofileUpdate.put(\"Gender\", \"M\");             // Can be either M or F\nprofileUpdate.put(\"DOB\", new Date());         // Date of Birth. Set the Date object to the appropriate value first\n\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate.put(\"MSG-email\", false);        // Disable email notifications\nprofileUpdate.put(\"MSG-push\", true);          // Enable push notifications\nprofileUpdate.put(\"MSG-sms\", false);          // Disable SMS notifications\nprofileUpdate.put(\"MSG-whatsapp\", true);      // Enable WhatsApp notifications\n\nArrayList<String> stuff = new ArrayList<String>();\nstuff.add(\"bag\");\nstuff.add(\"shoes\");\nprofileUpdate.put(\"MyStuff\", stuff);                        //ArrayList of Strings\n\nString[] otherStuff = {\"Jeans\",\"Perfume\"};\nprofileUpdate.put(\"MyStuff\", otherStuff);                   //String Array\n\n\nCleverTapAPI.getInstance(getApplicationContext()).onUserLogin(profileUpdate);",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// each of the below mentioned fields are optional \n// with the exception of one of Identity, Email, or FBID \nNSDictionary *profile = @{\n    @\"Name\": @\"Jack Montana\",       // String\n    @\"Identity\": @61026032,         // String or number\n    @\"Email\": @\"jack@gmail.com\",    // Email address of the user\n    @\"Phone\": @\"+14155551234\",      // Phone (with the country code, starting with +)\n    @\"Gender\": @\"M\",                // Can be either M or F                 \n\n// optional fields. controls whether the user will be sent email, push etc.\n    @\"MSG-email\": @NO,              // Disable email notifications\n    @\"MSG-push\": @YES,              // Enable push notifications\n    @\"MSG-sms\": @NO                 // Disable SMS notifications\n    @\"MSG-whatsapp\": @YES,          // Enable WhatsApp notifications\n};\n\n[[CleverTap sharedInstance] onUserLogin:profile];",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "// each of the below mentioned fields are optional\n// with the exception of one of Identity, Email, or FBID\nlet profile: Dictionary<String, AnyObject> = [\n    \"Name\": \"Jack Montana\",       // String\n    \"Identity\": 61026032,         // String or number\n    \"Email\": \"jack@gmail.com\",    // Email address of the user\n    \"Phone\": \"+14155551234\",      // Phone (with the country code, starting with +)\n    \"Gender\": \"M\",                // Can be either M or F               \n\n// optional fields. controls whether the user will be sent email, push etc.\n    \"MSG-email\": false,           // Disable email notifications\n    \"MSG-push\": true,             // Enable push notifications\n    \"MSG-sms\": false              // Disable SMS notifications\n    \"MSG-whatsapp\": true,         // Enable WhatsApp notifications\n]\n\nCleverTap.sharedInstance()?.onUserLogin(profile)",
      "language": "swift",
      "name": "iOS (Swift)"
    },
    {
      "code": "// each of the below mentioned fields are optional\n// with the exception of one of Identity, Email, or FBID\nclevertap.onUserLogin.push({\n \"Site\": {\n   \"Name\": \"Jack Montana\",            // String\n   \"Identity\": 61026032,              // String or number\n   \"Email\": \"jack@gmail.com\",         // Email address of the user\n   \"Phone\": \"+14155551234\",           // Phone (with the country code)\n   \"Gender\": \"M\",                     // Can be either M or F\n   \"DOB\": new Date(),                 // Date of Birth. Date object\n\n// optional fields. controls whether the user will be sent email, push etc.\n   \"MSG-email\": false,                // Disable email notifications\n   \"MSG-push\": true,                  // Enable push notifications\n   \"MSG-sms\": true,                   // Enable sms notifications\n   \"MSG-whatsapp\": true,              // Enable WhatsApp notifications\n   \n }\n});",
      "language": "javascript",
      "name": "Web"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Note : Using onUserLogin For Android",
  "body": "Do not call onUserLogin in the **onCreate()** Android LifeCycle method.\nSwitching from one identified user to another is a costly operation in that the current session for the previous user is automatically closed and data relating to the old user removed, and a new session is started for the new user and data for that user refreshed via a network call to CleverTap. In addition, any global frequency caps are reset as part of the switch along with the device token transfer."
}
[/block]

# Updating the User Profile

## Updating User Profile via Facebook
If the user signs up using FB connect, you can update the User Profile with name, gender etc. using the following code snippet.
[block:code]
{
  "codes": [
    {
      "code": "GraphRequest.newGraphPathRequest(accessToken,\n        \"/me?fields=id,name,email,birthday,gender,education,work\",\n        new GraphRequest.Callback() {\n            @Override\n            public void onCompleted(GraphResponse response) {\n                if (response != null && response.getJSONObject() != null) {\n                    cleverTap.pushFacebookUser(response.getJSONObject());\n                }\n            }\n        }).executeAsync();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "[[[FBSDKGraphRequest alloc] initWithGraphPath:@\"me?fields=id,name,email,birthday,gender,education,work\"\n                                   parameters:nil]\n startWithCompletionHandler:^(FBSDKGraphRequestConnection *connection, id result, NSError *error) {\n     if (!error) {\n         [[CleverTap push] graphUser:result];\n     }\n }];",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "FBSDKGraphRequest(graphPath: \"me?fields=id,name,email,birthday,gender,education,work\", parameters: nil).startWithCompletionHandler { (conn, result, error) in\n    if (error == nil) {\n        CleverTap.sharedInstance()?.profilePushGraphUser(result)\n    }\n}",
      "language": "swift",
      "name": "iOS (Swift)"
    },
    {
      "code": "FB.api('/me', {fields: 'id,name,email,birthday,gender,education,work'}, function(userData) {\n    clevertap.profile.push({\"Facebook\": userData});\n});",
      "language": "javascript",
      "name": "Web"
    },
    {
      "code": "CleverTapInstance.Profile.PushGraphUser(facebookUserJsonString);",
      "language": "csharp",
      "name": "WinRT"
    },
    {
      "code": "CleverTapInstance.Profile.PushGraphUser(facebookUserJsonString);",
      "language": "csharp",
      "name": "Silverlight"
    }
  ]
}
[/block]
The above code snippet will automatically update the user’s name and gender from Facebook. For a richer user profile, you’ll have to ask for the following permissions explicitly.
  * email – updates the user’s email address
  * user_birthday – used to show the age-group for demographic profile
[block:callout]
{
  "type": "warning",
  "body": "Note: Recommended permissions are email and user_birthday."
}
[/block]
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
[block:code]
{
  "codes": [
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nHashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Name\", \"Jack Montana\");                  // String\nprofileUpdate.put(\"Identity\", 61026032);                    // String or number\nprofileUpdate.put(\"Email\", \"jack@gmail.com\");               // Email address of the user\nprofileUpdate.put(\"Phone\", \"+14155551234\");                 // Phone (with the country code, starting with +)\nprofileUpdate.put(\"Gender\", \"M\");                           // Can be either M or F\nprofileUpdate.put(\"DOB\", new Date());                       // Date of Birth. Set the Date object to the appropriate value first\nprofileUpdate.put(\"Photo\", \"www.foobar.com/image.jpeg\");    // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate.put(\"MSG-email\", false);                      // Disable email notifications\nprofileUpdate.put(\"MSG-push\", true);                        // Enable push notifications\nprofileUpdate.put(\"MSG-sms\", false);                        // Disable SMS notifications\nprofileUpdate.put(\"MSG-whatsapp\", true);                    // Enable WhatsApp notifications\n\nArrayList<String> stuff = new ArrayList<String>();\nstuff.add(\"bag\");\nstuff.add(\"shoes\");\nprofileUpdate.put(\"MyStuff\", stuff);                        //ArrayList of Strings\n\nString[] otherStuff = {\"Jeans\",\"Perfume\"};\nprofileUpdate.put(\"MyStuff\", otherStuff);                   //String Array\n\ncleverTap.pushProfile(profileUpdate);\n",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nNSDateComponents *dob = [[NSDateComponents alloc] init];\ndob.day = 24;\ndob.month = 5;\ndob.year = 1992;\nNSDate *d = [[NSCalendar currentCalendar] dateFromComponents:dob];\nNSDictionary *profile = @{\n    @\"Name\": @\"Jack Montana\",               // String\n    @\"Identity\": @61026032,                 // String or number\n    @\"Email\": @\"jack@gmail.com\",            // Email address of the user\n    @\"Phone\": @\"+14155551234\",              // Phone (with the country code, starting with +)\n    @\"Gender\": @\"M\",                        // Can be either M or F\n    @\"DOB\": d,                              // Date of Birth. An NSDate object\n    @\"Photo\": @\"www.foobar.com/image.jpeg\", // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\n    @\"MSG-email\": @NO,                      // Disable email notifications\n    @\"MSG-push\": @YES,                      // Enable push notifications\n    @\"MSG-sms\": @NO                         // Disable SMS notifications\n    @\"MSG-whatsapp\": @YES,                  // Enable WhatsApp notifications\n};\n\n[[CleverTap sharedInstance] profilePush:profile];",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nlet dob = NSDateComponents()\ndob.day = 24\ndob.month = 5\ndob.year = 1992\nlet d = NSCalendar.currentCalendar().dateFromComponents(dob)\nlet profile: Dictionary<String, AnyObject> = [\n    \"Name\": \"Jack Montana\",                 // String\n    \"Identity\": 61026032,                   // String or number\n    \"Email\": \"jack@gmail.com\",              // Email address of the user\n    \"Phone\": \"+14155551234\",                // Phone (with the country code, starting with +)\n    \"Gender\": \"M\",                          // Can be either M or F\n    \"DOB\": d!,                              // Date of Birth. An NSDate object\n    \"Photo\": \"www.foobar.com/image.jpeg\",   // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\n    \"MSG-email\": false,                     // Disable email notifications\n    \"MSG-push\": true,                       // Enable push notifications\n    \"MSG-sms\": false                        // Disable SMS notifications\n    \"MSG-whatsapp\": true,                   // Enable WhatsApp notifications\n]\n\nCleverTap.sharedInstance()?.profilePush(profile)",
      "language": "swift",
      "name": "iOS (Swift)"
    },
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nclevertap.profile.push({\n \"Site\": {\n   \"Name\": \"Jack Montana\",                  // String\n   \"Identity\": 61026032,                    // String or number\n   \"Email\": \"jack@gmail.com\",               // Email address of the user\n   \"Phone\": \"+14155551234\",                 // Phone (with the country code)\n   \"Gender\": \"M\",                           // Can be either M or F\n   \"DOB\": new Date(), // Date of Birth. Javascript Date object\n   \"Photo\": 'www.foobar.com/image.jpeg',    // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\n   \"MSG-email\": false,                      // Disable email notifications\n   \"MSG-push\": true,                        // Enable push notifications\n   \"MSG-sms\": true                          // Enable sms notifications\n   \"MSG-whatsapp\": true,                    // Enable whatsapp notifications\n }\n});",
      "language": "javascript",
      "name": "Web"
    },
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nDictionary<string, object> profileUpdate = new Dictionary<string, object>();\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Name, \"Jack Montana\");                  //String\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Identity, 61026032);                    // String or number\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Email, \"jack@gmail.com\");              //Email address of the user\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Phone, 4155551234);                     //Phone (without the country code)\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Gender, \"M\");                           // Can be either M or F\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.DOB, new DateTime());                   // Date of Birth. Set the DateTime object to the appropriate value first\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Photo, \"www.foobar.com/image.jpeg\");    // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-email, false);                      // Disable email notifications\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-push, true);                        // Enable push notifications\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-sms, false);                        // Disable SMS notifications\nCleverTapInstance.Profile.Push(profileUpdate);",
      "language": "csharp",
      "name": "WinRT"
    },
    {
      "code": "// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nDictionary<string, object> profileUpdate = new Dictionary<string, object>();\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Name, \"Jack Montana\");                  // String\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Identity, 61026032);                    // String or number\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Email, \"jack@gmail.com\");              // Email address of the user\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Phone, 4155551234);                     // Phone (without the country code)\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Gender, \"M\");                           // Can be either M or F\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.DOB, new DateTime());                   // Date of Birth. Set the DateTime object to the appropriate value first                               //or a custom ID such as \"GMT-8:00\"\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.Photo, \"www.foodbar.com/image.jpeg\");   // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-email, false);                      // Disable email notifications\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-push, true);                        // Enable push notifications\nprofileUpdate.Add(CleverTapSDK.CleverTapProfile.MSG-sms, false);                        // Disable SMS notifications\nCleverTapInstance.Profile.Push(profileUpdate);",
      "language": "csharp",
      "name": "Silverlight"
    }
  ]
}
[/block]
## Manually Updating Single-Value User Profile Properties

CleverTap supports arbitrary (foo = bar) scalar-value (aka single-value) profile properties to be set against the User Profile as shown below.
[block:code]
{
  "codes": [
    {
      "code": "HashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Customer Type\", \"Silver\");\nprofileUpdate.put(\"Prefered Language\", \"English\");\n\ncleverTap.pushProfile(profileUpdate);\n\n/**\n * Data types\n * The value of a property can be of type Date (java.util.Date), an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n */",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "NSDictionary *profile = @{\n    @\"Customer Type\": @\"Silver\",\n    @\"Prefered Language\": @\"English\",\n};\n\n[[CleverTap sharedInstance] profilePush:profile];\n\n/**\n * Data types:\n * The value of a property can be of type NSDate, a NSNumber, a NSString, or a BOOL.\n */",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "let profile: Dictionary<String, AnyObject> = [\n    \"Customer Type\": \"Silver\",\n    \"Prefered Language\": \"English\"\n]\n\nCleverTap.sharedInstance()?.profilePush(profile)\n\n/**\n * Data types:\n * The value of a property can be of type NSDate, a Number, a String, or a Bool.\n */",
      "language": "swift",
      "name": "iOS (Swift)"
    },
    {
      "code": "clevertap.profile.push({\n \"Site\": {\n   \"Customer Type\": \"Silver\",\n   \"Prefered Language\": \"English\"\n }\n});\n\n/**\n * Data types\n * Event property keys must be Strings and property values must, with certain specific exceptions,\n * be scalar values, i.e. String, Boolean, Integer, or Float, or a Date object.\n *\n * Date object\n * When a property value is of type Date, the date and time are both recorded to the second.\n */",
      "language": "javascript",
      "name": "Web"
    },
    {
      "code": "Dictionary<string, object> profileUpdate = new Dictionary<string, object>();\nprofileUpdate.Add(\"Customer Type\", \"Silver\");\nprofileUpdate.Add(\"Prefered Language\", \"English\");\n\nCleverTapInstance.Profile.Push(profileUpdate);\n\n/**\n * Data types\n * The value of a property can be either a DateTime, an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n *\n * Date object\n * When you pass the value of the property as DateTime, the date and time are both recorded to the second.\n */",
      "language": "csharp",
      "name": "WinRT"
    },
    {
      "code": "Dictionary<string, object> profileUpdate = new Dictionary<string, object>();\nprofileUpdate.Add(\"Customer Type\", \"Silver\");\nprofileUpdate.Add(\"Prefered Language\", \"English\");\n\nCleverTapInstance.Profile.Push(profileUpdate);\n\n/**\n * Data types\n * The value of a property can be either a DateTime, an Integer, a Long, a Double,\n * a Float, a Character, a String, or a Boolean.\n *\n * Date object\n * When you pass the value of the property as DateTime, the date and time are both recorded to the second.\n */",
      "language": "csharp",
      "name": "Silverlight"
    }
  ]
}
[/block]
## Manually Updating Multi-Value User Profile Properties

CleverTap supports arbitrary multi-value profile properties to be set against the User Profile as shown below.
[block:callout]
{
  "type": "warning",
  "body": "Note that only the first property value of a multi-value user property is available when personalising notification content when creating campaigns."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "// To set a multi-value property\nArrayList<String> stuff = new ArrayList<String>();\nstuff.add(\"bag\");\nstuff.add(\"shoes\");\ncleverTap.setMultiValuesForKey(\"mystuff\", stuff);\n\n// To add an additional value(s) to a multi-value property\n cleverTap.addMultiValueForKey(\"mystuff\", \"coat\");\n// or\nArrayList<String> newStuff = new ArrayList<String>();\nnewStuff.add(\"socks\");\nnewStuff.add(\"scarf\");\ncleverTap.addMultiValuesForKey(\"mystuff\", newStuff);\n\n\n//To remove a value(s) from a multi-value property\ncleverTap.removeMultiValueForKey(\"mystuff\", \"bag\");\n// or\nArrayList<String> oldStuff = new ArrayList<String>();\noldStuff.add(\"shoes\");\noldStuff.add(\"coat\");\ncleverTap.removeMultiValuesForKey(\"mystuff\", oldStuff);\n//To remove the value of a property (scalar or multi-value)\ncleverTap.removeValueForKey(\"mystuff\");",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// To set a multi-value property\n[[CleverTap sharedInstance] profileSetMultiValues:@[@\"bag\", @\"shoes\"] forKey:@\"myStuff\"];\n\n// To add an additional value(s) to a multi-value property\n[[CleverTap sharedInstance] profileAddMultiValue:@\"coat\" forKey:@\"myStuff\"];\n// or\n[[CleverTap sharedInstance] profileAddMultiValues:@[@\"socks\", @\"scarf\"] forKey:@\"myStuff\"];\n\n//To remove a value(s) from a multi-value property\n[[CleverTap sharedInstance] profileRemoveMultiValue:@\"bag\" forKey:@\"myStuff\"];\n[[CleverTap sharedInstance] profileRemoveMultiValues:@[@\"shoes\", @\"coat\"] forKey:@\"myStuff\"];\n\n//To remove the value of a property (scalar or multi-value)\n[[CleverTap sharedInstance] profileRemoveValueForKey:@\"myStuff\"];",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "// To set a multi-value property\nCleverTap.sharedInstance()?.profileSetMultiValues([\"bag\", \"shoes\"], forKey: \"myStuff\")\n\n// To add an additional value(s) to a multi-value property\nCleverTap.sharedInstance()?.profileAddMultiValue(\"coat\", forKey: \"myStuff\")\n// or\nCleverTap.sharedInstance()?.profileAddMultiValues([\"socks\", \"scarf\"], forKey: \"myStuff\")\n\n//To remove a value(s) from a multi-value property\nCleverTap.sharedInstance()?.profileRemoveMultiValue(\"bag\", forKey: \"myStuff\")\nCleverTap.sharedInstance()?.profileRemoveMultiValues([\"shoes\", \"coat\"], forKey: \"myStuff\")\n\n//To remove the value of a property (scalar or multi-value)\nCleverTap.sharedInstance()?.profileRemoveValueForKey(\"myStuff\")",
      "language": "swift",
      "name": "iOS (Swift)"
    },
    {
      "code": "clevertap.profile.push({\n \"Site\": {\n   \"multi\":['bag','shoes'],\n }\n});",
      "language": "javascript",
      "name": "Web"
    }
  ]
}
[/block]
# Identifying a User

Use the Identity field of a User Profile to set it to your customer ID, email, or any other ID that uniquely identifies your user. You can also use the user’s Facebook ID (FBID) as the Identity. CleverTap uses Identity, Email, or FBID to merge events across different devices for the same user. A User Profile can have more than one Identity value associated with it.

Specifying the same Identity attribute on different devices for the same user will then automatically associate the user’s events on those devices with the consolidated CleverTap User Profile for that user.

You can pass identity via Profile Push or On-User Login on your mobile app or web browser.
Using one of the above methods, you can link one or more Identity values (Identity, Email, FBID ) to a specific User Profile.

## Accessing the CleverTap ID in Your App

CleverTap assigns each User Profile a unique identifier by default. This identifier is suitable for use in your application as a user identifier, particularly in the case where your application does not otherwise create a user identifier.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI.getInstance(getApplicationContext()).getCleverTapID();",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "[[CleverTap sharedInstance] profileGetCleverTapID];",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "CleverTap.sharedInstance()?.profileGetCleverTapID()",
      "language": "swift",
      "name": "iOS (Swift)"
    }
  ]
}
[/block]
# User Location Handling

The Android and iOS SDK’s provide convenience methods to access the device location. You can then set the location returned from these methods (or from your own location handling) on the User Profile to enable, among other things, finer-grained location based segmentation in CleverTap.
[block:code]
{
  "codes": [
    {
      "code": "//requires Location Permission in AndroidManifest e.g. \"android.permission.ACCESS_COARSE_LOCATION\"\nCleverTapAPI clevertap = CleverTapAPI.getInstance(getApplicationContext());\nLocation location = clevertap.getLocation();\n// do something with location, optionally set on CleverTap for use in segmentation etc\nclevertap.setLocation(location);",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "/*\nGet the device location if available. Will prompt the user location permissions dialog.\n \nPlease be sure to include the NSLocationWhenInUseUsageDescription key in your Info.plist.\nSee https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26\n\nUses desired accuracy of kCLLocationAccuracyHundredMeters.\n\nIf you need background location updates or finer accuracy please implement your own location handling.  \n Please see https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManager_Class/index.html for more info.\n \nOptional.  You can use location to pass it to CleverTap via the setLocation API\nfor, among other things, more fine-grained geo-targeting and segmentation purposes.\n*/\n\n[CleverTap getLocationWithSuccess:^(CLLocationCoordinate2D location) {\n     //do something with location here, optionally set on CleverTap for use in segmentation etc\n    [CleverTap setLocation:location];\n}\nandError:^(NSString *error) {\n    NSLog(@\"CleverTapLocation Error is %@\", error);\n}];",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "/*\nGet the device location if available. Will prompt the user location permissions dialog.\n \nPlease be sure to include the NSLocationWhenInUseUsageDescription key in your Info.plist.\nSee https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26\n\nUses desired accuracy of kCLLocationAccuracyHundredMeters.\n\nIf you need background location updates or finer accuracy please implement your own location handling.  \n Please see https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CLLocationManager_Class/index.html for more info.\n \nOptional.  You can use location to pass it to CleverTap via the setLocation API\nfor, among other things, more fine-grained geo-targeting and segmentation purposes.\n*/\n\nCleverTap.getLocationWithSuccess({(location: CLLocationCoordinate2D) -> Void in\n    // do something with location here, optionally set on CleverTap for use in segmentation etc\n    CleverTap.setLocation(location)\n}, andError: {(error: String?) -> Void in\n    if let e = error {\n        print(e)\n    }\n})",
      "language": "swift",
      "name": "iOS (Swift)"
    }
  ]
}
[/block]