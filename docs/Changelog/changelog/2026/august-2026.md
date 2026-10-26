---
title: August 2026
deprecated: false
hidden: false
link:
  new_tab: false
metadata:
  robots: index
---
We are excited to share the August SDK changelog for our product!

<Callout icon="📘" theme="info">
  #### Note

  Click each SDK header to view the corresponding GitHub repository for release details.
</Callout>

# August 27

## [CT Notification Content 1.5.1](https://github.com/CleverTap/CTNotificationContent/releases/tag/1.5.1)

### Bug Fixes

- Fixes per-page deep links in _pt_carousel&#x20;_&#x61;nd _pt_manual_carousel&#x20;_&#x74;emplates, so that _pt_dl2_ and _pt_dl3&#x20;_&#x61;re honored, and each page opens its own deep link instead of always falling back to pt_dl1.

## [Unity 5.5.5](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/CHANGELOG.md#version-555-aug-2026)

### What's New

- Updated to CleverTap Android SDK v8.4.1
- Updated to CleverTap iOS SDK v7.8.1
- `PauseSDK()`/`ResumeSDK()`- pause and resume all CleverTap event tracking and network operations. Events recorded while paused are buffered in the queue and automatically flushed on resume. Supported on Android, iOS, and Unity Native (Editor/PC/Mac/WebGL).
- `DismissPipInApp()` — dismisses the currently visible Picture-in-Picture (PiP) in-app notification, freeing the display slot for the next queued in-app. No-op on Native/Editor.
- Tap-to-open factory variable files in the example app opens .json variable files directly from the device on Android and iOS. Desktop/Editor fallback uses Application.OpenURL.

### Bug Fixes

- Fixes an issue where `SetOffline` had no effect on Android. The `setOffline` method was missing from the `(CleverTapUnityPlugin.java)` Java wrapper, causing the JNI call to silently fail. The method is now implemented and correctly delegates to the native CleverTap Android SDK.
- Fixes an issue where the second play session in Unity Editor causes a crash when Domain Reload is disabled. A hook in `CleverTap.cs` now resets all factories in dependency order at the start of every Play session, preventing stale object references.

# August 20

## [React Native 4.3.0](https://github.com/CleverTap/clevertap-react-native/releases/tag/4.3.0)

### What's New

- Supports CleverTap Android SDK [v8.4.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-841-august-2026).

- Supports CleverTap iOS SDK [v7.8.1](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-781-august-07-2026).

* _Spin-the-Wheel_ and _Scratch Card&#x20;_&#x67;amified in-app templates: available to campaigns built with the Advanced InApp Builder, no app-side wiring needed.

* Per-element click attribution for in-apps: the `Notification Clicked` event now records which element of the in-app was tapped, across all templates. Tap-outside (iOS) and swipe-to-dismiss (Android and iOS) gestures are now configurable per campaign and are tracked as `Notification Clicked` events too.

### Enhancements&#x20;

#### Android

- Accessibility improvements across in-apps and App Inbox, dynamic text scaling, screen-reader announcements, content descriptions for close buttons/images/media controls, a larger 48 DP (Density-independent Pixel) dismiss tap target, and corrected carousel navigation for TalkBack.

- Custom-HTML header and footer in-apps now render when the host Activity is not a `FragmentActivity` (for example, Unity or Unreal game engines). Off by default, so existing apps are unaffected. Opt in with the `CLEVERTAP_INAPP_FRAGMENTLESS_BANNERS` manifest flag.

#### iOS

- Accessibility improvements across in-apps and App Inbox: VoiceOver labels, a larger dismiss button tap area, and improved handling of image content descriptions.

### API changes

#### Android & iOS

- Adds `dismissPipInApp()`: dismisses the Picture-in-Picture in-app notification currently on screen. It does nothing when no PiP in-app is visible, and never affects other in-app types.

### Bug Fixes

#### Android&#x20;

- Fixes App Inbox messages not being saved on Android 6.0 through Android 10 (API 23–29), leaving the inbox permanently empty on those devices. Affects every app using App Inbox on React Native SDK v4.1.0 and v4.2.0; messages reappear on the next inbox fetch after updating, with no code change on your side.

