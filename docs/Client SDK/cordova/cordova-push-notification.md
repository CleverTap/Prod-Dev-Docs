---
title: Cordova Push
excerpt: Learn how to handle push notifications in Cordova.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Push Notifications and Deep Links

This section describes how to set up push notifications and deep links for Cordova. 

## iOS

For more information on how to set up and register for push notifications, refer to [Set up push notifications for your app](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6).

1. To set up a push notification for your iOS app, refer to the [Apple Documentation](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6).
2. If you plan to use deep links, in the [Apple Documentation](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html#//apple_ref/doc/uid/TP40007072-CH6-SW1).
3. Call the following from your Javascript file.

```javascript
CleverTap.registerPush();
```

## Android

To use CleverTap's default Push Notifications service, add the following code to your `AndroidManifest.xml` file.

```xml
<application>
         ....
         ....
        <service android:name="com.clevertap.android.sdk.pushnotification.fcm.FcmMessageListenerService">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>

 </application>
```

> 📘 Notification Trampolines in Android 12 and Above.
>
> Android has changed the push notifications handling for version 12 and above to improve performance. An app can no longer start an activity within a service or broadcast receiver. For more information, refer to [Android 12 Push Changes](https://developer.clevertap.com/docs/android-12-updates#android-12-and-above).

### Create Notification Channel

To create a notification channel, add the following code snippet:

```javascript
CleverTap.createNotificationChannel("CtCS", "Clever Tap Cordova Testing", "CT Cordova Testing", 1, true);
```

The notification channel importance can have any value from 1 to 5. A higher value means a more interruptive notification. For more information, refer to [Notification Channel Importance Levels](https://developer.clevertap.com/docs/android-push#notification-channel-importance-levels). 

### Delete Notification Channel

To delete a notification channel, add the following code snippet:

```javascript
CleverTap.deleteNotificationChannel("CordovaTesting");
```

### Create a Group Notification Channel

To create a group notification channel, add the following code snippet:

```javascript
CleverTap.createNotificationChannelGroup("groupId", "groupName");
```

### Delete a Group Notification Channel

To delete a group notification channel, add the following code snippet:

```javascript
CleverTap.deleteNotificationChannelGroup("groupId");
```

### Use Activity as a Deep Link

To use any Activity as a deep link, add your custom URL scheme to the `AndroidManifest.xml` for that Activity.

```xml
<intent-filter android:label="@string/app_name">
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="clevertapstarter" />
     </intent-filter>
```

For `AndroidManifest.xml` example, refer to the [example AndroidManifest.xml](https://github.com/CleverTap/clevertap-cordova/blob/master/Samples/Cordova/ExampleProject/platforms/android/app/src/main/AndroidManifest.xml) file.

# Push Notifications Permission

Starting from Android 13 and iOS 10.0, applications must request notification permission from the user before sending Push Notifications. All newly installed applications must seek user permission before they can send notifications. 

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

* [Push Primer Using Half-Interstitial InApp Notification Template](doc:cordova-push-notification#push-primer-using-half-interstitial-inapp-template)
* [Push Primer Using In-App Alert Template](doc:cordova-push-notification#push-primer-using-alert-inapp-template)

### Push Primer Using Half-Interstitial InApp Template

Using this template, you can send a customized push primer notification with an image, text, and button. You can also modify the notification's text, button, and background color.\
Refer to the following images for Push Primer using Half-Interstitial InApp Template:

* **In Android** 

<Image title="Push Primer Using Half-Interstitial InApp Template in Android" alt={398} align="center" width="smart" border={true} src="https://files.readme.io/36ac314-Android_1.png">
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

  fallbackToSettings: true, // If true, opens OS notification settings when permission is denied
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

CleverTap.isPushPermissionGranted(val => log("isPushPermissionGranted value is " + val));

```

### Available Callbacks for Push Primer

Based on whether the notification permission is granted or denied, the CleverTap Cordova SDK provides a callback with the permission status. 

To receive the callback, you can register the `onCleverTapPushPermissionResponseReceived` callback:

```javascript
document.addEventListener('onCleverTapPushPermissionResponseReceived', e => {
        log("onCleverTapPushPermissionResponseReceived")
        log(e.accepted) // grant or deny flag
    })
```
