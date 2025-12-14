---
title: Expo Plugin Quick Start Guide
excerpt: Learn how to integrate and start using the CleverTap Expo Plugin.
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

This document shows how to install the [CleverTap React Native SDK](doc:react-native) in your Expo app, configure the CleverTap Expo plugin, and track your first user event, all within the managed Expo workflow.

# Set Up CleverTap in Expo App

Set up CleverTap in your Expo project by installing dependencies and updating the `app.json`file. The CleverTap Expo Plugin automates native configurations and maintains your project in the managed workflow while enabling features such as push notifications and In-App messaging.

## Install and Configure CleverTap Expo

To start using CleverTap Expo, install the necessary dependencies and update your project configuration.

### Install CleverTap React Native SDK

Install the core SDK to enable CleverTap features in your React Native project.

```coffeescript Shell
npm install clevertap-react-native
```

### Install CleverTap Expo Plugin

Add the Expo config plugin to automate native configuration for CleverTap.

```coffeescript Shell
npx expo install @clevertap/clevertap-expo-plugin
```

> 🚧 Credentials and Access
>
> The **CleverTap Account ID** and **CleverTap Token** appear as **Project ID** and **Project Token** on the CleverTap dashboard.
>
> To view these, go to the CleverTap dashboard, then navigate to *Settings* > *Project*.
>
> Only users with Admin access can view the Passcode and Project Token. Member and Creator roles do not have permission to access these credentials.

#### Add Region Code