- Fixes the built-in App Inbox not repainting the message list after a pull-to-refresh. Newly fetched messages previously appeared only after closing and reopening the inbox.&#x20;

- Fixes a crash when a Picture-in-Picture in-app played video on Android 6.0. Only affects apps whose `minSdkVersion` is below 24; the same fix also covers in-app and App Inbox video playback on that OS version.

- Fixes a rare NPE (Null Pointer Exception) that occurs when the push permission prompt is triggered from a background thread by ensuring it is launched on the main thread.

#### iOS

- Fixes the built-in App Inbox not repainting the message list after a pull-to-refresh. Newly fetched messages previously appeared only after closing and reopening the inbox.

- Fixes in-app events that were missed while the app was in the background and weren't shown when the app returned to the foreground.

- Fixes the `Notification Clicked` event not carrying deeplink properties for custom-HTML in-apps.

- Fixes client-side in-apps not being persisted when an empty campaign list was received.

- Fixes full-screen cover in-apps built with the Advanced InApp Builder not respecting the device safe area.

- Fixes two potential crashes, one while saving variable diffs, and one during simultaneous variable updates.

# August 12

## [Unreal 1.3.0](https://github.com/CleverTap/clevertap-unreal-sdk/blob/dev/CHANGELOG.md#130---2026-08-12)

### What's New

* Android CleverTap SDK upgraded from 8.0.0 to 8.4.1.
* iOS CleverTap SDK upgraded from 7.6.0 to 7.8.1.
* `Unmute(): `Resumes event recording and network traffic after the SDK has been muted. Android-only; no-op on iOS and Null/Editor. Added in Android SDK 8.1.0.
* `DismissPipInApp()` : Dismisses the currently visible Picture-in-Picture (PIP) in-app notification, freeing the display slot for the next queued in-app. Fully implemented on Android (SDK 8.4.1) and iOS (SDK 7.8.1).
* `RecordDisplayUnitClickedEventForID(UnitID)` : No-op placeholder for API parity with Android SDK 8.4.1 and iOS SDK 7.8.1. Display Units are not yet implemented in the Unreal plugin.
* `PauseSDK()` / `ResumeSDK()` : Gaming-context aliases for `SetOffline(true/false)`. Call `PauseSDK()` at the start of latency-sensitive gameplay sequences — boss fights, cutscenes — to prevent SDK network calls from causing frame stutters. Call `ResumeSDK()` when the sequence ends to flush queued events.&#x20;
* `GetVariants(): `Returns the active A/B test variants currently assigned to the user as `TArray<TMap<FString,FString>>`. Each entry is a property dictionary for one variant (for example, `"id"`, `"name"`). Returns an empty array if no variants are active.

## Bug Fixes

### Android

* **Removed local AAR workaround**: The custom-patched `clevertap-android-sdk-8.4.0.aar` has been deleted. The SDK is now consumed from Maven Central (`com.clevertap.android:clevertap-android-sdk:8.4.1`), which natively includes fragmentless banner support. The `AndroidLocalCleverTapSdkAarPath` config key remains supported for overriding to a local AAR if needed.

* **Fragment-based in-apps crashing on&#x20;**`GameActivity`: Unreal Engine's `GameActivity` does not extend `AppCompatActivity`, causing fragment-based in-app renderers (header, footer) to throw a `ClassCastException` at display time. Fixed by adding `CLEVERTAP_INAPP_FRAGMENTLESS_BANNERS=1` metadata  `AndroidManifest.xml` via UPL to enable the SDK's fragment-free rendering path.

  <Callout icon="📘" theme="info">
    ### Note

    Campaigns must use the HTML banner type (not Native) for this rendering path to apply.
  </Callout>

* **FileProvider registration**: Added `androidx.core.content.FileProvider` declaration to `AndroidManifest.xml` via UPL, along with a `clevertap_file_provider_paths.xml` resource. Required for sharing private-storage file paths (for example, PE file variables) on Android 7 and above.

### Sample App

