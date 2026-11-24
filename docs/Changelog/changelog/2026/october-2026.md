---
title: October 2026
deprecated: false
hidden: false
metadata:
  robots: index
---
# October 26

## [Web 3.1.1](https://github.com/CleverTap/clevertap-web-sdk/blob/master/CHANGELOG.md#311-26th-october-2026)

### What's New

- Raise Notification Viewed event per carousel slide in Web Native Display.

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

## [Unity SDK 5.6.0](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/CHANGELOG.md#version-560)

### Bug Fixes

* Fixes `GetInboxMessageUnreadCount()` returning the total inbox message count on Android instead of the unread count.

# October 15

## [iOS SDK 7.9.0](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.9.0)

### What's New

#### Live Activities (iOS 17.2+)

Adds support for CleverTap-driven Live Activities using Apple's Push-to-Start model, allowing your backend to start, update, and end an activity through push without the user opening the app.

* `registerPushToStart(_:name:)` — registers an `ActivityAttributes` type for Push-to-Start and forwards the push-to-start token to CleverTap. Call this early in `didFinishLaunchingWithOptions`.
* `recordLiveActivityImpression(_:)` — raises the `Notification Viewed` event for an activity. An overload accepting the `wzrk` dictionary directly is also available.
* `recordLiveActivityClicked(_:)` — raises the `Notification Clicked` event for an activity. An overload accepting the `wzrk` dictionary directly is also available.
* Lifecycle events — `Started`, `Updated`, `Ended`, and `Dismissed` — and activity token register and unregister states are handled automatically.

<Callout icon="📘" theme="info">
  ### Note

  These APIs are Swift-only, as ActivityKit is a Swift-only framework. `ActivityKit` is linked as a weak framework, so apps with older deployment targets are unaffected. Live Activities are excluded from tvOS.
</Callout>

For the full integration guide, see [docs/LiveActivity.md](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/docs/LiveActivity.md). For a working reference, see the `SwiftStarter` sample app.

#### Native Display Frequency Caps

Native Display campaigns now honour the frequency caps set in the dashboard. Campaign limits are evaluated on the device — only eligible campaigns are reported to CleverTap, so content is not requested for campaigns that have already hit their cap. Account-wide daily and per-session caps are applied at delivery, and capped units are dropped before they reach your app.

* `fetchNativeDisplayMeta()` — requests a fresh set of cap rules mid-session. Rules are also delivered on app launch. Refreshed rules replace the ones the SDK currently holds.

<Callout icon="📘" theme="info">
  ### Note

  Caps are counted from `recordDisplayUnitViewedEventForID:`. The SDK does not render display units, so a campaign is only held back once your app has reported those views. Call it once per real impression — not from a reused table or collection view cell.
</Callout>

#### Native Display Split of Clicks

Adds per-element click and view attribution for Native Display units, so individual carousel slides can be tracked separately from the unit as a whole.

* `recordDisplayUnitElementViewedEventForID:additionalProperties:` — raises `Notification Viewed` for a specific element of a unit.
* `metaData(forContentAt:)` on `CleverTapDisplayUnit` and the `metaData` property on `CleverTapDisplayUnitContent` — expose the `wzrk_*` attribution for a content item. Pass this as `additionalProperties` when recording the view or click event.

#### Recommendations Support in In-Apps and Native Display

Adds support for In-App campaigns personalized with recommendations, ensuring only one In-App is shown on app launch. Native Display units from a recommendations response are now merged into the existing set, matched by unit ID, instead of replacing the whole set.

### Bug Fixes

* Fixes a bug where event name and property validation warnings were accumulated on the validator and replayed on later calls. Warnings are now collected per call and the actual validation message is logged rather than a count.

## [Android SDK 8.5.0](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-850-october-15-2026)

### What's New

#### Live Updates

The SDK now powers CleverTap-driven Live Updates, a single, continuously-updating notification that surfaces real-time information such as a live score or a delivery's progress, refreshed remotely without the user reopening the app. For more information, refer to Create Message: Live Updates.

