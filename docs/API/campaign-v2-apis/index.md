---
title: 'Campaign V2 APIs '
deprecated: false
hidden: true
link:
  new_tab: false
metadata:
  robots: index
---
# Overview

Campaign API v2 is the new version of CleverTap's campaign creation API. It replaces the flat payload structure of v1 with a clean three-block request model, richer audience targeting, structured error responses with machine-readable codes and JSON paths, and support for new channels.

## What is New in v2

- Three-block request structure: `who` defines the audience, `what` defines the creative, and `when` defines the display window
- Richer audience targeting using CQL v2 rule trees with up to 5 levels of nesting
- Structured validation errors that collect all problems in one response, each with a stable error code and the exact JSON path of the offending field
- Support for Native Display and In-App as dedicated online channels
- Monthly recurring schedule support
- Liquid tags namespace enforcement

## v1 and v2 are not interchangeable

Campaign API v1 (`/1/targets/create.json`) remains fully supported. Existing integrations built on v1 continue to work without any changes. v1 and v2 use different request structures, response formats, and error models, and cannot be used interchangeably.

If you are building a new integration, use v2. If you are on v1 and want to understand what changes, refer to the Migrating from v1 section in each channel doc.

## Available Channels

The following table lists the channels available on Campaign API v2.

| Channel        | Endpoint                      | Description                                                                                                                                                                                   |
| :------------- | :---------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Native Display | `POST /2/targets/create.json` | Display creatives inside your app triggered in real time when a user fires an event. Refer to [Create Campaign v2 - Native Display](doc:create-campaign-v2-native-display).                   |
| In-App         | `POST /2/targets/create.json` | Interactive in-app creatives across three template families (Custom HTML, Basic, Image Canvas), triggered in real time. Refer to [Create Campaign v2 - In-App](doc:create-campaign-v2-inapp). |

<br />

<br />
