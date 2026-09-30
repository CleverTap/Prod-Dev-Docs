---
title: Android In-App
excerpt: Learn how to send and control In-App notifications.
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

CleverTap provides the capability to send In-App messages with images, GIFs, video, and audio. You can also do A/B testing on your In-App.

<Callout icon="📘" theme="info">
  ### Note

  Ensure your activities extend FragmentActivity.
</Callout>

## Exclude In-App from Android Activity

If your application has a splash screen (for example, logo screen or loading page) displayed on app launch, then the In-App triggered on _App Launch_ would be attempted to be displayed on this screen. As soon as this screen is dismissed, the In-App will be dismissed, too.

In these types of cases, the splash screen must be excluded. Mention the _Activities_ on which you do not want In-App notifications to show in the android: value of the metadata tag in the AndroidManifest.xml file.

```xml
<meta-data
    android:name="CLEVERTAP_INAPP_EXCLUDE"
    android:value="YourSplashActivity1, YourSplashActivity2" />
```

## Fragmentless Banners (Game Engine Integration)

By default, CleverTap renders the HTML header and footer banner in-apps as fragments inside the host `FragmentActivity`. If your integration uses a non-`FragmentActivity` host, for example, a Unity or Unreal Engine game engine wrapper that drives rendering via a `SurfaceView`, Fragments composited over a GL surface can render unreliably.

The `CLEVERTAP_INAPP_FRAGMENTLESS_BANNERS` manifest flag switches the HTML header and footer banners to a `WindowManager` overlay instead of a content-view fragment, making them compatible with non-`FragmentActivity` hosts.

<Callout icon="📘" theme="info">
  #### Availability

  Fragmentless Banners are available from Android SDK 8.4.0.
</Callout>

Add the following entry to your `AndroidManifest.xml`:

```xml
<meta-data
    android:name="CLEVERTAP_INAPP_FRAGMENTLESS_BANNERS"
    android:value="1" />
```

<Callout icon="📘" theme="info">
  ### Scope

  - This flag affects only HTML header and footer banner in-app types (`CTInAppTypeHeaderHTML`, `CTInAppTypeFooterHTML`). Cover, Interstitial, and Half-Interstitial HTML types are not affected and continue to render via `InAppNotificationActivity`.
  - The flag is only activated by the string value `"1"`. Setting `android:value="true"` or a boolean will not enable it.
</Callout>

<Callout icon="🚧" theme="warn">
  ### Regular Integrations

  This flag is intended only for game-engine wrapper integrations that cannot use a `FragmentActivity` host. Do not set it for standard Android integrations.
</Callout>

## Control In-App Notifications

With CleverTap Android SDK 4.2.0 and above, you can control the rendering of In-App notifications by calling the following methods:

### Suspend

Suspends and saves In-App notifications until `resumeInAppNotifications` method is called for the current session.

```kotlin
CleverTapAPI.getDefaultInstance(ctx)!!.suspendInAppNotifications()
```
```java
CleverTapSingleton.getDefaultInstance(ctx).suspendInAppNotifications();
```

### Discard

Suspends the display of In-App Notifications and discards the display of any new In-App notification. It also discards In-App notifications until `resumeInAppNotifications`  method is called for the current session.

```kotlin
CleverTapAPI.getDefaultInstance(ctx)!!.discardInAppNotifications()
```
```java
CleverTapSingleton.getDefaultInstance(ctx).discardInAppNotifications();
```

From CleverTap Android SDK version 7.7.0 and above, you can optionally pass a parameter to control whether the currently visible In-App notification should be dismissed:

#### Parameters

- `dismissInAppIfVisible`
  - If `true`: Immediately dismisses the currently visible In-App notification (if any).
  - If `false`: Allows the currently visible In-App notification to remain on the screen.

```kotlin
fun discardInAppNotifications(dismissInAppIfVisible: Boolean) {
    // Implementation to suspend In-App notifications
}
```
```java
public void discardInAppNotifications(boolean dismissInAppIfVisible) {
    // Implementation to suspend In-App notifications
}
```

