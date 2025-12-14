---
title: Ad View - To be removed
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
## Android

# Setting Native Display in Android

We support Native Display for CleverTap Android SDK version 3.6.2 and above. Follow the steps to integrate Native Display with your App. 

## Step 1: Register the Native Display Listener

Use the registerAdListener method to implement AdListener for your class and register for CleverTap instance. 

```javascript
import com.clevertap.android.sdk.CleverTapAPI;
import com.clevertap.android.sdk.ads.AdListener;
import com.clevertap.android.sdk.ads.model.CleverTapAdUnit;

public class MainActivity extends AppCompatActivity implements AdListener{
   
       @Override
       protected void onCreate(Bundle savedInstanceState){
             super.onCreate(savedInstanceState);
             CleverTapAPI.getDefaultInstance(this).registerAdListener(this);
       }
   }
   @Override
   public void onAdUnitsLoaded(ArrayList<CleverTapAdUnit> arrayList) {
       // you will get ad units here
   }
}
```

## Step 2: Raise the event from the Android SDK 

You can raise the event for the current Ad Campaign.

```javascript
CleverTapAPI.getDefaultInstance(this).pushEvent("Your Event Name");
```

You will receive the list of AdUnits via the registered callback for all active Ad campaigns. Implement your logic to create Native Display using these AdUnits.

```javascript
@Override
   public void onAdUnitsLoaded(ArrayList<CleverTapAdUnit> arrayList) {
          // implement your logic to create your adviews using these Ad Units here
          for (int i = 0; i <arrayList.size() ; i++) {
                CleverTapAdUnit adUnit = arrayList.get(i);
                prepareAdView(adUnit);
           }
   }
```

Refer to our model classes CTAdUnit & CTAdUnitContent for creating Native Display.

```javascript CTAdUnit
/**
* This model class holds the data of an individual Ad Unit.
*/
public class CleverTapAdUnit implements Parcelable {

/**
* Ad unit identifier
*/
private String adID;

/**
* Could be (banner,carousel,custom key value etc.)
*/
private CTAdConstants.CtAdType adType;

/**
* background Color
*/
private String bgColor;

/**
* List of Ad Content Items
*/
private ArrayList<CleverTapAdUnitContent> adContentItems;
/**
* Custom Key Value Pairs
*/
private HashMap<String, String> customExtras;

/**
* @return the AdId of the adUnit
*/
public String getAdID() {
   return adID;
}

/**
* @return the Key Value pair of the adUnit
*/
public HashMap<String, String> getCustomExtras() {
   return customExtras;
}

/**
* @return the background color of the adUnit
*/
public String getBgColor() {
   return bgColor;
}

/**
* @return the AdType of the AdUnit
*/
public CTAdConstants.CtAdType getAdType() {
   return adType;
}

/**
* @return the list of Content Ad Items.
*/
public ArrayList<CleverTapAdUnitContent> getAdContentItems() {
   return adContentItems;
}
```
```javascript CTAdUnitContent
/**
* Content class for holding AdContent Data
*/
public class CleverTapAdUnitContent implements Parcelable {
       private String title;
       private String titleColor;
       private String message;
       private String messageColor;
       private String media;
       private String contentType;
       private String posterUrl;
       private String actionUrl;
       private String icon;

/**
* @return the title section of the AdUnit Content
*/
public String getTitle() {
   return title;
}

/**
* @return the message section of the AdUnit Content
*/
public String getMessage() {
   return message;
}
/**
* @return the media URL of the AdUnit Content
*/
public String getMedia() {
   return media;
}
/**
* @return the action URL of the body of the AdUnit Content
*/
public String getActionUrl() {
   return actionUrl;
}



/**
* @return the URL as String for the icon in case of Icon Message template
*/
public String getIcon() {
   return icon;
}

/**
* @return the hex-code value of the title color as String
*/
public String getTitleColor() {
   return titleColor;
}

/**
* @return the hex-code value of the message color as String
*/
public String getMessageColor() {
   return messageColor;
}

/**
* @return URL for the thumbnail of the video
*/
public String getPosterUrl() {
   return posterUrl;
}

/**
* @return the content type of the media
*/
public String getContentType() {
   return contentType;
}

/**
* Method to check whether media in the {@link CleverTapAdUnitContent} object is an image.
*
* @return true if the media type is image
* false if the media type is not an image
*/


public boolean mediaIsImage() {
   return contentType != null && this.media != null 
             && contentType.startsWith("image") && !contentType.equals("image/gif");
}

/**
* Method to check whether media in the {@link CleverTapAdUnitContent} object is a GIF.
*
* @return true if the media type is GIF
* false if the media type is not a GIF
*/
public boolean mediaIsGIF() {
   return contentType != null && this.media != null 
              && contentType.equals("image/gif");
}

/**
* Method to check whether media in the {@link CleverTapAdUnitContent} object is a video.
*
* @return true if the media type is video
* false if the media type is not a video
*/
public boolean mediaIsVideo() {
   return contentType != null && this.media != null 
              &&  contentType.startsWith("video");
}

/**
* Method to check whether media in the {@link CleverTapAdUnitContent } object is an audio.
*
* @return true if the media type is audio
* false if the media type is not an audio
*/
public boolean mediaIsAudio() {
   return contentType != null && this.media != null && contentType.startsWith("audio");
}
```

