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

```java Android
// Call the method enablePersonalization() on the CleverTapAPI instance (preferably in the onCreate() of your main activity):
public void onCreate(Bundle savedInstanceState) {
  CleverTapAPI cleverTap = CleverTapAPI.getInstance(getApplicationContext());
  cleverTap.enablePersonalization();
}
```
```objectivec iOS (Objective-C)
// To enable this feature, send the enablePersonalization message to the CleverTap SDK (preferably in your AppDelegate):
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    ...
    [CleverTap enablePersonalization];
    ...
}
```
```swift iOS (Swift)
// To enable this feature, send the enablePersonalization message to the CleverTap SDK (preferably in your AppDelegate):
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    ...
    [CleverTap enablePersonalization];
    ...
}
```
```javascript Web
// in your JS embed code, be sure to include clevertap.enablePersonalization = true; as shown below
<head>
    <script type="text/javascript">
    var clevertap = {event:[], profile:[], account:[]};
    clevertap.account.push({"id": "Your CleverTap Account ID"});
    clevertap.enablePersonalization = true; // enables Personalization
    (function () {
           var wzrk = document.createElement('script');
           wzrk.type = 'text/javascript';
           wzrk.async = true;
       	   wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';
	         var s = document.getElementsByTagName('script')[0];
           s.parentNode.insertBefore(wzrk, s);
       })();
    </script>
</head>
```
```text WinRT
//personalization is enabled by default
```
```csharp Silverlight
// Code
CleverTapConfig.enablePersonalization = true;

// XML
<?xml version="1.0" encoding="utf-8"?>
<CleverTap>
    <EnablePersonalization>true</EnablePersonalization>
</CleverTap>
```

# Step 2: Accessing User Profile Data

If you have enabled personalization in your app, you can access User Profile data, and App Usage data as shown below.

```java Android
// To access a scalar-value user profile property:
// If the property is not available, this call will return null
String customerType = (String) cleverTap.profile.getProperty("Customer Type");

// To access a multi-value user profile property:
// If the property is not available, this call will return null
// To support multi-value user profile properties, cleverTap.profile.getProperty(key) returns an Object.
JSONArray myStuff = (JSONArray ) clevertap.profile.getProperty("myStuff");
```
```objectivec iOS (Objective-C)
// To access a scalar-value user profile property:
// If the property is not available, this call will return nil
NSString *customerType = [[CleverTap sharedInstance] profileGet:@"Customer Type"];

// To access a multi-value user profile property:
// If the property is not available, this call will return nil
NSArray *myStuff = [[CleverTap sharedInstance] profileGet:@"myStuff"];
```
```swift iOS (Swift)
// To access a scalar-value user profile property:
// If the property is not available, this call will return nil
let customerType = CleverTap.sharedInstance()?.profileGet("Customer Type")

// To access a multi-value user profile property:
// If the property is not available, this call will return nil
let myStuff = (CleverTap.sharedInstance()?.profileGet("myStuff")) as! Array<AnyObject>
```
```javascript Web
// If the property is not available, this call will return undefined
var customerType = clevertap.profile.getAttribute("Customer type");
```
```csharp WinRT
// Get value of a profile property
CleverTapInstance.Profile.GetProperty("Customer Type");
```
```csharp Silverlight
// Get value of a profile property
CleverTapInstance.Profile.GetProperty("Customer Type");
```

# Step 3: Accessing Session Data

Once you have enabled personalization, you can access the user session information as shown below.

```java Android
// Returns the time elapsed in seconds
int timeElapsed = cleverTap.session.getTimeElapsed();

// Total App Opens
int totalVisits = cleverTap.session.getTotalVisits();

// Activity View Counts
int screenCount = cleverTap.session.getScreenCount();

// Last App Open
// If this is the user's first visit, the call below will return -1
Date lastVisit = new Date(cleverTap.session.getPreviousVisitTime() * 1000l);
```
```objectivec iOS (Objective-C)
// Returns the time elapsed in seconds
int timeElapsed = [[CleverTap sharedInstance] sessionGetTimeElapsed];

// Total App Opens
int totalVisits = [[CleverTap sharedInstance] userGetTotalVisits];

// View Controller Counts
int screenCount = [[CleverTap sharedInstance] userGetScreenCount];

// Last App Open
// If this is the user's first visit, the call below will return -1
NSDate *lastTimeAppLaunched = [[NSDate alloc] initWithTimeIntervalSince1970:[[CleverTap sharedInstance] userGetPreviousVisitTime]];
```
```swift iOS (Swift)
// Returns the time elapsed in seconds
let timeElapsed = CleverTap.sharedInstance()?.sessionGetTimeElapsed()

// Total App Opens
let totalVisits = CleverTap.sharedInstance()?.userGetTotalVisits()

// View Controller Counts
let screenCount = CleverTap.sharedInstance()?.userGetScreenCount()

// Last App Open
// If this is the user's first visit, the call below will return -1
let lastTimeAppLaunched = NSDate(timeIntervalSince1970: (CleverTap.sharedInstance()?.userGetPreviousVisitTime())!)
```
```javascript Web (JavaScript)
// Returns the time elapsed in seconds
var timeElapsed = clevertap.session.getTimeElapsed();

// Total Visit Count
var totalVisits = clevertap.user.getTotalVisits();

// Page Count
var pageCount = clevertap.session.getPageCount();

// Last Active Session
var lastVisit = clevertap.user.getLastVisit();
```
```csharp WinRT
// Time elapsed in current session
CleverTapInstance.Session.GetTimeElapsed();

// Total number of times user has opened the app
CleverTapInstance.Session.GetTotalVisits();

// Number of pages browsed in the current session
CleverTapInstance.Session.GetPageCount();

// Time of previous visit
CleverTapInstance.Session.GetPreviousVisitTime();
```
```text Silverlight
// Time elapsed in current session
CleverTapInstance.Session.GetTimeElapsed();

// Total number of times user has opened the app
CleverTapInstance.Session.GetTotalVisits();

// Number of pages browsed in the current session
CleverTapInstance.Session.GetPageCount();

// Time of previous visit
CleverTapInstance.Session.GetPreviousVisitTime();
```