* **Login not persisted across restarts on Android**: `SaveGameToSlot` silently failed on Android 11 and above due to scoped storage blocking writes outside the app's `files/` directory. Fixed by bypassing UE's `ISaveGameSystem` — save, load, and delete operations now use `FPlatformMisc::GamePersistentDownloadDir()` (maps to `getExternalFilesDir()`) with `FFileHelper` for direct file I/O.

* **"Delete Login Info" not logging out**: `DeleteSaveState()` deleted the save file, but `SyncSaveStateToViewModels` immediately re-created it when the ViewModel changes fired during UI teardown. Fixed by adding `EndDisplay_MainMenu()` to unbind the delegate before teardown, and calling `SetUIState(Login)` from `DeleteSaveState()` to navigate back to the login screen.

* **SaveState null crash on first launch**: `CppDemonstrationHUD` now correctly handles a null result from `LoadGameFromSlot` on first launch (no save file present) and falls back to the login state instead of dereferencing a null pointer.

* **PE tab, file variable preview**: Added `OpenPEFileVariable()` to `CleverTapSampleBlueprintFunctionLibrary`. Opens a PE file variable using the OS viewer, QuickLook on iOS, and FileProvider intent on Android.

# August 07

## [Android 8.4.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-841-august-7-2026)&#x20;

### What's New

- **Spin-the-Wheel and Scratch Card Templates**: Adds Spin-the-Wheel and Scratch Card gamified template support to Advanced InApp Builder in-app notifications.
- **Dismiss PIP In-App API**: New `dismissPipInApp()` on `CleverTapAPI` dismisses the currently visible Picture-in-Picture (PIP) In-App notification. Safe to call from any thread; a no-op when no PIP In-App is visible. Other in-app types are never affected.

* **HTML Header/Footer In-Apps Without FragmentActivity**: Custom HTML header and footer in-app notifications can now render when the host Activity is not a `FragmentActivity`. For example, in Unity or Unreal Engine integrations. Opt in via the `CLEVERTAP_INAPP_FRAGMENTLESS_BANNERS` manifest flag. The default setting is off, so existing integrations are unaffected.

### Bug Fixes

- Fixes an issue where the built-in App Inbox did not repaint the message list after a pull-to-refresh; newly fetched messages only appeared after closing and reopening the inbox.

## [iOS. 7.8.1](https://github.com/CleverTap/clevertap-ios-sdk/blob/master/CHANGELOG.md#version-781-august-7-2026)

### What's New

- **Spin-the-Wheel and Scratch Card Templates**: Adds Spin-the-Wheel and Scratch Card gamified template support to Advanced InApp Builder in-app notifications.
- **Dismiss PIP In-App API**: New `dismissPipInApp()` on `CleverTapAPI` dismisses the currently visible Picture-in-Picture (PIP) In-App notification. Dismissing frees the in-app display slot, so the next queued in-app (if any) may show immediately. To keep a screen free of all in-apps, pair this with `suspendInAppNotifications()` on-screen entry and `resumeInAppNotifications()` on exit.

### Bug Fixes

- Fixes an issue where the built-in App Inbox did not repaint the message list after a pull-to-refresh; newly fetched messages only appeared after closing and reopening the inbox.&#x20;

# August 07

## [Android 8.4.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-841-august-7-2026)&#x20;

### What's New

- **Spin-the-Wheel and Scratch Card Templates**: Adds Spin-the-Wheel and Scratch Card gamified template support to Advanced InApp Builder in-app notifications.
- **Dismiss PIP In-App API**: New `dismissPipInApp()` on `CleverTapAPI` dismisses the currently visible Picture-in-Picture (PIP) In-App notification. Safe to call from any thread; a no-op when no PIP In-App is visible. Other in-app types are never affected.

* **HTML Header/Footer In-Apps Without FragmentActivity**: Custom HTML header and footer in-app notifications can now render when the host Activity is not  a `FragmentActivity`, for example, in Unity or Unreal Engine integrations. Opt in via the `CLEVERTAP_INAPP_FRAGMENTLESS_BANNERS` manifest flag. Default setting is off, so existing integrations are unaffected.

### Bug Fixes

- Fixes an issue where the built-in App Inbox did not repaint the message list after a pull-to-refresh, newly fetched messages only appeared after closing and reopening the inbox.