---
title: Android SDK Quick Start Guide
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
[block:api-header]
{
  "title": "Overview"
}
[/block]
This section shows how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard in less than ten minutes. CleverTap provides an Android SDK that enables app developers to track, segment, and engage their users. 

#Install SDK

To use the CleverTap Android SDK, you have two options. You can install it automatically using Gradle in Android Studio or manually install it by including the SDK source code in your Android Studio project. 

##Option A: Install using Android Studio and Gradle (Recommended) 

CleverTap supports Firebase Cloud Messaging (FCM). 

1. For FCM, add the dependencies below in your application’s build.gradle file. You will also have to add the FCM generated google-services.json file to your project. 
[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n    implementation 'com.clevertap.android:clevertap-android-sdk:4.2.0'\n    implementation 'com.google.firebase:firebase-messaging:20.2.4'\n\t\timplementation 'androidx.core:core:1.3.0'\n  //Mandatory for CleverTap Android SDK v3.6.4 and above add the following -\n\t\timplementation 'com.android.installreferrer:installreferrer:2.1'\n}\n// at the end of the build.gradle file\napply plugin: 'com.google.gms.google-services'",
      "language": "groovy"
    }
  ]
}
[/block]
2. Add the following configuration to your gradle properties file. 
[block:code]
{
  "codes": [
    {
      "code": "android.useAndroidX=true\n",
      "language": "groovy"
    }
  ]
}
[/block]
3. Once you have updated your build.gradle file, sync your project by clicking on Sync Project With Gradle Files button. You can find this button by navigating to the *Tools* menu, then the Android sub-menu.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/001ca2d-image3.png",
        "image3.png",
        509,
        337,
        "#302f33"
      ]
    }
  ]
}
[/block]
##Option B: Manual Install
For a manual install, follow these steps:
1. [Download](https://github.com/CleverTap/clevertap-android-sdk/releases/download/core-v4.2.0/clevertap-android-sdk-4.2.0.aar) this AAR file.
2. Copy the AAR file to the `/libs` directory of your project.
3. Add it as a dependency to your project.

#Add Your CleverTap Credentials in AndroidManifest.xml

To associate your Android app with your CleverTap account, you need to add your CleverTap credentials in the AndroidManifest.xml file in your application.

1. Navigate to the AndroidManifest.xml file in your project navigator. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/be3caa0-android-2018-1.png",
        "android-2018-1.png",
        1027,
        401,
        "#383939"
      ],
      "border": true
    }
  ]
}
[/block]
2. Add your CleverTap Account ID and Token to your AndroidManifest.xml within the <application></application> tags.
[block:code]
{
  "codes": [
    {
      "code": "<meta-data\n    android:name=\"CLEVERTAP_ACCOUNT_ID\"\n    android:value=\"Your CleverTap Account ID\"/>\n<meta-data\n    android:name=\"CLEVERTAP_TOKEN\"\n    android:value=\"Your CleverTap Account Token\"/>\n<!-- IMPORTANT: To force use Google AD ID to uniquely identify  users, use the following meta tag. GDPR mandates that if you are using this tag, there is prominent disclousure to your end customer in their application. Read more about GDPR here - https://clevertap.com/blog/in-preparation-of-gdpr-compliance/ -->\n<meta-data\n    android:name=\"CLEVERTAP_USE_GOOGLE_AD_ID\"\n    android:value=\"1\"/> \n",
      "language": "xml"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Using Google AD ID to uniquely identify users",
  "body": "GDPR mandates that if you are using Google AD ID to uniquely identify users, there should be prominent disclosure in your application that explains this to your end customer. For more information, refer to this [GDPR article](https://clevertap.com/blog/in-preparation-of-gdpr-compliance/). \n\nNote: If you still want to use Google AD ID to generate a user's identity, so that you can identify users across re-installs, add the meta tag that forces the use of the Google AD ID to generate an identity in CleverTap."
}
[/block]
3. Insert the account ID and account token values from your CleverTap account. These values are available on the *Settings* page at the bottom left-hand corner of your dashboard. 
[block:image]
{
  "images": [
    {
      "image": [],
      "sizing": "smart",
      "border": false
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d1467d9-2.png",
        "2.png",
        1449,
        1419,
        "#f9f9fb"
      ],
      "border": true,
      "sizing": "80"
    }
  ]
}
[/block]
#Enable Tracking by Adding Permissions

To enable tracking by adding permissions with the following steps:

1. In your AndroidManifest.xml file, add the following snippet within the <application></application> tags.
[block:code]
{
  "codes": [
    {
      "code": "<application\n    android:label=\"@string/app_name\"\n    android:icon=\"@drawable/ic_launcher\"\n    android:name=\"com.clevertap.android.sdk.Application\">",
      "language": "xml"
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/319c1ad-image6.png",
        "image6.png",
        796,
        207,
        "#37393e"
      ]
    }
  ]
}
[/block]
2. Next, add the snippet below in the same file, so the CleverTap SDK can access the internet.
[block:code]
{
  "codes": [
    {
      "code": "<!-- Required to allow the app to send events and user profile information -->\n<uses-permission android:name=\"android.permission.INTERNET\"/>\n<!-- Recommended so that CleverTap knows when to attempt a network call -->\n<uses-permission android:name=\"android.permission.ACCESS_NETWORK_STATE\"/>",
      "language": "xml"
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/da53966-image2.png",
        "image2.png",
        935,
        233,
        "#36383a"
      ],
      "border": true
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "If you have a custom application class, call ActivityLifecycleCallback.register(this); before super.onCreate() in your class.",
  "title": "Custom Application Class"
}
[/block]
#Initialize the CleverTap SDK

In the onCreate() method of your application’s main activity, include the code below. This code initializes the CleverTap SDK.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI clevertapDefaultInstance = CleverTapAPI.getDefaultInstance(getApplicationContext());",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "var cleverTapDefaultInstance: CleverTapAPI? = null\ncleverTapDefaultInstance = CleverTapAPI.getDefaultInstance(applicationContext)",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
#Run Your Application

To build and run your application, follow these steps:

1. Navigate to your CleverTap dashboard > *Boards* > *Mobile App*. If you successfully integrated the CleverTap SDK, you will see a new active user.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9e07e5a-New_mobile_users.png",
        "New mobile users.png",
        1405,
        421,
        "#f9f9fa"
      ]
    }
  ]
}
[/block]

