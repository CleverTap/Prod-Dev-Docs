---
title: Unity Troubleshooting and FAQs
excerpt: >-
  Learn how to resolve common issues and get answers to frequently asked
  questions when integrating the CleverTap Unity SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Troubleshooting

Refer to the [Unity SDK Troubleshooting Guide](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/docs/Troubleshooting.md) for help with integration issues, standard errors, and platform-specific limitations such as the following:

- Missing or outdated native dependencies.
- Incorrect method signatures or plugin errors.
- Platform-specific issues (for example, iOS Framework not linked, Android service not registered).

This guide addresses common integration problems, debug tips, and how to validate the SDK setup across platforms.

# FAQs

### Q: Why am I not receiving push notifications on Android?

Check for the following:

- The correct FCM token is being passed to CleverTap using `CleverTap.PushFcmRegistrationId(token, true);`
- Battery optimization settings do not restrict the device.
- `AndroidManifest.xml` includes required FCM services, receivers, and permissions.
- To enable push notifications, add the Firebase Unity SDK to your app. For setup instructions, refer to the [Firebase Unity Setup Docs](https://firebase.google.com/docs/unity/setup).

### Q: How do I verify if events are being sent to CleverTap?

- Enable `SetDebugLevel(3)` to view verbose logs during development.
- Look for event payloads in the device log.
- Open the Events dashboard in CleverTap to confirm event delivery.

### Q: Why is App Inbox not showing messages?

Check that the following conditions are met:

- `CleverTap.InitializeInbox()` is called before `ShowAppInbox()`.
- The App Inbox campaign is live and targeted to the correct test user.
- The Unity allows rendering of native views or webviews.
- There are no issues such as a test profile mismatch, campaign expiration or draft status, or incorrect platform targeting (iOS/Android).

### Q: How do I fix "method not found" or "undefined symbol" errors?

- Confirm that your Unity version is compatible with the SDK.
- Ensure all required native platform dependencies are correctly linked for Android and iOS.
- Clean the project and rebuild to clear any stale plugin cache.

# Need More Help?

If you’ve followed the integration steps and still face issues, explore:

- [Unity SDK GitHub Issues](https://github.com/CleverTap/clevertap-unity-sdk/issues)  
- [CleverTap Support Portal](https://support.clevertap.com/)