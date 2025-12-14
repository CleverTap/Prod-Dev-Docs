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
[block:api-header]
{
  "title": "Android"
}
[/block]

#Setting Native Display in Android
We support Native Display for CleverTap Android SDK version 3.6.2 and above. Follow the steps to integrate Native Display with your App. 

##Step 1: Register the Native Display Listener
Use the registerAdListener method to implement AdListener for your class and register for CleverTap instance. 
[block:code]
{
  "codes": [
    {
      "code": "\nimport com.clevertap.android.sdk.CleverTapAPI;\nimport com.clevertap.android.sdk.ads.AdListener;\nimport com.clevertap.android.sdk.ads.model.CleverTapAdUnit;\n\npublic class MainActivity extends AppCompatActivity implements AdListener{\n   \n       @Override\n       protected void onCreate(Bundle savedInstanceState){\n             super.onCreate(savedInstanceState);\n             CleverTapAPI.getDefaultInstance(this).registerAdListener(this);\n       }\n   }\n   @Override\n   public void onAdUnitsLoaded(ArrayList<CleverTapAdUnit> arrayList) {\n       // you will get ad units here\n   }\n}\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
##Step 2: Raise the event from the Android SDK 

You can raise the event for the current Ad Campaign.

[block:code]
{
  "codes": [
    {
      "code": " CleverTapAPI.getDefaultInstance(this).pushEvent(\"Your Event Name\");\n\n\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
You will receive the list of AdUnits via the registered callback for all active Ad campaigns. Implement your logic to create Native Display using these AdUnits.
[block:code]
{
  "codes": [
    {
      "code": " @Override\n   public void onAdUnitsLoaded(ArrayList<CleverTapAdUnit> arrayList) {\n          // implement your logic to create your adviews using these Ad Units here\n          for (int i = 0; i <arrayList.size() ; i++) {\n                CleverTapAdUnit adUnit = arrayList.get(i);\n                prepareAdView(adUnit);\n           }\n   }\n\n\n\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
Refer to our model classes CTAdUnit & CTAdUnitContent for creating Native Display.
[block:code]
{
  "codes": [
    {
      "code": "/**\n* This model class holds the data of an individual Ad Unit.\n*/\npublic class CleverTapAdUnit implements Parcelable {\n\n/**\n* Ad unit identifier\n*/\nprivate String adID;\n\n/**\n* Could be (banner,carousel,custom key value etc.)\n*/\nprivate CTAdConstants.CtAdType adType;\n\n/**\n* background Color\n*/\nprivate String bgColor;\n\n/**\n* List of Ad Content Items\n*/\nprivate ArrayList<CleverTapAdUnitContent> adContentItems;\n/**\n* Custom Key Value Pairs\n*/\nprivate HashMap<String, String> customExtras;\n\n/**\n* @return the AdId of the adUnit\n*/\npublic String getAdID() {\n   return adID;\n}\n\n/**\n* @return the Key Value pair of the adUnit\n*/\npublic HashMap<String, String> getCustomExtras() {\n   return customExtras;\n}\n\n/**\n* @return the background color of the adUnit\n*/\npublic String getBgColor() {\n   return bgColor;\n}\n\n/**\n* @return the AdType of the AdUnit\n*/\npublic CTAdConstants.CtAdType getAdType() {\n   return adType;\n}\n\n/**\n* @return the list of Content Ad Items.\n*/\npublic ArrayList<CleverTapAdUnitContent> getAdContentItems() {\n   return adContentItems;\n}\n\n\n",
      "language": "javascript",
      "name": "CTAdUnit"
    },
    {
      "code": "/**\n* Content class for holding AdContent Data\n*/\npublic class CleverTapAdUnitContent implements Parcelable {\n       private String title;\n       private String titleColor;\n       private String message;\n       private String messageColor;\n       private String media;\n       private String contentType;\n       private String posterUrl;\n       private String actionUrl;\n       private String icon;\n\n/**\n* @return the title section of the AdUnit Content\n*/\npublic String getTitle() {\n   return title;\n}\n\n/**\n* @return the message section of the AdUnit Content\n*/\npublic String getMessage() {\n   return message;\n}\n/**\n* @return the media URL of the AdUnit Content\n*/\npublic String getMedia() {\n   return media;\n}\n/**\n* @return the action URL of the body of the AdUnit Content\n*/\npublic String getActionUrl() {\n   return actionUrl;\n}\n\n\n\n/**\n* @return the URL as String for the icon in case of Icon Message template\n*/\npublic String getIcon() {\n   return icon;\n}\n\n/**\n* @return the hex-code value of the title color as String\n*/\npublic String getTitleColor() {\n   return titleColor;\n}\n\n/**\n* @return the hex-code value of the message color as String\n*/\npublic String getMessageColor() {\n   return messageColor;\n}\n\n/**\n* @return URL for the thumbnail of the video\n*/\npublic String getPosterUrl() {\n   return posterUrl;\n}\n\n/**\n* @return the content type of the media\n*/\npublic String getContentType() {\n   return contentType;\n}\n\n/**\n* Method to check whether media in the {@link CleverTapAdUnitContent} object is an image.\n*\n* @return true if the media type is image\n* false if the media type is not an image\n*/\n\n\npublic boolean mediaIsImage() {\n   return contentType != null && this.media != null \n             && contentType.startsWith(\"image\") && !contentType.equals(\"image/gif\");\n}\n\n/**\n* Method to check whether media in the {@link CleverTapAdUnitContent} object is a GIF.\n*\n* @return true if the media type is GIF\n* false if the media type is not a GIF\n*/\npublic boolean mediaIsGIF() {\n   return contentType != null && this.media != null \n              && contentType.equals(\"image/gif\");\n}\n\n/**\n* Method to check whether media in the {@link CleverTapAdUnitContent} object is a video.\n*\n* @return true if the media type is video\n* false if the media type is not a video\n*/\npublic boolean mediaIsVideo() {\n   return contentType != null && this.media != null \n              &&  contentType.startsWith(\"video\");\n}\n\n/**\n* Method to check whether media in the {@link CleverTapAdUnitContent } object is an audio.\n*\n* @return true if the media type is audio\n* false if the media type is not an audio\n*/\npublic boolean mediaIsAudio() {\n   return contentType != null && this.media != null && contentType.startsWith(\"audio\");\n}\n\n\n\n  ",
      "language": "javascript",
      "name": "CTAdUnitContent"
    }
  ]
}
[/block]
###Get Ad Units

We have also provided a few utility methods to get Adunits corresponding to the last event raised.
Get all the AdUnits
[block:code]
{
  "codes": [
    {
      "code": "/**\n* Getter for retrieving all the adUnits.\n*\n* @return ArrayList<CleverTapAdUnit> - could be null, if there is no ad campaigns running\n*/\n\nCleverTapAPI.getDefaultInstance(this).getAllAdUnits();\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
###Getting AdUnit using AdID:
[block:code]
{
  "codes": [
    {
      "code": "/**\n* Getter for retrieving AdUnit using AdID\n*\n* @param adUnit - ADId {@link CleverTapAdUnit#getAdID()}\n* @return CleverTapAdUnit - could be null, if there is no ad campaign with the identifier\n*/\n\nCleverTapAPI.getDefaultInstance(this).getAdUnitForId(String adUnit);\n\n",
      "language": "javascript"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "If you want to change the display data after it is sent by Clevertap, we recommend that you store (deep copy) the display data locally, update the data and display it.",
  "title": "Note"
}
[/block]



##Step 3: Raise Events for Tracking
You can raise the following events for tracking:

###Notification Viewed Event
You can raise Notification Viewed event using pushAdViewedEventForID method, whenever the user sees the Ad. You must provide the adID of the AdUnit corresponding to the Native Display.
[block:code]
{
  "codes": [
    {
      "code": "/**\n* Raises the Ad Unit Viewed event\n* @param adID - Unique id of the Ad Unit{@link CleverTapAdUnit#getAdID()}\n*/ \nCleverTap API.getDefaultInstance(this).pushAdViewedEventForID(adUnit.getAdID());\n",
      "language": "javascript"
    }
  ]
}
[/block]
###Notification Clicked Event

You can raise the Notification Clicked event using the pushAdClickedEventForID method whenever the Ad is clicked by the user.  You must provide the adID of the AdUnit corresponding to the AdView.

[block:code]
{
  "codes": [
    {
      "code": "\n/**\n* Raises the Ad Unit Clicked event\n* @param adID - Unique id of the Ad Unit{@link CleverTapAdUnit#getAdID()}\n*/\nCleverTap API.getDefaultInstance(this).pushAdClickedEventForID(adUnit.getAdID());\n\n",
      "language": "javascript"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "The events raised by you are automatically accepted by the CleverTap dashboard. We recommend using these methods per your requirements.",
  "title": "Note"
}
[/block]
The most commonly encountered scenario is handling dedup for Notification Viewed event. For example, you have implemented the Native Display using RecyclerView and you raise a Notification Viewed event whenever the Native Display appears on the browser's viewport. If the users scroll through the app multiple times, then multiple Notification Viewed events are raised. 

You may not want to raise multiple events. You can handle this scenario suitably using the android framework callbacks.

###Getting AdUnit using Send Test 
In the send test flow, the AdUnit is delivered via Push Notification. When we receive the notification, we parse the notification payload containing the AdUnits and keep them in memory.

You can get the AdUnit payload by **either** of the following ways:
####Method 1:  By getting all the AdUnits:
[block:code]
{
  "codes": [
    {
      "code": "\nCleverTapAPI.getDefaultInstance(this).getAllAdUnits();\n\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
####Method 2:   Register the AdView Listener in the Application Class. You will receive the AdUnits.
[block:code]
{
  "codes": [
    {
      "code": "\nimport com.clevertap.android.sdk.CleverTapAPI;\nimport com.clevertap.android.sdk.ads.AdListener;\nimport com.clevertap.android.sdk.ads.model.CleverTapAdUnit;\npublic class MyApplication extends Application implements AdListener{\n       CleverTapAPI mCleverTapInstance;\n          @Override\n       protected void onCreate(){\n             super.onCreate();\n             CleverTapAPI.getDefaultInstance(this).registerAdListener(this);\n       }\n   }\n   @Override\n   public void onAdUnitsLoaded(ArrayList<CleverTapAdUnit> arrayList) {\n       // you will get send test ad units here\n   }\n}\n\n\n\n",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "iOS"
}
[/block]
#Setting Native Display in iOS
We support Native Display for CleverTap iOS SDK version 3.7.2 and above. Follow the steps to integrate Native Display with your App. 

##Step 1: Register the CleverTap Native Display

Import the CleverTap Native Display Header.


[block:code]
{
  "codes": [
    {
      "code": "#import <CleverTapSDK/CleverTap+AdUnit.h>\n",
      "language": "objectivec"
    },
    {
      "code": "import CleverTapSDK;\n",
      "language": "swift"
    }
  ]
}
[/block]
To use this, check that your class implements the CleverTapAdUnitDelegate
and set the setAdUnitDelegate to the class.

[block:code]
{
  "codes": [
    {
      "code": "#import \"ViewController.h\"\n#import <CleverTapSDK/CleverTap.h>\n#import <CleverTapSDK/CleverTap+AdUnit.h>\n\n@interface ViewController () <CleverTapAdUnitDelegate>\n\n@end\n\n@implementation ViewController\n\n- (void)viewDidLoad {\n    [super viewDidLoad];\n    ...\n    [[CleverTap sharedInstance] setAdUnitDelegate:self];\n}\n\n@end\n  \n  \n\n\n\n",
      "language": "objectivec"
    },
    {
      "code": "import UIKit\nimport CleverTapSDK\n\nclass ViewController: UIViewController, CleverTapAdUnitDelegate  {\n\n    override func viewDidLoad() {\n        super.viewDidLoad()\n        ...\n        CleverTap.sharedInstance()?.setAdUnitDelegate(self)\n    }\n}\n\n",
      "language": "swift"
    }
  ]
}
[/block]
##Step 2: Handle Native Display data
Use the following method:
[block:code]
{
  "codes": [
    {
      "code": "- (void)adUnitsDidReceive:(NSArray<CleverTapAdUnit *> *)adUnits {\n       // you will get ad units here\n    NSArray <CleverTapAdUnit *> *units = adUnits;\n}\n\n",
      "language": "objectivec"
    },
    {
      "code": "func adUnitsDidReceive(_ adUnits: [CleverTapAdUnit]) {   \n       // you will get ad units here\n    var units:[CleverTapAdUnit] = adUnits;\n}\n\n",
      "language": "swift"
    }
  ]
}
[/block]
##Public APIs

[block:code]
{
  "codes": [
    {
      "code": "@interface CleverTapAdUnit : NSObject\n/*!\n* json defines the ad unit data in the form of NSDictionary.\n*/\n@property (nullable, nonatomic, copy, readonly) NSDictionary *json;\n/*!\n* adID defines the ad unit identifier.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *adID;\n/*!\n* type defines the ad unit type.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *type;\n/*!\n* bgColor defines the backgroundColor of the ad unit.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *bgColor;\n/*!\n* customExtras defines the extra data in the form of an NSDictionary. The extra key/value pairs set in the CleverTap dashboard.\n*/\n@property (nullable, nonatomic, copy, readonly) NSDictionary *customExtras;\n/*!\n* content defines the content of the ad unit.\n*/\n@property (nullable, nonatomic, copy, readonly) NSArray<CleverTapAdUnitContent *> *content;\n\n@end",
      "language": "objectivec",
      "name": "CleverTapAdUnit"
    },
    {
      "code": "\n@interface CleverTapAdUnitContent : NSObject\n/*!\n* title  defines the title section of the ad unit content.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *title;\n/*!\n* titleColor defines hex-code value of the title color as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *titleColor;\n/*!\n* message  defines the message section of the ad unit content.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *message;\n/*!\n* messageColor defines hex-code value of the message color as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *messageColor;\n/*!\n* videoPosterUrl defines video URL of the ad unit as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *videoPosterUrl;\n/*!\n* actionUrl defines action URL of the ad unit as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *actionUrl;\n/*!\n* mediaUrl defines media URL of the ad unit as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *mediaUrl;\n/*!\n* iconUrl defines icon URL of the ad unit as String.\n*/\n@property (nullable, nonatomic, copy, readonly) NSString *iconUrl;\n/*!\n* mediaIsAudio check whether mediaUrl is an audio.\n*/\n@property (nonatomic, readonly, assign) BOOL mediaIsAudio;\n/*!\n* mediaIsVideo check whether mediaUrl is a video.\n*/\n@property (nonatomic, readonly, assign) BOOL mediaIsVideo;\n/*!\n* mediaIsImage check whether mediaUrl is an image.\n*/\n@property (nonatomic, readonly, assign) BOOL mediaIsImage;\n/*!\n* mediaIsGif check whether mediaUrl is a gif.\n*/\n@property (nonatomic, readonly, assign) BOOL mediaIsGif;\n\n\n\n\n\n\n//This method returns all the ad units.\n- (NSArray<CleverTapAdUnit *>*_Nonnull)getAllAdUnits;\n\n//This method returns ad unit for the provided adID\n- (CleverTapAdUnit *_Nullable)getAdUnitForID:(NSString *_Nonnull)adID;\n\n\n",
      "language": "objectivec",
      "name": "CleverTapAdUnitContent"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "If you want to change the display data after it is sent by Clevertap, we recommend that you store (deep copy) the display data locally, update the data and display it.",
  "title": "Note"
}
[/block]
##Step 3:  Raise the events

Raise the Notification Viewed and Notification Clicked events for the Native Display using the following methods, as per the logic.
[block:code]
{
  "codes": [
    {
      "code": "\nCleverTapAdUnit *adUnit;\n[[CleverTap sharedInstance]recordAdUnitViewedEventForID:adUnit.adID];\n\n\n\n\nCleverTapAdUnit *adUnit;\n[[CleverTap sharedInstance]recordAdUnitClickedEventForID:adUnit.adID];\n",
      "language": "objectivec"
    },
    {
      "code": "\nCleverTap.sharedInstance()?.recordAdUnitViewedEvent(forID: adUnit.adID)\n\n\nCleverTap.sharedInstance()?.recordAdUnitViewedEvent(forID: adUnit.adID)\n",
      "language": "swift"
    }
  ]
}
[/block]