<Callout icon="📘" theme="info">
  ### Note

  Dismissing a PIP In-App frees the in-app display slot, so the next queued in-app (if any) may show immediately. To keep a screen clear of all in-apps, pair this with `suspendInAppNotifications()` on screen entry and `resumeInAppNotifications()` on exit.
</Callout>

### Resume

The `resumeInAppNotifications` method resumes displaying in-app notifications.

If you call this method after the `discardInAppNotifications()` method, it resumes the In-App notifications for events raised after the call is performed.

However, if you call the `resumeInAppNotifications` method after the `suspendInAppNotifications()` method, then it displays all queued In-App notifications and also resumes In-App notifications for events raised after the call is performed.

```kotlin
CleverTapAPI.getDefaultInstance(ctx)!!.resumeInAppNotifications()
```
```java
CleverTapSingleton.getDefaultInstance(ctx).resumeInAppNotifications();
```

### Dismiss PIP In-App

The `dismissPipInApp()` method programmatically dismisses the currently visible PIP (Picture-in-Picture) in-app notification. It is a no-op if no in-app is currently displayed or if the visible in-app is not a PIP type.

```swift
CleverTap.sharedInstance()?.dismissPipInApp()
```
```objectivec
#import <CleverTapSDK/CleverTap+InAppNotifications.h>
[[CleverTap sharedInstance] dismissPipInApp];
```

When called, the SDK fires a Notification Clicked event with `wzrk_element_id = "dismissApi"` and `wzrk_c2a = "Dismiss PiP API"`. This is the same Notification Clicked path used by the dismiss button and gesture-based dismissals, distinguished by the `wzrk_element_id` and `wzrk_c2a` values.

<Callout icon="📘" theme="info">
  #### PIP In-App Notes

  - Dismissing a PIP in-app frees the in-app display slot, so the next queued in-app (if any) may show immediately. To keep a screen clear of all in-apps, pair this call with `suspendInAppNotifications()` on screen entry and `resumeInAppNotifications()` on exit.
  - The `dismissPipInApp()` method is available from iOS SDK v7.8.1.
</Callout>

## Callbacks for In-App Notifications

Implement the `InAppNotificationListener` interface to control and respond to the In-App notification lifecycle. This includes rendering logic, display tracking, and dismissal handling.

### In-App Notification `beforeShow`callback

Called before an In-App notification is displayed. Use this callback to determine whether the notification should be shown.

```kotlin
/**
 * Called before an In-App notification is displayed.
 * Return `false` to suppress the notification.
 *
 * @param extras A map of key-value pairs configured in the CleverTap dashboard for this notification.
 * @return `true` to render the notification; `false` to suppress it.
 */
fun beforeShow(extras: Map<String, Any>): Boolean
```
```java
/**
 * Called before an in-app notification is displayed.
 * Return {@code false} to suppress the notification.
 *
 * @param extras A map of key/value pairs configured in the CleverTap dashboard for this notification.
 * @return {@code true} to render the notification, {@code false} to prevent it from being displayed.
 */
boolean beforeShow(Map<String, Object> extras);
```

### In-App Notification`onShow` callback

Called when an In-App notification is displayed to the user.

```kotlin
/**
 * Called when the In-App notification is rendered on screen.
 *
 * @param ctInAppNotification The [CTInAppNotification] object representing the displayed content.
 */
fun onShow(ctInAppNotification: CTInAppNotification)
```
```java
/**
 * Called when the in-app notification is rendered on screen.
 *
 * @param ctInAppNotification The CTInAppNotification object representing the displayed content.
 */
void onShow(CTInAppNotification ctInAppNotification);
```

### In-App Notification `onDismissed` callback

Called after the In-App notification is dismissed through user interaction or auto-close.