# Step 4: Accessing Events

You can use the API to query information about the action that the user has performed in the past.

```text Android
// If the details for a given event is not available, this call will return null
EventDetail charged = cleverTap.event.getDetails("Charged");
Date firstTime = new Date(charged.getFirstTime() * 1000l);
Date lastTime = new Date(charged.getLastTime() * 1000l);
int count = charged.getCount();

//Action Event History
// If the user history is not available, this call will return null
Map<String, EventDetail> history = cleverTap.event.getHistory();
```
```objectivec iOS (Objective-C)
// If the details for a given event is not available, this call will return nil
CleverTapEventDetail *ed = [[CleverTap sharedInstance] eventGetDetail:@"Charged"];
NSDate *firstTime = [[NSDate alloc] initWithTimeIntervalSince1970:ed.firstTime];
NSDate *lastTime = [[NSDate alloc] initWithTimeIntervalSince1970:ed.lastTime];
int count = ed.count;
Action Event History

// If the user history is not available, this call will return nil
// The dictionary returned is keyed by the event name
// The values are CleverTapEventDetail objects
NSDictionary *history = [[CleverTap sharedInstance] userGetEventHistory];
```
```swift iOS (Swift)
// If the details for a given event is not available, this call will return nil
let ed = CleverTap.sharedInstance()?.eventGetDetail("Charged")
let firstTime = NSDate(timeIntervalSince1970: (ed?.firstTime)!)
let lastTime = NSDate(timeIntervalSince1970: (ed?.lastTime)!)
let count = ed?.count
Action Event History

// If the user history is not available, this call will return nil
// The dictionary returned is keyed by the event name
// The values are CleverTapEventDetail objects
let history = CleverTap.sharedInstance()?.userGetEventHistory()
```
```javascript Web
var eventDetail = clevertap.event.getDetails("Charged");
var firstTime = eventDetail.firstTime; // Date
var lastTime = eventDetail.lastTime; // Date
var count = eventDetail.count; // int
```
```csharp WinRT
// First time the event occurred
CleverTapInstance.Event.GetFirstTime("Charged");

// Last time the event occurred
CleverTapInstance.Event.GetLastTime("Charged");

// Number of times event occurred previously
CleverTapInstance.Event.GetOccurences("Charged");
```
```csharp Silverlight
// First time the event occurred
CleverTapInstance.Event.GetFirstTime("Charged");

// Last time the event occurred
CleverTapInstance.Event.GetLastTime("Charged");

// Number of times event occurred previously
CleverTapInstance.Event.GetOccurences("Charged");
```

# Step 5: User Profile Synchronization

The SDK periodically synchronizes the User Profile on the device to reflect changes made to the profile outside of the device.

```java Android
/**
 * You can register to have your application code be notified of User Profile synchronization updates
 * by implementing the com.clevertap.android.sdk.SyncListener.
 *
 * Your listener will then be called with a JSONObject in the form:
 */

{
    "profile": {
        "<property1>": {
            "oldValue": < value > ,
            "newValue": < value >
        },
        ...
    },
    "events": {
        "< eventName> ": {
            "count": {
                "oldValue": < old count > ,
                "newValue": < new count >
            },
            "firstTime": {
                "oldValue": < old first time event occurred > ,
                "newValue": < new first time event occurred >
            },
            "lastTime": {
                "oldValue": < old last time event occurred > ,
                "newValue": < new last time event occurred >
            },
        }
    }
}
```
```objectivec iOS
/**
 * The SDK leverages the NSNotification broadcast mechanism to notify your application when these changes occur.
 * You can receive these change updates by observing the CleverTapProfileDidChangeNotification.
 * Change data will be returned in the userInfo property of the NSNotification, and is of the form:
 */
{
    "profile": {
        "<property1>": {
            "oldValue": < value > ,
            "newValue": < value >
        },
        ...
    },
    "events": {
        "< eventName> ": {
            "count": {
                "oldValue": < old count > ,
                "newValue": < new count >
            },
            "firstTime": {
                "oldValue": < old first time event occurred > ,
                "newValue": < new first time event occurred >
            },
            "lastTime": {
                "oldValue": < old last time event occurred > ,
                "newValue": < new last time event occurred >
            },
        }
    }
}

/**
 * The CleverTapSyncDelegate protocol provides an additional/alternative method for notifying
 * your application (the adopting delegate) about synchronization-related changes to the User Profile.
 */
```
