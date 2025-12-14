---
title: Set CleverTap ID
excerpt: >-
  Set and manage unique device identifiers using Custom CleverTap ID across
  platforms.
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

Using the custom CleverTap ID capability, customers can generate and create their own unique identifiers for end-user devices. This allows you to have your own device identifier across all your systems without having to maintain a mapping of the device ID across different systems.

> 📘 Platform Availability
>
> Set CleverTap ID capability is only available on both Android and iOS versions **3.5.0 and above.**

> 🚧 Custom ID Assignment
>
> The custom CleverTap ID is only applied to new users or those without an existing ID.

# Rules for setting the CleverTap ID

The following validations apply when setting a custom CleverTap ID:

* The device identifier should be a maximum of 64 characters and a minimum of 1 character.
* Characters should be alphanumeric (a-z, 0-9) and case insensitive.
* Allowed characters are (any character apart from the below mentioned will be treated as an invalid ID)\
  **A-Z\&#xA;** a-z\
  **0-9\&#xA;** ( ) ! : @ $ \_ -

For storing this ID in CleverTap, we will add prefix to the custom CleverTap ID as per the following rules:

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Platform
      </th>

      <th>
        <p>adid</p>
      </th>

      <th>
        <p>vendor</p>
      </th>

      <th>
        <p>Rest(random)</p>
      </th>

      <th>
        <p>set ct id</p>
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        <p>Android</p>
      </td>

      <td>
        <p>\_ \_g</p>
      </td>

      <td>
        <p>n/a</p>
      </td>

      <td>
        <p>\_ \_</p>
      </td>

      <td>
        <p>\_ \_h</p>
      </td>
    </tr>

    <tr>
      <td>
        <p>iOS</p>
      </td>

      <td>
        <p>-g</p>
      </td>

      <td>
        <p>-v</p>
      </td>

      <td>
        \- <br />
      </td>

      <td>
        \-

        h
      </td>
    </tr>
  </tbody>
</Table>

# Search

To search any particular profile by ID, use custom ID along with the prefix in the search box.

# Android

In order to send a custom ID, client must set the flag CLEVERTAP\_USE\_CUSTOM\_ID to 1 in the AndroidManifest file. The flag can be set to 1 at AndroidManifest.xml.

```xml
<meta-data
    android:name="CLEVERTAP_USE_CUSTOM_ID"
    android:value="1"/>
```

Additionally, you must call any of the following methods to pass the Custom CleverTap ID.

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
```kotlin Kotlin
// To create a default instance with Custom CleverTap ID
val clevertapDefaultInstance =
    CleverTapAPI.getDefaultInstance(applicationContext, "Custom_CleverTap_ID")


// To create an instance with CleverTapInstanceConfig object and Custom CleverTap ID
val instance =
    CleverTapAPI.instanceWithConfig(
        clevertapAdditionalInstanceConfig,
        "Custom_CleverTap_ID"
    )


// To pass Custom CleverTap ID to onUserLogin method
// Do not call onUserLogin directly in the onCreate() lifecycle method

// each of the below mentioned fields are optional
// with the exception of one of Identity, Email, or FBID
val profileUpdate = HashMap<String, Any>()
profileUpdate["Name"] = "Jack Montana" // String
profileUpdate["Identity"] = 61026032 // String or number
profileUpdate["Email"] = "jack@gmail.com" // Email address of the user
profileUpdate["Phone"] = "+14155551234" // Phone (with the country code, starting with +)
profileUpdate["Gender"] = "M" // Can be either M or F
profileUpdate["DOB"] =
    Date() // Date of Birth. Set the Date object to the appropriate value first


// optional fields. controls whether the user will be sent email, push etc.
profileUpdate["MSG-email"] = false // Disable email notifications
profileUpdate["MSG-push"] = true // Enable push notifications
profileUpdate["MSG-sms"] = false // Disable SMS notifications
profileUpdate["MSG-whatsapp"] = true // Enable WhatsApp notifications

val stuff = ArrayList<String>()
stuff.add("bag")
stuff.add("shoes")
profileUpdate["MyStuff"] = stuff //ArrayList of Strings

val otherStuff = arrayOf("Jeans", "Perfume")
profileUpdate["MyStuff"] = otherStuff //String Array


CleverTapAPI.getInstance(applicationContext)
    .onUserLogin(profileUpdate, "Custom_CleverTap_ID")
```

# iOS

* In order to send a custom ID, client must set the flag CleverTapUseCustomId to TRUE in the Plist file. 
* Navigate to the Info.plist file in your project navigator, and then create a key called `CleverTapUseCustomId` with type Boolean and set the value to YES.

<Image title="Add a Custom Key" alt={1792} align="center" border={true} src="https://files.readme.io/6f3cdef-Screenshot_2019-05-17_at_5.36.59_PM.png">
  Add a Custom Key
</Image>

Additionally, you must call any of the following methods to pass the Custom CleverTap ID.