To learn more about adding a region code, refer to [Region Codes](doc:idc#enable-clevertap-region).

### Add Plugin Configuration in `app.json`

Modify your `app.json` file to include the CleverTap Expo Plugin configuration:

```json app.json
{
  "expo": {
    "plugins": [
      [
        "@clevertap/clevertap-expo-plugin",
        {
          "accountId": "YOUR_ACCOUNT_ID", // Required: CleverTap Account ID
          "accountToken": "YOUR_ACCOUNT_TOKEN", // Required: CleverTap Account Token
          "accountRegion": "YOUR_REGION_CODE", // Required: CleverTap account region (e.g., "in1", "us1")
          "disableAppLaunchedEvent": false, // Optional: Disable "App Launched" event
          "logLevel": 0, // Logging level (-1: off, 0: minimal, 2: debug, 3: verbose)
          "encryptionLevel": 1, // Optional: Encrypt PII data
          "encryptionInTransit": true, //Set to true to enable encryption over the network for PII data.
          "proxyDomain": "analytics.example.com", // Optional: Custom proxy domain
          "spikyProxyDomain": "spiky.example.com", // Optional: Proxy for push impressions
          "customIdentifiers": "Email,Phone", // Required if using custom IDs

          "android": {
            "features": {
              "enablePush": true, // Required: Enable push notifications
              "enablePushTemplates": true, // Optional: Enable rich push templates
              "enableInApp": true, // Required: Enable in-app messages
              "enableInbox": true, // Required: Enable App Inbox
              "enableMediaForInAppsInbox": true, // Optional: Enable media in inbox & in-apps
              "enableInstallReferrer": true, // Optional: Enable install tracking
              "enableHmsPush": false, // Required for Huawei push (HMS)
              "enableGoogleAdId": false, // Optional: Use Google Ad ID
              "enablePlayReview": true // Optional: Enable Google Play In-App Review feature
            },
            "customNotificationSound": ["notification.mp3"], // Optional: Custom sound files
            "backgroundSync": "1", // Optional: Enable background push sync
            "defaultNotificationChannelId": "default_channel", // Required: Default push channel
            "inAppExcludeActivities": "SplashActivity", // Optional: Exclude in-apps from screens
            "sslPinning": "1", // Optional: Enable SSL pinning
            "registerActivityLifecycleCallbacks": true // Recommended: Track session events
          },

          "ios": {
            "mode": "development", // Required: APNs mode ("development" or "production")
            "disableIDFV": false, // Optional: Disable IDFV collection
            "enableFileProtection": true, // Optional: Enable file security
            "enableURLDelegateChannels": [0, 1, 2], // Optional: Handle deep links
            "notifications": {
              "enableRichMedia": true, // Optional: Enable rich push media
              "enablePushImpression": true, // Optional: Track push impressions
              "enablePushTemplate": true, // Optional: Enable push templates
              "enablePushInForeground": true, // Optional: Show push when app is open
              "iosPushAppGroup": "group.com.clevertap.expoDemo", // Required for push tracking
              "notificationCategories": [
                {
                  "identifier": "CTNotification",
                  "actions": [{ "identifier": "action1", "title": "title1" }] // Optional: Custom actions
                }
              ]
            }
          }
        }
      ]
    ]
  }
}
```

### Configure `app.json`

The CleverTap Expo plugin supports a wide range of configuration options to customize your integration:

#### Core Configuration Options

| Property                  | Type      | Description                                                                                                                                                                                                                                                      | Default Behavior                                                                                 |
| :------------------------ | :-------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------- |
| Account ID                | `string`  | Required. Your CleverTap Account ID is available on the CleverTap dashboard under Settings.                                                                                                                                                                      | Required. Must be explicitly provided. No default.                                               |
| Account Token             | `string`  | Required. Your CleverTap Account Token is available on the CleverTap dashboard under Settings.                                                                                                                                                                   | Required. Must be explicitly provided. No default.                                               |
| Account Region            | `string`  | (Optional) Your CleverTap account region (for example, `in1`, `us1`, `sg1`, and so on).                                                                                                                                                                          | Auto-detected if not set.                                                                        |
| `disableAppLaunchedEvent` | `boolean` | (Optional) Set to true to disable automatic App Launched event tracking.                                                                                                                                                                                         | Defaults to `false`. Automatically tracks the App Launched event.                                |
| `logLevel`                | `number`  | (Optional) The logging level.                                                                                                                                                                                                                                    | Defaults to `-1` (no logging). Use `0` for minimal logs, `2` for debug, or `3` for verbose logs. |
| `encryptionLevel`         | `number`  | (Optional) Set to `1` to enable encryption of PII data.                                                                                                                                                                                                          | Defaults to `0` (encryption disabled).                                                           |
| `encryptionInTransit`     | `boolean` | (Optional) Set to true to enable encryption over the network for PII data.                                                                                                                                                                                       | Default is `false`.                                                                              |
| `proxyDomain`             | `string`  | (Optional) Your custom proxy domain(for example, `analytics.yourdomain.com`).                                                                                                                                                                                    | Defaults to `null`. Uses standard CleverTap endpoints.                                           |
| `spikyProxyDomain`        | `string`  | (Optional) Your custom spiky proxy domain for push impression events.                                                                                                                                                                                            | Defaults to `null`. Uses standard CleverTap endpoints.                                           |
| `customIdentifiers`       | `string`  | Comma-separated list of custom identifiers to enable custom identity management. Specify the identifiers (for example, `Email`, `Phone number`, `Identity`, or any combination of them), that CleverTap must use to identify users during `onUserLogin()` calls. | Defaults to `Identity`, `Email`.                                                                 |

## Platform-Specific Configurations

Configure platform-specific settings to enable features such as push notifications, In-App messaging, and App Inbox.

* [Android Specific Configuration](doc:expo-plugin-quick-start-guide#android-specific-configuration)
* [iOS Specific Configuration](doc:expo-plugin-quick-start-guide#ios-specific-configuration)

### Prebuild your application

Run the following command to generate the native files required for the CleverTap Expo plugin:

```coffeescript Shell
npx expo prebuild
```

Use `npx expo prebuild --clean` to regenerate native code by deleting existing native directories. This is useful for major config or native code changes to ensure a fresh build.

### Import and use CleverTap React Native SDK APIs in your application

After setting up the CleverTap Expo Plugin, import the CleverTap React Native SDK so you can use CleverTap's features in your mobile app, such as tracking events and user activity. Use the following APIs to record events and update user location:

```coffeescript javascript
const CleverTap = require('clevertap-react-native');

// Track a custom event
CleverTap.recordEvent('testEvent');

// Update user location
CleverTap.setLocation(34.15, -118.2);
```

## Test Your Integration

To verify your CleverTap integration is working correctly, follow these steps:

1. Install and run your app.
2. Call the `onUserLogin` method with a test user profile.
3. Record a few test events.
4. Check the CleverTap dashboard to ensure the user profile and events are being tracked.
5. Send a test push notification campaign from the CleverTap dashboard to verify delivery.

This helps validate that CleverTap is integrated correctly and is collecting user data as expected.

# Android-Specific Configuration

Use the following settings to configure CleverTap for Android, including support for Firebase Cloud Messaging (FCM), App Inbox, and notification customization.

#### Android Configuration Options

| Property                                     | Type                    | Description                                                                                                                                                                                                                                                                                                                                      | Default Behavior                                             |
| :------------------------------------------- | :---------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------- |
| `android.features.enablePush`                | `boolean`               | Enables Firebase Cloud Messaging (FCM) integration for push notifications. If disabled, developers must handle notifications manually.                                                                                                                                                                                                           | `false` (automatic FCM handling disabled)                    |
| `android.features.enablePushTemplates`       | `boolean`               | Enables advanced push templates such as auto-carousel push, rating notifications, and product display notifications.                                                                                                                                                                                                                             | `false` (push templates disabled)                            |
| `android.features.enableInApp`               | `boolean`               | Enables in-app messaging from CleverTap campaigns.                                                                                                                                                                                                                                                                                               | `false` (In-App messages disabled)                           |
| `android.features.enableInbox`               | `boolean`               | Enables App Inbox, allowing users to receive and access persistent content from the CleverTap dashboard.                                                                                                                                                                                                                                         | `false` (App Inbox disabled)                                 |
| `android.features.enableMediaForInAppsInbox` | `boolean`               | Enables media support for In-App messages and App Inbox, allowing video content.                                                                                                                                                                                                                                                                 | `false` (media support disabled)                             |
| `android.features.enableInstallReferrer`     | `boolean`               | Enables install attribution tracking to measure campaign effectiveness.                                                                                                                                                                                                                                                                          | `false` (install attribution disabled)                       |
| `android.features.enableHmsPush`             | `boolean`               | Enables Huawei Push Service (HMS) to ensure push notifications reach Huawei devices that don't support Google services.                                                                                                                                                                                                                          | `false` (HMS push disabled)                                  |
| `android.features.enableGoogleAdId`          | `boolean`               | Enables collection of the Google Advertising ID for user identification instead of CleverTap's generated device ID.                                                                                                                                                                                                                              | `false` (Google Ad ID collection disabled)                   |
| `android.features.enablePlayReview`          | `boolean`               | Enable Google Play In-App Review feature. When enabled, CleverTap supports the Google Play In-App Review API as a System In-App Function, allowing users to rate and review your app without leaving the app. This can be triggered as a button action within an in-app message or as a standalone campaign action from the CleverTap dashboard. | Default is `false` (Play In-App Review disabled)             |
| `android.customNotificationSound`            | `string` or `string\[]` | Specifies custom notification sound files stored in the assets folder. Can be used with `CleverTapAPI.createNotificationChannel()`.                                                                                                                                                                                                              | `null` (uses default system sound)                           |
| `android.backgroundSync`                     | `string`                | Enables background sync for [CleverTap’s Pull Notification](https://staging.docs.user.clevertap.net/docs/push-optimize-delivery#optimize-delivery-via-pull-notification) service to deliver notifications when FCM is restricted.                                                                                                                | `0` (background sync disabled)                               |
| `android.defaultNotificationChannelId`       | `string`                | Sets a default notification channel ID for push notifications. Used as a fallback when a push notification specifies a non-existent channel.                                                                                                                                                                                                     | `null` (falls back to the CleverTap "Miscellaneous" channel) |
| `android.inAppExcludeActivities`             | `string`                | A comma-separated list of activities where In-App messages must not be displayed.                                                                                                                                                                                                                                                                | `null` (In-Apps are shown in all activities)                 |
| `android.sslPinning`                         | `string`                | Enables [SSL pinning](doc:multi-instance-sdk-update#ssl-pinning) for enhanced security by setting it to `1`.                                                                                                                                                                                                                                     | `0` (SSL pinning disabled)                                   |
| `android.registerActivityLifecycleCallbacks` | `boolean`               | Automatically registers for Android activity lifecycle callbacks to track session activity, In-App messages, and user engagement metrics.                                                                                                                                                                                                        | `true` (lifecycle callbacks enabled)                         |

## Additional Android Configurations

Configure additional Android-specific settings in your `app.json` file to ensure CleverTap functions properly and supports push notifications, background services, and custom notification icons.

### Android Permissions

Add the following permissions to `app.json` to enable CleverTap's core functionality:

```json
{
  "expo": {
    "android": {
      "permissions": [
        "android.permission.ACCESS_NETWORK_STATE",
        "android.permission.INTERNET",
        "android.permission.POST_NOTIFICATIONS"
      ]
    }
  }
}
```

### Permission Descriptions

The following Android permissions are required for CleverTap to function properly in your Expo app:

* `ACCESS_NETWORK_STATE` – Allows CleverTap to check network connectivity before sending data.
* `INTERNET` – Enables CleverTap to communicate with servers for data exchange.
* `POST_NOTIFICATIONS` – Required for **Android 13+** devices to display push notifications. 

### Additional Android Configuration for Push Notifications (Optional)

After setting up the basic configuration in `app.json`, configure platform-specific settings to ensure push notifications work correctly. These steps cover Firebase Cloud Messaging (FCM), Huawei Mobile Services (HMS) Push, and custom notification icons.

### Firebase Cloud Messaging (FCM) Configuration

This setting is required when integrating Firebase features such as [Firebase Cloud Messaging](https://firebase.google.com/docs/android/setup) (FCM) for push notifications, Firebase Analytics, App verification, and so on. Add the `google-services.json` file in your project `assets` folder and update `app.json` as follows: 

```json
{
  "expo": {
    "android": {
      "googleServicesFile": "./assets/google-services.json"
    }
  }
}
```

### Huawei Messaging Services (HMS) Push Configuration

If [HMS Push](doc:clevertap-huawei-push-integration#enable-the-push-service) is enabled (`android.features.enableHmsPush = true`), place `agconnect-services.json` in the `assets` folder. No additional configuration is required in the`app.json`, but ensure the file exists for HMS Push to work.

### Custom Notification Icon

Customize your push notification icon by adding the following to `app.json`:

```json
{
  "expo": {
    "notification": {
      "icon": "./assets/notification-icon.png"
    }
  }
}
```

The CleverTap Expo Plugin automatically applies this [ icon](doc:android-push#set-the-small-notification-icon) to all CleverTap-created notifications. For best results, ensure the icon follows [Android notification icon guidelines](https://developer.android.com/design/ui/mobile/guides/home-screen/notifications).

For common issues and troubleshooting, refer to [FAQs for Android](doc:expo-plugin-quick-start-guide#faqs-recommendations-for-android).

# iOS-Specific Configuration

Use the following settings to configure CleverTap for iOS, enable Apple Push Notification service (APNs), push notifications, rich media, and App Groups for seamless user engagement.

## iOS Configuration Options

| Property                                   | Type                   | Description                                                                                                                                                | Default Behavior                                                                                                  |
| ------------------------------------------ | ---------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `iOS.mode`                                 | `string`               | Sets the APNs environment to `"development"` or `"production"` for push notifications. Determines whether the app uses sandbox or production APNs servers. | Automatically set based on the provisioning profile.                                                              |
| `iOS.disableIDFV`                          | `boolean`              | Disables the collection of **Identifier for Vendor (IDFV)** on iOS devices.                                                                                | `true` (uses IDFV as the unique identifier)                                                                       |
| `iOS.enableFileProtection`                 | `boolean`              | Enables **file protection** by setting `"NSDataWritingFileProtectionComplete"` when writing data to disk, restricting access until the device is unlocked. | `NSFileProtectionCompleteUntilFirstUserAuthentication` (files are inaccessible until the first unlock after boot) |
| `iOS.notifications.notificationCategories` | [NotificationCategory] | Enables custom notification categories for **interactive notification actions** like buttons or custom UI elements.                                        | `null`                                                                                                            |
| `iOS.notifications.enablePushInForeground` | `boolean`              | Enables receiving push notifications while the app is in the **foreground**.                                                                               | `false`                                                                                                           |
| `iOS.notifications.enableRichMedia`        | `boolean`              | Enables **rich push notifications** using the `CTNotificationService Extension`.                                                                           | `false` (fallback to standard push notifications)                                                                 |
| `iOS.notifications.enablePushTemplate`     | `boolean`              | Enables **push templates** by adding a Notification Content Extension target.                                                                              | `false` (fallback to standard push notifications)                                                                 |
| `iOS.notifications.enablePushImpression`   | `boolean`              | Enables logging **Push Impressions** on the CleverTap dashboard.                                                                                           | `false`                                                                                                           |
| `iOS.notifications.iosPushAppGroup`        | `string`               | Enables **Push Impressions logging** by specifying an **App Group Identifier**. The saved profile details are used to log Push Impressions correctly.      | `null` (should be set to log Push Impressions)                                                                    |

## Additional iOS Configuration for Notification Extensions

Set up `app.json` and provisioning profiles to enable iOS notification extensions and push impression tracking.

### Configure Provisioning Profiles for Notification Extensions

After configuring the basic iOS project setup, you must configure additional entitlements and App Group values to support **Notification Service Extension** or **Notification Content Extension**. These extensions enable your app to handle rich notifications and background processing.

Update your `app.json` file as shown below. Ensure you have created separate App IDs and provisioning profiles for each extension.

```json
{
  "expo": {
    "extra": {
      "eas": {
        "projectId": "YOUR_PROJECT_ID",
        "build": {
          "experimental": {
            "ios": {
              "appExtensions": [
                {
                  "targetName": "NotificationService",
                  "bundleIdentifier": "com.example.notificationService",
                  "entitlements": {
                    "com.apple.security.application-groups": [
                      "group.com.example.expoDemo"
                    ]
                  }
                },
                {
                  "targetName": "NotificationContent",
                  "bundleIdentifier": "com.example.notificationContent",
                  "entitlements": {
                    "com.apple.security.application-groups": [
                      "group.com.clevertap.expoDemo"
                    ]
                  }
                }
              ]
            }
          }
        }
      }
    }
  }
}
```

### Configure Push Impressions for iOS (Optional)

To track push notification impressions on iOS, create an App Group Identifier to enable data sharing between the main app and the Notification Service Extension (NSE). This configuration ensures the NSE can access relevant user data when receiving a notification.

#### Update `app.json`

```json
"ios": {
  "notifications": {
    "enablePushImpression": true,
    "iosPushAppGroup": "group.com.clevertap.expoDemo"
  }
}
```

Within your app, store profile data such as Name, Identity, Email, and Phone Number in the shared `UserDefaults` file. The NSE retrieves this stored data and sends it to CleverTap’s servers.

For detailed implementation, refer to the [iOS Push Impression](doc:react-native-push-notification#push-impression-in-ios).

#### Example Code for Storing Profile Data in `UserDefaults`

```javascript
userDefaults.set("CTProfileName", 'testUserA1', "group.com.clevertap.expoDemo", (err, data) => {
  if (!err) console.log('Saved CTProfileName: testUserA1');
});
// Store additional profile data here
```

# FAQs

Find answers to common questions about CleverTap's Android configuration, including file placements, push notifications, and feature dependencies.

## File Locations

### Where should I place the `agconnect-services.json` file for Huawei Push Services?

Place it at the root level of your `assets/` folder. This is required for HMS push integration to function properly.

### Can I store notification sound files in subdirectories?

No, all custom notification sound files must be stored at the root level of your `assets/` folder. Subdirectories are not supported.

### Where can I store `google-services.json` and notification icons?

These files can be placed in subdirectories within `assets/`, but they must be properly referenced in `app.json`.

## Google Services Configuration

#### Is `google-services.json` required for Firebase Cloud Messaging (FCM)?

Yes, if you enable FCM (`android.features.enablePush = true`), you must include `google-services.json` in your project.

### Where should I place `google-services.json`?

Place it in the assets/ folder and reference it in `app.json` as follows:

```json
{
  "expo": {
    "android": {
      "googleServicesFile": "./assets/google-services.json"
    }
  }
}
```

### How do I obtain `google-services.json`?

Download it from your Firebase console project settings.

### How can I verify that FCM is working correctly?

After integration, send a test push notification from the CleverTap dashboard to confirm the setup.

## Huawei Push Services

### Is `agconnect-services.json` required for HMS push notifications?

Yes, this file is required if you enable HMS push (`android.features.enableHmsPush = true`).

### Where should I store `agconnect-services.json`?

Place it at the root level of your `assets/` folder.

### Do I need to reference `agconnect-services.json` in `app.json`?

No, unlike `google-services.json`, you do not need to reference it in `app.json`, but the file must be present.

### How do I obtain `agconnect-services.json`?

Generate it from the Huawei AppGallery Connect console.

## Custom Notification Sounds

### What file formats are supported for notification sounds?

You can use `.mp3`, `.wav`, or `.ogg` files.

### Where should I store custom notification sounds?

Store them at the root level of your `assets/` folder. Subdirectories are not supported.

### How do I register a notification channel with a custom sound?

Use the following code in your app after running `expo prebuild`:

```javascript
CleverTap.createNotificationChannel(
  "channel_id",         // Unique ID for the notification channel
  "Channel Name",       // Visible name of the channel in system settings
  "Channel Description",// Description of the channel's purpose
  5,                    // Importance level (1-5, with 5 being highest)
  true,                 // Enable vibration for notifications in this channel
  "sound_file.mp3"      // Custom sound file for notifications (must be in assets folder)
);
```

## Custom Notification Icons

### What are the requirements for a custom notification icon?

The icon must follow [Android notification icon guidelines](https://developer.android.com/design/ui/mobile/guides/home-screen/notifications).

### How do I configure a custom notification icon in `app.json`?

Add the following to `app.json`:

```json
{
  "expo": {
    "notification": {
      "icon": "./assets/notification-icon.png"
    }
  }
}
```

This sets a custom icon that will appear in the Android notification drawer when your app receives a push notification.

## Feature Dependencies

### What should I enable to use push notification templates?

You must enable `android.features.enablePush` for push templates to work.

### How do I enable media support in In-App messages and App Inbox?

Enable `android.features.enableMediaForInAppsInbox` to display video content in In-App messages and App Inbox.
