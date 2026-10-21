---
title: September 2026
deprecated: false
hidden: false
metadata:
  robots: index
---
We are excited to share the September SDK changelog for our product!

<Callout icon="📘" theme="info">
  #### Note

  Click each SDK header to view the corresponding GitHub repository for release details.
</Callout>

# September 04

## [iOS 7.8.2](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-782-september-4-2026)

### Bug Fixes

- Fixes a bug where in-app campaigns triggered by custom events could not match device and app properties.
- Fixes a bug where discarded event names were stored in preferences and sent back to the server on every request.
- Fixes a bug where stored in-app messages were cleared when the delivery mode was missing.

# September 03

## [React Native 4.3.1](https://github.com/CleverTap/clevertap-react-native/releases/tag/4.3.1)

### Bug Fixes

#### Android&#x20;

- Fixes a rare crash at app startup that could occur when a listener is still registering when the SDK is delivering buffered events.
- Fixes a crash and occasional loss of remote-config variables when they are defined while updated values arrive from the server, by holding the variable values in a thread-safe map.

# September 02

## [Flutter 4.2.0](https://github.com/CleverTap/clevertap-flutter/releases/tag/4.2.0)

### What's new

#### Android&#x20;

- Supports CleverTap Android SDK v8.4.1.
- Custom-HTML header and footer in-apps now render when the host Activity is not a `FragmentActivity`. Opt in by adding the `CLEVERTAP_INAPP_FRAGMENTLESS_BANNERS` flag to your app's `AndroidManifest.xml`. The default is off, so existing integrations are unaffected.
- Improves accessibility across in-app notifications and App Inbox with dynamic text scaling, screen-reader announcements and content descriptions, a larger 48dp dismiss-button tap area, and corrected carousel TalkBack navigation.

#### iOS&#x20;

- Supports CleverTap iOS SDK v7.8.1.
- Adds Swift Package Manager (SPM) support alongside the existing CocoaPods integration. See [Integrate-iOS-SPM.md](https://github.com/CleverTap/clevertap-flutter/blob/master/doc/Integrate-iOS-SPM.md) for setup and migration guidance.
- Adds UIScene lifecycle support via a new `sceneWillConnectWithOptions:` method on CleverTapPlugin.&#x20;
- Improves accessibility across in-app notifications and App Inbox with VoiceOver labels, a larger dismiss-button tap area, and better handling of image content descriptions.

#### Android and iOS&#x20;

- Adds Spin-the-Wheel and Scratch Card gamified templates to Advanced InApp Builder in-app notifications, campaign-driven, no app-side wiring needed.
- Adds per-element click attribution to the in-app Notification Clicked event across all in-app templates, plus configurable dismiss gestures (swipe-to-dismiss, and tap-outside on iOS) that are also tracked as Notification Clicked events.

### API changes

#### Android and iOS&#x20;

- New API: `dismissPipInApp()` - Dismisses the currently visible Picture-in-Picture (PIP) in-app notification. A no-op when no PIP in-app is visible, and other in-app types are never affected. Dismissing frees the in-app display slot, so pair it with `suspendInAppNotifications()` and `resumeInAppNotifications()` to keep a screen free of all in-apps.

### Bug Fixes

#### Android &#x20;

- Fixes a critical issue where App Inbox messages were not saved to the device on Android 6.0 through Android 10 (API 23–29), leaving the inbox permanently empty.&#x20;
- Fixes an `AbstractMethodError` crash when a Picture-in-Picture in-app notification played a video on Android 6.0 (API 23).&#x20;

#### iOS&#x20;

- Fixes `getAppLaunchNotification()` not being implemented on iOS, which caused a `PlatformException (MissingPluginException)` when called.
- Fixes an issue where in-apps that were missed while the app was in the background were not shown when the app returned to the foreground.
- Fixes an issue where the Notification Clicked event did not include deeplink properties for custom-HTML in-apps.
- Fixes an issue where the Advanced InApp Builder full-screen cover in-apps did not respect the device's safe area.
- Fixes an issue where client-side in-apps were not persisted when an empty campaign list was received.
- Fixes a potential crash while saving variable diffs, and a potential crash when `syncVariables()` and concurrent variable updates are run at the same time.

#### Android and iOS&#x20;

- Fixes an issue where the built-in App Inbox did not repaint the message list after a pull-to-refresh, newly fetched messages only appeared after closing and reopening the inbox.