---
title: App Personalization
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
CleverTap SDK offers a powerful API to access User Profile data in your app to personalize the user experience. 

Developers can use the CleverTap Personalization API’s to query current session information, past event history, and profile properties to personalize the experience for users while they are in your app.

# Step 1: Enable Personalization

To enable Personalization, you must make an explicit activation call in the relevant CleverTap SDK.
[block:code]
{
  "codes": [
    {
      "code": "// Call the method enablePersonalization() on the CleverTapAPI instance (preferably in the onCreate() of your main activity):\npublic void onCreate(Bundle savedInstanceState) {\n  CleverTapAPI cleverTap = CleverTapAPI.getInstance(getApplicationContext());\n  cleverTap.enablePersonalization();\n}",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// To enable this feature, send the enablePersonalization message to the CleverTap SDK (preferably in your AppDelegate):\n- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {\n    ...\n    [CleverTap enablePersonalization];\n    ...\n}",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "// To enable this feature, send the enablePersonalization message to the CleverTap SDK (preferably in your AppDelegate):\n- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {\n    ...\n    [CleverTap enablePersonalization];\n    ...\n}",
      "language": "swift",
      "name": "iOS (Swift)"
    },
    {
      "code": "// in your JS embed code, be sure to include clevertap.enablePersonalization = true; as shown below\n<head>\n    <script type=\"text/javascript\">\n    var clevertap = {event:[], profile:[], account:[]};\n    clevertap.account.push({\"id\": \"Your CleverTap Account ID\"});\n    clevertap.enablePersonalization = true; // enables Personalization\n    (function () {\n           var wzrk = document.createElement('script');\n           wzrk.type = 'text/javascript';\n           wzrk.async = true;\n       \t   wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';\n\t         var s = document.getElementsByTagName('script')[0];\n           s.parentNode.insertBefore(wzrk, s);\n       })();\n    </script>\n</head>",
      "language": "javascript",
      "name": "Web"
    },
    {
      "code": "//personalization is enabled by default",
      "language": "text",
      "name": "WinRT"
    },
    {
      "code": "// Code\nCleverTapConfig.enablePersonalization = true;\n\n// XML\n<?xml version=\"1.0\" encoding=\"utf-8\"?>\n<CleverTap>\n    <EnablePersonalization>true</EnablePersonalization>\n</CleverTap>",
      "language": "csharp",
      "name": "Silverlight"
    }
  ]
}
[/block]
# Step 2: Accessing User Profile Data

If you have enabled personalization in your app, you can access User Profile data, and App Usage data as shown below.
[block:code]
{
  "codes": [
    {
      "code": "// To access a scalar-value user profile property:\n// If the property is not available, this call will return null\nString customerType = (String) cleverTap.profile.getProperty(\"Customer Type\");\n\n// To access a multi-value user profile property:\n// If the property is not available, this call will return null\n// To support multi-value user profile properties, cleverTap.profile.getProperty(key) returns an Object.\nJSONArray myStuff = (JSONArray ) clevertap.profile.getProperty(\"myStuff\");",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// To access a scalar-value user profile property:\n// If the property is not available, this call will return nil\nNSString *customerType = [[CleverTap sharedInstance] profileGet:@\"Customer Type\"];\n\n// To access a multi-value user profile property:\n// If the property is not available, this call will return nil\nNSArray *myStuff = [[CleverTap sharedInstance] profileGet:@\"myStuff\"];",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "// To access a scalar-value user profile property:\n// If the property is not available, this call will return nil\nlet customerType = CleverTap.sharedInstance()?.profileGet(\"Customer Type\")\n\n// To access a multi-value user profile property:\n// If the property is not available, this call will return nil\nlet myStuff = (CleverTap.sharedInstance()?.profileGet(\"myStuff\")) as! Array<AnyObject>",
      "language": "swift",
      "name": "iOS (Swift)"
    },
    {
      "code": "// If the property is not available, this call will return undefined\nvar customerType = clevertap.profile.getAttribute(\"Customer type\");",
      "language": "javascript",
      "name": "Web"
    },
    {
      "code": "// Get value of a profile property\nCleverTapInstance.Profile.GetProperty(\"Customer Type\");",
      "language": "csharp",
      "name": "WinRT"
    },
    {
      "code": "// Get value of a profile property\nCleverTapInstance.Profile.GetProperty(\"Customer Type\");",
      "language": "csharp",
      "name": "Silverlight"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "SDK Personalisation",
  "body": "For Web SDKs, CleverTap only supports user properties that have been set in the current session. \nFor example, properties set via the API will not be available for personalization in Mobile and Web SDKs."
}
[/block]