---
title: November 2026
deprecated: false
hidden: false
metadata:
  robots: index
---
We are excited to share the November SDK changelog for our product!

<Callout icon="📘" theme="info">
  ### Note

  Click each SDK header to view the corresponding GitHub repository for release details.
</Callout>

# November 10

## [Android SDK 8.6.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-860-november-10-2026)

### Enhancements

* **Send Test Previews**: Hardens how Send Test previews (In-App, App Inbox, Display Unit) are opened. A preview is now shown only when the preview push actually reached the SDK; previews from any other launch Intent are ignored.

<Callout icon="📘" theme="info">
  ### Note

  Apps that render push themselves must continue calling `pushNotificationViewedEvent(extras)` after showing the notification, as already documented.
</Callout>

* **Large In-App Queue Handling**: Improves handling of a very large In-App queue. If the queue grows too large to save or read, the SDK now drops it instead of crashing with an `OutOfMemoryError`, and reports the drop to CleverTap on the next event.

### Bug Fixes

* Fixes an app crash (`TransactionTooLargeException`) when a full-screen custom HTML In-App with a very large HTML payload is shown. The In-App screen now receives only the In-App ID and reads the payload from memory, instead of passing the whole In-App through the Intent. Regular In-Apps were not affected.
* Fixes a regression where full-screen In-Apps killed by Android while on screen were not restored when the user returned to the app. The In-App is now shown again when the user returns, as long as its time-to-live has not passed. The restored impression is not counted again — no second `Notification Viewed` is raised.
* Fixes a crash at app start (`NoSuchMethodException: androidx.work.impl.WorkDatabase_Impl.<init>`) in minified release builds made with AGP 9. The SDK now ships the required Room constructor keep rule. AGP 8 apps are unaffected.
* Fixes SSL pinning (`CLEVERTAP_SSL_PINNING`) silently falling back to normal HTTPS when the bundled certificate could not be loaded. Pinning is now applied correctly and trusts the five Amazon Trust Services root certificates.

<Callout icon="⚠️" theme="warn">
  ### Note

  If you use SSL pinning with a custom proxy or handshake domain, ensure that domain's certificate chains to an Amazon Trust Services root — otherwise SDK requests will fail. If pinning cannot be set up on a device, the SDK logs an error and falls back to normal HTTPS.
</Callout>

* Fixes WorkManager compatibility. The SDK now works with WorkManager 2.9.1 through 2.10.2.

## [Web 3.2.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v3.2.0)

### What's New

- Added `CleverTap.removeStaleInstances()` to clear SDK storage of inactive accounts.
- Migrated existing accounts' un-prefixed storage keys to account-scoped keys.
- Fixed multi-instance bugs: campaign data, profile, ARP, and discarded events are now read from and saved to each instance's own storage.
- Resolved circular dependencies between SDK modules.

## [Roku SDK 1.0.0](https://github.com/CleverTap/clevertap-roku-sdk/blob/main/CHANGELOG.md#version-100-november-3-2026)

Initial release of the CleverTap Roku SDK — a SceneGraph node named `CleverTap`, called through `callfunc`.

### What's New

* **Core event recording**: `init`, `recordEvent`, `recordChargedEvent`, `onUserLogin`, `profilePush`, and `getCleverTapID`.
* **Profile commands**: Eight calls for modifying individual profile properties — including removing a property, editing multi-value lists, and incrementing or decrementing numbers.
* **Sessions**: An `App Launched` event is recorded at the start of each session. Sessions end after 20 minutes of inactivity.
* **Device fields**: Model, maker, OS version, screen size, and locale are sent with each batch. Country and time zone are set once per launch from the device; Roku time zone names are mapped to standard names, and unrecognized ones are reported and omitted.
* **Privacy**: `setOptOut` stops reporting for the current viewer, with an option to retain the SDK's own launch event. Opt-out status is stored on the device and sent to the server. Setting any marketing channel flag to `false` via `profilePush` opts the viewer out of all marketing channels.
* **Network info reporting**: Off by default. Enable with `enableDeviceNetworkInfoReporting` to include WiFi status.
* **Delivery**: Network calls run in the background. Events sent close together are batched; failed sends are retried with exponential backoff up to 60 seconds.
* **Configuration**: Account ID, token, region, and log level can be set in the channel manifest; values passed to `init()` take precedence.
* **Logging and validation**: Four log levels available. Invalid event names and profile keys are rejected; keys that differ from reserved names only in letter case are reported.

### Notes on Data Handling

* **Nested properties**: Maps and lists are sent as written. The server stores them only once nested properties are enabled on the account; until then the data is silently dropped.
* **Dates**: Dates in profile attributes and event properties are converted to a server-readable format. Dates the device cannot parse are reported and skipped.
* **Phone numbers**: Invalid numbers are reported but still sent.

# November 02

## [Web 3.1.2](https://github.com/CleverTap/clevertap-web-sdk/blob/HEAD/CHANGELOG.md#312-2nd-november-2026)

### Bug Fixes

- Fixed the global _per day_ Web Popup limit not being enforced.

## [Cordova 5.2.0](https://github.com/CleverTap/clevertap-cordova/releases/tag/5.2.0)

### What's New

#### Android

* Supports [CleverTap Android SDK v8.5.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-850-october-15-2026).

#### iOS

* Supports [CleverTap iOS SDK v7.9.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-790-october-15-2026).
* **Swift Package Manager support** for `cordova-ios 8.0.0` and above. Cordova picks the dependency manager automatically — apps on cordova-ios 8+ resolve the CleverTap iOS SDK through the plugin's `Package.swift` (minimum iOS 13), while apps below cordova-ios 8 continue using CocoaPods. No manual dependency configuration is needed. See [Integrate-iOS.md](https://github.com/CleverTap/clevertap-cordova/blob/5.2.0/docs/Integrate-iOS.md#ios-dependency-management) for upgrade steps and troubleshooting.

#### Android and iOS

- **Native Display Frequency Caps**: Adds session, daily, per-day/per-week, and lifetime cap support for Native Display (Display Units), tracked separately from In-App frequency caps. Campaign limits are evaluated on the device, and capped units are dropped before they reach your app.
- **Native Display Split-Click and View Attribution**: Adds per-element and per-slide (carousel) click and view attribution for Native Display units.
- `pushDisplayUnitViewedEventForID()` now counts toward Native Display frequency caps. Call it once per real impression — not on every render or scroll.
- App-Launch In-App can now wait up to 3 seconds for personalized (recommendations) content before it is shown. Native Display units from a recommendations response are merged into the existing set by unit ID instead of replacing it.

### API Changes

#### Android and iOS

* `pushDisplayUnitElementViewedEventForID(unitId, additionalProperties)` — records a view on a specific element or carousel slide within a Display Unit. The element key must be named exactly `wzrk_element_id`; any other key is recorded as a regular custom property and the event is not attributed to the element.
* `fetchNativeDisplayMeta()` — requests fresh Native Display frequency-cap rules mid-session.

### Bug Fixes

#### Android

* Fixes App-Launch In-Apps personalized with recommendations sometimes showing twice, and Native Display units being lost after a recommendations fetch.
* Fixes App Inbox video not playing when switching to a tab that contains a video message.
* Fixes "N times in D days/weeks" In-App and Native Display caps resetting one period later than expected.

#### iOS

* Fixes event name and property validation warnings accumulating and being replayed on later calls. Warnings are now collected per call and the actual validation message is logged.