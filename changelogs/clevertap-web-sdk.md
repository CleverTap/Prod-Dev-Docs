---
title: CleverTap Web SDK
author: Bajrang
hidden: true
published_at: '2023-01-20T08:56:29.264Z'
---
Welcome to the CleverTap Web SDK changelog. All changes to the CleverTap Web SDK are documented on this page or in their individual GitHub repositories. To install and understand the example usage, refer to the CleverTap Web SDK [NPM](https://www.npmjs.com/package/clevertap-web-sdk) here.

# Version 1.3.5

### Fixed

* Fixed cases where a request was processed twice from the cache.

### Added

* Multiple On User Login requests can be fired consecutively.
* Dynamic image dimensions for web carousel.
* Adds offline mode, where events can be queued and sent at a desired time later.

To know more, refer to the [documentation](https://developer.clevertap.com/docs/web-quickstart-guide#offline-mode) and the [GitHub Release Tag](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.3.5).

# Version 1.3.4

### Fixed

* Fixed cases where multiple requests can be fired without a `gcookie`.

To know more, refer to the [GitHub Release Tag](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.3.4).

# Version 1.3.3

### Updated

* Banner HTML for mobile view.

### Fixed

* Fixed a bug where multiple profiles were created on a slow network due to multiple requests being fired without a `gcookie`.

To know more, refer tot he [GitHub Release Tag](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v1.3.3).

## Previous Versions

<details>
  <summary>Click to expand</summary>

To view all changes to the CleverTap SDKs, refer to the [Previous Changelogs](https://developer.clevertap.com/docs/changelog).

</details>