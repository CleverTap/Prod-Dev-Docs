---
title: Android Quickstart Guide
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
CleverTap provides an Android SDK that enables app developers to track, segment, and engage their users. 

This guide will show you how to install the CleverTap SDK, track your first user event, and see this information within the CleverTap dashboard in less than ten minutes. 

# Step 1: Install SDK

To use the CleverTap Android SDK, you have two options. You can install it automatically using Gradle in Android Studio or manually install it by including the SDK source code in your Android Studio project. 

**Option A - Install using Android Studio and Gradle (Recommended) **

CleverTap supports both Firebase Cloud Messaging (FCM) and Google Cloud Messaging (GCM). Depending on which of these you are using in your application, you will install a different set of CleverTap dependencies. 

If you are using GCM, add the dependencies below in your application’s build.gradle file. 
[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n    compile 'com.clevertap.android:clevertap-android-sdk:3.1.10'\n    compile 'com.google.android.gms:play-services-gcm:11.4.0'\n    compile 'com.google.android.gms:play-services-base:11.4.0'\n    compile 'com.android.support:support-v4:23.1.1'\n}",
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
        "https://files.readme.io/2cfc027-android-studio-1.png",
        "android-studio-1.png",
        1200,
        400,
        "#353637"
      ],
      "border": true
    }
  ]
}
[/block]
If you are using FCM, add the dependencies below in your application’s build.gradle file. You will also have to add the FCM generated google-services.json file to your project.
[block:code]
{
  "codes": [
    {
      "code": "dependencies {\n    compile 'com.clevertap.android:clevertap-android-sdk:3.1.10'\n    compile 'com.google.firebase:firebase-messaging:11.4.0'\n    compile 'com.google.android.gms:play-services-base:11.4.0'\n    compile 'com.android.support:support-v4:23.1.1'\n}\n// at the end of the build.gradle file\napply plugin: 'com.google.gms.google-services'\n",
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
        "https://files.readme.io/bf3d393-android-studio-2.png",
        "android-studio-2.png",
        1200,
        340,
        "#353738"
      ],
      "border": true
    }
  ]
}
[/block]
Once you have updated your build.gradle file, sync your project by clicking on Sync Project With Gradle Files button. You could find this button by navigating to the Tools menu and then the Android submenu.


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
**Option B - Manual Install**

[Download](https://github.com/CleverTap/clevertap-android-sdk/releases/download/3.1.10/CleverTapAndroidSDK.jar) and unzip the CleverTap SDK. 
Copy the JAR file to the /libs directory of your project and then add it as dependency to your project.

#Step 2: Add Your CleverTap Credentials in AndroidManifest.xml

To associate your Android app with your CleverTap account, you will need to add your CleverTap credentials in the AndroidManifest.xml file in your application.

Navigate to the AndroidManifest.xml file in your project navigator. 
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
Add your CleverTap Account ID and Token to your AndroidManifest.xml, within the <application></application> tags.
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
  "body": "GDPR mandates that if you are using Google AD ID to uniquely identify users, there should be prominent disclosure in your application that explains this to your end customer.  You can read more about  GDPR [here](https://clevertap.com/blog/in-preparation-of-gdpr-compliance/). \n\nNote: if you still want to use Google AD ID to generate a users identity, so that you can identify users across re-installs, add the meta tag that force uses the Google AD ID to generate an identity in CleverTap. \n\nContact support@clevertap.com for any questions."
}
[/block]
Insert the Account ID and Account Token values from your CleverTap account. These values are available on the Settings page. To navigate to the Settings page, log into [your CleverTap account](https://eu1.dashboard.clevertap.com/login.html), click on the gear icon on the top right navigation, and select Settings dashboard.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/c1c024b-android-2018-2.png",
        "android-2018-2.png",
        489,
        258,
        "#f2f4f5"
      ],
      "border": true
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/73f51f6-image8.png",
        "image8.png",
        769,
        425,
        "#eaebf2"
      ],
      "border": true
    }
  ]
}
[/block]
#Step 3: Enable Tracking by Adding Permissions

