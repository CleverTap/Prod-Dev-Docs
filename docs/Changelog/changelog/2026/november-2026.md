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

# November 19

## [Web 3.3.0](https://github.com/CleverTap/clevertap-web-sdk/blob/master/CHANGELOG.md#330-19th-november-2026)

### What's New

- Account-scoped storage keys and cookies are now named `KEY:{accountId}` (e.g. `WZRK_G:{accountId}`) instead of `{accountId}:KEY`, so allow-lists matching `WZRK_*` keep working. Existing data in either older format is migrated automatically.

<Callout icon="far fa-circle-info" theme="info">
  ### Note

  Version 3.2.0 introduced the `{accountId}:KEY` naming, for example `{accountId}:WZRK_G`. Firewall (WAF) or CDN rules that match cookie names against `WZRK_*` do not match that format and can block requests, for example with a 403 Forbidden response. Version 3.3.0 restores names that start with `WZRK_`, so those rules match again. If you are on version 3.2.x and cannot upgrade yet, allow both `WZRK_*` and `*:WZRK_*`.
</Callout>

# November 10

## [Web 3.2.0](https://github.com/CleverTap/clevertap-web-sdk/releases/tag/v3.2.0)

### What's New

- Added `CleverTap.removeStaleInstances()` to clear SDK storage of inactive accounts.
- Migrated existing accounts' un-prefixed storage keys to account-scoped keys.
- Fixed multi-instance bugs: campaign data, profile, ARP, and discarded events are now read from and saved to each instance's own storage.
- Resolved circular dependencies between SDK modules.

<Callout icon="far fa-triangle-exclamation" theme="warn">
  ### Warning

  This version introduced the `{accountId}:KEY` naming, for example `{accountId}:WZRK_G`. Firewall (WAF) or CDN rules that match cookie names against `WZRK_*` do not match that format and can block requests. Upgrade to version 3.3.0 to allow both `WZRK_*` and `*:WZRK_*`.
</Callout>

## [Roku SDK 1.0.0](https://github.com/CleverTap/clevertap-roku-sdk/blob/main/CHANGELOG.md#version-100-november-3-2026)

### What's New

- This is the first release of the CleverTap Roku SDK, providing basic integration for analytics based features and event tracking.

# November 02

## [Web 3.1.2](https://github.com/CleverTap/clevertap-web-sdk/blob/HEAD/CHANGELOG.md#312-2nd-november-2026)

### Bug Fixes

- Fixed the global _per day_ Web Popup limit not being enforced.

##