```kotlin
/**
 * Called when the In-App notification is dismissed.
 *
 * @param extras A map of key-value pairs set in the dashboard for this notification.
 * @param actionExtras A map of key-value pairs from the selected action, if any. Can be null.
 */
fun onDismissed(
    extras: Map<String, Any>,
    actionExtras: Map<String, Any>?
)
```
```java
/**
 * Called when the in-app notification is dismissed.
 *
 * @param extras       The key/value pairs set in the dashboard for this notification.
 * @param actionExtras The key/value pairs from the selected action (if any). May be null.
 */
void onDismissed(Map<String, Object> extras, @Nullable Map<String, Object> actionExtras);
```

## Javascript Support in In-App Notifications

Android SDK v3.5.0 and above supports embedding JavaScript code inside custom In-Apps. To ensure your JavaScript code works on the app while creating the In-App campaign, select the checkbox for _Enabling JavaScript during the In-App campaign_ creation.


<Image src="https://files.readme.io/185fa3f-image_1.png" alt="Custom HTML Template in In-App Notifications" align="center" caption="Custom HTML Template in In-App Notifications" border={true} />


<br />


<Image src="https://files.readme.io/3edcde1-image_2.png" alt="Include JavaScript in In-App Notifications" align="center" caption="Include JavaScript in In-App Notifications" border={true} />


Consider the following methods explained with examples:

```javascript Javascript
//Recording a User Event called Product Viewed in JS enabled custom In-Apps
if (window.CleverTap) {
  // Call Android interface             
 CleverTap.pushEvent("Product Viewed");          
}

//Recording a User Event called Product Viewed with properties, in JS enabled custom In-Apps
var props = {foo: 'xyz', lang: 'French'};
if (window.CleverTap) {
  // Call Android interface             
 CleverTap.pushEvent("Product Viewed",JSON.stringify(props));
           }

//Updating profile properties of the User in JS enabled custom In-Apps
var props = {foo: 'xyz', lang: 'French'};
if (window.CleverTap) {
  // Call Android interface             
 CleverTap.pushProfile(JSON.stringify(props));          
}

//Adding user property for the User in JS enabled custom In-Apps
if (window.CleverTap) {
  // Call Android interface 
CleverTap.addMultiValueForKey('membership', 'gold');
}

//Adding multiple user properties for the User in JS enabled custom In-Apps
var cars = ['Saab', 'Volvo', 'BMW', 'Kia'];
if (window.CleverTap) {
  // Call Android interface 
CleverTap.addMultiValuesForKey('Cars', JSON.stringify(cars));
}

//removing a user property for a specific key in JS enabled custom In-Apps
if (window.CleverTap) {
// Call Android interface 
CleverTap.removeMultiValueForKey(‘Cars’, 'Saab');
}

//Removing multiple user properties for a specific key in JS enabled custom In-Apps
var cars = ['BMW', 'Kia'];
if (window.CleverTap) {
  // Call Android interface 
CleverTap.removeMultiValuesForKey('Cars', JSON.stringify(cars));
}

//Removing a user property by specifying a key in JS enabled custom In-Apps
if (window.CleverTap) {
  // Call Android interface 
CleverTap.removeValueForKey('Cars');
}

//Setting a user property by specifying the key in JS enabled custom In-Apps

var values = ['Mercedes','Bentley']
if (window.CleverTap) {
  // Call Android interface 
CleverTap.setMultiValueForKey('Cars', JSON.stringify(values));
}


// Increment user property value by specifying the key in JS enabled custom In-Apps

if (window.CleverTap) {
  // Call Android interface 
CleverTap.incrementValue('Cars',2);
}

// Decrement user property value by specifying the key in JS enabled custom In-Apps

if (window.CleverTap) {
  // Call Android interface 
CleverTap.decrementValue('Cars',2)
}

//Calling onUserLogin 

var props = {Identity: '12434', Email: 'xyz@xyz.com'};
if (window.CleverTap) {
  // Call Android interface 
CleverTap.onUserLogin(JSON.stringify(props));
}
```

## In-App Notification Button onClick Callbacks

Android SDK v3.6.1 and above supports callback on the click of In-App notification buttons by returning a map of key-value pairs. To use this, make sure your activity implements the `InAppNotificationButtonListener` and override the following method:

