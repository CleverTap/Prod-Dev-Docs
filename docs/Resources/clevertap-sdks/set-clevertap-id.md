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
## Introduction

Using the custom CleverTap ID capability, customers can generate and create their own unique identifiers for end user devices. This allows you to have your own device identifier across all your systems without having to maintain a mapping of the device id across different systems

> 📘 Note
>
> Set CleverTap id capability is only available on both Android and iOS versions **3.5.0 and above**

# Rules for setting the CleverTap id

Following validations are applicable for setting custom CleverTap ID - 

* The device identifier should be a maximum 64 characters and a minimum of 1 character
* Characters should be alphanumeric (a-z, 0-9) case insensitive
* Allowed characters are (any character apart from the below mentioned will be treated as invalid id)\
  **A-Z\&#xA;** a-z\
  **0-9\&#xA;** ( ) ! : @ $ \_ -

For storing this ID in CleverTap, we will add prefix to custom CleverTap ID as per following rules

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>

      </th>

      <th>
        adid
      </th>

      <th>
        vendor
      </th>

      <th>
        Rest(random)
      </th>

      <th>
        set ct id
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Android
      </td>

      <td>
        * \_g
      </td>

      <td>
        n/a
      </td>

      <td>
        \_ \_
      </td>

      <td>
        * \_h
      </td>
    </tr>

    <tr>
      <td>
        iOS
      </td>

      <td>
        -g
      </td>

      <td>
        -v
      </td>

      <td>
        *
      </td>

      <td>
        -h
      </td>
    </tr>
  </tbody>
</Table>

# Search

To search any particular profile by ID, use custom ID along with prefix in the search box

# Android

In order to send a custom ID, client must set the flag CLEVERTAP\_USE\_CUSTOM\_ID to 1 in the AndroidManifest file. The flag can be set to 1 at AndroidManifest.xml

```xml
<meta-data
    android:name="CLEVERTAP_USE_CUSTOM_ID"
    android:value="1"/>
```

Additionally, you must call any of the following method to pass the Custom CleverTap ID

```java
// To create a default instance with Custom CleverTap ID
CleverTapAPI clevertapDefaultInstance =
  CleverTapAPI.getDefaultInstance(getApplicationContext(),"Custom_CleverTap_ID");

// To create an instance with CleverTapInstanceConfig object and Custom CleverTap ID
CleverTapAPI instance =
CleverTapAPI.instanceWithConfig(clevertapAdditionalInstanceConfig,"Custom_CleverTap_ID");

// To pass Custom CleverTap ID to onUserLogin method
// Do not call onUserLogin directly in the onCreate() lifecycle method

// each of the below mentioned fields are optional
// with the exception of one of Identity, Email, or FBID
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


CleverTapAPI.getInstance(getApplicationContext()).onUserLogin(profileUpdate,"Custom_CleverTap_ID");
```

# iOS

* In order to send a custom ID, client must set the flag CleverTapUseCustomId to TRUE in the Plist file. 
* Navigate to the Info.plist file in your project navigator, and then create a key called CleverTapUseCustomId with type Boolean and set the value to YES.

<Image title="Screenshot 2019-05-17 at 5.36.59 PM.png" alt={1792} className="border" border={true} src="https://files.readme.io/6f3cdef-Screenshot_2019-05-17_at_5.36.59_PM.png" />

Additionally, you must call any of the following method to pass the Custom CleverTap ID

```c Objective-C
[CleverTap autoIntegrateWithCleverTapID:@"CUSTOM-CLEVERTAP-ID"];

[CleverTap sharedInstanceWithCleverTapID:@"CUSTOM-CLEVERTAP-ID"];

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

[[CleverTap sharedInstance] onUserLogin:profile withCleverTapID:@"CUSTOM-CLEVERTAP-ID"];
```
```swift Swift
CleverTap.autoIntegrate(withCleverTapID: "CUSTOM-CLEVERTAP-ID")

CleverTap.sharedInstance(withCleverTapID: "CUSTOM-CLEVERTAP-ID")

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

CleverTap.sharedInstance()?.onUserLogin(profile, withCleverTapID: "CUSTOM-CLEVERTAP-ID")
```

> ❗️ Error Cases
>
> * In case of invalid id OR id not set, CleverTap will NOT create a profile from the said device. 
> * No events will be logged against these profiles. 
> * All such errors will be logged with a profile created with the prefix \_ \_i 
> * These profiles will be visible under the error stream under the *account/stream.html* page on the dashboard
