---
title: Set CleverTap ID
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
Using the custom CleverTap ID capability, customers can generate and create their own unique identifiers for end-user devices. This allows you to have your own device identifier across all your systems without having to maintain a mapping of the device id across different systems
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Set CleverTap id capability is only available on both Android and iOS versions **3.5.0 and above**"
}
[/block]
# Rules for setting the CleverTap id

Following validations are applicable for setting custom CleverTap ID - 
* The device identifier should be a maximum 64 characters and a minimum of 1 character
* Characters should be alphanumeric (a-z, 0-9) case insensitive
* Allowed characters are (any character apart from the below mentioned will be treated as invalid id)
** A-Z
** a-z
** 0-9
** ( ) ! : @ $ _ -


For storing this ID in CleverTap, we will add prefix to custom CleverTap ID as per following rules
[block:parameters]
{
  "data": {
    "h-1": "adid",
    "h-2": "vendor",
    "h-3": "Rest(random)",
    "0-4": "_ _h",
    "h-4": "set ct id",
    "0-0": "Android",
    "0-1": "_ _g",
    "0-2": "n/a",
    "0-3": "_ _",
    "1-0": "iOS",
    "1-1": "-g",
    "1-2": "-v",
    "1-3": "-",
    "1-4": "-h"
  },
  "cols": 5,
  "rows": 2
}
[/block]
# Search
To search any particular profile by ID, use custom ID along with prefix in the search box


# Android
In order to send a custom ID, client must set the flag CLEVERTAP_USE_CUSTOM_ID to 1 in the AndroidManifest file. The flag can be set to 1 at AndroidManifest.xml

[block:code]
{
  "codes": [
    {
      "code": "<meta-data\n    android:name=\"CLEVERTAP_USE_CUSTOM_ID\"\n    android:value=\"1\"/>",
      "language": "xml"
    }
  ]
}
[/block]
Additionally, you must call any of the following method to pass the Custom CleverTap ID
[block:code]
{
  "codes": [
    {
      "code": "// To create a default instance with Custom CleverTap ID\nCleverTapAPI clevertapDefaultInstance =\n  CleverTapAPI.getDefaultInstance(getApplicationContext(),\"Custom_CleverTap_ID\");\n\n// To create an instance with CleverTapInstanceConfig object and Custom CleverTap ID\nCleverTapAPI instance =\nCleverTapAPI.instanceWithConfig(clevertapAdditionalInstanceConfig,\"Custom_CleverTap_ID\");\n\n// To pass Custom CleverTap ID to onUserLogin method\n// Do not call onUserLogin directly in the onCreate() lifecycle method\n\n// each of the below mentioned fields are optional\n// with the exception of one of Identity, Email, or FBID\nHashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Name\", \"Jack Montana\");    // String\nprofileUpdate.put(\"Identity\", 61026032);      // String or number\nprofileUpdate.put(\"Email\", \"jack@gmail.com\"); // Email address of the user\nprofileUpdate.put(\"Phone\", \"+14155551234\");   // Phone (with the country code, starting with +)\nprofileUpdate.put(\"Gender\", \"M\");             // Can be either M or F\nprofileUpdate.put(\"DOB\", new Date());         // Date of Birth. Set the Date object to the appropriate value first\n\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate.put(\"MSG-email\", false);        // Disable email notifications\nprofileUpdate.put(\"MSG-push\", true);          // Enable push notifications\nprofileUpdate.put(\"MSG-sms\", false);          // Disable SMS notifications\nprofileUpdate.put(\"MSG-whatsapp\", true);      // Enable WhatsApp notifications\n\nArrayList<String> stuff = new ArrayList<String>();\nstuff.add(\"bag\");\nstuff.add(\"shoes\");\nprofileUpdate.put(\"MyStuff\", stuff);                        //ArrayList of Strings\n\nString[] otherStuff = {\"Jeans\",\"Perfume\"};\nprofileUpdate.put(\"MyStuff\", otherStuff);                   //String Array\n\n\nCleverTapAPI.getInstance(getApplicationContext()).onUserLogin(profileUpdate,\"Custom_CleverTap_ID\");",
      "language": "java"
    }
  ]
}
[/block]
# iOS
* In order to send a custom ID, client must set the flag CleverTapUseCustomId to TRUE in the Plist file. 
* Navigate to the Info.plist file in your project navigator, and then create a key called CleverTapUseCustomId with type Boolean and set the value to YES.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6f3cdef-Screenshot_2019-05-17_at_5.36.59_PM.png",
        "Screenshot 2019-05-17 at 5.36.59 PM.png",
        1792,
        862,
        "#e6e5e6"
      ],
      "border": true
    }
  ]
}
[/block]
Additionally, you must call any of the following method to pass the Custom CleverTap ID