#Identify Users

A user profile is automatically created in CleverTap for each user launching your application.

Initially, the user profile starts out as anonymous which means the profile does not contain any identifiable information about the user. You can enrich the profile with pre-defined attributes from the CleverTap data model, such as name and email. You can also add custom attributes that you define to extend the CleverTap data model.

Sending user profile information to CleverTap using our Android SDK requires two steps:
1. You have to build a HashMap with the profile properties. 
2. You have to call the SDK's `OnUserLogin` method and pass the HashMap you created as a parameter. 
[block:code]
{
  "codes": [
    {
      "code": "// each of the below mentioned fields are optional\nHashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Name\", \"Jack Montana\");    // String\nprofileUpdate.put(\"Identity\", 61026032);      // String or number\nprofileUpdate.put(\"Email\", \"jack@gmail.com\"); // Email address of the user\nprofileUpdate.put(\"Phone\", \"+14155551234\");   // Phone (with the country code, starting with +)\nprofileUpdate.put(\"Gender\", \"M\");             // Can be either M or F\nprofileUpdate.put(\"DOB\", new Date());         // Date of Birth. Set the Date object to the appropriate value first\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate.put(\"MSG-email\", false);        // Disable email notifications\nprofileUpdate.put(\"MSG-push\", true);          // Enable push notifications\nprofileUpdate.put(\"MSG-sms\", false);          // Disable SMS notifications\nprofileUpdate.put(\"MSG-whatsapp\", true);      // Enable WhatsApp notifications\nArrayList<String> stuff = new ArrayList<String>();\nstuff.add(\"bag\");\nstuff.add(\"shoes\");\nprofileUpdate.put(\"MyStuff\", stuff);                        //ArrayList of Strings\nString[] otherStuff = {\"Jeans\",\"Perfume\"};\nprofileUpdate.put(\"MyStuff\", otherStuff);                   //String Array\nCleverTapAPI.getInstance(getApplicationContext()).onUserLogin(profileUpdate);",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "// each of the below mentioned fields are optional\nval profileUpdate = HashMap<String, Any>()\nprofileUpdate[\"Name\"] = \"Jack Montana\" // String\nprofileUpdate[\"Identity\"] = 61026032 // String or number\nprofileUpdate[\"Email\"] = \"jack@gmail.com\" // Email address of the user\nprofileUpdate[\"Phone\"] = \"+14155551234\" // Phone (with the country code, starting with +)\nprofileUpdate[\"Gender\"] = \"M\" // Can be either M or F\nprofileUpdate[\"DOB\"] = Date() // Date of Birth. Set the Date object to the appropriate value first\n// optional fields. controls whether the user will be sent email, push etc.\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate[\"MSG-email\"] = false // Disable email notifications\nprofileUpdate[\"MSG-push\"] = true // Enable push notifications\nprofileUpdate[\"MSG-sms\"] = false // Disable SMS notifications\nprofileUpdate[\"MSG-whatsapp\"] = true // Enable WhatsApp notifications\nval stuff = ArrayList<String>()\nstuff.add(\"bag\")\nstuff.add(\"shoes\")\nprofileUpdate[\"MyStuff\"] = stuff //ArrayList of Strings\nval otherStuff = arrayOf(\"Jeans\", \"Perfume\")\nprofileUpdate[\"MyStuff\"] = otherStuff //String Array\nCleverTapAPI.getDefaultInstance(applicationContext)?.onUserLogin(profileUpdate)",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
When the `OnUserLogin` method is called, the user profile information will be sent to CleverTap.

To see how this information displays within the CleverTap dashboard, perform the following:
1. Navigate to *Segments* > *Find People*.
2. In the *By Identity* box, enter the email you set on the user profile record, then click the **Find** button.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f0467dc-1_By_identity_search_box.png",
        "1 By identity search box.png",
        1407,
        396,
        "#fafafb"
      ],
      "border": true
    }
  ]
}
[/block]
If CleverTap finds a user profile with this email, you will be taken to that user record. On that page, you will see the name and email as pre-defined fields, and *Customer type* and *Age* as custom fields.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c635bdd-2_User_Profile.png",
        "2_User_Profile.png",
        2912,
        1674,
        "#f8f8f9"
      ],
      "border": true
    }
  ]
}
[/block]
#Track Custom Events