On Android, Live Updates are delivered as ordinary FCM pushes and rendered by the Push Templates `pt_progress` template (v2.7.0) or a custom `ICleverTapLiveUpdateNotificationFactory`. No token or activity-registration step is required. Each update is keyed by `wzrk_activityId`, from which the SDK derives a deterministic notification ID so successive `update` and `end` pushes refresh the same notification in place. A single `Live Activity` lifecycle event is raised with a `state` field — `Started`, `Updated`, `Ended`, or `Dismissed`.

For setup and payload reference, see [Live Updates](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTLIVEUPDATES.md).

#### Native Display Frequency Caps

Adds frequency-cap support for Native Display (Display Units). A campaign is held back only after your app reports impressions via `pushDisplayUnitViewedEventForID`. Supports server-side and legacy caps — session, daily, per-day / per-week, and lifetime — with per-channel state kept fully separate from In-App frequency caps.

For details, see [Native Display Frequency Caps](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/NativeDisplayFrequencyCaps.md).

#### Native Display Split-Click and View Attribution

Adds per-element and per-slide (carousel) click and view attribution for Native Display units, aligned with iOS. The click action and data are derived from the item URL, so attribution works even for items without explicit metadata.

### API Changes

* `CleverTapAPI.setLiveUpdateNotificationFactory(ICleverTapLiveUpdateNotificationFactory)` / `getLiveUpdateNotificationFactory()` — set or get the factory that builds Live Update notifications without a `pt_id`.
* `ICleverTapLiveUpdateNotificationFactory` — implement this interface to build a Live Update notification yourself.
* `CleverTapAPI.handleLiveUpdateDismissed(Context, Bundle)` — records the `Dismissed` lifecycle event when your factory sets its own `deleteIntent`.
* `pushDisplayUnitElementViewedEventForID(String, HashMap)` — records a view event for a specific slide or element of a Native Display unit.
* `pushDisplayUnitElementClickedEventForID(String, HashMap)` — records a click event for a specific slide or element of a Native Display unit.
* `CleverTapDisplayUnit.getMetaDataForContent(int)` — returns the metadata to pass to the per-element view and click APIs above.
* `CleverTapAPI.fetchNativeDisplayMeta()` — requests fresh Native Display frequency-cap rules mid-session.

### Enhancements

* `pushDisplayUnitViewedEventForID` now counts toward Native Display frequency caps. Call it once per real impression — when the unit becomes visible to the user — not on every bind or scroll.
* **App-Launch In-App** can now wait up to 3 seconds for personalized (recommendations) content before displaying, ensuring only one In-App is shown on launch.

### Bug Fixes

* Fixes App-Launch In-Apps personalized with recommendations sometimes showing twice, and Native Display units being lost after a recommendations fetch.
* Fixes App Inbox video not playing or resuming when switching to a tab that contains a video message. Playback now triggers when the pager becomes idle rather than mid-animation, with a retry after layout to cover tabs restored after rotation. Also fixes a blurry placeholder shown for video messages without a poster image.
* Fixes "N times in D days/weeks" In-App frequency caps resetting a day or week later than on iOS. The day/week window now starts at the first midnight after the first impression, matching iOS behaviour. The same fix applies to Native Display day and week caps.

# October 13

## [Web 3.1.0](https://github.com/CleverTap/clevertap-web-sdk/blob/master/CHANGELOG.md#310-13th-october-2026)

### What's New

- Capped WZRK_L backup queue at 2MB and dropped backups older than 3 days to prevent localStorage bloat.
- Added 'Recommendations' support for Web channels(Web Popup, Web Native Display,Web Inbox).

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

# October 01

## [CT Push Template 2.6.0 ](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTPUSHTEMPLATESCHANGELOG.md#version-260-october-1-2026)

### What's New

- Icons Template: Adds a new Icons template (pt_icons) that shows a row of up to 5 icons, with an optional title and message in the expanded view. The collapsed view shows only the icons.
- Image Border and Rounded Corners: Adds configurable corner radius, border width, and border color for static images in the Basic, Auto Carousel, Manual Carousel, Rating, Timer, and Image with CTA templates. Available on Android 12 (API 31) and above.

### Bug Fixes

- Fixes the Manual Carousel template not moving to the next image in a campaign that has no deep links configured.
