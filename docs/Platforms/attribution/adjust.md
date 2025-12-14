---
title: Adjust
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
CleverTap helps you track mobile app installations via Adjust.

After your app has been installed from an Adjust media source, you can retrieve the install data using the Adjust SDK in your app, and let CleverTap know about the installation source, medium, and campaign.

Once you integrate Adjust and CleverTap as shown below, the install attribution data will show up in the specific Campaign Dashboard.

> 🚧 The Adjust SDK must be separately integrated in your app

# Android Integration

In your first activity, add the below listed code before you initialize the Adjust SDK (before the line Adjust.onCreate(config);)

```java
String appToken = "{ADJUST_APP_TOKEN}";
String environment = AdjustConfig.ENVIRONMENT_PRODUCTION;
AdjustConfig config = new AdjustConfig(this, appToken, environment);

config.setOnAttributionChangedListener(new OnAttributionChangedListener() {
    @Override
    public void onAttributionChanged(AdjustAttribution attribution) {
        String source = attribution.network;
        String medium = attribution.trackerName;
        String campaign = attribution.campaign;
        cleverTap.pushInstallReferrer(source, medium, campaign);
    }
});
Adjust.onCreate(config);
```

# iOS Integration

Update your app delegate’s interface declaration to include the AdjustDelegate protocol.

```objectivec
#import <UIKit/UIKit.h>
#import "Adjust.h"

@interface AppDelegate : UIResponder <UIApplicationDelegate, AdjustDelegate>
@property (strong, nonatomic) UIWindow *window;
@end
```

Implement the method adjustAttributionChanged: in your app delegate.

```objectivec
- (void) adjustAttributionChanged:(ADJAttribution *)attribution {
    NSString *campaign = attribution.campaign;
    NSString *source = attribution.network;
    NSString *medium = attribution.trackerName;

    [CleverTap pushInstallReferrer source:source medium:medium campaign:campaign];
}
```

Update your application:didFinishLaunchingWithOptions: to set the Adjust delegate.

```objectivec
- (BOOL) application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *) launchOptions {
    ...
    NSString *yourAppToken = @"{ADJUST_APP_TOKEN}";
    NSString *environment = ADJEnvironmentProduction;
    ADJConfig *adjustConfig = [ADJConfig configWithAppToken:yourAppToken
                                                environment:environment];

    [adjustConfig setDelegate:self];
    [Adjust appDidLaunch:adjustConfig];
    ...
    return YES;
}
```

# React Native

In your React-Native App, add the following code:

```javascript
var adjustConfig = new AdjustConfig(appToken, environment);

adjustConfig.setAttributionCallbackListener(function(attribution) {
    CleverTap.pushInstallReferrer(attribution.network,
attribution.trackerName,attribution.campaign);
});

Adjust.create(adjustConfig);
```

# Finding Users

To find those users that were acquired via a particular app install campaign, follow the steps below.

* Go to CleverTap Dashboard -> Find users.
* Select App Launched as the “DID” event.
* Select First Time as “Yes” as shown below.
* Select UTM Source and set the appropriate value.

<Image title="28.png" alt={2618} className="border" width="100%" border={true} src="https://files.readme.io/1dc6b1d-28.png" />