Once you integrate the CleverTap SDK, we automatically start tracking events, such as *App Launch* and *Notification Viewed*. In addition to the [default events tracked by CleverTap](https://developer.clevertap.com/docs/concepts-events), you can also track custom events.

To send custom events to CleverTap using our Android SDK, you have to call the pushEvent method with the name of the custom event you want to track. 
[block:code]
{
  "codes": [
    {
      "code": "clevertapDefaultInstance.pushEvent(\"Product viewed\");",
      "language": "java",
      "name": "JAVA"
    },
    {
      "code": "clevertapDefaultInstance?.pushEvent(\"Product viewed\")",
      "language": "kotlin",
      "name": "Kotlin"
    }
  ]
}
[/block]
To take a look at the same user profile in CleverTap from the last section, click on the **Activity** button within the user profile record. This shows a list of activities associated with the user profile. 

If the custom event was successfully sent from your application to CleverTap, you will see it in this list.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1a8aecb-3_User_Activities.png",
        "3_User_Activities.png",
        2756,
        1676,
        "#f9f9f9"
      ],
      "border": true
    }
  ]
}
[/block]
#Next Steps 

By completing this guide, you are automatically tracking user events like app launches, and associating that information with profiles for each of your users. You also learned how to add information to a user profile and how to track custom events. You now have integrated CleverTap into your Android application

In the [next Android guide](doc:android), you will learn more advanced options for tracking custom events and enriching user profiles.