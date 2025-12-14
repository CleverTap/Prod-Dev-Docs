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
# Step 3: Accessing Session Data

Once you have enabled personalization, you can access the user session information as shown below.
[block:code]
{
  "codes": [
    {
      "code": "// Returns the time elapsed in seconds\nint timeElapsed = cleverTap.session.getTimeElapsed();\n\n// Total App Opens\nint totalVisits = cleverTap.session.getTotalVisits();\n\n// Activity View Counts\nint screenCount = cleverTap.session.getScreenCount();\n\n// Last App Open\n// If this is the user's first visit, the call below will return -1\nDate lastVisit = new Date(cleverTap.session.getPreviousVisitTime() * 1000l);",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Returns the time elapsed in seconds\nint timeElapsed = [[CleverTap sharedInstance] sessionGetTimeElapsed];\n\n// Total App Opens\nint totalVisits = [[CleverTap sharedInstance] userGetTotalVisits];\n\n// View Controller Counts\nint screenCount = [[CleverTap sharedInstance] userGetScreenCount];\n\n// Last App Open\n// If this is the user's first visit, the call below will return -1\nNSDate *lastTimeAppLaunched = [[NSDate alloc] initWithTimeIntervalSince1970:[[CleverTap sharedInstance] userGetPreviousVisitTime]];\n",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "// Returns the time elapsed in seconds\nlet timeElapsed = CleverTap.sharedInstance()?.sessionGetTimeElapsed()\n\n// Total App Opens\nlet totalVisits = CleverTap.sharedInstance()?.userGetTotalVisits()\n\n// View Controller Counts\nlet screenCount = CleverTap.sharedInstance()?.userGetScreenCount()\n\n// Last App Open\n// If this is the user's first visit, the call below will return -1\nlet lastTimeAppLaunched = NSDate(timeIntervalSince1970: (CleverTap.sharedInstance()?.userGetPreviousVisitTime())!)",
      "language": "swift",
      "name": "iOS (Swift)"
    },
    {
      "code": "// Returns the time elapsed in seconds\nvar timeElapsed = clevertap.session.getTimeElapsed();\n\n// Total Visit Count\nvar totalVisits = clevertap.user.getTotalVisits();\n\n// Page Count\nvar pageCount = clevertap.session.getPageCount();\n\n// Last Active Session\nvar lastVisit = clevertap.user.getLastVisit();",
      "language": "javascript",
      "name": "Web (JavaScript)"
    },
    {
      "code": "// Time elapsed in current session\nCleverTapInstance.Session.GetTimeElapsed();\n\n// Total number of times user has opened the app\nCleverTapInstance.Session.GetTotalVisits();\n\n// Number of pages browsed in the current session\nCleverTapInstance.Session.GetPageCount();\n\n// Time of previous visit\nCleverTapInstance.Session.GetPreviousVisitTime();",
      "language": "csharp",
      "name": "WinRT"
    },
    {
      "code": "// Time elapsed in current session\nCleverTapInstance.Session.GetTimeElapsed();\n\n// Total number of times user has opened the app\nCleverTapInstance.Session.GetTotalVisits();\n\n// Number of pages browsed in the current session\nCleverTapInstance.Session.GetPageCount();\n\n// Time of previous visit\nCleverTapInstance.Session.GetPreviousVisitTime();",
      "language": "text",
      "name": "Silverlight"
    }
  ]
}
[/block]
# Step 4: Accessing Events

