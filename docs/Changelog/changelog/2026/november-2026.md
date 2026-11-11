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

# November 02

## [Web 3.1.2](https://github.com/CleverTap/clevertap-web-sdk/blob/HEAD/CHANGELOG.md#312-2nd-november-2026)

### Bug Fixes

- Fixed the global _per day_ Web Popup limit not being enforced.

# November 03

## [CleverTap Native UI Kit — React Native 1.0.0](https://github.com/CleverTap/clevertap-native-ui-kit/releases/tag/react-native-v1.0.0)

Initial release of `@clevertap/native-display-sdk` (package version 1.0.0). Renders CleverTap Native Display campaigns as native React Native components from JSON — no WebViews. Works with both bare React Native and Expo (via `expo prebuild` or EAS Build; not Expo Go). Pure TypeScript — no native module of its own.

### What's New

* **Public API**: `NativeDisplayBridge`, `NativeDisplayView`, `NativeDisplaySlotManager`, `NativeDisplaySlot`, `wireCleverTap`, `NDLogLevel`.
* **Optional UI features** via `registerComponents`: linear gradient, SVG, Reanimated, WebView, video, masked view, blur, and fast image (or `expo-image` under Expo). The SDK never imports these itself — a feature whose library is not registered degrades gracefully.
* **Console log levels** through `NDLogger` / `NDLogLevel`.
* **Self-reported attribution**: `Notification Viewed` and `Notification Clicked` events are raised by the SDK — host apps do not need to double-report.
* **Element-level click attribution**: `action_type`, `action_url`, and `wzrk_*` attribution fields are exposed per element.

### Requirements

* React Native 0.73 or above; React 18 or above.
* `clevertap-react-native` 4.5 or above.

For setup, see [react-native/README.md](https://github.com/CleverTap/clevertap-native-ui-kit/blob/main/react-native/README.md).

# November 02

## [Cordova 5.2.0](https://github.com/CleverTap/clevertap-cordova/releases/tag/5.2.0)

### What's New

#### Android

* Supports [CleverTap Android SDK v8.5.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-850-october-15-2026).
* **Native Display Frequency Caps**: Adds session, daily, per-day/per-week, and lifetime cap support for Native Display (Display Units), tracked separately from In-App frequency caps. Campaign limits are evaluated on the device, and capped units are dropped before they reach your app.
* **Native Display Split-Click and View Attribution**: Adds per-element and per-slide (carousel) click and view attribution for Native Display units.
* `pushDisplayUnitViewedEventForID()` now counts toward Native Display frequency caps. Call it once per real impression — not on every render or scroll.
* App-Launch In-App can now wait up to 3 seconds for personalized (recommendations) content before it is shown.

#### iOS

* Supports [CleverTap iOS SDK v7.9.0](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-790-october-15-2026).
* **Swift Package Manager support** for `cordova-ios 8.0.0` and above. Cordova picks the dependency manager automatically — apps on cordova-ios 8+ resolve the CleverTap iOS SDK through the plugin's `Package.swift` (minimum iOS 13), while apps below cordova-ios 8 continue using CocoaPods. No manual dependency configuration is needed. See [Integrate-iOS.md](https://github.com/CleverTap/clevertap-cordova/blob/5.2.0/docs/Integrate-iOS.md#ios-dependency-management) for upgrade steps and troubleshooting.
* **Native Display Frequency Caps**: Campaign caps are evaluated on the device, and capped units are dropped before they reach your app.
* **Native Display Split-Click and View Attribution**: Adds per-element and per-slide (carousel) click and view attribution for Native Display units.
* `pushDisplayUnitViewedEventForID()` now counts toward Native Display frequency caps. Call it once per real impression — not on every render or scroll.
* Adds support for In-App campaigns personalized with recommendations, ensuring only one In-App is shown on app launch. Native Display units from a recommendations response are merged into the existing set by unit ID instead of replacing it.

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
