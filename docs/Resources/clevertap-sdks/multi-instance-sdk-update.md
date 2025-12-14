---
title: Multi-Instance SDK Update
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

In version 3.2.0 of our [Android](https://github.com/CleverTap/clevertap-android-sdk) and [iOS](https://github.com/CleverTap/clevertap-android-sdk) SDKs, we released a major update that enables you to use multiple CleverTap accounts in a single app. 

For example, let's say you have an app that is composed of multiple smaller apps (chat, map, shopping). With this new SDK update, you can track each of the smaller apps with a different CleverTap account. 

Another use case for multiple CleverTap accounts in a single app is using a different CleverTap account depending on the app store or country that the app was downloaded from.

# Android

## Version 3.2.0 Changes

In [version 3.2.0 of the Android SDK](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/3.2.0), we released the following new features: 
* Added support for creating additional CleverTap instances to send data to multiple CleverTap accounts from your app.
* Added APIs for setting the SDK to offline and SSL Pinning.

We also deprecated the following SDK methods: 
* Deprecated CleverTapException, CleverTapMetaDataNotFoundException, CleverTapPermissionsNotSatisfied, and InvalidEventNameException.
* Deprecated CleverTapAPI.CHARGED_EVENT.
* Deprecated CleverTapAPI.getInstance() method. 

## Create Additional Instances

The first step is to create a CleverTapInstanceConfig object, and specify the CleverTap account needed with its account id and account token.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapInstanceConfig clevertapAdditionalInstanceConfig =  CleverTapInstanceConfig.createInstance(context, \"ADDITIONAL_CLEVERTAP_ACCOUNT_ID\", \"ADDITIONAL_CLEVERTAP_ACCOUNT_TOKEN\");",
      "language": "java"
    }
  ]
}
[/block]
The second step is to configure the CleverTapInstanceConfig object with the options needed for the debug level, analytics, Google Ad Id, and personalization.
[block:code]
{
  "codes": [
    {
      "code": "clevertapAdditionalInstanceConfig.setDebugLevel(CleverTapAPI.LogLevel.DEBUG); // default is CleverTapAPI.LogLevel.INFO\n\nclevertapAdditionalInstanceConfig.setAnalyticsOnly(true); // disables the user engagement features of the instance, default is false\n\nclevertapAdditionalInstanceConfig.useGoogleAdId(true); // enables the collection of the Google ADID by the instance, default is false\n\nclevertapAdditionalInstanceConfig.enablePersonalization(false); //enables personalization, default is true.\n",
      "language": "java"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "All configuration to the CleverTapInstanceConfig object must be done prior to calling CleverTapAPI.instanceWithConfig. Subsequent changes to the CleverTapInstanceConfig object will have no effect on the additional CleverTap instance created."
}
[/block]
The third step is to instantiate the additional CleverTap instance by calling CleverTapAPI.instanceWithConfig method with the CleverTapInstanceConfig object you created.
[block:code]
{
  "codes": [
    {
      "code": "CleverTapAPI clevertapAdditionalInstance = CleverTapAPI.instanceWithConfig(clevertapAdditionalInstanceConfig);\n",
      "language": "java"
    }
  ]
}
[/block]
## Track Events with Additional Instances
[block:code]
{
  "codes": [
    {
      "code": "//Create a new instance\nCleverTapInstanceConfig config =  CleverTapInstanceConfig.createInstance(this,\"RWW-WWW-WWWZ\",\"000-002\");\nCleverTapAPI cleverTapAPI = CleverTapAPI.instanceWithConfig(this,config);\n\n//Push an event\ncleverTapAPI.pushEvent(“Event Name”);\n\n\n// Push an event with properties\nHashMap<String, Object> prodViewedAction = new HashMap<String, Object>();\nprodViewedAction.put(\"Product Name\", \"Casio Chronograph Watch\");\nprodViewedAction.put(\"Category\", \"Mens Accessories\");\nprodViewedAction.put(\"Price\", 59.99);\nprodViewedAction.put(\"Date\", new java.util.Date());\n\ncleverTapAPI.pushEvent(\"Product viewed\", prodViewedAction);\n\n//Push a charged event\nHashMap<String, Object> chargeDetails = new HashMap<String, Object>();\nchargeDetails.put(\"Amount\", 300);\nchargeDetails.put(\"Payment Mode\", \"Credit card\");\nchargeDetails.put(\"Charged ID\", 24052013);\n\nHashMap<String, Object> item1 = new HashMap<String, Object>();\nitem1.put(\"Product category\", \"books\");\nitem1.put(\"Book name\", \"The Millionaire next door\");\nitem1.put(\"Quantity\", 1);\n\nHashMap<String, Object> item2 = new HashMap<String, Object>();\nitem2.put(\"Product category\", \"books\");\nitem2.put(\"Book name\", \"Achieving inner zen\");\nitem2.put(\"Quantity\", 1);\n\nHashMap<String, Object> item3 = new HashMap<String, Object>();\nitem3.put(\"Product category\", \"books\");\nitem3.put(\"Book name\", \"Chuck it, let's do it\");\nitem3.put(\"Quantity\", 5);\n\nArrayList<HashMap<String, Object>> items = new ArrayList<HashMap<String, Object>>();\nitems.add(item1);\nitems.add(item2);\nitems.add(item3);\n\ncleverTapAPI.pushChargedEvent(chargeDetails, items);",
      "language": "java"
    }
  ]
}
[/block]
## Update Profiles with Additional Instances
[block:code]
{
  "codes": [
    {
      "code": "//Create a new instance\nCleverTapInstanceConfig config =  CleverTapInstanceConfig.createInstance(this,\"RWW-WWW-WWWZ\",\"000-002\");\nCleverTapAPI cleverTapAPI = CleverTapAPI.instanceWithConfig(this,config);\n\n//Push a simple profile update\nHashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Customer Type\", \"Silver\");\nprofileUpdate.put(\"Prefered Language\", \"English\");\n\ncleverTapAPI.profile.push(profileUpdate);\n\n\n//Push a complex profile update\nHashMap<String, Object> profileUpdate = new HashMap<String, Object>();\nprofileUpdate.put(\"Name\", \"Jack Montana\");                  // String\nprofileUpdate.put(\"Identity\", 61026032);                    // String or number\nprofileUpdate.put(\"Email\", \"jack@gmail.com\");               // Email address of the user\nprofileUpdate.put(\"Phone\", \"+14155551234\");                 // Phone (with the country code, starting with +)\nprofileUpdate.put(\"Gender\", \"M\");                           // Can be either M or F\nprofileUpdate.put(\"Employed\", \"Y\");                         // Can be either Y or N\nprofileUpdate.put(\"Education\", \"Graduate\");                 // Can be either Graduate, College or School\nprofileUpdate.put(\"Married\", \"Y\");                          // Can be either Y or N\nprofileUpdate.put(\"DOB\", new Date());                       // Date of Birth. Set the Date object to the appropriate value first\nprofileUpdate.put(\"Age\", 28);                               // Not required if DOB is set\nprofileUpdate.put(\"Tz\", \"Asia/Kolkata\");                    //an abbreviation such as \"PST\", a full name such as \"America/Los_Angeles\", \n                                                            //or a custom ID such as \"GMT-8:00\"\nprofileUpdate.put(\"Photo\", \"www.foobar.com/image.jpeg\");    // URL to the Image\n\n// optional fields. controls whether the user will be sent email, push etc.\nprofileUpdate.put(\"MSG-email\", false);                      // Disable email notifications\nprofileUpdate.put(\"MSG-push\", true);                        // Enable push notifications\nprofileUpdate.put(\"MSG-sms\", false);                        // Disable SMS notifications\n\nArrayList<String> stuff = new ArrayList<String>();\nstuff.add(\"bag\");\nstuff.add(\"shoes\");\nprofileUpdate.put(\"MyStuff\", stuff);                        //ArrayList of Strings\n\nString[] otherStuff = {\"Jeans\",\"Perfume\"};\nprofileUpdate.put(\"MyStuff\", otherStuff);                   //String Array\n\ncleverTapAPI.pushProfile(profileUpdate);",
      "language": "java"
    }
  ]
}
[/block]
## Set Offline

You can set the CleverTap SDK to offline by using the method below. By default, it is set to false.

Once offline, events will be recorded and queued locally, but will not be sent to the server until offline is disabled. Calling this method again with offline set to NO will allow events to be sent to server, and the SDK instance will immediately attempt to send events that have been queued while offline.
[block:code]
{
  "codes": [
    {
      "code": "cleverTapAPI.setOffline(true);\n",
      "language": "java"
    }
  ]
}
[/block]
## SSL Pinning

To enable SSL Pinning, please contact [customer support](doc:developer-support).

## Deprecated Methods

We deprecated the SDK methods listed in the table below. Note that some of the functionality supported by the deprecated methods is now supported through new methods.
[block:parameters]
{
  "data": {
    "h-1": "New Method",
    "h-0": "Deprecated Method",
    "0-0": "CleverTapException",
    "1-0": "CleverTapMetaDataNotFoundException",
    "2-0": "CleverTapPermissionsNotSatisfied",
    "3-0": "InvalidEventNameException",
    "0-1": "*Not supported in new SDK versions* ",
    "1-1": "*Not supported in new SDK versions*",
    "2-1": "*Not supported in new SDK versions*",
    "3-1": "*Not supported in new SDK versions*",
    "4-0": "CleverTapAPI.CHARGED_EVENT",
    "4-1": "cleverTapAPI.pushChargedEvent()",
    "5-0": "CleverTapAPI.getInstance()",
    "5-1": "CleverTapAPI.getDefaultInstance()"
  },
  "cols": 2,
  "rows": 6
}
[/block]
# iOS

## Version 3.2.0 Changes

In [version 3.2.0 of the iOS SDK](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/3.2.0), we released the following new features: 
* Added support for creating additional CleverTap instances to send data to multiple CleverTap accounts from your app.
* Added APIs for setting the SDK to offline and SSL Pinning.
* Added support for integration via Carthage.
* Moved from a static to dynamic framework.

## Create Additional Instances

First, import the header in your class.
[block:code]
{
  "codes": [
    {
      "code": "#import <CleverTapSDK/CleverTapInstanceConfig.h>\n",
      "language": "objectivec",
      "name": "Objective-C"
    },
    {
      "code": "#import \"CleverTapSDK/CleverTapInstanceConfig.h\"",
      "language": "text",
      "name": "Swift"
    }
  ]
}
[/block]
Second, configure the instance with the CleverTap account id and account token. 

You can also configure the CleverTapInstanceConfig object with the options needed for the debug level, analytics, IDFA, and personalization.
[block:code]
{
  "codes": [
    {
      "code": "// Config an additional instance\nCleverTapInstanceConfig *ctConfig = [[CleverTapInstanceConfig alloc] initWithAccountId:@\"YOUR_ACCOUNT_ID\" accountToken:@\"YOUR_ACCOUNT_TOKEN\"];\n\n//By default, CleverTap logs are set to CleverTapLogLevel.info. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level to CleverTapLogLevel.debug. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTapLogLevel.off\n[ctConfig setLogLevel:CleverTapLogDebug];\n\n//By default,  the CleverTap iOS SDK will not use the IDFA (IFA) to generate CleverTap IDs. You can choose to enable the use of IDFA to generate CleverTap IDs by call below method.\n[ctConfig setUseIDFA:YES];\n\n//Setting this boolean value to true will disable all engagement.\n[ctConfig setAnalyticsOnly:YES];\n\n//Call this method to enable the Profile/Events Read and Synchronization API.\n[ctConfig setEnablePersonalization:NO];",
      "language": "objectivec"
    },
    {
      "code": "// Config an additional instance\nlet ctConfig = CleverTapInstanceConfig.init(accountId: \"YOUR_ACCOUNT_ID\", accountToken:  \"YOUR_ACCOUNT_TOKEN\")\n\n//By default, CleverTap logs are set to CleverTapLogLevel.info. During development, we recommend that you set the SDK to DEBUG mode, in order to log warnings or other important messages to the iOS logging system. This can be done by setting the debug level to CleverTapLogLevel.debug. If you want to disable CleverTap logs for production environment, you can set the debug level to CleverTapLogLevel.off\nctConfig.logLevel = CleverTapLogLevel.debug\n\n//By default,  the CleverTap iOS SDK will not use the IDFA (IFA) to generate CleverTap IDs. You can choose to enable the use of IDFA to generate CleverTap IDs by call below method.\nctConfig.useIDFA = true\n\n//Setting this boolean value to true will disable all engagement.\nctConfig.analyticsOnly = true\n\n//Call this method to enable the Profile/Events Read and Synchronization API.\nctConfig.enablePersonalization = false",
      "language": "swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "All configuration to the CleverTapInstanceConfig object must be done prior to calling instanceWithConfig:ctConfig. Subsequent changes to the CleverTapInstanceConfig object will have no effect on the additional CleverTap instance created."
}
[/block]
The third step is to instantiate the additional CleverTap instance by calling the instanceWithConfig:ctConfig method with the CleverTapInstanceConfig object you created.
[block:code]
{
  "codes": [
    {
      "code": "// Init an additional instance \nCleverTap *additionalCleverTapInstance = [CleverTap instanceWithConfig:ctConfig];",
      "language": "objectivec"
    },
    {
      "code": "// Init an additional instance\nlet cleverTapAdditionalInstance = CleverTap.instance(with: ctConfig)",
      "language": "swift"
    }
  ]
}
[/block]
## Track Events with Additional Instances
[block:code]
{
  "codes": [
    {
      "code": "CleverTapInstanceConfig *ctConfig = [[CleverTapInstanceConfig alloc] initWithAccountId:@\"YOUR_ACCOUNT_ID\" accountToken:@\"YOUR_ACCOUNT_TOKEN\"];\n\nCleverTap *cleverTapAdditionalInstance = [CleverTap instanceWithConfig:ctConfig];\n\n[cleverTapAdditionalInstance recordEvent:@\"TestCT1WProps\" withProps:@{@\"one\": @1}];\n",
      "language": "objectivec"
    },
    {
      "code": "let ctConfig = CleverTapInstanceConfig.init(accountId: \"YOUR_ACCOUNT_ID\", accountToken:  \"YOUR_ACCOUNT_TOKEN\")\n       \nlet cleverTapAdditionalInstance = CleverTap.instance(with: ctConfig)\n\ncleverTapAdditionalInstance.recordEvent(\"TestCT1WProps\", withProps: [\"one\": NSNumber.init(integerLiteral: 1)])",
      "language": "swift",
      "name": "Swift"
    }
  ]
}
[/block]
## Update Profiles with Additional Instances
[block:code]
{
  "codes": [
    {
      "code": "CleverTapInstanceConfig *ctConfig = [[CleverTapInstanceConfig alloc] initWithAccountId:@\"YOUR_ACCOUNT_ID\" accountToken:@\"YOUR_ACCOUNT_TOKEN\"];\n   \nCleverTap *cleverTapAdditionalInstance = [CleverTap instanceWithConfig:ctConfig];\n\n[cleverTapAdditionalInstance profileSetMultiValues:@[@\"bag\", @\"shoes\"] forKey:@\"myCart\"];",
      "language": "objectivec"
    },
    {
      "code": "let ctConfig = CleverTapInstanceConfig.init(accountId: \"YOUR_ACCOUNT_ID\", accountToken:  \"YOUR_ACCOUNT_TOKEN\")\n       \nlet cleverTapAdditionalInstance = CleverTap.instance(with: ctConfig)\n\ncleverTapAdditionalInstance.profileSetMultiValues([\"a\"], forKey: \"letters\")",
      "language": "swift"
    }
  ]
}
[/block]
## Install CleverTap with Carthage

CleverTap now supports Carthage to package your dependencies as a framework. 

To integrate CleverTap into your Xcode project using Carthage, first specify it in your Cartfile. Second, run the 'carthage update' command to build the framework. Third, drag the built CleverTapSDK.framework into your Xcode project.
[block:code]
{
  "codes": [
    {
      "code": "github \"CleverTap/clevertap-ios-sdk\"",
      "language": "text"
    }
  ]
}
[/block]
## SSL Pinning

To enable SSL Pinning, please contact [customer support](doc:developer-support).

## Set Offline

You can set the CleverTap SDK to offline by using the method below. By default, it is set to false.

Once offline, events will be recorded and queued locally, but will not be sent to the server until offline is disabled. Calling this method again with offline set to NO will allow events to be sent to server, and the SDK instance will immediately attempt to send events that have been queued while offline.
[block:code]
{
  "codes": [
    {
      "code": "[cleverTapAdditionalInstance setOffline:YES];",
      "language": "objectivec"
    },
    {
      "code": "cleverTapAdditionalInstance.setOffline = YES",
      "language": "swift"
    }
  ]
}
[/block]