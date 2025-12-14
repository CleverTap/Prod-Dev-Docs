---
title: React Native Push
excerpt: Learn how to handle push notifications in React Native.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
# Push Notifications

## Android

### Using Default Push Notification Service

To use Push Notifications out of the box using CleverTap, add the following entries to your `AndroidManifest.xml`.

```xml
<application>
         ....
         ....
        <service android:name="com.clevertap.android.sdk.pushnotification.fcm.FcmMessageListenerService"  android:exported="true">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>

 </application>
```

> 📘 📘 Notification trampolines in Android 12 and above.
>
> To improve performance, Android has changed the handling of push notifications for version 12 and above. An app is no longer allowed to start an activity from within a service or broadcast receiver. For more information, refer to [Android 12 Push Changes](https://developer.clevertap.com/docs/android-push#android-12-changes).

### Creating Notification Channel

Note: You must register at least 1 notification channel for Default Push notification Service to work

```javascript
CleverTap.createNotificationChannel("CtRNS", "Clever Tap React Native Testing", "CT React Native Testing", 5, true) // The notification channel importance can have any value from 1 to 5. A higher value means a more interruptive notification.
```

### Delete Notification Channel

```javascript
CleverTap.deleteNotificationChannel("RNTesting");
```

### Creating a group notification channel

```javascript
CleverTap.createNotificationChannelGroup("groupID", "groupName");
```

### Push Notification Callback

The CleverTap SDK gives you a callback when a Push Notification is clicked along with the entire payload. For this You can register the `CleverTapPushNotificationClicked` callback .

```javascript
CleverTap.addListener(CleverTap.CleverTapPushNotificationClicked, (e)=>{/*consume the event*/})
```

### Integrate RenderMax Push SDK with React Native

The RenderMax Push SDK is supported for Android Native v4.6.6, Android React Native v0.9.3, and above. RenderMax Push Delivery delivers and renders notifications on the user's device even if the FCM delivery fails or the device is optimized for battery consumption. To use CleverTap's RenderMax Push Delivery with your app, update the app's build.gradle file with the following:

```groovy
dependencies {
    implementation 'com.clevertap.android:clevertap-rendermax-sdk:1.0.1'
}
```

> ❗️ Custom Rendering with RenderMax Push SDK
>
> If the app is custom rendering the push notification and not passing the payload to CleverTap SDK, add the following code before you render the notification:
>
> `CleverTapAPI.processPushNotification(getApplicationContext(),extras);`

### Setting a custom Push Notification icon.

By default, our SDK uses the app's icon for both the notification icon and the notification bar icon. However, You can provide a custom app icon by adding the icon in your app's android resources and registering it with clevertap in your `AndroidManifest.xml`\
Checkout [this section](https://developer.clevertap.com/docs/android-push#set-the-small-notification-icon) for more info

### Delete a group notification channel

```javascript
CleverTap.deleteNotificationChannelGroup("groupId");
```

### Using Custom Push Notification Service

If you have your custom implementation for managing Android push notifications, you can inform CleverTap about the user’s FCM registration ID. Refer [Advanced Features Section] ([https://developer.clevertap.com/docs/react-native-advanced-features#registering-fcm-token](https://developer.clevertap.com/docs/react-native-advanced-features#registering-fcm-token)) for more information

### Deeplink or External URL (Android)

To use external URLs, whitelist the IPs or prefix the URL with  *http or https*. For more information, refer to [Deeplinking](https://developer.clevertap.com/docs/android-push#deeplinkexternal-url).

## iOS

1. Please refer to [iOS Push Notifications setup](https://developer.clevertap.com/docs/push-notifications-ios) and follow the same steps till step 4 in the document and in your application in Xcode, please configure the push notifications in your React Native project.

2. Call. the following from your Javascript:

```javascript JavaScript
CleverTap.registerForPush();
```

3. Now go to this page on how to create a push notification [Campaign](https://docs.clevertap.com/docs/push-creation) on CleverTap dashboard or you can follow the remaining steps on CleverTap iOS Push Notifications [page](https://developer.clevertap.com/docs/push-notifications-ios).

## React Native Push Notification Callback on iOS

The CleverTap SDK gives you a callback when a Push Notification is clicked along with the entire payload. For this You can register the `CleverTapPushNotificationClicked` callback .

```javascript
CleverTap.addListener(CleverTap.CleverTapPushNotificationClicked, (e)=>{/*consume the event*/})
```

### Deeplink or External URL (iOS)

A deeplink helps you open a particular activity in your app after a click on the notification. If left empty, the notification on click will open the launcher activity of the app. Deep links allow you to land the user on a particular part of your app. Your app's OpenURL method is called with the deep link specified here. If you want to use external URLs, you will have to whitelist the IPs or provide http/https before the URL so that the SDK can appropriately handle them. For more information, refer to [Deeplinking](https://developer.apple.com/documentation/xcode/supporting-universal-links-in-your-app?language=objc). 

With CleverTap React Native SDK, you can implement custom handling for URLs of in-app notification CTAs, push notifications, and app Inbox messages. 

Check that your class conforms to the `CleverTapURLDelegate` protocol by first calling `setURLDelegate`. Use the following protocol method  `shouldHandleCleverTap(_ : forChannel:)`  to handle URLs received from channels such as in-app notification CTAs, push notifications, and app Inbox messages:

```objectivec
#import <CleverTapSDK/CleverTapURLDelegate.h>

// Set the URL Delegate
[[CleverTap sharedInstance]setUrlDelegate:self];

// CleverTapURLDelegate method 
- (BOOL)shouldHandleCleverTapURL:(NSURL *)url forChannel:(CleverTapChannel)channel {
    NSLog(@"Handling URL: \(%@) for channel: \(%d)", url, channel);
    return YES;
}
```
```swift
// Set the URL Delegate
CleverTap.sharedInstance()?.setUrlDelegate(self)


 // CleverTapURLDelegate method
public func shouldHandleCleverTap(_ url: URL?, for channel: CleverTapChannel) -> Bool {  
    print("Handling URL: \(url!) for channel: \(channel)") 
    return true 
}
```

### Manually Enabling support for deeplink tracking in the application itself

Please visit [iOS Deeplink Tracking](https://developer.clevertap.com/docs/advanced-options-ios#manually-enable-support-for-universal-deep-link-tracking) for reference.

# Push Notification Permission

Starting from the Android 13 and iOS 10.0, applications must request notification permission from the user before pushing notifications. All newly-installed applications must get user permission before they can send notifications. 

## Push Primer

A Push Primer explains the need for push notifications to your users and helps to improve your engagement rates. It is an InApp notification that you can show to your users before requesting notification permission.

Push Primer helps with the following :

* Allows you to educate your users on why you are asking for this permission before invoking a system dialog that asks the user to allow/deny this permission.
* Acts as a precursor to the hard system dialog and thus allows you to ask for permission multiple times if previously denied, without making your users search the device settings.

CleverTap React Native supports push primer for push notification starting with the v0.9.5 release.

> 📘 iOS and Android Versions for Push Primer
>
> * The minimum supported version for the iOS platform is 10.0. 
> * The minimum supported version for the Android platform is Android 13.

There are two ways to handle the new push notification changes:

1. Use a push primer using [Half-Interstitial InApp notification](doc:react-native-push#push-primer-using-half-interstitial-inapp)
2. Use a push primer using [In-App alert](doc:react-native-push#push-primer-using-alert-inapp)

### Push Primer using Half-Interstitial InApp

This template can send a customized push primer notification, such as displaying an image and modifying the text, button, and background color.\
Refer to the below images for Push Primer using Half-Interstitial InApp in Android and iOS:

#### Android

<Image title="Android 1.png" alt={398} className="border" width="smart" border={true} src="https://files.readme.io/36ac314-Android_1.png" />

#### iOS

<Image title="iOS 1.png" alt={948} width="smart" src="https://files.readme.io/bcc4383-iOS_1.png" />

Use the following to create Push Primer using Half-Interstitial InApp:

```javascript
let localInApp = {
              inAppType: 'half-interstitial',
              titleText: 'Get Notified',
              messageText:
                'Please enable notifications on your device to use Push Notifications.',
              followDeviceOrientation: true,
              positiveBtnText: 'Allow',
              negativeBtnText: 'Cancel',
              // Optional parameters:
              backgroundColor: '#FFFFFF',
              btnBorderColor: '#0000FF',
              titleTextColor: '#0000FF',
              messageTextColor: '#000000',
              btnTextColor: '#FFFFFF',
              btnBackgroundColor: '#0000FF',
              btnBorderRadius: '2'
              fallbackToSettings: true, //Setting this parameter to true will open an in-App to redirect you to Mobile's OS settings page.
            };

CleverTap.promptPushPrimer(localInApp);
```

### Push Primer using Alert InApp

This template can send a basic push primer notification with a title, message, and two buttons.\
Refer to the below images for Push Primer using Alert InApp in Android and iOS:

#### Android

<Image title="Android 2.png" alt={398} className="border" width="smart" border={true} src="https://files.readme.io/3b9cd6a-Android_2.png" />

#### iOS

<Image title="iOS 2.png" alt={948} width="smart" src="https://files.readme.io/d52aead-iOS_2.png" />

Use the following to create a Push Primer using Alert InApp:

```javascript
let localInApp = {
              inAppType: 'alert',
              titleText: 'Get Notified',
              messageText: 'Enable Notification permission',
              followDeviceOrientation: true,
              positiveBtnText: 'Allow',
              negativeBtnText: 'Cancel',
              fallbackToSettings: true, //Setting this parameter to true will open an in-App to redirect you to Mobile's OS settings page.
            };

CleverTap.promptPushPrimer(localInApp);
```

### Methods Descriptions

The following table describes all the methods used to create In-App templates:

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Key Name
      </th>

      <th>
        Parameters
      </th>

      <th>
        Description
      </th>

      <th>
        Required
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `inAppType`
      </td>

      <td>
        "half-interstitial" or "alert"
      </td>

      <td>
        Accepts only half-interstitial & alert type to display the local in-app.
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        `titleText`
      </td>

      <td>
        String
      </td>

      <td>
        Sets the title of the local in-app.
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        `messageText`
      </td>

      <td>
        String
      </td>

      <td>
        Sets the subtitle of the local in-app.
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        `followDeviceOrientation`
      </td>

      <td>
        true or false
      </td>

      <td>
        If true, then the local InApp is shown for both portrait and landscape. If false, then local InApp only displays for portrait mode.
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        `positiveBtnText`
      </td>

      <td>
        String
      </td>

      <td>
        Sets the text of the positive button.
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        `negativeBtnText`
      </td>

      <td>
        String
      </td>

      <td>
        Sets the text of the negative button.
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        `fallbackToSettings`
      </td>

      <td>
        true or false
      </td>

      <td>
        If true, and the permission is denied then we fall back to the app’s notification settings, if false, then we just give the callback saying permission is denied.
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        `backgroundColor`
      </td>

      <td>
        Hex color as String
      </td>

      <td>
        Sets the background color of the local in-app.
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        `btnBorderColor`
      </td>

      <td>
        Hex color as String
      </td>

      <td>
        Sets the border color of both positive/negative buttons.
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        `titleTextColor`
      </td>

      <td>
        Hex color as String
      </td>

      <td>
        Sets the title color of the local in-app.
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        `messageTextColor`
      </td>

      <td>
        Hex color as String
      </td>

      <td>
        Sets the sub-title color of the local in-app.
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        `btnTextColor`
      </td>

      <td>
        Hex color as String
      </td>

      <td>
        Sets the color of text for both positive/negative buttons.
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        `btnBackgroundColor`
      </td>

      <td>
        Hex color as String
      </td>

      <td>
        Sets the background color for both positive/negative buttons.
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        `btnBorderRadius`
      </td>

      <td>
        String
      </td>

      <td>
        Sets the radius for both positive/negative buttons. If not set, the default radius is **2** .
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        `fallbackToSettings`
      </td>

      <td>
        true or false
      </td>

      <td>
        If true, then we fallback to app’s notification settings in case the permission is denied. If false, then we just give the callback saying permission is denied.
      </td>

      <td>
        Optional
      </td>
    </tr>
  </tbody>
</Table>

### Invoke the Notification Permission Dialog (without Push Primer)

You can choose to call the push permission dialogue directly without a Push Primer using the `promptForPushPermission(boolean)` method. It takes a boolean value as a parameter. 

* If the value is set to true and permission is denied, then the user will be redirected to the app’s notification settings. 
* If the value is set to false, then the callback will be given, saying permission is denied.

```javascript
CleverTap.promptForPushPermission(true);
```

### Check the Push Notification Permission Status

The `isPushPermissionGranted` method can be used to check the status of the push notification permission for your application. The method returns the status of the push permission in the callback handler.

```javascript
CleverTap.isPushPermissionGranted((err, res) => {
  console.log('isPushPermissionGranted', res, err);
});
```

### Available Callbacks for Push Primer

Based on whether the notification permission is granted or denied, the CleverTap React Native SDK provides a callback with the permission status.\
To receive the callback, you can register the `CleverTapPushPermissionResponseReceived` instance:

```javascript
CleverTap.addListener(CleverTap.CleverTapPushPermissionResponseReceived, (e)=>{/*consume the event*/})
```

To unregister the callback, use the below method:

```javascript
CleverTap.removeListener(CleverTap.CleverTapPushPermissionResponseReceived);
```
