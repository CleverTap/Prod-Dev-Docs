---
title: Segment Integration in React Native - iOS
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
Once the Segment library is integrated, toggle CleverTap on in your Segment destinations, and add your CleverTap Account ID and CleverTap Account Token which you can find in the CleverTap Dashboard under Settings.

You can integrate CleverTap via a server-side or mobile destination (iOS or Andriod). If you are interested in using CleverTap’s push notifications or in-app notifications products, you should use mobile destinations.

For Adding Account Id and Account Token ->

## Step 1:

Go to Destination in Segment -> Select CleverTap -> Configure CleverTap.

```javascript
npm add  @segment/analytics-react-native
```

## Step 2:

Add CleverTap Account Id, Account Token and Enable the Switch from the top. For CleverTap Account Id and Account Token, Go to CleverTap Dashboard > Your Application > Settings.

```javascript
pod install
```

## Step 3: Then in your application file App.js, setup the SDK like so:

```javascript
// replace with your write key
analytics.setup('YOUR_WRITE_KEY', {
  // Record screen views automatically!
  recordScreenViews: true,
  // Record certain application events automatically!
  trackAppLifecycleEvents: true
})
```

And of course, import the SDK in the files that you use it with:

```javascript
import analytics from '@segment/analytics-react-native'
```

## Step 4: Now add CleverTap-Segment SDK in react native project.

```javascript
npm add  @segment/analytics-react-native-clevertap
```

And of course, install the pods navigating inside the iOS directory.

```javascript
pod install
```

## Step 4: Now Run the project to check for compile errors. If there are no errors then continue further steps, else check below steps to resolve the errors

## Resolve Compile Error (CleverTap.h file not found):

* Open SEGCleverTapIntegration.m file from pods directory under segment-clevertap pods.
* Change  #import \<CleverTapSDK/CleverTap.h> to #import \<CleverTap-iOS-SDK/CleverTap.h> and hit unlock button to edit and save the changes.
* Run the project again and this should compile successfully without error.

## OR (Recommended Approach) : 

1. Add **use\_modular\_headers!** in your PodFile to enable the stricter search paths and module map generation for all of your pods.
2. Run pod install
3. Navigate to the SEGCleverTapIntegration.m class, and replace `#import<CleverTapSDK/CleverTap.h>` with `@import CleverTapSDK;`.
4. Also, in this scenario you need to import header as `@import CleverTapSDK;` instead of `<CleverTapSDK/CleverTap.h>` in the App Delegate class as well.

## Step 5: Then in your application file App.js, configure CleverTap like so:

```javascript
analytics.setup('YOUR_WRITE_KEY', {
  // Record screen views automatically!
  recordScreenViews: true,
  // Record certain application events automatically!
  trackAppLifecycleEvents: true
  using : [CleverTap]
})
```

And of course, import the ClevertTap SDK in the application file:

```javascript
import CleverTap from '@segment/analytics-react-native-clevertap'
```

## Step 6: Enabling Push Notifications.

In AppDelegate.m file, add code to register for Push Notification. (Assuming PN capability is added to the project)

```javascript
UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
    center.delegate = self;
    [center requestAuthorizationWithOptions:(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge) completionHandler:^(BOOL granted, NSError * _Nullable error){
    if(!error){
      dispatch_async(dispatch_get_main_queue(), ^{
         [[UIApplication sharedApplication] registerForRemoteNotifications];
      });
    }
    }];
```

Initialise CleverTap SDK to register the device token within didFinishLaunchingWithOptions method in AppDelegate.m file.

```javascript
[CleverTap autoIntegrate];
```

## Step 7: Pushing Profile and Events.

Add below code to push profile in application file App.js

```javascript
analytics.identify("User_ID", {
  		email: "test@example.com",
		name : "Test User"
});
//User_ID - Unique Identity of the user
//This method maps to OnUserLogin() method of CleverTap.
```

Add below code to push custom event against a user profile.

```javascript
analytics.track('Sample Event', {
  		'name': 'Sword of Heracles',
  		'eventId': '1234'
});
```

You can see a [sample project](https://github.com/jaysmehta/CleverTapSegmentReactNative) here.

# Related Articles

[https://segment.com/docs/connections/destinations/catalog/clevertap/](https://segment.com/docs/connections/destinations/catalog/clevertap/)\
[https://segment.com/docs/connections/sources/catalog/libraries/mobile/react-native/](https://segment.com/docs/connections/sources/catalog/libraries/mobile/react-native/)\
[https://developer.clevertap.com/docs/react-native](https://developer.clevertap.com/docs/react-native)
