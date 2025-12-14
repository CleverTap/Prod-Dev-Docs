---
title: September 2024
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
We are excited to share the September SDK changelog for our product!

> 📘 Note
> 
> Click each SDK header to view the corresponding GitHub repository for release details.

# September 23

## [Web 1.9.6](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.9.6)

- Adds Visual Editor support for Shopify websites.

# September 23

## [Web 1.9.5](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.9.5)

- Removes the use of migration service workers.

# September 18

## [Web 1.9.4](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.9.4)

- Adds anti-flicker solution for Visual Editor.

# September 16

## [Web 1.9.3](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.9.3)

- Fixes a bug where the multivalue feature was giving errors.

# September 4

## [Web 1.9.2](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.9.2)

- Sends SDK and Account ID via post-message

# September 2

## [Flutter 2.4.1](https://github.com/CleverTap/clevertap-flutter/releases/tag/2.4.1)

### Web

- Adds the `addKVDataChangeListener()` method to fetch the custom key-value pair data.
- Supports [CleverTap Web SDK v1.9.1](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.9.1).

### Android

- Fixes a date conversion issue for dates before January 1, 1970.
- Fixes a `ClassCastException` issue  in `ProductConfig` for `setMinimumFetchIntervalInSeconds` function.
- Adds support for `profileGetProperty`.

### iOS

- Fixes the issue where iOS platform channel messages were not sent on the platform thread.
- Adds support for `profileGetProperty` method. 

# September 2

## [Android 7.0.1](https://github.com/CleverTap/clevertap-android-sdk/tree/corev7.0.1)

### Bug Fixes

- Fixes a rare `ArrayIndexOutOfBoundsException` caused by an unsynchronized list during race conditions.
- Fixes an issue where the Android permission popup was not triggered by `PushPrimer` when using `InAppType.HALF_INTERSTITIAL` enum.