```kotlin Kotlin
override fun onInAppButtonClick(hashMap: HashMap<String, String>?) {
        // Read the values
    }
```
```java JAVA
@Override
public void onInAppButtonClick(HashMap<String, String> hashMap) {
  if(hashMap != null){
    //Read the values
  }
}
```

Set the `InAppNotificationButtonListener` using the following code:

```text Kotlin
cleverTapDefaultInstance?.apply {
       setInAppNotificationButtonListener(this@YourAndroidActivity)
   }
```
```java JAVA
cleverTapInstance.setInAppNotificationButtonListener(this);
```

Also please note that you must define custom key value pairs to receive this callback.

## JavaScript Button Actions in Custom HTML In-Apps

The CleverTap Android SDK provides the `triggerInAppAction()` method to handle button clicks in custom HTML In-App templates using JavaScript. This method supports various types of actions, such as `close`, `open url`, `button click`, `custom template`, `key-value`, and records a `Notification Clicked` event automatically. For more information, refer to the [triggerInAppAction() method](https://github.com/CleverTap/clevertap-android-sdk/blob/8bc9292983acd1d33ea34818908e8647794391e0/docs/InAppNotificationsJavascriptInterface.md#triggerinappactionactionjson-calltoaction-buttonid).

```javascript
window.CleverTap.triggerInAppAction(actionJson, callToAction, buttonId); // Always ensure `window.CleverTap` is available before invoking this method.
```

## Android Push Primer

A _Push Primer_ can explain to your users the need for push notifications and help improve your engagement rates. It is an In-App notification that shows your users the details of message types they can expect before requesting notification permission. It helps with the following:

- Educates your users about the purpose of requesting this permission before invoking a system dialog that asks the user to allow/deny this permission.
- Acts as a precursor to the hard system dialog and thus allows you to ask for permission multiple times if previously denied without making your users search the device settings.

For more information on push primer, refer to [Push Notification Permission](https://developer.clevertap.com/docs/android-push#push-notification-permission).

### Push Primer using Half-Interstitial InApp

This template can send a customized push primer notification, such as displaying an image and modifying the text, button, and background color.


<Image src="https://files.readme.io/1919d3a-push_primer_half_interstitial.png" alt="398" align="center" width="smart" caption="Push Primer Using Half-Interstitial InApp" border={true} />


Use the following to create your template:

```kotlin
val builder = CTLocalInApp.builder()
    .setInAppType(CTLocalInApp.InAppType.HALF_INTERSTITIAL)
    .setTitleText("Get Notified")
    .setMessageText("Please enable notifications on your device to use Push Notifications.")
    .followDeviceOrientation(true)
    .setPositiveBtnText("Allow")
    .setNegativeBtnText("Cancel")
    .setBackgroundColor(Constants.WHITE)
    .setBtnBorderColor(Constants.BLUE)
    .setTitleTextColor(Constants.BLUE)
    .setMessageTextColor(Constants.BLACK)
    .setBtnTextColor(Constants.WHITE)
    .setBtnBackgroundColor(Constants.BLUE)
    .setImageUrl("https://icons.iconarchive.com/icons/treetog/junior/64/camera-icon.png", "Alternate Text")
    .build()
cleverTapAPI.promptPushPrimer(builder)
```
```java
JSONObject jsonObject = CTLocalInApp.builder()
        .setInAppType(CTLocalInApp.InAppType.HALF_INTERSTITIAL)
        .setTitleText("Get Notified")
        .setMessageText("Please enable notifications on your device to use Push Notifications.")
        .followDeviceOrientation(true)
        .setPositiveBtnText("Allow")
        .setNegativeBtnText("Cancel")
        .setBackgroundColor(Constants.WHITE)
        .setBtnBorderColor(Constants.BLUE)
        .setTitleTextColor(Constants.BLUE)
        .setMessageTextColor(Constants.BLACK)
        .setBtnTextColor(Constants.WHITE)
        .setBtnBackgroundColor(Constants.BLUE)
        .setImageUrl("https://icons.iconarchive.com/icons/treetog/junior/64/camera-icon.png", "Alternate Text")
        .build();
cleverTapAPI.promptPushPrimer(jsonObject);
```

### Push Primer using Alert InApp

You can use this template to send a basic push primer notification with a title, message, and two buttons.


<Image src="https://files.readme.io/682aa29-Alert_push_primer.png" alt="398" align="center" width="smart" caption="Push Primer Using Alert In-App" border={true} />


Use the following to create your template:

```kotlin
val builder = CTLocalInApp.builder()
    .setInAppType(CTLocalInApp.InAppType.ALERT)
    .setTitleText("Get Notified")
    .setMessageText("Enable Notification permission")
    .followDeviceOrientation(true)
    .setPositiveBtnText("Allow")
    .setNegativeBtnText("Cancel")
    .build()
cleverTapAPI.promptPushPrimer(builder)
```
```java
JSONObject jsonObject = CTLocalInApp.builder()
        .setInAppType(CTLocalInApp.InAppType.ALERT)
        .setTitleText("Get Notified")
        .setMessageText("Enable Notification permission")
        .followDeviceOrientation(true)
        .setPositiveBtnText("Allow")
        .setNegativeBtnText("Cancel")
        .build();
cleverTapAPI.promptPushPrimer(jsonObject);
```

### Method Description

The following is a description of all the methods used to create In-App templates.

```java
setInAppType(InAppType)
//Required param
//InAppType - CTLocalInApp.InAppType.HALF_INTERSTITIAL & CTLocalInApp.InAppType.ALERT
//Description - Accepts only HALF_INTERSTITIAL & ALERT type to display the type of InApp.

setFallbackToSettings(Boolean)
//Optional param
//Boolean - true/false
//Description - If true and the permission  is denied then we fallback to app’s notification settings, if it’s false then we just throw a verbose log saying permission is denied.

setImageUrl(imageUrl: String, contentDescription: String?)
// Sets the image URL along with an optional content description for accessibility support.

setTitleText(String)
//Required param
//String - Text
//Description - Sets the title of the local InApp.

setMessageText(String)
//Required param
//String - Text
//Description - Sets the subtitle of the local InApp.

followDeviceOrientation(Boolean)
//Required param
//Boolean - true/false
//Description - If true then the local InApp is shown for both portrait and landscape. If it sets false then local InApp only displays for portrait mode.

setPositiveBtnText(String)
//Required param
//String - Text
//Description - Sets the text of the positive button.

setNegativeBtnText(String)
//Required param
//String - Text
//Description - Sets the text of the negative button.

setBackgroundColor(String)
//Optional param
//String - Accepts Hex color as String
//Description - Sets the background color of the local InApp.

setBtnBorderColor(String)
//Optional param
//String - Accepts Hex color as String
//Description - Sets the border color of both positive/negative buttons.

setTitleTextColor(String)
//Optional param
//String - Accepts Hex color as String
//Description - Sets the title color of the local InApp.

setMessageTextColor(String)
//Optional param
//String - Accepts Hex color as String
//Description - Sets the sub-title color of the local InApp.

setBtnTextColor(String)
//Optional param
//String - Accepts Hex color as String
//Description - Sets the color of text for both positive/negative buttons.

setBtnBackgroundColor(String)
//Optional param
//String - Accepts Hex color as String
//Description - Sets the background color for both positive/negative buttons.

setBtnBorderRadius(String)
//Optional param
//String - String
//Description - Sets the radius for both positive/negative buttons. Default radius is “2” if not set.
```

## JavaScript Interface for System Actions in HTML In-App Messages

The CleverTap Android SDK provides a set of JavaScript methods that allow you to trigger predefined actions within _Custom HTML In-App templates_. These methods are accessible only on Android devices, through the following `window.CleverTap` interface:

```javascript
if (window.CleverTap) {
  // Safe to invoke CleverTap JavaScript interface methods
}
```

### Prompt Push Notification Permission

Use this method to request push notification permission on Android 13+ devices. If permission has been denied earlier, you can optionally redirect the user to the app’s notification settings.

```javascript
window.CleverTap.promptPushPermission(shouldShowFallbackSettings);
```

#### Parameters:

- `shouldShowFallbackSettings` _(Boolean)_:

  - `true`: If permission was denied, opens the app’s notification settings screen.
  - `false`: It does not open settings; it logs the denial silently.

#### Example:

```javascript
document.querySelector('#notify_btn').addEventListener('click', () => {
  if (window.CleverTap) {
    window.CleverTap.promptPushPermission(true);
  }
});
```

### Dismiss Current In-App Notification

Dismisses the currently displayed HTML In-App notification from the screen.

```javascript
window.CleverTap.dismissInAppNotification();
```

#### Example:

```javascript
document.querySelector('#close_btn').addEventListener('click', () => {
  if (window.CleverTap) {
    window.CleverTap.dismissInAppNotification();
  }
});
```

### JavaScript API Reference

For the complete list of available JavaScript methods, including event tracking, profile updates, multi-value operations, SDK version checks, and more, refer to the [Custom HTML In-App JavaScript Interface](https://github.com/CleverTap/clevertap-android-sdk/blob/8bc9292983acd1d33ea34818908e8647794391e0/docs/InAppNotificationsJavascriptInterface.md#custom-html-in-app-notifications-javascript-interface).

# Delayed In-App

The CleverTap Android SDK 7.7.0 and above supports Delayed In-App.

This feature allows you to configure a delay of _1 - 1200 seconds_ before displaying an In-App message after a trigger event. Delayed delivery helps you present messages at more meaningful moments. For example:

- Show a discount offer _5 seconds_ after a user views a product.
- Display an upgrade prompt _15 seconds_ after content consumption begins.

For details on creating and managing campaigns using Delayed In-App, refer to [Create In-App Message](https://docs.clevertap.com/docs/create-message-inapp#in-app-delay).

# System In-App Functions

The CleverTap Android SDK (v7.4.0 and above) supports a set of built-in in-app functions. These functions can be triggered via in-app notification button actions or launched as stand-alone campaigns. For more information about the implementation flows, refer to [CleverTap Github Repository](https://github.com/CleverTap/clevertap-android-sdk/blob/f42b633513998bf58a814bc473a8de7035093379/docs/SystemInAppFunctions.md#system-in-app-functions). For more information about using the App Functions from the dashboard, refer to [App Functions](https://docs.clevertap.com/docs/in-app-system-functions).

## Open URL

The _Open URL_ function launches an `Intent` with the `Intent.ACTION_VIEW` action for the specified Android URL. This URL is configured via the CleverTap Dashboard when using a stand-alone in-app template. All query parameters are appended as extras to the `Intent`, which is especially useful when deep linking within the app. This function is commonly used to guide users to product pages, support portals, or feedback forms.

## Push Permission Request

The _Push Permission Request_ function triggers the system prompt to request push notification permissions from users. To listen for the result of the permission request, use:

```java
CleverTapAPI.registerPushPermissionNotificationResponseListener(...)
```

## Request App Rating

The _Request App Rating_ function invokes the native Google Play Store review prompt using the Google Play[ In-App Review API](https://developer.android.com/guide/playcore/in-app-review). It allows users to rate your app without leaving the app experience, typically after a successful purchase or positive interaction.

To enable this function, include the following dependency in your application`build.gradle` file:

```groovy
dependencies {
    // This dependency is downloaded from Google's Maven repository.
    // So, make sure you also include that repository in your project's build.gradle file.
    implementation 'com.google.android.play:review:2.0.2'
}
```

<Callout icon="📘" theme="info">
  ### Note

  For more information on testing In-App reviews, refer to  [Testing Google In-App reviews](https://developer.android.com/guide/playcore/in-app-review) . Prompts are rate-limited and not always guaranteed to show.
</Callout>