[block:code]
{
  "codes": [
    {
      "code": "[CleverTap autoIntegrateWithCleverTapID:@\"CUSTOM-CLEVERTAP-ID\"];\n\n[CleverTap sharedInstanceWithCleverTapID:@\"CUSTOM-CLEVERTAP-ID\"];\n\n// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nNSDateComponents *dob = [[NSDateComponents alloc] init];\ndob.day = 24;\ndob.month = 5;\ndob.year = 1992;\nNSDate *d = [[NSCalendar currentCalendar] dateFromComponents:dob];\nNSDictionary *profile = @{\n    @\"Name\": @\"Jack Montana\",               // String\n    @\"Identity\": @61026032,                 // String or number\n    @\"Email\": @\"jack@gmail.com\",            // Email address of the user\n    @\"Phone\": @\"+14155551234\",              // Phone (with the country code, starting with +)\n    @\"Gender\": @\"M\",                        // Can be either M or F\n    @\"Employed\": @\"Y\",                      // Can be either Y or N\n    @\"Education\": @\"Graduate\",              // Can be either Graduate, College or School\n    @\"Married\": @\"Y\",                       // Can be either Y or N\n    @\"DOB\": d,                              // Date of Birth. An NSDate object\n    @\"Age\": @28,                            // Not required if DOB is set\n    @\"Tz\": @\"Asia/Kolkata\",                 //an abbreviation such as \"PST\", a full name such as \"America/Los_Angeles\", \n                                            //or a custom ID such as \"GMT-8:00\"\n    @\"Photo\": @\"www.foobar.com/image.jpeg\", // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\n    @\"MSG-email\": @NO,                      // Disable email notifications\n    @\"MSG-push\": @YES,                      // Enable push notifications\n    @\"MSG-sms\": @NO                         // Disable SMS notifications\n};\n\n[[CleverTap sharedInstance] onUserLogin:profile withCleverTapID:@\"CUSTOM-CLEVERTAP-ID\"];",
      "language": "c",
      "name": "Objective-C"
    },
    {
      "code": "CleverTap.autoIntegrate(withCleverTapID: \"CUSTOM-CLEVERTAP-ID\")\n\nCleverTap.sharedInstance(withCleverTapID: \"CUSTOM-CLEVERTAP-ID\")\n\n// each of the below mentioned fields are optional\n// if set, these populate demographic information in the Dashboard\nlet dob = NSDateComponents()\ndob.day = 24\ndob.month = 5\ndob.year = 1992\nlet d = NSCalendar.currentCalendar().dateFromComponents(dob)\nlet profile: Dictionary<String, AnyObject> = [\n    \"Name\": \"Jack Montana\",                 // String\n    \"Identity\": 61026032,                   // String or number\n    \"Email\": \"jack@gmail.com\",              // Email address of the user\n    \"Phone\": \"+14155551234\",                // Phone (with the country code, starting with +)\n    \"Gender\": \"M\",                          // Can be either M or F\n    \"Employed\": \"Y\",                        // Can be either Y or N\n    \"Education\": \"Graduate\",                // Can be either School, College or Graduate\n    \"Married\": \"Y\",                         // Can be either Y or N\n    \"DOB\": d!,                              // Date of Birth. An NSDate object\n    \"Age\": 28,                              // Not required if DOB is set\n    \"Tz\":\"Asia/Kolkata\",                    //an abbreviation such as \"PST\", a full name such as \"America/Los_Angeles\", \n                                            //or a custom ID such as \"GMT-8:00\"\n    \"Photo\": \"www.foobar.com/image.jpeg\",   // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\n    \"MSG-email\": false,                     // Disable email notifications\n    \"MSG-push\": true,                       // Enable push notifications\n    \"MSG-sms\": false                        // Disable SMS notifications\n]\n\nCleverTap.sharedInstance()?.onUserLogin(profile, withCleverTapID: \"CUSTOM-CLEVERTAP-ID\")",
      "language": "swift",
      "name": "Swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "danger",
  "body": "* In case of invalid id OR id not set, CleverTap will NOT create a profile from the said device. \n* No events will be logged against these profiles. \n* All such errors will be logged with a profile created with the prefix _ _i \n* These profiles will be visible under the error stream under the *account/stream.html* page on the dashboard",
  "title": "Error Cases"
}
[/block]