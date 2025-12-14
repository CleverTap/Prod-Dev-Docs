---
title: Xamarin
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
The CleverTap Xamarin SDK for Mobile Customer Engagement and Analytics solutions 

CleverTap brings together real-time user insights, an advanced segmentation engine, and easy-to-use marketing tools in one mobile marketing platform — giving your team the power to create amazing experiences that deepen customer relationships. Our intelligent mobile marketing platform provides the insights you need to keep users engaged and drive long-term retention and growth.

For more information check out our [website](https://clevertap.com "CleverTap") and [documentation](https://developer.clevertap.com/docs/ "CleverTap Technical Documentation").

## Getting Started

### Steps for Android

1. Install the SDK

Use the [CleverTap.Bindings.Android DLL file](https://github.com/CleverTap/clevertap-xamarin/tree/master/clevertap-component/lib/android) and add it to the References.

2. Add Your CleverTap Credentials in AndroidManifest.xml


[block:code]
{
  "codes": [
    {
      "code": "<meta-data\n    android:name=\"CLEVERTAP_ACCOUNT_ID\"\n    android:value=\"Your CleverTap Account ID\"/>\n<meta-data\n    android:name=\"CLEVERTAP_TOKEN\"\n    android:value=\"Your CleverTap Account Token\"/>\n<!-- IMPORTANT: To force use Google AD ID to uniquely identify  users, use the following meta tag. GDPR mandates that if you are using this tag, there is prominent disclousure to your end customer in their application. Read more about GDPR here - https://clevertap.com/blog/in-preparation-of-gdpr-compliance/ -->\n<meta-data\n    android:name=\"CLEVERTAP_USE_GOOGLE_AD_ID\"\n    android:value=\"1\"/>\n",
      "language": "xml"
    }
  ]
}
[/block]
3. Enable Tracking by Adding Permissions

In your AndroidManifest.xml file, add the following snippet within the <application></application> tags.
[block:code]
{
  "codes": [
    {
      "code": "<application\n    android:label=\"@string/app_name\"\n    android:icon=\"@drawable/ic_launcher\"\n    android:name=\"com.clevertap.android.sdk.Application\">\n  ",
      "language": "xml"
    }
  ]
}
[/block]
Next add the snippet below in the same file, so the CleverTap Xamarin SDK can access the internet.

[block:code]
{
  "codes": [
    {
      "code": "\n<!-- Required to allow the app to send events and user profile information -->\n<uses-permission android:name=\"android.permission.INTERNET\"/>\n<!-- Recommended so that CleverTap knows when to attempt a network call -->\n<uses-permission android:name=\"android.permission.ACCESS_NETWORK_STATE\"/>\n\n",
      "language": "xml"
    }
  ]
}
[/block]
4. Initialize the CleverTap SDK
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI cleverTapAPI = CleverTapAPI.GetDefaultInstance(Android.App.Application.Context);\n\n",
      "language": "csharp"
    }
  ]
}
[/block]
5. Add Information to a User Profile
[block:code]
{
  "codes": [
    {
      "code": "IDictionary<string, Java.Lang.Object> profileData = new Dictionary<string, Java.Lang.Object>();\n\nprofileData.Add(\"Name\", \"Jack Montana\");    // String\nprofileData.Add(\"Identity\", 61026032);      // String or number\nprofileData.Add(\"Email\", \"jack@gmail.com\"); // Email address of the user\nprofileData.Add(\"Phone\", \"+14155551234\");   // Phone (with the country code, starting with +)\nprofileData.Add(\"Gender\", \"M\");             // Can be either M or F\nprofileData.Add(\"DOB\", new Date());         // Date of Birth. Set the Date object to the appropriate value first - requires java.util\n\n\ncleverTapAPI.PushProfile(profileData);",
      "language": "csharp"
    }
  ]
}
[/block]
6. Track Custom Events
[block:code]
{
  "codes": [
    {
      "code": "\n\ncleverTapAPI.PushEvent(\"Product View Via Xamarin\");\n\n",
      "language": "csharp"
    }
  ]
}
[/block]
Follow the [Xamarin Demo Project](https://github.com/CleverTap/clevertap-xamarin/blob/master/clevertap-component/sample/android/XamarinDemo/XamarinDemo/MainActivity.cs) for examples


### Steps for iOS

1. Install the SDK 

     Use the [DLL](https://github.com/CleverTap/clevertap-xamarin/tree/master/clevertap-component/lib/ios) and add it to the Reference

2. Integrating the CleverTap SDK

      * Follow the integration instruction [starting with Step 2 here](https://developer.clevertap.com/docs/ios-quickstart-guide#section-step-2-add-clever-tap-credentials).
      * Add CleverTap to your AppDelegate.cs in your FinishedLaunching method.

[block:code]
{
  "codes": [
    {
      "code": "\nusing CleverTapSDK;\n\npublic bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)\n{\n    // Override point for customization after application launch.\n    // If not required for your application you can safely delete this method\n\n    CleverTap.AutoIntegrate();\n    return true;\n}\n\n",
      "language": "csharp"
    }
  ]
}
[/block]
3. Track Custom Events

[block:code]
{
  "codes": [
    {
      "code": "CleverTap.SharedInstance()?.RecordEvent(\"Product Viewed Via Xamarin\");\n\n",
      "language": "csharp"
    }
  ]
}
[/block]
4. Add Information to a User Profile

[block:code]
{
  "codes": [
    {
      "code": "\nvar profileData = new NSDictionary(\n    new NSString(\"Name\"), new NSString(\"Jack Montana\"),\n    new NSString(\"Identity\"), new NSString(\"61026032\"),\n    new NSString(\"Email\"), new NSString(\"Jack Montana\"),\n    new NSString(\"Phone\"), new NSString(\"14155551234\"),\n    new NSString(\"Gender\"), new NSString(\"F\"),\n    new NSString(\"DOB\"), new NSDate()\n);\nCleverTap.SharedInstance()?.OnUserLogin(profileData);\n\n\n",
      "language": "csharp"
    }
  ]
}
[/block]
See the [Starter Project](https://github.com/CleverTap/clevertap-xamarin/tree/master/clevertap-component/sample/ios/Starter) directory for the sample app.