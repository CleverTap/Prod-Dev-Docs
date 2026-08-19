---
title: August 2026
deprecated: false
hidden: false
metadata:
  robots: index
---
We are excited to share the August SDK changelog for our product!

<Callout icon="📘" theme="info">
  #### Note

  Click each SDK header to view the corresponding GitHub repository for release details.
</Callout>

# August 07

## [Android 8.4.1](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTCORECHANGELOG.md#version-841-august-7-2026)&#x20;

### What's New

- **Spin-the-Wheel and Scratch Card Templates**: Adds Spin-the-Wheel and Scratch Card gamified template support to Advanced InApp Builder in-app notifications.
- **Dismiss PIP In-App API**: New `dismissPipInApp()` on `CleverTapAPI` dismisses the currently visible Picture-in-Picture (PIP) In-App notification. Safe to call from any thread; a no-op when no PIP In-App is visible. Other in-app types are never affected.

* **HTML Header/Footer In-Apps Without FragmentActivity**: Custom HTML header and footer in-app notifications can now render when the host Activity is not  a `FragmentActivity`, for example, in Unity or Unreal Engine integrations. Opt in via the `CLEVERTAP_INAPP_FRAGMENTLESS_BANNERS` manifest flag. Default setting is off, so existing integrations are unaffected.

### Bug Fixes

- Fixes an issue where the built-in App Inbox did not repaint the message list after a pull-to-refresh, newly fetched messages only appeared after closing and reopening the inbox.
