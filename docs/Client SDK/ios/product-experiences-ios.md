---
title: iOS Product Experiences (Legacy)
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
> 📘 Feature Availability
>
> A new and enhanced version of Product Experiences is coming soon. New customers (CleverTap for Enterprise or CleverTap for Startups) who have not enabled the current functionalities can use this feature only when the new version is released. However, the existing users can continue to use it. The [methods](https://developer.clevertap.com/docs/may-2023#ios-500) for the Product Experiences feature have been deprecated and will be removed from the code by September 2024.

# Overview

With Product Experiences, you can change the behavior and appearance of your app remotely without an update. This helps you to deliver in-app personalization experience to your app users and test their response. You can use product config to modify app behaviour and feature flags to add or remove features from your app without performing an app store deployment. Product Config is available for CleverTap iOS SDK 3.8.0 and above.

## 1. Integrate Product Config

You have to import the CleverTap Product Config Header.

```objectivec
#import <CleverTapSDK/CleverTap+ProductConfig.h>
```
```swift
import CleverTapSDK;
```

## 2. Grab the Product Config Singleton Object

The singleton object stores in-app default parameter values, fetches updated parameter values from the backend, and controls when fetched values are made available to your app.

```swift
var productConfig: CleverTapProductConfig? = CleverTap.sharedInstance()?.productConfig
```
```objectivec
CleverTapProductConfig *productConfig = [[CleverTap sharedInstance] productConfig];
```

## 3. Set default parameter values

You can set in-app default parameter values in the Product Config object so that your app behaves as intended before values are fetched from CleverTap, and so that default values are available if none are set on the dashboard.\
Define a set of parameter names, and default parameter values using a `plist` file or an NSDictionary object.

**plist file** 

```swift
CleverTap.sharedInstance()?.productConfig.setDefaultsFromPlistFileName("ProductConfigDefaults")
```
```objectivec
[[[CleverTap sharedInstance] productConfig] setDefaultsFromPlistFileName:@"ProductConfigDefaults"];
```

**NSDictionary object**

```swift
let defaults = NSMutableDictionary()
 defaults.setValue("value", forKey: "key")
 CleverTap.sharedInstance()?.productConfig.setDefaults(defaults as? [String : NSObject])
```
```objectivec
NSMutableDictionary *defaults = [NSMutableDictionary new];
[defaults setValue:@"value" forKey:@"key"];
[[[CleverTap sharedInstance] productConfig] setDefaults:[defaults mutableCopy]];
```

## 4. Fetch and activate values

To fetch parameter values from CleverTap, call the `fetch()` method. Any values that you set on the dashboard are fetched and stored in the Product Config object.\
To make fetched parameter values available to your app, call the `activate()` method.

For cases where you want to fetch and activate values in one call, you can use a `fetchAndActivate()`request to fetch values from CleverTap and make them available to the app:

## 5. Fetch:

```swift
CleverTap.sharedInstance()?.productConfig.fetch()
```
```objectivec
[[[CleverTap sharedInstance] productConfig] fetch];
```

By default, the fetch calls are throttled, which is controlled from the CleverTap servers as well as SDK. To know more see the Throttling section. 

In some specific cases if you require to call fetch without the default throttling, you can use fetchWithMinimumInterval:

```swift
CleverTap.sharedInstance()?.productConfig.fetch(withMinimumInterval: 60*10)
```
```objectivec
[[[CleverTap sharedInstance] productConfig] fetchWithMinimumInterval:60*10];
```

Here the interval is used to decide whether we can fetch or not. You can keep the interval as per your requirement, however, you should keep it high to avoid making frequent requests to the CleverTap servers.

## 6. Activate

```swift
CleverTap.sharedInstance()?.productConfig.activate()
```
```objectivec
[[[CleverTap sharedInstance] productConfig] activate];
```

## 7. FetchAndActivate

```swift
CleverTap.sharedInstance()?.productConfig.fetchAndActivate()
```
```objectivec
[[[CleverTap sharedInstance] productConfig] fetchAndActivate];
```

## 8. Throttling

If an app fetches too many times in a short time period, fetch calls are throttled.\
Throttling is controlled from the CleverTap servers as well from the SDK. 

The maximum value of the two will be considered to make the next fetch request. The default fetch interval defined in the SDK is 12 Minutes (`CTProductConfigConstants.DEFAULT_MIN_FETCH_INTERVAL_SECONDS`)

You can set the next fetch interval from the SDK below.

```swift
CleverTap.sharedInstance()?.productConfig.setMinimumFetchInterval(60*10)
```
```objectivec
[[[CleverTap sharedInstance] productConfig] setMinimumFetchInterval:60*10];
```

## 9. Register Listener to Receive Product Config Callbacks:

Check that your class implements the CleverTapProductConfigDelegate and set to the class.

```swift
import UIKit
import CleverTapSDK

class ViewController: UIViewController, CleverTapProductConfigDelegate  {

    override func viewDidLoad() {
        super.viewDidLoad()
        ...
        CleverTap.sharedInstance()?.productConfig.delegate = self;
    }
}
```
```objectivec
#import "ViewController.h"
#import <CleverTapSDK/CleverTap.h>
#import <CleverTapSDK/CleverTap+ProductConfig.h>

@interface ViewController () <CleverTapProductConfigDelegate>

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    ...
    [CleverTap sharedInstance].productConfig.delegate = self;
}
@end
```

To receive callbacks whenever the product config is initialized, fetched, and activated, you can implement the below method defined in `CleverTapFeatureFlagsDelegate` protocol:

```swift
func ctProductConfigInitialized() {
    ...  
    print("Product Config Initialized")
}
    
func ctProductConfigFetched() {
    ...
    print("Product Config Fetched")
}
    
func ctProductConfigActivated() {
    ...
    print("Product Config Activated")
}
```
```objectivec
- (void)ctProductConfigInitialized {
    ...
    NSLog(@"Product Config Initialized");
}
 
- (void)ctProductConfigFetched {
    ...
    NSLog(@"Product Config Fetched");
}
 
- (void)ctProductConfigActivated {
    ...
    NSLog(@"Product Config Activated");
}
```

## 10. Get parameter values to use in your app

Now you can get parameter values from the Product Config object. If you set values on the dashboard, fetch them, and then activate them, those values are available to your app. To get these values, call the method listed below that maps to the data type expected by your app, providing the parameter key as an argument:

```swift
let ctValue_bool = CleverTap.sharedInstance()?.productConfig.get("key_bool")
let ctValue_double = CleverTap.sharedInstance()?.productConfig.get("key_double")
let ctValue_long = CleverTap.sharedInstance()?.productConfig.get("key_long")
let ctValue_string = CleverTap.sharedInstance()?.productConfig.get("key_string")
let ctValue_json = CleverTap.sharedInstance()?.productConfig.get("key_json")
```
```objectivec
NSString *stringValue = [[[[CleverTap sharedInstance] productConfig] get:@"key"] stringValue];

BOOL boolValue = [[[[CleverTap sharedInstance] productConfig] get:@"key"] boolValue];

NSNumber *numberValue = [[[[CleverTap sharedInstance] productConfig] get:@"key"] numberValue];

id jsonValue = [[[[CleverTap sharedInstance] productConfig] get:@"key"] jsonValue];
```

The Supported data types are: Boolean, Double, Long, String, and JSON.\
\##Considerations:\
JSON operations will be similar to that of String.\
The getters return the default values of the type in case we don’t find any value for a key.

| Type           | Default Value |
| :------------- | :------------ |
| String or JSON | “”            |
| Long           | 0L            |
| Double         | 0.0D          |
| Boolean        | false         |

## Considerations:

JSON operations will be similar to that of String.\
The getters return the default values of the type in case we don’t find any value for a key.

## 11. Resetting the Product Config

Anytime you want to reset the settings and/or already stored data to start fresh you can do that via the reset method.

```swift
CleverTap.sharedInstance()?.productConfig.reset()
```
```objectivec
[[[CleverTap sharedInstance] productConfig] reset];
```

## 12. Last Fetched Time Stamp

You can always get the timestamp of the configuration fetched last time.

```swift
CleverTap.sharedInstance()?.productConfig.getLastFetchTimeStamp()
```
```objectivec
[[[CleverTap sharedInstance] productConfig] getLastFetchTimeStamp];
```

# Set Feature Flag

Feature flags let you toggle a feature on and off controlled via CleverTap Backend.

## 1. Grab the Feature Flag singleton object

```swift
var featureFlags: CleverTapFeatureFlags = CleverTap.sharedInstance()?.featureFlags
```
```objectivec
CleverTapFeatureFlags *featureFlags = [[CleverTap sharedInstance] featureFlags];
```

Feature flags are automatically fetched every time a new app session is created. Once the flags are fetched you can get it via the getters. 

## 2. Register the Delegate

```swift
import CleverTapSDK;
```
```objectivec
#import <CleverTapSDK/CleverTap+FeatureFlags.h>
```

To use this, make sure your class implements the `CleverTapFeatureFlagsDelegate` and set to the class.

```swift
import UIKit
import CleverTapSDK

class ViewController: UIViewController, CleverTapFeatureFlagsDelegate  {

    override func viewDidLoad() {
        super.viewDidLoad()
        ...
        CleverTap.sharedInstance()?.featureFlags.delegate = self;
    }
}
```
```objectivec
#import "ViewController.h"
#import <CleverTapSDK/CleverTap.h>
#import <CleverTapSDK/CleverTap+FeatureFlags.h>

@interface ViewController () <CleverTapFeatureFlagsDelegate>

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    ...
    [CleverTap sharedInstance].featureFlags.delegate = self;
}

@end
```

To receive callbacks whenever feature flags are fetched, you can implement the below method defined in `CleverTapFeatureFlagsDelegate` protocol.

```swift
func ctFeatureFlagsUpdated() {
    ...
    NSLog("CleverTap feature flags updated")
}
```
```objectivec
- (void)ctFeatureFlagsUpdated {
    ...
    NSLog(@"CleverTap feature flags updated");
}
```

## 3. Getting the Feature Flags

To get the value of a feature, simply call `get: withDefaultValue:` . The default value is the one that will be returned if there are no feature flags with the key.

```swift
let featureFlag = CleverTap.sharedInstance()?.featureFlags.get("key", withDefaultValue:false)
```
```objectivec
BOOL featureFlag = [[[CleverTap sharedInstance] featureFlags] get:@"key" withDefaultValue:true];
```