In your AndroidManifest.xml file, add the following snippet within the <application></application> tags.
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
Next add the snippet below in the same file, so the CleverTap SDK can access the internet.
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
If you have a custom application class, call ActivityLifecycleCallback.register(this); before super.onCreate() in your class.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/473f7e7-Screen_Shot_2018-01-29_at_8.04.57_PM.png",
        "Screen Shot 2018-01-29 at 8.04.57 PM.png",
        582,
        288,
        "#333434"
      ]
    }
  ]
}
[/block]
#Step 4: Initialize the CleverTap SDK

In the onCreate() method of your application’s main activity include the code below. This code will initialize the CleverTap SDK.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI ct;\ntry {\n  ct = CleverTapAPI.getInstance(getApplicationContext());\n} catch (CleverTapMetaDataNotFoundException e) {\n  // handle appropriately\n} catch (CleverTapPermissionsNotSatisfied e) {\n  // handle appropriately\n}",
      "language": "java"
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/39d3d66-image1.png",
        "image1.png",
        846,
        367,
        "#333437"
      ],
      "border": true
    }
  ]
}
[/block]
#Step 5: Run Your Application

Build and run your application. 

Navigate to your CleverTap dashboard. If you successfully integrated the CleverTap SDK, you will see a new active user.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3a402b2-image5.png",
        "image5.png",
        1320,
        520,
        "#1f2f45"
      ]
    }
  ]
}
[/block]

# Step 6: Add Information to a User Profile

A User Profile is automatically created in CleverTap for each user launching your application.

Initially, the User Profile starts out as anonymous, which means the profile does not contain any identifiable information about the user. You can enrich the profile with pre-defined attributes from the CleverTap data model, such as name and email. You can also add custom attributes that you define to extend the CleverTap data model.

Sending user profile information to CleverTap using our Android SDK requires two steps. First, you have to build a HashMap with the profile properties. Second, you have to call the SDK's profile.push method and pass the HashMap you created as a parameter. 
[block:code]
{
  "codes": [
    {
      "code": "HashMap<String, Object> profileUpdate = new HashMap<String, Object>();\n\n//Update pre-defined profile properties\nprofileUpdate.put(\"Name\", \"Jack Montana\");\nprofileUpdate.put(\"Email\", \"jack@gmail.com\");      \n//Update custom profile properties\nprofileUpdate.put(\"Plan Type\", \"Silver\");\nprofileUpdate.put(\"Favorite Food\", \"Pizza\");\n\ncleverTap.profile.push(profileUpdate);\n",
      "language": "java"
    }
  ]
}
[/block]
When the profile.push method is called, the user profile information will be sent to CleverTap.

Let's see how this information appears within the CleverTap dashboard. Log into the dashboard, and then click on the Find People button under the Segment tab. In the By Identity box, enter the email you set on the user profile record, and then click the Find button.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4f5cab3-android1.png",
        "android1.png",
        1254,
        345,
        "#e2e2e5"
      ],
      "border": true
    }
  ]
}
[/block]
If CleverTap finds a user profile with this email, you will be taken to that user record. On that page, we'll see name and email as pre-defined fields, and plan type and favorite food as custom fields.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9ec6ba3-a3.png",
        "a3.png",
        1080,
        597,
        "#f2f0f2"
      ],
      "border": true
    }
  ]
}
[/block]
# Step 7: Track Custom Events

Once you integrate the CleverTap SDK, we automatically start tracking events, such as App Launch and Notification Viewed. In addition to the [default events tracked by CleverTap](https://developer.clevertap.com/docs/concepts-events), you can also track custom events.

To send custom events to CleverTap using our Android SDK, you will have to call the event.push method with the name of the custom event you want to track. 
[block:code]
{
  "codes": [
    {
      "code": "cleverTap.event.push(\"Product viewed\");",
      "language": "java"
    }
  ]
}
[/block]
Let's look at the same user profile in CleverTap from step 6. Click on the Activity button within the user profile record. This will show a list of activities associated with the user profile. If the custom event was successfully sent from your application to CleverTap, you will see it in this list.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/7fc4332-a4.png",
        "a4.png",
        1248,
        692,
        "#e1e1e5"
      ],
      "border": true
    }
  ]
}
[/block]
#Next Steps 

Congrats on successfully integrating CleverTap into your Android application! 

You are now automatically tracking user events like app launches, and associating that information with profiles for each of your users. You also learned how to add information to a user profile and how to track custom events.

In the [next Android guide](doc:android), you will learn more advanced options for tracking custom events and enriching user profiles.