```objectivec Objective-C
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
    @"Name": @"Jack Montana",                            // String
    @"Identity": @61026032,                              // String or number
    @"Email": @"jack@gmail.com",                         // Email address of the user
    @"Phone": @"+14155551234",                           // Phone (with the country code, starting with +)
    @"Gender": @"M",                                     // Can be either M or F
    @"Employed": @"Y",                                   // Can be either Y or N
    @"Education": @"Graduate",                           // Can be either School, College or Graduate
    @"Married": @"Y",                                    // Can be either Y or N
    @"DOB": d,                                           // Date of Birth. An NSDate object
    @"Age": @26,                                         // Not required if DOB is set
    @"Tz": @"Asia/Kolkata",                              //an abbreviation such as "PST", a full name such as "America/Los_Angeles",
    //or a custom ID such as "GMT-8:00"
    @"Photo": @"www.foobar.com/image.jpeg",              // URL to the Image
    // optional fields. controls whether the user will be sent email, push etc.
    @"MSG-email": @NO,                                   // Disable email notifications
    @"MSG-push": @YES,                                   // Enable push notifications
    @"MSG-sms": @NO,                                     // Disable SMS notifications
    //custom fields
    @"score": @15,
    @"cost": @10.5
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
let d = NSCalendar.current.date(from: dob as DateComponents)
let profile: Dictionary<String, AnyObject> = [
    "Name": "Jack Montana" as AnyObject,                 // String
    "Identity": 61026032 as AnyObject,                   // String or number
    "Email": "jack@gmail.com" as AnyObject,              // Email address of the user
    "Phone": "+14155551234" as AnyObject,                // Phone (with the country code, starting with +)
    "Gender": "M" as AnyObject,                          // Can be either M or F
    "Employed": "Y" as AnyObject,                        // Can be either Y or N
    "Education": "Graduate" as AnyObject,                // Can be either School, College or Graduate
    "Married": "Y" as AnyObject,                         // Can be either Y or N
    "DOB": d! as AnyObject,                              // Date of Birth. An NSDate object
    "Age": 26 as AnyObject,                              // Not required if DOB is set
    "Tz":"Asia/Kolkata" as AnyObject,                    //an abbreviation such as "PST", a full name such as "America/Los_Angeles",
    //or a custom ID such as "GMT-8:00"
    "Photo": "www.foobar.com/image.jpeg" as AnyObject,   // URL to the Image

    // optional fields. controls whether the user will be sent email, push etc.
    "MSG-email": false as AnyObject,                     // Disable email notifications
    "MSG-push": true as AnyObject,                       // Enable push notifications
    "MSG-sms": false as AnyObject,                        // Disable SMS notifications

    //custom fields
    "score": 15 as AnyObject,
    "cost": 10.5 as AnyObject
]

CleverTap.sharedInstance()?.onUserLogin(profile, withCleverTapID: "CUSTOM-CLEVERTAP-ID")
```

> ❗️ Error Cases
>
> * In case of invalid ID or ID not set, CleverTap will NOT create a profile from the device.
> * No events will be logged against these profiles.
> * All such errors will be logged with a profile created with the prefix \_ \_i.
> * These profiles will be visible under the error stream under the *account/stream.html* page on the dashboard.

# Web

To send a custom ID via Web SDK, the `customId` parameter is required, because the `init()` function accepts `customId` as an optional parameter. Including this parameter enables you to set a Custom CleverTap ID for new users when initializing the SDK.

> 📘 Platform Availability
>
> Custom CleverTap ID capability is only available on Web SDK versions **1.15.0 and above**.

All Custom IDs assigned through the Web SDK are stored with a `_w_` prefix. Example: `user_123` is stored as `_w_user_123`. This feature is available through both NPM and CDN integrations.

#### Using NPM

To use a custom ID with NPM-based integration:

```javascript
clevertap.init('ACCOUNT_ID', 'REGION', 'TARGET_DOMAIN', 'TOKEN', {
  customId : 'your_custom_user_id',
});
```

#### Using CDN

To use a custom ID with CDN-based integration:

```html JavaScript
<script type="text/javascript">
  var clevertap = {
    event: [],
    profile: [],
    account: [],
    onUserLogin: [],
    notifications: [],
    privacy: [],
    config: {
      customId: "your_custom_user_id",
    },
  };

  clevertap.account.push(
    { id: "CLEVERTAP_ACCOUNT_ID" },
    "Region",
    "Target_Domain"
  );
  clevertap.privacy.push({ optOut: false });
  clevertap.privacy.push({ useIP: false });

  (function () {
    var wzrk = document.createElement("script");
    wzrk.type = "text/javascript";
    wzrk.async = true;
    wzrk.src =
      ("https:" == document.location.protocol
        ? "<https://d2r1yp2w7bby2u.cloudfront.net>"
        : "<http://static.clevertap.com>") + "/js/clevertap.min.js";
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

> ❗️ Error Handling
>
> * If `customId` fails validation or is not provided, CleverTap will default to generating a system GUID.
> * This temporary ID is created and stored with the prefix `__i` for Android and `-i` for iOS.
> * These profiles and errors can be monitored in the browser console.
