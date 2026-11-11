---
title: October 2026
deprecated: false
hidden: false
metadata:
  robots: index
---
# October 01

## [CT Push Template 2.6.0 ](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTPUSHTEMPLATESCHANGELOG.md#version-260-october-1-2026)

### What's New

- Icons Template: Adds a new Icons template (pt_icons) that shows a row of up to 5 icons, with an optional title and message in the expanded view. The collapsed view shows only the icons.
- Image Border and Rounded Corners: Adds configurable corner radius, border width, and border color for static images in the Basic, Auto Carousel, Manual Carousel, Rating, Timer, and Image with CTA templates. Available on Android 12 (API 31) and above.

### Bug Fixes

- Fixes the Manual Carousel template not moving to the next image in a campaign that has no deep links configured.
- Fixes the Image with CTA template layout so the image is no longer flush against the notification edge.

# October 09

## [React Native 4.4.0](https://github.com/CleverTap/clevertap-react-native/blob/4.4.0/CHANGELOG.md#version-440-october-9-2026)

### What's New

#### iOS

* Supports [CleverTap iOS SDK v7.8.2](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-782-september-4-2026).

#### Android and iOS

* **Multi-instance support**: One app can now connect to multiple CleverTap accounts simultaneously. Use `CleverTap.createInstance(config)` to create an additional account from JavaScript, and `CleverTap.getInstance(accountId)` to retrieve a handle to an existing one. A handle exposes the same methods as the top-level `CleverTap` object, every call and listener on it is scoped to that account only. The default account (configured via manifest or plist) continues to work exactly as before. See the [Multiple CleverTap Accounts guide](https://github.com/CleverTap/clevertap-react-native/blob/4.4.0/docs/multiinstance.md) for setup details.&#x20;

* **Launch configs**: Register additional accounts at process start so they are created alongside the default account and can receive push notifications and launch-time content from the first screen.
  * Android: pass configs to `CleverTapRnAPI.initReactNativeIntegration(context, launchConfigs)`, or override `CleverTapApplication.launchConfigs()`.
  * iOS: call `applicationDidLaunchWithOptions:launchConfigs:`.

* `High`**&#x20;encryption level**: Encrypts all stored data, not only PII fields. Set via the `encryptionLevel` key in `createInstance` (value `2`), or via the existing `CLEVERTAP_ENCRYPTION_LEVEL` / `CleverTapEncryptionLevel` keys for the default account.

### Enhancements

#### iOS

* Callback methods now report an error (`CleverTap is not initialized`) when the addressed account does not exist. Previously most callbacks were silently dropped. Android already behaved this way; its error text is `CleverTap not initialized`.

#### Android and iOS

* `CleverTap.removeListener(eventName)` now removes only the listeners registered via `CleverTap.addListener`. Previously it removed all listeners for that event name app-wide, including those added by other libraries on the raw event emitter.
* After `setInstanceWithAccountId('B')`, top-level listeners receive only account B's events. Previously they received events from both the old and new accounts merged.
* Calling `NativeModules.CleverTapReact.*` directly is no longer supported — every bridge method gained a trailing account argument, and the old-architecture Android bridge throws if the argument is missing. Always call through the `CleverTap` object.

# October 23

## [Flutter 4.3.0](https://github.com/CleverTap/clevertap-flutter/blob/master/CHANGELOG.md#version-430-23-october-2026)

### What's new

#### Android&#x20;

- Supports CleverTap Android SDK v8.5.0.
- Adds Native Display frequency caps. Call pushDisplayUnitViewedEvent once per real impression, as it now counts toward the caps.
- Adds per-element view attribution for Native Display units.
- The App-Launch In-App can now wait up to 3 seconds for personalized (recommendations) content before it is shown.
- Supports Live Updates - a single ongoing notification that is refreshed in place over the course of a campaign. These arrive as standard push notifications and are rendered by the native SDK, so there is no Flutter API to call. Setup and any custom rendering are done on the Android side of your app.

#### iOS

- Supports CleverTap iOS SDK v7.9.0.
- Adds Native Display frequency caps. Call pushDisplayUnitViewedEvent once per real impression, as it now counts toward the caps.
- Adds per-element view attribution for Native Display units.
- Adds support for In-App campaigns personalized with recommendations, so only one In-App is shown on app launch. Native Display units from a recommendations response are now merged by unit ID instead of replacing the existing set.

#### Web

- Supports CleverTap Web SDK v3.1.0.
