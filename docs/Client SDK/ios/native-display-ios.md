---
title: iOS Native Display
excerpt: Learn how to integrate and test Native Display.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

Native Display helps to display content natively within your app without interrupting the user. It also provides the ability to change the content of your app dynamically and deliver relevant and contextual content to your users.

We support Native Display for CleverTap iOS SDK version 3.7.2 and above. Follow the steps listed in the following sections to integrate Native Display with your App. 

> 📘 Support for Linked Content in Native Display
>
> Starting with iOS SDK version 7.3.0, Native Display units support [Linked Content](https://docs.clevertap.com/docs/linked-content), enabling richer, personalized In-App experiences with dynamic data.

## Step 1: Register the CleverTap Display Unit

Import the CleverTap Display Unit Header.

```swift
import CleverTapSDK;
```
```objectivec
#import <CleverTapSDK/CleverTap+DisplayUnit.h>
```

To use this, check that your class implements the CleverTapAdUnitDelegate and set the setAdUnitDelegate to the class.

```swift
import UIKit
import CleverTapSDK

class ViewController: UIViewController, CleverTapDisplayUnitDelegate  {

    override func viewDidLoad() {
        super.viewDidLoad()
        ...
        CleverTap.sharedInstance()?.setDisplayUnitDelegate(self)
    }
}
```
```objectivec
#import "ViewController.h"
#import <CleverTapSDK/CleverTap.h>
#import <CleverTapSDK/CleverTap+DisplayUnit.h>

@interface ViewController () <CleverTapDisplayUnitDelegate>

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    ...
    [[CleverTap sharedInstance] setDisplayUnitDelegate:self];
}

@end
```

## Step 2: Handle Native Display Data

You receive the list of CleverTapDisplayUnits via the registered callback for all active Native Display campaigns. Implement your logic to create Native Display views using these CleverTapDisplayUnits.\
Use the following method:

```swift
func displayUnitsUpdated(_ displayUnits: [CleverTapDisplayUnit]) {   
       // you will get display units here
    var units:[CleverTapDisplayUnit] = displayUnits;
}
```
```objectivec
- (void)displayUnitsUpdated:(NSArray<CleverTapDisplayUnit *>*_Nonnull)displayUnits {
       // you will get display units here
    NSArray <CleverTapDisplayUnit *> *units = displayUnits;
}
```

### Public APIs

Refer to our model classes CleverTapDisplayUnit and CleverTapDisplayUnitContent for creating the Native Display view. We have also provided a few utility methods to get Adunits corresponding to the last event raised.

```objectivec CleverTapDisplayUnit
@interface CleverTapDisplayUnit : NSObject
/*!
* json defines the display unit data in the form of NSDictionary.
*/
@property (nullable, nonatomic, copy, readonly) NSDictionary *json;
/*!
* unitID defines the display unit identifier.
*/
@property (nullable, nonatomic, copy, readonly) NSString *unitID;
/*!
* type defines the display unit type.
*/
@property (nullable, nonatomic, copy, readonly) NSString *type;
/*!
* bgColor defines the backgroundColor of the display unit.
*/
@property (nullable, nonatomic, copy, readonly) NSString *bgColor;
/*!
* customExtras defines the extra data in the form of an NSDictionary. The extra key/value pairs set in the CleverTap dashboard.
*/
@property (nullable, nonatomic, copy, readonly) NSDictionary *customExtras;
/*!
* content defines the content of the display unit.
*/
@property (nullable, nonatomic, copy, readonly) NSArray<CleverTapDisplayUnitContent *> *contents;
@end
  
  /*!
@method
@abstract
This method returns all the display units.
*/
- (NSArray<CleverTapDisplayUnit *>*_Nonnull)getAllDisplayUnits;
 
 /*!
 @method
 @abstract
 This method return display unit for the provided unitID
 */
- (CleverTapDisplayUnit *_Nullable)getDisplayUnitForID:(NSString *_Nonnull)unitID;
```
```objectivec CleverTapDisplayUnitContent
@interface CleverTapDisplayUnitContent : NSObject
/*!
* title defines the title section of the display unit content.
*/
@property (nullable, nonatomic, copy, readonly) NSString *title;
/*!
* titleColor defines hex-code value of the title color as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *titleColor;
/*!
* message defines the message section of the display unit content.
*/
@property (nullable, nonatomic, copy, readonly) NSString *message;
/*!
* messageColor defines hex-code value of the message color as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *messageColor;
/*!
* videoPosterUrl defines video URL of the display unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *videoPosterUrl;
/*!
* actionUrl defines action URL of the display unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *actionUrl;
/*!
* mediaUrl defines media URL of the display unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *mediaUrl;
/*!
* iconUrl defines icon URL of the display unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *iconUrl;
/*!
* mediaIsAudio check whether mediaUrl is an audio.
*/
@property (nonatomic, readonly, assign) BOOL mediaIsAudio;
/*!
* mediaIsVideo check whether mediaUrl is a video.
*/
@property (nonatomic, readonly, assign) BOOL mediaIsVideo;
/*!
* mediaIsImage check whether mediaUrl is an image.
*/
@property (nonatomic, readonly, assign) BOOL mediaIsImage;
/*!
* mediaIsGif check whether mediaUrl is a gif.
*/
@property (nonatomic, readonly, assign) BOOL mediaIsGif;
@end

/*!
@method
@abstract
This method returns all the display units.
*/
- (NSArray<CleverTapDisplayUnit *>*_Nonnull)getAllDisplayUnits;
 
 /*!
 @method
 @abstract
 This method return display unit for the provided unitID
 */
- (CleverTapDisplayUnit *_Nullable)getDisplayUnitForID:(NSString *_Nonnull)unitID;
```

## Step 3:  Raise Events for Tracking

You can raise the following events for tracking:

### Notification Viewed Event

You can raise the *Notification Viewed* event using the `recordDisplayUnitViewedEventForID` method whenever the user sees the Native Display. You must provide the `unitID` of the `CleverTapDisplayUnit` corresponding to the Native Display.

```swift
CleverTap.sharedInstance()?.recordDisplayUnitViewedEvent(forID: displayUnit.unitID)
```
```objectivec
CleverTapDisplayUnit *displayUnit;
[[CleverTap sharedInstance]recordDisplayUnitViewedEventForID:displayUnit.unitID];
```

### Notification Clicked Event

You can raise the *Notification Clicked* event using the `recordDisplayUnitClickedEventForID` method whenever the user clicks the Native Display.  You must provide the `unitID` of the `CleverTapDisplayUnit` corresponding to the Native Display.

```swift
CleverTap.sharedInstance()?.recordDisplayUnitClickedEvent(forID: displayUnit.unitID)
```
```objectivec
CleverTapDisplayUnit *displayUnit;
[[CleverTap sharedInstance]recordDisplayUnitClickedEventForID:displayUnit.unitID];
```

> 📘 Custom Handling
>
> The events that you raise are accepted automatically by the CleverTap dashboard.\
> For example, you have implemented the Native Display using RecyclerView, and you raise a Notification Viewed event whenever the Native Display appears on the browser's viewport. If the users scroll through the app multiple times, multiple Notification Viewed events are raised.

# Setting Account Credentials

If you do not wish to insert your account credentials in your app’s `Info.plist`, or you want to set your account ID programmatically, you can do so in your app delegate, in application:didFinishLaunchingWithOptions:

```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject:AnyObject]?) -> Bool {
    CleverTap.setCredentialsWithAccountID("Your account ID here", andToken: "Your account token here")
    CleverTap.autoIntegrate()
    ...
    return true
}
```
```objectivec Objective-C
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [CleverTap setCredentialsWithAccountID:@"Your account ID here" andToken:@"Your account token here"];
    [CleverTap autoIntegrate];
    ...
    return YES;
}
```

If you have set used this method to set your CleverTap Account ID and Token, please ensure that you do not make an entry for these values in your info.plist files.
