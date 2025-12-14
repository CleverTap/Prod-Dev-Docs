---
title: React Native Push
excerpt: Learn how to handle push notifications in React Native.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: link
      title: React Native In-App
      url: https://developer.clevertap.com/docs/react-native-in-app
    - type: link
      title: React Native App Inbox
      url: https://developer.clevertap.com/docs/react-native-app-inbox
    - type: link
      title: React Native Advanced Features
      url: https://developer.clevertap.com/docs/react-native-advanced-features
    - type: link
      title: React Native Resources
      url: https://developer.clevertap.com/docs/react-native-resources
---
# Push Notifications

CleverTap React Native SDK allows you to send push notifications to your React Native applications from the CleverTap dashboard. Using push notifications, you can **push** information to your users even when they are inactive.

## Android

To use CleverTap's default Push Notifications service, add the following entries to your `AndroidManifest.xml`:

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

> 📘 📘 Notification Trampolines in Android 12 and Above
>
> Android has changed the handling of push notifications for version 12 and above to improve performance. An app can no longer start an activity within a service or broadcast receiver. For more information, refer to [Android 12 Push Changes](doc:android-push#android-12-changes).

### Create Notification Channel

> 📘 Note
>
> You must register at least one notification channel for the Default Push notification Service to work.

Add the following to your Javascript file to create a notification channel:

```javascript
CleverTap.createNotificationChannel("CtRNS", "Clever Tap React Native Testing", "CT React Native Testing", 5, true) // The notification channel importance can have any value from 1 to 5. A higher value means a more interruptive notification.
```

The notification channel importance can have any value from 1 to 5. A higher value means a more interruptive notification. For more information, refer to [Notification Channel Importance Levels](https://staging.docs.dev.clevertap.net/docs/android-push#notification-channel-importance-levels).

### Delete Notification Channel

Add the following to your Javascript file to delete a notification channel:

```javascript
CleverTap.deleteNotificationChannel("RNTesting");
```

### Create a group notification channel

Add the following to your Javascript file to create a group notification channel:

```javascript
CleverTap.createNotificationChannelGroup("groupID", "groupName");
```

### Handle Push Notification Callback

The CleverTap SDK sends the callback when a Push Notification is clicked along with the entire payload. For this, you must register the `CleverTapPushNotificationClicked` callback using the following code:

```javascript
CleverTap.addListener(CleverTap.CleverTapPushNotificationClicked, (e)=>{/*consume the event*/})
```

Additional handling to get the notification click callback in the following scenarios:

* Notification Trampoline Handling in Android 12 and Above
* Notification Trampoline Handling when the App is Terminated or Killed

#### Handle Notification Trampolines in Android 12 and Above to get the Notification Click Callback

Due to [notification trampoline restrictions](https://developer.android.com/about/versions/12/behavior-changes-12#notification-trampolines), Android 12 and above do not directly support the `CleverTap.CleverTapPushNotificationClicked` callback. Hence, apps need to add manual handling for Android 12 and above to inform the CleverTap SDK about the notification click and get the `CleverTap.CleverTapPushNotificationClicked` callback.

Add the following code in the `onNewIntent()` method of the Launcher `ReactActivity`:

```kotlin
class MainActivity : ReactActivity() {

override fun onNewIntent(intent: Intent?) {
   super.onNewIntent(intent)

   // On Android 12 and onwards, raise notification clicked event and get the click callback
   if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S) {
       cleverTapDefaultInstance?.pushNotificationClickedEvent(intent!!.extras)
        }
    }
}
```

For detailed information, refer to [Android 12 Push Changes](https://developer.clevertap.com/docs/android-push#android-12-changes).

#### Handle Notification Click Callback when the App is Terminated or Killed

None of the previously mentioned handlings support the `CleverTap.CleverTapPushNotificationClicked` callback in the killed state. To achieve this, your Android platform-specific `Application` class should extend the `CleverTapApplication` class instead of the `Application` class.

```java
import com.clevertap.react.CleverTapApplication;

public class MainApplication extends CleverTapApplication implements ReactApplication {
     @Override
  public void onCreate() {
    CleverTapAPI.setDebugLevel(CleverTapAPI.LogLevel.VERBOSE);
    // ActivityLifecycleCallback.register(this); Required only for v2.2.1 or below
    super.onCreate();
    
    //rest code
  }
}
```

### Enable RenderMax with Android

Starting from CleverTap React Native SDK version 1.2.0, the RenderMax Push SDK functionality is now supported directly within the CleverTap Android SDK. We strongly recommend updating to CleverTap React Native SDK to version 1.2.0 and CleverTap Android SDK to version 5.2.0 to take advantage of this integration and to ensure stability.

> ❗️ Please remove the integrated RenderMax SDK before you upgrade to CleverTap React Native SDK v1.2.0.

> ❗️ Custom Rendering with RenderMax Push SDK
>
> If the app is custom rendering the push notification and not passing the payload to CleverTap SDK, add the following code before you render the notification:
>
> `CleverTapAPI.processPushNotification(getApplicationContext(),extras);`

### Set Up a Custom Push Notification Icon

By default, CleverTap SDK uses the app's icon for both the notification icon and the notification bar icon. However, You can provide a custom app icon by adding the icon in your app's android resources and registering it with CleverTap in your `AndroidManifest.xml`\
For more details, refer to [Android Push](https://developer.clevertap.com/docs/android-push#set-the-small-notification-icon) document.

### Delete a Group Notification Channel

Add the following to your Javascript file to delete a group notification channel:

```javascript
CleverTap.deleteNotificationChannelGroup("groupId");
```

### Handle Custom Push Notification Service

If you have your custom implementation for managing Android push notifications, you can inform CleverTap about the user’s FCM registration ID. Refer to [Advanced Features Section](doc:react-native-advanced-features#registering-fcm-token) for more information

### Deeplink or External URL (Android)

To use external URLs, whitelist the IPs or prefix the URL with  *http* or *https*. For more information, refer to [Deeplinking](doc:android-push#deeplinkexternal-url).

## iOS

Perform the following steps to send push notifications on the iOS platform from your React Native application:

1. Refer to [iOS Push Notifications setup](doc:push-notifications-ios) and follow steps *1* through *4* listed in the document.

2. In your Xcode project settings, perform the following steps to enable the push notification capability:\
   a. Select *Targets* and then  select the *Signing & Capabilities* tab.\
   b. Click **+ Capability** and enter **push** in the filter field and select *Push Notifications*. 

<Image title="Enable Push Notification Capability" alt={1168} align="center" border={true} src="https://files.readme.io/a99cd41-2023-01-25_14-37-31.png">
  Enable Push Notification Capability
</Image>

3. Call the following from your Javascript file:

```javascript JavaScript
CleverTap.registerForPush();
```

4. [Create a Push Notification campaign](https://docs.clevertap.com/docs/push-creation) on CleverTap or follow the steps listed under [CleverTap iOS Push Notifications](doc:push-notifications-ios).

### iOS Foreground Notifications

To receive push notifications when your app is in the foreground, check your AppDelegate class conforms to the`UNUserNotificationCenterDelegate`  and then add `userNotificationCenter:willPresentNotification:withCompletionHandler:` delegate method in your AppDelegate class.

```objectivec
- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler{
    completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}
```
```swift
func userNotificationCenter(_ center: UNUserNotificationCenter,
                                willPresent notification: UNNotification,
                                withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
	completionHandler([.badge, .sound, .alert])
}
```

### Handle React Native Push Notification Callback on iOS

The CleverTap SDK sends a callback when a Push Notification is clicked along with the entire payload. For this, you must register the `CleverTapPushNotificationClicked` callback in your Javascript file, as shown below:

```javascript
CleverTap.addListener(CleverTap.CleverTapPushNotificationClicked, (e)=>{/*consume the event*/})
```

### Deeplink or External URL (iOS)

A deeplink helps you open a particular activity in your app after a click on the notification. If left empty, the notification on click opens the launcher activity of the app. Deep links allow you to land users on a particular part of your app. Your app's OpenURL method is called with the deep link specified here. To use external URLs, you must whitelist the IPs or add *http*/*https* before the URL so that the SDK can appropriately handle them. For more information, refer to [Deeplinking](https://developer.apple.com/documentation/xcode/supporting-universal-links-in-your-app?language=objc). 

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

### Enable Support for Deeplink Tracking Manually in the Application

Refer to [iOS Deeplink Tracking](doc:advanced-options-ios#manually-enable-support-for-universal-deep-link-tracking) for reference.

### Push Impression in iOS

You can now raise and record push notifications delivered to your user's iOS devices. To raise Push Impressions for iOS, you must enable the [Notification Service Extension](https://developer.apple.com/reference/usernotifications/unnotificationserviceextension) on your native iOS side. For more information, refer to our [iOS Push Impressions](https://developer.clevertap.com/docs/push-notifications-ios#push-impressions) documentation. 

# Push Notifications Permission

Starting from Android 13 and iOS 10.0, applications must request notification permission from the user before sending Push Notifications. All newly-installed applications must seek user permission before they can send notifications. 

## Push Primer

A Push Primer explains the need for push notifications to your users and helps to improve your engagement rates. It is an InApp notification that you can show to your users before requesting notification permission.

Push Primer helps with the following:

* Allows you to educate your users on why you are asking for this permission before invoking a system dialog to seek user permission.
* Acts as a precursor to the hard system dialog and thus allows you to seek permission multiple times if previously denied without making your users search the device settings.

CleverTap React Native supports push primer for push notifications starting with the v0.9.5 release.

> 📘 iOS and Android Versions for Push Primer
>
> * The minimum supported version for the iOS platform is **10.0**. 
> * The minimum supported version for the Android platform is **Android 13**.

The following are the two ways to handle the new push notification changes:

* [Push Primer Using Half-Interstitial InApp Notification Template](doc:react-native-push-notification#push-primer-using-half-interstitial-inapp-template)
* [Push Primer Using In-App Alert Template](doc:react-native-push-notification#push-primer-using-alert-inapp-template)

### Push Primer Using Half-Interstitial InApp Template

Using this template, you can send a customized push primer notification with an image, text, and button. You can also modify the notification's text, button, and background color.\
Refer to the following images for Push Primer using Half-Interstitial InApp Template:

* **In Android** 

<Image title="Push Primer Using Half-Interstitial InApp Template in Android" alt={398} align="center" width="sma% " border={true} src="https://files.readme.io/36ac314-Android_1.png">
  Push Primer Using Half-Interstitial InApp Template in Android
</Image>

* **In iOS** 

<Image title="Push Primer Using Half-Interstitial InApp Template in iOS" alt={948} align="center" width="70% " border={true} src="https://files.readme.io/bcc4383-iOS_1.png">
  Push Primer Using Half-Interstitial InApp Template in iOS
</Image>

Add the following code to your Javascript file to create Push Primer using the Half-Interstitial InApp template:

```javascript
let localInApp = {
  inAppType: 'half-interstitial',
  titleText: 'Get Notified',
  messageText: 'Please enable notifications on your device to use Push Notifications.',
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
  btnBorderRadius: '2',

  // Setting this to true will open an in-app redirect to the mobile OS settings page
  fallbackToSettings: true,
  imageUrl: 'https://icons.iconarchive.com/icons/treetog/junior/64/camera-icon.png',
  altText: 'Alternate Image'
};

CleverTap.promptPushPrimer(localInApp);
```

### Push Primer using Alert InApp Template

Using this template, you can create a basic push primer notification with a title, message, and two buttons.\
Refer to the following images for Push Primer using the Alert InApp template:

* **In Android**

<Image title="Push Primer using Alert InApp Template in Android" alt={398} align="center" width="50% " border={true} src="https://files.readme.io/3b9cd6a-Android_2.png">
  Push Primer using Alert InApp Template in Android
</Image>

* **In iOS** 

<Image title="Push Primer using Alert InApp Template in iOS" alt={948} align="center" width="60% " border={true} src="https://files.readme.io/d52aead-iOS_2.png">
  Push Primer using Alert InApp Template in iOS
</Image>

Add the following code to your Javascript file to create a Push Primer using the Alert InApp template:

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

### Method Description

The following table describes all the keys used to create In-App templates:

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        <p>Key Name</p>
      </th>

      <th>
        <p>Parameters</p>
      </th>

      <th>
        <p>Description</p>
      </th>

      <th>
        <p>Required</p>
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        <p><code>inAppType</code></p>
      </td>

      <td>
        <p>"half-interstitial" or "alert"</p>
      </td>

      <td>
        <ul><li>Accepts only the following values: half-interstitial and alert type. </li><li>Used to display the local in-app notification.</li></ul>
      </td>

      <td>
        <p>Required</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>titleText</code></p>
      </td>

      <td>
        <p>String</p>
      </td>

      <td>
        <p>Sets the title of the local in-app notification.</p>
      </td>

      <td>
        <p>Required</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>messageText</code></p>
      </td>

      <td>
        <p>String</p>
      </td>

      <td>
        <p>Sets the subtitle of the local in-app notification.</p>
      </td>

      <td>
        <p>Required</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>followDeviceOrientation</code></p>
      </td>

      <td>
        <p>true or false</p>
      </td>

      <td>
        <ul><li>If the key is set to true, then the local InApp notification is displayed in the same orientation (Landscape or Portrait) as that set on your device.</li><li>If the key is false, then the local InApp notification only displays in portrait mode.</li></ul>
      </td>

      <td>
        <p>Required</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>positiveBtnText</code></p>
      </td>

      <td>
        <p>String</p>
      </td>

      <td>
        <ul><li>Sets the text of the button which performs the positive action (for example, Allow, OK,..etc). </li><li>This button continues the push primer flow.</li></ul>
      </td>

      <td>
        <p>Required</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>negativeBtnText</code></p>
      </td>

      <td>
        <p>String</p>
      </td>

      <td>
        <ul><li>Sets the button's text, which performs the negative action (Example: Deny, Cancel,..etc.). </li><li>This button aborts the push primer flow.</li></ul>
      </td>

      <td>
        <p>Required</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>fallbackToSettings</code></p>
      </td>

      <td>
        <p>true or false</p>
      </td>

      <td>
        <ul><li>If the value is set to true and permission is denied, then the user is redirected to the app’s notification settings.  </li><li>If the value is set to false, the callback is sent stating that the permission is denied.</li></ul>
      </td>

      <td>
        <p>Optional</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>backgroundColor</code></p>
      </td>

      <td>
        <p>Hex color as String</p>
      </td>

      <td>
        <p>Sets the background color of the local in-app notification.</p>
      </td>

      <td>
        <p>Optional</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>btnBorderColor</code></p>
      </td>

      <td>
        <p>Hex color as String</p>
      </td>

      <td>
        <p>Sets the border color of both positive and negative buttons.</p>
      </td>

      <td>
        <p>Optional</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>titleTextColor</code></p>
      </td>

      <td>
        <p>Hex color as String</p>
      </td>

      <td>
        <p>Sets the title text color of the local in-app notification.</p>
      </td>

      <td>
        <p>Optional</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>messageTextColor</code></p>
      </td>

      <td>
        <p>Hex color as String</p>
      </td>

      <td>
        <p>Sets the sub-title text color of the local in-app notification.</p>
      </td>

      <td>
        <p>Optional</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>btnTextColor</code></p>
      </td>

      <td>
        <p>Hex color as String</p>
      </td>

      <td>
        <p>Sets the color of text for both positive/negative buttons.</p>
      </td>

      <td>
        <p>Optional</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>btnBackgroundColor</code></p>
      </td>

      <td>
        <p>Hex color as String</p>
      </td>

      <td>
        <p>Sets the background color for both positive/negative buttons.</p>
      </td>

      <td>
        <p>Optional</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>btnBorderRadius</code></p>
      </td>

      <td>
        <p>String</p>
      </td>

      <td>
        <ul><li>Sets the radius for both positive/negative buttons. </li><li>If not set, the default radius is <strong>2</strong>.</li></ul>
      </td>

      <td>
        <p>Optional</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>fallbackToSettings</code></p>
      </td>

      <td>
        <p>true or false</p>
      </td>

      <td>
        <ul><li>If the value is set to true and permission is denied, then the user is redirected to the app’s notification settings. </li><li>If the value is set to false, the callback is sent stating that the permission is denied.</li></ul>
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        <p><code>imageUrl</code></p>
      </td>

      <td>
        <p>String (URL)</p>
      </td>

      <td>
        <p>Sets the image to be shown in the local in-app notification. Must be a valid URL pointing to an image resource.</p>
      </td>

      <td>
        <p>Optional</p>
      </td>
    </tr>

    <tr>
      <td>
        <p><code>altText</code></p>
      </td>

      <td>
        <p>String</p>
      </td>

      <td>
        <p>Alternate text shown if the image cannot be loaded. Improves accessibility and provides fallback context.</p>
      </td>

      <td>
        <p>Optional</p>
      </td>
    </tr>
  </tbody>
</Table>

### Invoke Notification Permission Dialog without Push Primer

You can call the push permission dialogue directly without a Push Primer using the `promptForPushPermission(boolean)` method. It takes a boolean value as a parameter. 

* If the value is set to true and permission is denied, then the user is redirected to the app’s notification settings. 
* If the value is set to false, the callback is sent stating that the permission is denied.

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

Based on whether the notification permission is granted or denied, the CleverTap React Native SDK provides a callback with the permission status. 

To receive the callback, you must register the `CleverTapPushPermissionResponseReceived` instance:

```javascript
CleverTap.addListener(CleverTap.CleverTapPushPermissionResponseReceived, (e)=>{/*consume the event*/})
```

To unregister the callback, use the following method:

```javascript
CleverTap.removeListener(CleverTap.CleverTapPushPermissionResponseReceived);
```