You can use the API to query information about the action that the user has performed in the past.
[block:code]
{
  "codes": [
    {
      "code": "// If the details for a given event is not available, this call will return null\nEventDetail charged = cleverTap.event.getDetails(\"Charged\");\nDate firstTime = new Date(charged.getFirstTime() * 1000l);\nDate lastTime = new Date(charged.getLastTime() * 1000l);\nint count = charged.getCount();\n\n//Action Event History\n// If the user history is not available, this call will return null\nMap<String, EventDetail> history = cleverTap.event.getHistory();",
      "language": "text",
      "name": "Android"
    },
    {
      "code": "// If the details for a given event is not available, this call will return nil\nCleverTapEventDetail *ed = [[CleverTap sharedInstance] eventGetDetail:@\"Charged\"];\nNSDate *firstTime = [[NSDate alloc] initWithTimeIntervalSince1970:ed.firstTime];\nNSDate *lastTime = [[NSDate alloc] initWithTimeIntervalSince1970:ed.lastTime];\nint count = ed.count;\nAction Event History\n\n// If the user history is not available, this call will return nil\n// The dictionary returned is keyed by the event name\n// The values are CleverTapEventDetail objects\nNSDictionary *history = [[CleverTap sharedInstance] userGetEventHistory];",
      "language": "objectivec",
      "name": "iOS (Objective-C)"
    },
    {
      "code": "// If the details for a given event is not available, this call will return nil\nlet ed = CleverTap.sharedInstance()?.eventGetDetail(\"Charged\")\nlet firstTime = NSDate(timeIntervalSince1970: (ed?.firstTime)!)\nlet lastTime = NSDate(timeIntervalSince1970: (ed?.lastTime)!)\nlet count = ed?.count\nAction Event History\n\n// If the user history is not available, this call will return nil\n// The dictionary returned is keyed by the event name\n// The values are CleverTapEventDetail objects\nlet history = CleverTap.sharedInstance()?.userGetEventHistory()",
      "language": "swift",
      "name": "iOS (Swift)"
    },
    {
      "code": "var eventDetail = clevertap.event.getDetails(\"Charged\");\nvar firstTime = eventDetail.firstTime; // Date\nvar lastTime = eventDetail.lastTime; // Date\nvar count = eventDetail.count; // int",
      "language": "javascript",
      "name": "Web"
    },
    {
      "code": "// First time the event occurred\nCleverTapInstance.Event.GetFirstTime(\"Charged\");\n\n// Last time the event occurred\nCleverTapInstance.Event.GetLastTime(\"Charged\");\n\n// Number of times event occurred previously\nCleverTapInstance.Event.GetOccurences(\"Charged\");",
      "language": "csharp",
      "name": "WinRT"
    },
    {
      "code": "// First time the event occurred\nCleverTapInstance.Event.GetFirstTime(\"Charged\");\n\n// Last time the event occurred\nCleverTapInstance.Event.GetLastTime(\"Charged\");\n\n// Number of times event occurred previously\nCleverTapInstance.Event.GetOccurences(\"Charged\");",
      "language": "csharp",
      "name": "Silverlight"
    }
  ]
}
[/block]
# Step 5: User Profile Synchronization

The SDK periodically synchronizes the User Profile on the device to reflect changes made to the profile outside of the device.
[block:code]
{
  "codes": [
    {
      "code": "/**\n * You can register to have your application code be notified of User Profile synchronization updates\n * by implementing the com.clevertap.android.sdk.SyncListener.\n *\n * Your listener will then be called with a JSONObject in the form:\n */\n\n{\n    \"profile\": {\n        \"<property1>\": {\n            \"oldValue\": < value > ,\n            \"newValue\": < value >\n        },\n        ...\n    },\n    \"events\": {\n        \"< eventName> \": {\n            \"count\": {\n                \"oldValue\": < old count > ,\n                \"newValue\": < new count >\n            },\n            \"firstTime\": {\n                \"oldValue\": < old first time event occurred > ,\n                \"newValue\": < new first time event occurred >\n            },\n            \"lastTime\": {\n                \"oldValue\": < old last time event occurred > ,\n                \"newValue\": < new last time event occurred >\n            },\n        }\n    }\n}",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "/**\n * The SDK leverages the NSNotification broadcast mechanism to notify your application when these changes occur.\n * You can receive these change updates by observing the CleverTapProfileDidChangeNotification.\n * Change data will be returned in the userInfo property of the NSNotification, and is of the form:\n */\n{\n    \"profile\": {\n        \"<property1>\": {\n            \"oldValue\": < value > ,\n            \"newValue\": < value >\n        },\n        ...\n    },\n    \"events\": {\n        \"< eventName> \": {\n            \"count\": {\n                \"oldValue\": < old count > ,\n                \"newValue\": < new count >\n            },\n            \"firstTime\": {\n                \"oldValue\": < old first time event occurred > ,\n                \"newValue\": < new first time event occurred >\n            },\n            \"lastTime\": {\n                \"oldValue\": < old last time event occurred > ,\n                \"newValue\": < new last time event occurred >\n            },\n        }\n    }\n}\n\n/**\n * The CleverTapSyncDelegate protocol provides an additional/alternative method for notifying\n * your application (the adopting delegate) about synchronization-related changes to the User Profile.\n */",
      "language": "objectivec",
      "name": "iOS"
    }
  ]
}
[/block]