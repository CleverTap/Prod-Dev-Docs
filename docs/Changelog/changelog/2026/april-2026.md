---
title: April 2026
deprecated: false
hidden: false
metadata:
  robots: index
---
We are excited to share the April SDK changelog for our product!

<Callout icon="📘" theme="info">
  #### Note

  Click each SDK header to view the corresponding GitHub repository for release details.
</Callout>

# April 14

## [Unity 5.5.3](https://github.com/CleverTap/clevertap-unity-sdk/blob/master/CHANGELOG.md#version-553-14-april-2026)

### What's new

* Supports [CleverTap Android SDK v8.0.0](https://github.com/CleverTap/clevertap-android-sdk/releases/tag/corev8.0.0).
* Supports [CleverTap iOS SDK v7.5.1](https://github.com/CleverTap/clevertap-ios-sdk/releases/tag/7.5.1).

### Enhancements

* Automates Android build fixes via `AndroidProjectPostProcessor` for AGP 8.x compatibility.

### Bug Fixes

* Fixes `SetDebugLevel` to correctly route to `CleverTapLogger` in desktop/editor builds.
* Fixes `.meta` file platform assignments to ensure proper OS targeting for SQLite (Unity 2021).

# April 13

## [Push Template 2.4.0 ](https://github.com/CleverTap/clevertap-android-sdk/blob/master/docs/CTPUSHTEMPLATESCHANGELOG.md#version-240-april-13-2026)

### What’s New

* **Image with CTA**: Adds a new Vertical Image with CTA push notification template with support for multiple images, action buttons, gradient backgrounds, and rounded corners. Button border radius `pt_btn_border_radius` and width `pt_btn_border_width` are also configurable.
* **Timer Template Enhancements**: Adds configurable border radius `pt_chrono_border_radius`, border width `pt_chrono_border_width`, border color `pt_chrono_border_clr`, and gradient background support `pt_chrono_style, pt_chrono_grad_clr1, pt_chrono_grad_clr2, pt_chrono_grad_dir` for the chronometer display.
