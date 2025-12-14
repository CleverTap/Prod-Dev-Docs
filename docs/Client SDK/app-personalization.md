---
title: App Personalization
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
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
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        CleverTap.enablePersonalization()
        return true
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

> 📘 SDK Personalisation
>
> For Web SDKs, CleverTap only supports user properties that have been set in the current session.\
> For example, properties set via the API will not be available for personalization in Mobile and Web SDKs.
