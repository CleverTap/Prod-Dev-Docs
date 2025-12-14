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


##Integrate Product Config/AB Tests
You have to import the CleverTap Product Config/AB Tests Header.
[block:code]
{
  "codes": [
    {
      "code": "import CleverTapSDK;\n",
      "language": "swift"
    },
    {
      "code": "#import <CleverTapSDK/CleverTap+ProductConfig.h>\n  \n  ",
      "language": "objectivec"
    }
  ]
}
[/block]
##Grab the Product Config/AB Tests singleton object
[block:code]
{
  "codes": [
    {
      "code": "var productConfig: CleverTapProductConfig? = CleverTap.sharedInstance()?.productConfig\n\n",
      "language": "swift"
    },
    {
      "code": "CleverTapProductConfig *productConfig = [[CleverTap sharedInstance] productConfig];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
##Set in-app default parameter values
You can set in-app default parameter values in the Product Config/AB Tests object so that your app behaves as intended before values are fetched from CleverTap, and so that default values are available if none are set on the dashboard.
Define a set of parameter names, and default parameter values using a  plist file or an NSDictionary object.

**plist file** 
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.sharedInstance()?.productConfig.setDefaultsFromPlistFileName(\"ProductConfigDefaults\")\n\n",
      "language": "swift",
      "name": null
    },
    {
      "code": "[[[CleverTap sharedInstance] productConfig] setDefaultsFromPlistFileName:@\"ProductConfigDefaults\"];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
**NSDictionary object **
[block:code]
{
  "codes": [
    {
      "code": " let defaults = NSMutableDictionary()\n defaults.setValue(\"value\", forKey: \"key\")\n CleverTap.sharedInstance()?.productConfig.setDefaults(defaults as? [String : NSObject])\n\n",
      "language": "swift"
    },
    {
      "code": "NSMutableDictionary *defaults = [NSMutableDictionary new];\n[defaults setValue:@\"value\" forKey:@\"key\"];\n[[[CleverTap sharedInstance] productConfig] setDefaults:[defaults mutableCopy]];",
      "language": "objectivec"
    }
  ]
}
[/block]
##Fetch and activate values

To fetch parameter values from CleverTap, call the `fetch()` method. Any values that you set on the dashboard are fetched and stored in the Product Config/AB Tests object.
To make fetched parameter values available to your app, call the `activate()` method.

For cases where you want to fetch and activate values in one call, you can use a `fetchAndActivate()`request to fetch values from CleverTap and make them available to the app:

###Fetch
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.sharedInstance()?.productConfig.fetch()",
      "language": "swift"
    },
    {
      "code": "[[[CleverTap sharedInstance] productConfig] fetch];\n\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
By default, the fetch calls are throttled, which is controlled from the CleverTap servers as well as SDK. To know more see the Throttling section.

In some specific cases if you require to call fetch without the default throttling, you can use fetchWithMinimumInterval:
[block:code]
{
  "codes": [
    {
      "code": " CleverTap.sharedInstance()?.productConfig.fetch(withMinimumInterval: 60*10)\n",
      "language": "swift"
    },
    {
      "code": " [[[CleverTap sharedInstance] productConfig] fetchWithMinimumInterval:60*10];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
Here the interval is used to decide whether we can fetch or not. You can keep the interval as per your requirement, however, you should keep it high to avoid making frequent requests to the backend.

###Activate
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.sharedInstance()?.productConfig.activate()\n",
      "language": "swift"
    },
    {
      "code": "[[[CleverTap sharedInstance] productConfig] activate];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
###FetchAndActivate

[block:code]
{
  "codes": [
    {
      "code": "CleverTap.sharedInstance()?.productConfig.fetchAndActivate()\n",
      "language": "swift"
    },
    {
      "code": "[[[CleverTap sharedInstance] productConfig] fetchAndActivate];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
##Throttling

If an app fetches too many times in a short time period, fetch calls are throttled.
Throttling is controlled from the CleverTap servers as well from the SDK. The maximum value of the two will be considered to make the next fetch request. You can set the next fetch interval from the SDK below. 

[block:code]
{
  "codes": [
    {
      "code": "CleverTap.sharedInstance()?.productConfig.setMinimumFetchInterval(60*10)\n",
      "language": "swift"
    },
    {
      "code": " [[[CleverTap sharedInstance] productConfig] setMinimumFetchInterval:60*10];\n",
      "language": "objectivec"
    }
  ]
}
[/block]
##Resetting the Product Config/AB Tests
Anytime you want to reset the settings and/or already stored data to start fresh you can do that via the reset method.
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.sharedInstance()?.productConfig.reset()\n\n",
      "language": "swift"
    },
    {
      "code": "[[[CleverTap sharedInstance] productConfig] reset];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
##Last Fetched Time Stamp

You can always get the timestamp of the configuration fetched last time.
[block:code]
{
  "codes": [
    {
      "code": "CleverTap.sharedInstance()?.productConfig.getLastFetchTimeStamp()\n\n",
      "language": "swift"
    },
    {
      "code": "[[[CleverTap sharedInstance] productConfig] getLastFetchTimeStamp];\n",
      "language": "objectivec"
    }
  ]
}
[/block]
##Registering the CleverTap Product Config/AB Tests

Check that your class implements the CleverTapProductConfigDelegate and set it to the class.
[block:code]
{
  "codes": [
    {
      "code": "\nimport UIKit\nimport CleverTapSDK\n\nclass ViewController: UIViewController, CleverTapProductConfigDelegate  {\n\n    override func viewDidLoad() {\n        super.viewDidLoad()\n        ...\n        CleverTap.sharedInstance()?.productConfig.delegate = self;\n    }\n}\n\n",
      "language": "swift"
    },
    {
      "code": "#import \"ViewController.h\"\n#import <CleverTapSDK/CleverTap.h>\n#import <CleverTapSDK/CleverTap+ProductConfig.h>\n\n@interface ViewController () <CleverTapProductConfigDelegate>\n\n@end\n\n@implementation ViewController\n\n- (void)viewDidLoad {\n    [super viewDidLoad];\n    ...\n    [CleverTap sharedInstance].productConfig.delegate = self;\n}\n@end\n  \n  ",
      "language": "objectivec"
    }
  ]
}
[/block]
To receive callbacks whenever Product Config/AB Tests is initialized, fetched, and activated, you can implement the below method defined in CleverTapFeatureFlagsDelegate  protocol:
[block:code]
{
  "codes": [
    {
      "code": "\nfunc ctProductConfigInitialized() {\n    ...  \n    print(\"Product Config Initialized\")\n}\n    \nfunc ctProductConfigFetched() {\n    ...\n    print(\"Product Config Fetched\")\n}\n    \nfunc ctProductConfigActivated() {\n    ...\n    print(\"Product Config Activated\")\n}\n\n",
      "language": "swift"
    },
    {
      "code": "- (void)ctProductConfigInitialized {\n    ...\n    NSLog(@\"Product Config Initialized\");\n}\n \n- (void)ctProductConfigFetched {\n    ...\n    NSLog(@\"Product Config Fetched\");\n}\n \n- (void)ctProductConfigActivated {\n    ...\n    NSLog(@\"Product Config Activated\");\n}\n",
      "language": "objectivec"
    }
  ]
}
[/block]
##Get parameter values to use in your app

[block:code]
{
  "codes": [
    {
      "code": "let ctValue = CleverTap.sharedInstance()?.productConfig.get(\"key\")\n\n",
      "language": "swift"
    },
    {
      "code": "\nNSString *stringValue = [[[[CleverTap sharedInstance] productConfig] get:@\"key\"] stringValue];\n\nBOOL boolValue = [[[[CleverTap sharedInstance] productConfig] get:@\"key\"] boolValue];\n\nNSNumber *numberValue = [[[[CleverTap sharedInstance] productConfig] get:@\"key\"] numberValue];\n\nid jsonValue = [[[[CleverTap sharedInstance] productConfig] get:@\"key\"] jsonValue];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
#Set Feature Flag
Feature flags let you toggle a feature on and off controlled via CleverTap Backend.

##Grab the Feature Flag singleton object

[block:code]
{
  "codes": [
    {
      "code": "var featureFlags: CleverTapFeatureFlags = CleverTap.sharedInstance()?.featureFlags\n",
      "language": "swift"
    },
    {
      "code": "CleverTapFeatureFlags *featureFlags = [[CleverTap sharedInstance] featureFlags];\n",
      "language": "objectivec"
    }
  ]
}
[/block]
Feature flags are automatically fetched every time a new app session is created. Once the flags are fetched you can get it via the getters. 

##Register the Delegate
[block:code]
{
  "codes": [
    {
      "code": "import CleverTapSDK;\n\n",
      "language": "swift"
    },
    {
      "code": "#import <CleverTapSDK/CleverTap+FeatureFlags.h>\n  ",
      "language": "objectivec"
    }
  ]
}
[/block]
To use this, make sure your class implements the CleverTapFeatureFlagsDelegate and set to the class.
[block:code]
{
  "codes": [
    {
      "code": "import UIKit\nimport CleverTapSDK\n\nclass ViewController: UIViewController, CleverTapFeatureFlagsDelegate  {\n\n    override func viewDidLoad() {\n        super.viewDidLoad()\n        ...\n        CleverTap.sharedInstance()?.featureFlags.delegate = self;\n    }\n}\n\n",
      "language": "swift"
    },
    {
      "code": "#import \"ViewController.h\"\n#import <CleverTapSDK/CleverTap.h>\n#import <CleverTapSDK/CleverTap+FeatureFlags.h>\n\n@interface ViewController () <CleverTapFeatureFlagsDelegate>\n\n@end\n\n@implementation ViewController\n\n- (void)viewDidLoad {\n    [super viewDidLoad];\n    ...\n    [CleverTap sharedInstance].featureFlags.delegate = self;\n}\n\n@end\n  ",
      "language": "objectivec"
    }
  ]
}
[/block]
To receive callbacks whenever feature flags are fetched, you can implement the below method defined in CleverTapFeatureFlagsDelegate protocol.

[block:code]
{
  "codes": [
    {
      "code": "\nfunc ctFeatureFlagsUpdated() {\n    ...\n    NSLog(\"CleverTap feature flags updated\")\n}\n",
      "language": "swift"
    },
    {
      "code": "\n- (void)ctFeatureFlagsUpdated {\n    ...\n    NSLog(@\"CleverTap feature flags updated\");\n}\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
##Getting the Feature Flags

To get the value of a feature simply call get: withDefaultValue: Default value is the value which will be returned in case there are no feature flags with the key.





[block:code]
{
  "codes": [
    {
      "code": "let featureFlag = CleverTap.sharedInstance()?.featureFlags.get(\"key\", withDefaultValue:false)\n\n",
      "language": "swift"
    },
    {
      "code": "BOOL featureFlag = [[[CleverTap sharedInstance] featureFlags] get:@\"key\" withDefaultValue:true];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]