### Get Ad Units

We have also provided a few utility methods to get Adunits corresponding to the last event raised.\
Get all the AdUnits

```javascript
/**
* Getter for retrieving all the adUnits.
*
* @return ArrayList<CleverTapAdUnit> - could be null, if there is no ad campaigns running
*/

CleverTapAPI.getDefaultInstance(this).getAllAdUnits();
```

### Getting AdUnit using AdID:

```javascript
/**
* Getter for retrieving AdUnit using AdID
*
* @param adUnit - ADId {@link CleverTapAdUnit#getAdID()}
* @return CleverTapAdUnit - could be null, if there is no ad campaign with the identifier
*/

CleverTapAPI.getDefaultInstance(this).getAdUnitForId(String adUnit);
```

> 📘 Note
>
> If you want to change the display data after it is sent by Clevertap, we recommend that you store (deep copy) the display data locally, update the data and display it.

## Step 3: Raise Events for Tracking

You can raise the following events for tracking:

### Notification Viewed Event

You can raise Notification Viewed event using pushAdViewedEventForID method, whenever the user sees the Ad. You must provide the adID of the AdUnit corresponding to the Native Display.

```javascript
/**
* Raises the Ad Unit Viewed event
* @param adID - Unique id of the Ad Unit{@link CleverTapAdUnit#getAdID()}
*/ 
CleverTap API.getDefaultInstance(this).pushAdViewedEventForID(adUnit.getAdID());
```

### Notification Clicked Event

You can raise the Notification Clicked event using the pushAdClickedEventForID method whenever the Ad is clicked by the user.  You must provide the adID of the AdUnit corresponding to the AdView.

```javascript
/**
* Raises the Ad Unit Clicked event
* @param adID - Unique id of the Ad Unit{@link CleverTapAdUnit#getAdID()}
*/
CleverTap API.getDefaultInstance(this).pushAdClickedEventForID(adUnit.getAdID());
```

> 📘 Note
>
> The events raised by you are automatically accepted by the CleverTap dashboard. We recommend using these methods per your requirements.

The most commonly encountered scenario is handling dedup for Notification Viewed event. For example, you have implemented the Native Display using RecyclerView and you raise a Notification Viewed event whenever the Native Display appears on the browser's viewport. If the users scroll through the app multiple times, then multiple Notification Viewed events are raised. 

You may not want to raise multiple events. You can handle this scenario suitably using the android framework callbacks.

### Getting AdUnit using Send Test 

In the send test flow, the AdUnit is delivered via Push Notification. When we receive the notification, we parse the notification payload containing the AdUnits and keep them in memory.

You can get the AdUnit payload by **either** of the following ways:\
\####Method 1:  By getting all the AdUnits:

```javascript
CleverTapAPI.getDefaultInstance(this).getAllAdUnits();
```

#### Method 2:   Register the AdView Listener in the Application Class. You will receive the AdUnits.

```javascript
import com.clevertap.android.sdk.CleverTapAPI;
import com.clevertap.android.sdk.ads.AdListener;
import com.clevertap.android.sdk.ads.model.CleverTapAdUnit;
public class MyApplication extends Application implements AdListener{
       CleverTapAPI mCleverTapInstance;
          @Override
       protected void onCreate(){
             super.onCreate();
             CleverTapAPI.getDefaultInstance(this).registerAdListener(this);
       }
   }
   @Override
   public void onAdUnitsLoaded(ArrayList<CleverTapAdUnit> arrayList) {
       // you will get send test ad units here
   }
}
```

## iOS

# Setting Native Display in iOS

