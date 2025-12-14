---
title: Product Experiences_iOS_AB Tests
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
Product Experiences allow you to control and enhance the user experience for your products. 

The CleverTap Product Experiences comprise the following:

* Product Config - Change the behavior and appearance of your app remotely without an update. This helps you to deliver in-app personalization experience to your app users and test their response. 
* A/B Tests - Compare different versions of an app to each other and determine which one performs better. 
* Feature Flags - Toggle a feature on and off.

Product Config is available for CleverTap iOS SDK 3.8.0 and above.

# Set Product Config/AB Tests

The steps to implement Product Config and AB Tests are common. 

## Integrate Product Config/AB Tests

You have to import the CleverTap Product Config/AB Tests Header.

```swift
import CleverTapSDK;
```
```objectivec
#import <CleverTapSDK/CleverTap+ProductConfig.h>
```

## Grab the Product Config/AB Tests singleton object

```swift
var productConfig: CleverTapProductConfig? = CleverTap.sharedInstance()?.productConfig
```
```objectivec
CleverTapProductConfig *productConfig = [[CleverTap sharedInstance] productConfig];
```

## Set in-app default parameter values

You can set in-app default parameter values in the Product Config/AB Tests object so that your app behaves as intended before values are fetched from CleverTap, and so that default values are available if none are set on the dashboard.\
Define a set of parameter names, and default parameter values using a  plist file or an NSDictionary object.

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

## Fetch and activate values

To fetch parameter values from CleverTap, call the `fetch()` method. Any values that you set on the dashboard are fetched and stored in the Product Config/AB Tests object.\
To make fetched parameter values available to your app, call the `activate()` method.

For cases where you want to fetch and activate values in one call, you can use a `fetchAndActivate()`request to fetch values from CleverTap and make them available to the app:

### Fetch

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

Here the interval is used to decide whether we can fetch or not. You can keep the interval as per your requirement, however, you should keep it high to avoid making frequent requests to the backend.

### Activate

```swift
CleverTap.sharedInstance()?.productConfig.activate()
```
```objectivec
[[[CleverTap sharedInstance] productConfig] activate];
```

### FetchAndActivate

```swift
CleverTap.sharedInstance()?.productConfig.fetchAndActivate()
```
```objectivec
[[[CleverTap sharedInstance] productConfig] fetchAndActivate];
```

## Throttling

If an app fetches too many times in a short time period, fetch calls are throttled.\
Throttling is controlled from the CleverTap servers as well from the SDK. The maximum value of the two will be considered to make the next fetch request. You can set the next fetch interval from the SDK below. 

```swift
CleverTap.sharedInstance()?.productConfig.setMinimumFetchInterval(60*10)
```
```objectivec
[[[CleverTap sharedInstance] productConfig] setMinimumFetchInterval:60*10];
```

## Resetting the Product Config/AB Tests

Anytime you want to reset the settings and/or already stored data to start fresh you can do that via the reset method.

```swift
CleverTap.sharedInstance()?.productConfig.reset()
```
```objectivec
[[[CleverTap sharedInstance] productConfig] reset];
```

## Last Fetched Time Stamp

You can always get the timestamp of the configuration fetched last time.

```swift
CleverTap.sharedInstance()?.productConfig.getLastFetchTimeStamp()
```
```objectivec
[[[CleverTap sharedInstance] productConfig] getLastFetchTimeStamp];
```

## Registering the CleverTap Product Config/AB Tests

Check that your class implements the CleverTapProductConfigDelegate and set it to the class.

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

To receive callbacks whenever Product Config/AB Tests is initialized, fetched, and activated, you can implement the below method defined in CleverTapFeatureFlagsDelegate  protocol:

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

## Get parameter values to use in your app

```swift
let ctValue = CleverTap.sharedInstance()?.productConfig.get("key")
```
```objectivec
NSString *stringValue = [[[[CleverTap sharedInstance] productConfig] get:@"key"] stringValue];

BOOL boolValue = [[[[CleverTap sharedInstance] productConfig] get:@"key"] boolValue];

NSNumber *numberValue = [[[[CleverTap sharedInstance] productConfig] get:@"key"] numberValue];

id jsonValue = [[[[CleverTap sharedInstance] productConfig] get:@"key"] jsonValue];
```

# Set Feature Flag

Feature flags let you toggle a feature on and off controlled via CleverTap Backend.

## Grab the Feature Flag singleton object

```swift
var featureFlags: CleverTapFeatureFlags = CleverTap.sharedInstance()?.featureFlags
```
```objectivec
CleverTapFeatureFlags *featureFlags = [[CleverTap sharedInstance] featureFlags];
```

Feature flags are automatically fetched every time a new app session is created. Once the flags are fetched you can get it via the getters. 

## Register the Delegate

```swift
import CleverTapSDK;
```
```objectivec
#import <CleverTapSDK/CleverTap+FeatureFlags.h>
```

To use this, make sure your class implements the CleverTapFeatureFlagsDelegate and set to the class.

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

To receive callbacks whenever feature flags are fetched, you can implement the below method defined in CleverTapFeatureFlagsDelegate protocol.

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

## Getting the Feature Flags

To get the value of a feature simply call get: withDefaultValue: Default value is the value which will be returned in case there are no feature flags with the key.

```swift
let featureFlag = CleverTap.sharedInstance()?.featureFlags.get("key", withDefaultValue:false)
```
```objectivec
BOOL featureFlag = [[[CleverTap sharedInstance] featureFlags] get:@"key" withDefaultValue:true];
```
