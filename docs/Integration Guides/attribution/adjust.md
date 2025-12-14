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
[block:callout]
{
  "type": "warning",
  "body": "The Adjust SDK must be separately integrated in your app"
}
[/block]
# Android Integration

In your first activity, add the below listed code before you initialize the Adjust SDK (before the line Adjust.onCreate(config);)
[block:code]
{
  "codes": [
    {
      "code": "String appToken = \"{ADJUST_APP_TOKEN}\";\nString environment = AdjustConfig.ENVIRONMENT_PRODUCTION;\nAdjustConfig config = new AdjustConfig(this, appToken, environment);\n\nconfig.setOnAttributionChangedListener(new OnAttributionChangedListener() {\n    @Override\n    public void onAttributionChanged(AdjustAttribution attribution) {\n        String source = attribution.network;\n        String medium = attribution.trackerName;\n        String campaign = attribution.campaign;\n        cleverTap.pushInstallReferrer(source, medium, campaign);\n    }\n});\nAdjust.onCreate(config);",
      "language": "java"
    }
  ]
}
[/block]
# iOS Integration

Update your app delegate’s interface declaration to include the AdjustDelegate protocol.
[block:code]
{
  "codes": [
    {
      "code": "#import <UIKit/UIKit.h>\n#import \"Adjust.h\"\n\n@interface AppDelegate : UIResponder <UIApplicationDelegate, AdjustDelegate>\n@property (strong, nonatomic) UIWindow *window;\n@end",
      "language": "objectivec"
    }
  ]
}
[/block]
Implement the method adjustAttributionChanged: in your app delegate.
[block:code]
{
  "codes": [
    {
      "code": "- (void) adjustAttributionChanged:(ADJAttribution *)attribution {\n    NSString *campaign = attribution.campaign;\n    NSString *source = attribution.network;\n    NSString *medium = attribution.trackerName;\n\n    [CleverTap pushInstallReferrer source:source medium:medium campaign:campaign];\n}",
      "language": "objectivec"
    }
  ]
}
[/block]
Update your application:didFinishLaunchingWithOptions: to set the Adjust delegate.
[block:code]
{
  "codes": [
    {
      "code": "- (BOOL) application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *) launchOptions {\n    ...\n    NSString *yourAppToken = @\"{ADJUST_APP_TOKEN}\";\n    NSString *environment = ADJEnvironmentProduction;\n    ADJConfig *adjustConfig = [ADJConfig configWithAppToken:yourAppToken\n                                                environment:environment];\n\n    [adjustConfig setDelegate:self];\n    [Adjust appDidLaunch:adjustConfig];\n    ...\n    return YES;\n}",
      "language": "objectivec"
    }
  ]
}
[/block]
# Finding Users
To find those users that were acquired via a particular app install campaign, follow the steps below.
  * Go to CleverTap Dashboard -> Find users.
  * Select App Launched as the “DID” event.
  * Select First Time as “Yes” as shown below.
  * Select UTM Source and set the appropriate value.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/097c4a0-adjust.png",
        "adjust.png",
        893,
        410,
        "#f7f8f6"
      ],
      "border": true
    }
  ]
}
[/block]