We support Native Display for CleverTap iOS SDK version 3.7.2 and above. Follow the steps to integrate Native Display with your App. 

## Step 1: Register the CleverTap Native Display

Import the CleverTap Native Display Header.

```objectivec
#import <CleverTapSDK/CleverTap+AdUnit.h>
```
```swift
import CleverTapSDK;
```

To use this, check that your class implements the CleverTapAdUnitDelegate\
and set the setAdUnitDelegate to the class.

```objectivec
#import "ViewController.h"
#import <CleverTapSDK/CleverTap.h>
#import <CleverTapSDK/CleverTap+AdUnit.h>

@interface ViewController () <CleverTapAdUnitDelegate>

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    ...
    [[CleverTap sharedInstance] setAdUnitDelegate:self];
}

@end
```
```swift
import UIKit
import CleverTapSDK

class ViewController: UIViewController, CleverTapAdUnitDelegate  {

    override func viewDidLoad() {
        super.viewDidLoad()
        ...
        CleverTap.sharedInstance()?.setAdUnitDelegate(self)
    }
}
```

## Step 2: Handle Native Display data

Use the following method:

```objectivec
- (void)adUnitsDidReceive:(NSArray<CleverTapAdUnit *> *)adUnits {
       // you will get ad units here
    NSArray <CleverTapAdUnit *> *units = adUnits;
}
```
```swift
func adUnitsDidReceive(_ adUnits: [CleverTapAdUnit]) {   
       // you will get ad units here
    var units:[CleverTapAdUnit] = adUnits;
}
```

## Public APIs

```objectivec CleverTapAdUnit
@interface CleverTapAdUnit : NSObject
/*!
* json defines the ad unit data in the form of NSDictionary.
*/
@property (nullable, nonatomic, copy, readonly) NSDictionary *json;
/*!
* adID defines the ad unit identifier.
*/
@property (nullable, nonatomic, copy, readonly) NSString *adID;
/*!
* type defines the ad unit type.
*/
@property (nullable, nonatomic, copy, readonly) NSString *type;
/*!
* bgColor defines the backgroundColor of the ad unit.
*/
@property (nullable, nonatomic, copy, readonly) NSString *bgColor;
/*!
* customExtras defines the extra data in the form of an NSDictionary. The extra key/value pairs set in the CleverTap dashboard.
*/
@property (nullable, nonatomic, copy, readonly) NSDictionary *customExtras;
/*!
* content defines the content of the ad unit.
*/
@property (nullable, nonatomic, copy, readonly) NSArray<CleverTapAdUnitContent *> *content;

@end
```
```objectivec CleverTapAdUnitContent
@interface CleverTapAdUnitContent : NSObject
/*!
* title  defines the title section of the ad unit content.
*/
@property (nullable, nonatomic, copy, readonly) NSString *title;
/*!
* titleColor defines hex-code value of the title color as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *titleColor;
/*!
* message  defines the message section of the ad unit content.
*/
@property (nullable, nonatomic, copy, readonly) NSString *message;
/*!
* messageColor defines hex-code value of the message color as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *messageColor;
/*!
* videoPosterUrl defines video URL of the ad unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *videoPosterUrl;
/*!
* actionUrl defines action URL of the ad unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *actionUrl;
/*!
* mediaUrl defines media URL of the ad unit as String.
*/
@property (nullable, nonatomic, copy, readonly) NSString *mediaUrl;
/*!
* iconUrl defines icon URL of the ad unit as String.
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






//This method returns all the ad units.
- (NSArray<CleverTapAdUnit *>*_Nonnull)getAllAdUnits;

//This method returns ad unit for the provided adID
- (CleverTapAdUnit *_Nullable)getAdUnitForID:(NSString *_Nonnull)adID;
```

> 📘 Note
>
> If you want to change the display data after it is sent by Clevertap, we recommend that you store (deep copy) the display data locally, update the data and display it.

## Step 3:  Raise the events

Raise the Notification Viewed and Notification Clicked events for the Native Display using the following methods, as per the logic.

```objectivec
CleverTapAdUnit *adUnit;
[[CleverTap sharedInstance]recordAdUnitViewedEventForID:adUnit.adID];




CleverTapAdUnit *adUnit;
[[CleverTap sharedInstance]recordAdUnitClickedEventForID:adUnit.adID];
```
```swift
CleverTap.sharedInstance()?.recordAdUnitViewedEvent(forID: adUnit.adID)


CleverTap.sharedInstance()?.recordAdUnitViewedEvent(forID: adUnit.adID)
```
