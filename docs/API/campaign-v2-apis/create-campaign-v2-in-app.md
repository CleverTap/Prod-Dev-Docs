---
title: Create Campaign v2 - In App
deprecated: false
hidden: true
link:
  new_tab: false
metadata:
  robots: index
---
# Overview

In-App delivers interactive creatives directly inside your app in real time. When a user fires the trigger event, the SDK receives the matching creative and renders it immediately, no notification, no delay.

Unlike push or email, In-App campaigns are displayed, not sent. The audience is evaluated live at trigger time, and none of the standard send-side delivery controls (DND, TTL, throttling) apply.

Every request is organized into three blocks.

| Block  | What it defines                                                                                                                         |
| :----- | :-------------------------------------------------------------------------------------------------------------------------------------- |
| `who`  | The trigger event, optional event property filters, device targeting, and an optional user profile filter are evaluated at trigger time |
| `what` | The creative, template family, and message strategy (single, split, A/B, or property-based)                                             |
| `when` | The display window and optional display policy (schedule and priority)                                                                  |

<Callout icon="📘" theme="info">
  ### **In-App is always a Live Campaign**

  `segmentType` is always `Live` for In-App. You may omit it or send `Live`. Any other value, including `PBS` or `External Trigger`, is rejected with `UNSUPPORTED_COMBINATION` on `$.segmentType`. `when.dateTime.type` must always be `userAction`, all other schedule types are rejected.
</Callout>

# Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/2/targets/create.json`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

# HTTP Method

POST

# Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details. Every request must include the following headers:

| Header                 | Required | Description                      |
| :--------------------- | :------- | :------------------------------- |
| X-CleverTap-Account-Id | Yes      | Your CleverTap Account ID.       |
| X-CleverTap-Passcode   | Yes      | Your CleverTap Account Passcode. |
| Content-Type           | Yes      | `application/json`               |

# Rate Limits

The following table lists the rate limits for this endpoint:

| Limit                                       | Scope                                            | On Breach                                               |
| :------------------------------------------ | :----------------------------------------------- | :------------------------------------------------------ |
| 50,000 campaign creations per day (default) | Per account, all API-created campaigns (v1 + v2) | HTTP 400, `"Daily API campaign creation limit reached"` |

# Supported Combinations

The following table shows the supported `what.messageType` values for In-App. Every In-App campaign uses `segmentType: Live` and `when.dateTime.type: userAction`. Any other `when.dateTime.type` is rejected with `UNSUPPORTED_COMBINATION` on `$.when.dateTime.type`.

| messageType             | Supported                                             |
| :---------------------- | :---------------------------------------------------- |
| `single`                | Yes                                                   |
| `split`                 | Yes                                                   |
| `ab` (manual)           | Yes                                                   |
| `ab` (automated)        | No, `UNSUPPORTED_COMBINATION` on `$.what.abTest.mode` |
| `messageOnUserProperty` | Yes                                                   |

<Callout icon="📘" theme="info">
  ### **Test with Draft Mode**

  Set `"draft": true` when testing a new campaign. The campaign is saved to your account and can be reviewed in the dashboard before you make it live. Set `"draft": false` or omit the field when you are ready to activate.
</Callout>

# Request Parameters

The following table lists the top-level request parameters:

| Field          | Type    | Required | Default | Description                                                                                                                                                                     |
| :------------- | :------ | :------- | :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name           | string  | Yes      | \-      | Campaign name. Must be non-blank.                                                                                                                                               |
| channel        | string  | Yes      | \-      | `InApp`. Case-insensitive.                                                                                                                                                      |
| segmentType    | string  | No       | `Live`  | Must be `Live` or omitted. Any other value is rejected.                                                                                                                         |
| who            | object  | Yes      | \-      | Trigger event, device targeting, optional profile filter, and control group. For more information, refer [who - Audience](doc:create-campaign-in-app#who---audience).           |
| what           | object  | Yes      | \-      | Creative and message-type strategy. For more information, refer [what - Content](doc:create-campaign-v2-in-app#what---content).                                                 |
| when           | object  | Yes      | \-      | Display window and display policy. For more information, refer [when - Display Window and Display Policy](doc:create-campaign-in-app#when---display-window-and-display-policy). |
| draft          | boolean | No       | `false` | `true` saves the campaign in `DRAFT` status instead of activating it. A draft is not shown to users until it is launched from the dashboard.                                    |
| conversionGoal | object  | No       | \-      | Conversion tracking goal. For more information, refer [conversionGoal](doc:create-campaign-in-app#conversiongoal).                                                              |

<Callout icon="📘" theme="info">
  ### **isClientSide is Set Automatically**

  The `isClientSide` flag is stamped automatically from the account configuration and cannot be set in the request. You do not need to include it.
</Callout>

# who - Audience

The In-App audience is event-triggered. A user qualifies the instant they raise the trigger event, optionally filtered by the event's properties, a geo-fence, and a user profile filter evaluated at that moment. Device targeting is a separate required block.

<Callout icon="📘" theme="info">
  ### **Shared with Native Display**

  The `who` contract is identical to the Native Display channel. All rules, restrictions, and error messages are the same.
</Callout>

The following table lists the `who` fields:

| Field        | Type   | Required | Description                                                                                                                                                                |
| :----------- | :----- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| trigger      | object | Yes      | The trigger event and its filters. For more information, refer [who.trigger](doc:create-campaign-in-app#whotrigger).                                                       |
| deliverTo    | object | Yes      | Operating systems and device types on which the campaign may be shown. For more information, refer [who.deliverTo](doc:create-campaign-v2-in-app#whodeliverto).            |
| query        | object | No       | Mini-who profile filter, a CQL v2 rule tree evaluated at trigger time. For more information, refer [who.query](doc:create-campaign-in-app#whoquery---user-profile-filter). |
| controlGroup | array  | No       | Control-group (holdout) settings. For more information, refer [who.controlGroup.](doc:/create-campaign-v2-in-app#whocontrolgroup)                                          |

The following keys are rejected for In-App with `INVALID_WHO` and the corresponding JSON path: `exclusion`, `inaction`, `trigger_property`. `prompt` returns `501`.

## who.trigger

The following table lists the `who.trigger` fields:

| Field            | Type    | Required     | Description                                                                                                                                                                             |
| :--------------- | :------ | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| event            | string  | Yes          | Name of the event that triggers the display. Must exist in the account. System events cannot be used.                                                                                   |
| first_time       | boolean | No (`false`) | `true` fires only on the user's first ever occurrence of the event.                                                                                                                     |
| property_filters | array   | No           | Event-property conditions on the trigger event. All entries are AND-ed. For more information, refer [Trigger Property Filters](doc:create-campaign-v2-in-app#trigger-property-filters). |
| geo_filters      | array   | No           | Geo-fence(s), the trigger event must fall inside. Non-empty when present. For more information, refer [Trigger Geo Filters](doc:create-campaign-v2-in-app#trigger-geo-filters).         |

Restricted (system) trigger events that cannot be used: `Notification Sent`, `Notification Failed`, `Stayed`, `Identity Set`, `Identity Reset`, `Identity Error`, `Reachable By`, `State Transitioned`, `Recommendation Served`. Matching is case- and space-insensitive.

## Trigger Property Filters

Each entry of `trigger.property_filters` is a condition on a property of the trigger event. All entries are AND-ed.

The following table lists the trigger property filter fields:

| Field     | Type   | Required                                                | Description                                                                                                              |
| :-------- | :----- | :------------------------------------------------------ | :----------------------------------------------------------------------------------------------------------------------- |
| property  | string | Yes                                                     | Event property name. Must be a known property of the trigger event.                                                      |
| operator  | string | No (`=`)                                                | For more information, refer to the table below.                                                                          |
| value     | any    | Required unless the operator is `exists` or `notExists` | Not needed for `exists`/`notExists`, required for every other operator.                                                  |
| data_type | string | No                                                      | `string` (default), `number`, or `date`. `date` switches the filter to calendar-date comparison in the account timezone. |

The following table lists the supported trigger property filter operators.

| Operator                  | Value                   | Notes                                                      |
| :------------------------ | :---------------------- | :--------------------------------------------------------- |
| `=`                       | scalar                  | Equals                                                     |
| `!=`                      | scalar                  | Not equals                                                 |
| `in`                      | array                   | Value is one of the list. Must be a JSON array.            |
| `notIn`                   | array                   | Value is none of the list.                                 |
| `>`, `<`                  | number                  | Numeric comparison.                                        |
| `between`                 | 2-element numeric array | Inclusive range                                            |
| `contains`, `notContains` | string                  | Substring match                                            |
| `exists`, `notExists`     | \-                      | Presence check. No `value` field.                          |
| `>=`, `<=`                | \-                      | Not supported for a trigger filter. Returns `INVALID_WHO`. |

With `"data_type": "date"`, values are ISO strings `YYYY-MM-DD`. Supported operators are `=`, `>`, `<`, `between`, `exists`, `notExists`. `!=`, `in`, `notIn`, `contains`, `notContains` are rejected for a date property.

## Trigger Geo Filters

`trigger.geo_filters` restricts the trigger to events raised inside one or more circles. Each entry is an object with the following fields:

| Field | Type   | Required | Description                                         |
| :---- | :----- | :------- | :-------------------------------------------------- |
| lat   | number | Yes      | Latitude, -90 to 90.                                |
| lng   | number | Yes      | Longitude, -180 to 180.                             |
| rad   | number | Yes      | Radius in kilometers, greater than 0.               |
| addr  | string | No       | Human-readable label kept for the dashboard editor. |

## who.deliverTo

Device and platform targeting. The campaign is only shown on the selected operating systems and device types. Both fields are required and must be non-empty. Labels are case- and space-insensitive.

| Field            | Type             | Required | Allowed Values           |
| :--------------- | :--------------- | :------- | :----------------------- |
| operatingSystems | array of strings | Yes      | `Android`, `iOS`         |
| deviceTypes      | array of strings | Yes      | `Mobile`, `Tablet`, `TV` |

## who.query - User Profile Filter

An optional CQL v2 rule tree is evaluated against the user's profile at trigger time. It is a group with exactly one of `any` (OR) or `all` (AND) holding an array of single-key rule objects. Groups nest up to 5 levels. Only the trigger-time subset of the CQL v2 vocabulary is available, this is a narrower grammar than a batch (PBS) audience.

The following table lists the supported user filter rules:

| Rule            | Key Fields                                                                                     | Supported Operators                                                            | Notes                                                                                                                                                                                                              |
| :-------------- | :--------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `event`         | `event_name`, `operator`, `value`, optional `when` and `occurrence`                            | `count =`, `count >`, `count >=`, `count between`                              | Lifetime count. `count between` takes `[min, max]`. `count >= 0`, `count <`, `count <=`, `count !=` are rejected. `property_filters`, `session_filters`, `geo_filters` on user profile filter events are rejected. |
| `event + when`  | `event_name`, `when: { "kind": "relative", "mode": "last", "days": N }`, optional `occurrence` | \-                                                                             | "Did the event in the last N units." `kind: relative`, `mode: last`.                                                                                                                                               |
| `event_did_not` | `event_name`                                                                                   | \-                                                                             | The user never did the event. No `operator`, `value`, or `when` allowed.                                                                                                                                           |
| `user_property` | `property`, `operator`, `value`, `data_type`                                                   | `=`, `!=`, `>`, `<`, `contains`, `between`                                     | `>=`, `<=`, `in`, `notIn`, `notContains`, `exists`, `notExists` not supported.                                                                                                                                     |
| `user_bucket`   | `operator`, `value`                                                                            | `in` only                                                                      | `value` is a non-empty array of integers 0–999.                                                                                                                                                                    |
| `app_fields`    | `attribute`, `operator`, `value`                                                               | `=`, `!=`, `in`, `notIn`, `>`, `>=`, `<`, `contains`, `notContains`, `between` | `attribute`: `OS Version`, `SDK Version`, `Make`, `Model`, `App Version`. `<=` rejected.                                                                                                                           |
| `device_fields` | `attribute`, `operator`, `value`                                                               | `=`, `!=`, `in`, `notIn`, `>`, `<`                                             | `attribute`: `width`, `height`, `ppi`. `>=`/`<=` rejected. Requires super device properties enabled.                                                                                                               |
| `geographic`    | `level`, country/state/city values                                                             | `in` only                                                                      | `notIn` rejected for live campaigns. `city` requires `level: city` plus `state`.                                                                                                                                   |
| `segments`      | `names[]` (1–20 saved segment names), `operator`                                               | `in`, `notIn`                                                                  | Membership in saved segments by name.                                                                                                                                                                              |

The following user filter rules are not available and return `INVALID_WHO`: `events_group`, `interest`, `multi_app`, `geo_radius`, `demographic`, `technographics`, `reachability`, `subscription_groups`. Analytics roots are also rejected.

## who.controlGroup

Optional array of control-group (holdout) entries. A `type` may appear at most once. `campaign` and `custom` are mutually exclusive. `system` is independent.

| type       | Fields                      | Description                                                                                   |
| :--------- | :-------------------------- | :-------------------------------------------------------------------------------------------- |
| `system`   | `apply` (boolean, required) | `true` holds out the account's system control group. Requires an active system control group. |
| `campaign` | `size` (integer, required)  | Campaign-specific random holdout, 1–99 percent.                                               |
| `custom`   | `name` (string, required)   | Name of an existing, active custom control group.                                             |

# what - Content

The following table lists the `what` fields:

| Field                 | Type   | Required                                            | Default  | Description                                                                                                                          |
| :-------------------- | :----- | :-------------------------------------------------- | :------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| messageType           | string | No                                                  | `single` | `single`, `split`, `ab`, or `messageOnUserProperty`.                                                                                 |
| message               | object | Required for `single`                               | \-       | The single template object. For more information, refer [Template Object](doc:create-campaign-v2-in-app#template-object).            |
| variants              | array  | Required for `split`, `ab`, `messageOnUserProperty` | \-       | An array of template objects, each with a variant envelope key (`name` or `value`).                                                  |
| distribution          | object | Required for `split`                                | \-       | `{ "<variantName>": <percent> }`.                                                                                                    |
| abTest                | object | Required for `ab`                                   | \-       | A/B configuration. For more information, refer [messageType: ab](doc:create-campaign-v2-in-app#messagetype-single).                  |
| userProperty          | object | Required for `messageOnUserProperty`                | \-       | `{ "property": "<profile property name>" }`.                                                                                         |
| personalizationConfig | object | No                                                  | \-       | Catalog and Recommendation 2.0 config. For more information, refer [Personalization](doc:create-campaign-v2-in-app#personalization). |

## Template Object

Each `what.message` or entry in `what.variants[]` is an In-App template: a `type` plus its creative. The template `type` selects one of three families, each with its own field set.

The following table summarizes the three template families:

| Family       | Type Values                                                                                                                                                          | Shape                                                                                      |
| :----------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------- |
| Custom HTML  | `custom-html`                                                                                                                                                        | Your own HTML (`d.html`) rendered into one of seven fixed layouts chosen by `layout_type`. |
| Basic        | `interstitial`, `cover`, `half-interstitial`, `header-template`, `footer-template`, `alert-template`, `interstitial-image`, `cover-image`, `half-interstitial-image` | Title, message, media, background, and action buttons over a shared shape.                 |
| Image canvas | `image-interstitial`                                                                                                                                                 | One creative image plus percentage-positioned invisible tap targets.                       |

<Callout icon="⚠️" theme="warning">
  ### **type vs layout_type**

  `interstitial`, `cover`, and `half-interstitial` exist both as Basic template types and as `custom-html` layouts. They are told apart by `type` alone: `{ "type": "interstitial" }` is the Basic template. `{ "type": "custom-html", "layout_type": "interstitial" }` is the Custom HTML layout. `layout_type` is meaningful only when `type` is `custom-html`. The Custom HTML layout names `header` and `footer` are not template types, the Basic counterparts are `header-template` and `footer-template`.
</Callout>

Unknown keys are rejected on every family. Any other key on the template object or inside its nested objects is rejected, with `INVALID_VALUE` naming the key and a list of what is supported.

Variants in `split` and `ab` may mix families. For example, a `custom-html` variant and a Basic `cover` variant in the same A/B test is valid.

## Family 1 - custom-HTML

Your own HTML creative rendered into a fixed layout. You send the `layout_type` and the HTML (`d.html`), the display envelope comes from the layout.

The following table lists the `custom-html` fields:

| Field       | Type    | Required | Default     | Description                                                                                                                                                                                                                       |
| :---------- | :------ | :------- | :---------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type        | string  | Yes      | \-          | `custom-html`. For more information, refer [Template Types](doc:create-campaign-v2-in-app#personalization) table.                                                                                                                 |
| layout_type | string  | Yes      | \-          | The layout type.                                                                                                                                                                                                                  |
| d           | object  | Yes      | \-          | The creative container.                                                                                                                                                                                                           |
| d.html      | string  | Yes      | \-          | The complete inline HTML. Must be a non-blank string. Supports Liquid.                                                                                                                                                            |
| w           | object  | No       | From layout | Window overrides. Only `dk`, `sc`, `yp` are honored. `pos` and `xp` are accepted but always replaced by the layout's own values.                                                                                                  |
| w\.dk       | boolean | No       | `false`     | Dim the app behind the creative. Forced off for `header` and `footer`.                                                                                                                                                            |
| w\.sc       | boolean | No       | `false`     | Show a dismiss button on the creative. Forced off for `header`, `footer`, `spin-the-wheel`, and `scratch-the-card`.                                                                                                               |
| w\.yp       | number  | No       | From layout | Height as a percentage of the screen. Honored only for `header` and `footer`. Accepts a whole number 1–30. Values outside that range are silently clamped to 30. A non-numeric value is rejected. Ignored for every other layout. |
| isJsEnabled | boolean | No       | Per layout  | Allow JavaScript in the HTML. Default off, except on for `spin-the-wheel` and `scratch-the-card`. An explicit Boolean wins.                                                                                                       |
| inlineHtml  | boolean | No       | `true`      | If sent, must be `true`. `false` (hosted-URL mode) is rejected.                                                                                                                                                                   |

`d.url` (hosted template URL) is not supported. Supply the creative inline via `d.html`.

The following table lists the `custom-html` layout types, their placement, and key properties:

| layout_type          | Placement     | Width | Height                      | JavaScript default | Dismiss button (w\.sc)    | Dim background (w\.dk) |
| :------------------- | :------------ | :---- | :-------------------------- | :----------------- | :------------------------ | :--------------------- |
| `interstitial`       | Centre        | 90%   | 85%                         | Off                | Available                 | Available              |
| `cover`              | Centre        | 100%  | 100%                        | Off                | Available                 | Available              |
| `half-interstitial`  | Centre        | 90%   | 50%                         | Off                | Available                 | Available              |
| `header`             | Docked top    | 100%  | `w.yp` (1–30%, default 30%) | Off                | Forced off                | Forced off             |
| `footer`             | Docked bottom | 100%  | `w.yp` (1–30%, default 30%) | Off                | Forced off                | Forced off             |
| `spin-the-wheel` †   | Centre        | 90%   | 85%                         | On                 | Forced off (gamification) | Available              |
| `scratch-the-card` † | Centre        | 90%   | 85%                         | On                 | Forced off (gamification) | Available              |

Gamification layouts are feature-gated. Accepted only when the gamification templates feature is enabled for the account. Otherwise rejected with `FEATURE_NOT_ENABLED` on `$.what.message.layout_type`.

## Family 2 - Basic Templates

Nine editor templates over one shared shape: title, message, media, background color, and action buttons. You send only the content you choose. Every other setting is filled in with the dashboard editor's defaults, so the campaign opens in the editor exactly as if it had been built there.

The following table lists the Basic template fields:

| Field          | Type                                              | Required                | Default                     | Description                                                                                                                                             |
| :------------- | :------------------------------------------------ | :---------------------- | :-------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type           | string                                            | Yes                     | \-                          | One of the nine Basic template types.                                                                                                                   |
| title          | string or `{ "text", "color" }`                   | No                      | Empty text, color `#434761` | Title text. Supports Liquid. Not accepted on `*-image` templates. `color` not accepted on `alert-template`. Length-capped per template.                 |
| message        | string or `{ "text", "color" }`                   | No                      | Empty text, color `#434761` | Body text. Same rules as `title`.                                                                                                                       |
| bg             | string                                            | No                      | `#ffffff`                   | Background color. Not accepted on `alert-template`.                                                                                                     |
| media          | string or `{ "url", "content_type", "alt_text" }` | No                      | None                        | Portrait media. `url` is required in the object form. Not accepted on `alert-template`.                                                                 |
| mediaLandscape | string or `{ "url", "content_type", "alt_text" }` | No                      | None                        | Landscape media. Requires `hasLandscape` to be on. Not accepted on `alert-template`.                                                                    |
| buttons        | array                                             | Depends on the template | `[]`                        | Action buttons. Shape depends on the template's button style. For more information, refer [Button Styles](doc:create-campaign-v2-in-app#button-styles). |
| close          | boolean                                           | No                      | `true`                      | Include a close button. Forced on when the creative has no buttons. Not accepted on `header-template`, `footer-template`, `alert-template`.             |
| hasPortrait    | boolean                                           | No                      | `true`                      | Portrait orientation enabled. At least one of `hasPortrait`/`hasLandscape` must stay on.                                                                |
| hasLandscape   | boolean                                           | No                      | Per template                | Landscape orientation enabled. Default on for `interstitial`, `header-template`, `footer-template`, `alert-template`. Off for the others.               |
| tablet         | boolean                                           | No                      | `false`                     | Tablet-specific layout flag.                                                                                                                            |

### **media and mediaLandscape Object Fields**

The following table lists the media object fields:

| Field        | Type   | Required             | Description                                                                                                    |
| :----------- | :----- | :------------------- | :------------------------------------------------------------------------------------------------------------- |
| url          | string | Yes (in object form) | Direct URL to the image or video file. Must be non-blank.                                                      |
| content_type | string | No                   | MIME type. When absent, derived from the URL's file extension. Unrecognized extensions default to `image/jpg`. |
| alt_text     | string | No                   | Accessibility text.                                                                                            |

Accepted types: `image/jpg`, `image/jpeg`, `image/png`, `image/gif` on every media-capable template. `video/mp4` only on `interstitial`, `interstitial-image`, `cover-image`, `half-interstitial-image`. Audio is never accepted.

### **Per-Template Rules**

The following table lists the per-template constraints on title/message character limits, button min/max, video support, and `hasLandscape` default.

| type                      | title/message max chars | bg  | media/video     | buttons min–max | Button style | close | hasLandscape default |
| :------------------------ | :---------------------- | :-- | :-------------- | :-------------- | :----------- | :---- | :------------------- |
| `interstitial`            | 20/75                   | Yes | Yes/video (mp4) | 0–2             | Full         | Yes   | On                   |
| `cover`                   | 30/128                  | Yes | Yes/no video    | 0–2             | Full         | Yes   | Off                  |
| `half-interstitial`       | 25/100                  | Yes | Yes/no video    | 0–2             | Full         | Yes   | Off                  |
| `header-template`         | 22/75                   | Yes | Yes/no video    | 1–2 (required)  | No-border    | No    | On                   |
| `footer-template`         | 22/75                   | Yes | Yes/no video    | 1–2 (required)  | No-border    | No    | On                   |
| `alert-template`          | 25/100                  | No  | No              | 1–2             | Text-only    | No    | On                   |
| `interstitial-image`      | Not accepted            | Yes | Yes/video (mp4) | Exactly 1       | Action-only  | Yes   | Off                  |
| `cover-image`             | Not accepted            | Yes | Yes/no video    | Exactly 1       | Action-only  | Yes   | Off                  |
| `half-interstitial-image` | Not accepted            | Yes | Yes/no video    | Exactly 1       | Action-only  | Yes   | Off                  |

### **Button Styles**

The following table lists the button styles and the fields each accepts:

| Style       | Templates                                                      | Accepted keys                                        | Defaults when omitted                                                          |
| :---------- | :------------------------------------------------------------- | :--------------------------------------------------- | :----------------------------------------------------------------------------- |
| Full        | `interstitial`, `cover`, `half-interstitial`                   | `text`, `color`, `bg`, `border`, `radius`, `actions` | text `"Button"`, color `#000000`, bg `#1EB858`, border `#1EB858`, radius `"4"` |
| No-border   | `header-template`, `footer-template`                           | `text`, `color`, `bg`, `actions`                     | text `"Button"`, color `#000000`, bg `#1EB858`                                 |
| Text-only   | `alert-template`                                               | `text`, `actions`                                    | text `"Button"`                                                                |
| Action-only | `interstitial-image`, `cover-image`, `half-interstitial-image` | `actions`                                            | The image is the button.                                                       |

The following table lists the button fields:

| Field   | Type                        | Description                                                                                                                         |
| :------ | :-------------------------- | :---------------------------------------------------------------------------------------------------------------------------------- |
| text    | string or `{ "text": "…" }` | Button label. Supports Liquid.                                                                                                      |
| color   | string                      | Label color.                                                                                                                        |
| bg      | string                      | Button background color.                                                                                                            |
| border  | string                      | Border color.                                                                                                                       |
| radius  | string                      | Corner radius, an even number from 0 to 24, sent as a string (for example `"4"`, `"12"`). A numeric JSON value is rejected.         |
| actions | object                      | On-tap action. Default `{ "type": "close" }`. For more information, [Button Actions](doc:create-campaign-v2-in-app#button-actions). |

### **Button Actions**

The following table lists the button action fields:

| Field   | Type                        | Required                                            | Description                                                                                                                                                             |
| :------ | :-------------------------- | :-------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type    | string                      | No (`close`)                                        | `close`, dismiss the creative. `url`, open a deep link or URL. `kv`, return custom key-value pairs to the app. `kv` is not available on `alert-template`.               |
| android | string or `{ "text": "…" }` | Required for `url`: at least one of `android`/`ios` | Android deep link or URL. Supports Liquid.                                                                                                                              |
| ios     | string or `{ "text": "…" }` | Required for `url`: at least one of `android`/`ios` | iOS deep link or URL. Supports Liquid.                                                                                                                                  |
| kv      | array                       | Required for `kv`                                   | `[{ "key": "<non-blank string>", "value": "<string>" }]`. Each `key` must be non-blank. `value` may also be `{ "text": "…" }`. When omitted, stored as an empty string. |

`custom-code` (app functions) is not supported.

## Family 3 - image-interstitial (Image Canvas)

One creative image plus up to 10 invisible, percentage-positioned tap targets drawn over it. This is a different shape from the Basic family.

The following table lists the `image-interstitial` fields:

| Field                       | Type                        | Required | Default               | Description                                                                                                                       |
| :-------------------------- | :-------------------------- | :------- | :-------------------- | :-------------------------------------------------------------------------------------------------------------------------------- |
| type                        | string                      | Yes      | \-                    | Template type. Must be `image-interstitial` for the image canvas family.                                                          |
| content                     | object                      | Yes      | \-                    | The creative image and its settings.                                                                                              |
| content.backgroundImage     | string or `{ "text": "…" }` | Yes      | \-                    | URL of the creative image, it is the whole creative. Supports Liquid.                                                             |
| content.altText             | string                      | No       | `""`                  | Accessibility text, max 150 characters.                                                                                           |
| content.margin              | integer                     | No       | `10`                  | Inset of the image from the screen edge, as a percentage, 0–40.                                                                   |
| content.actions             | object                      | No       | `{ "type": "close" }` | Tap action on the image itself (outside any button). Same shape as Button Actions, additionally accepts `"type": ""` (no action). |
| overlay                     | object                      | No       | Defaults below        | The backdrop behind the image.                                                                                                    |
| overlay.backgroundColor     | string                      | No       | `rgba(0, 0, 0, 0)`    | Backdrop color (CSS color).                                                                                                       |
| overlay.backgroundImage     | string or `{ "text": "…" }` | No       | None                  | Backdrop image URL.                                                                                                               |
| overlay.backgroundRepeat    | string                      | No       | `no-repeat`           | `no-repeat`, `repeat`, `repeat-x`, `repeat-y`.                                                                                    |
| overlay.backgroundSize      | string                      | No       | `cover`               | `auto`, `contain`, `cover`.                                                                                                       |
| overlay.dismissOnTapOrClick | boolean                     | No       | `true`                | Tapping the backdrop dismisses the creative.                                                                                      |
| device                      | object                      | No       | \-                    | Design-time preview only. Does not affect rendering on the device.                                                                |
| device.orientation          | string                      | No       | `portrait`            | `portrait` or `landscape`.                                                                                                        |
| buttons                     | array                       | No       | `[]`                  | Tap targets, at most 10.                                                                                                          |
| buttons\[].name             | string                      | No       | `Button <index>`      | Reporting name for the tap target. Must be unique across the creative's buttons.                                                  |
| buttons\[].position         | `{ "x", "y" }`              | No       | Centred               | Top-left corner as whole percentages of the image box, each 0–100.                                                                |
| buttons\[].size             | `{ "width", "height" }`     | No       | 20 × 14               | Size as whole percentages of the image box, each 0–100.                                                                           |
| buttons\[].actions          | object                      | No       | `{ "type": "close" }` | Tap action. Same shape as Button Actions, additionally accepts `"type": ""` (no action).                                          |

**Geometry rule**: `position.x + size.width <= 100` and `position.y + size.height <= 100`. A tap target may not extend past the image. Values are percentages, never pixels.

## messageType: single

Content comes from `what.message` (one template object).

## messageType: split

Qualifying users are distributed across variants by fixed weights. No winner is selected.

| Constraint        | Value                                                                                                             |
| :---------------- | :---------------------------------------------------------------------------------------------------------------- |
| `variants` count  | 2–3                                                                                                               |
| `variants[].name` | Required, non-blank, unique                                                                                       |
| `distribution`    | Required. Keys must match variant names exactly. Each weight must be a positive integer. Weights must sum to 100. |

<Callout icon="📘" theme="info">
  ### **Variant Naming in Reports**

  For `split` and `ab`, the `name` you send is used only to match the `distribution` map. Variants are stored internally in declaration order as Variant A, Variant B, Variant C, that is how they appear in campaign reports. For `messageOnUserProperty`, variants are keyed by their `value` as sent. Variants in `ab` may mix families.
</Callout>

## messageType: ab (manual only)

Variants are shown in an even split until the conversion window elapses, then the winner is selected. In-App supports manual A/B only, automated A/B is rejected with `UNSUPPORTED_COMBINATION`.

The following table lists the `abTest` fields:

| Field                      | Type    | Required                    | Description                                                                                                                          |
| :------------------------- | :------ | :-------------------------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| mode                       | string  | No (default `manual`)       | Must be `manual`. `automated` is rejected.                                                                                           |
| waitTime                   | integer | Yes, must be greater than 0 | Conversion window in minutes. Online A/B has no test sample; the winner is decided solely from this window, and it must be positive. |
| userPercentage, sampleSize | \-      | No                          | Not used for an online channel. Ignored if sent.                                                                                     |

Variants count: 2–3. `abTest` is required when `messageType` is `ab`.

## messageType: messageOnUserProperty

Each user is shown the variant that matches their value for a pivot profile property.

| Constraint              | Value                                                                                                                    |
| :---------------------- | :----------------------------------------------------------------------------------------------------------------------- |
| `userProperty.property` | Required. Must resolve to a known profile property for the account.                                                      |
| `variants[].value`      | Required. Unique across variants.                                                                                        |
| Variants count          | 1–52                                                                                                                     |
| Backup variant          | Required. One variant must have `"value": "wzrk_default"`. Shown to users whose property value matches no other variant. |

## Personalization

**Liquid**

Every string creative field supports Liquid: `d.html` in a custom HTML creative. `title`, `message`, button `text`, `url` action links (`android`/`ios`), and `kv` values in the Basic family. `content.backgroundImage` and action links in the image canvas.

The following table lists the Liquid rules:

| Rule                          | Detail                                                                                                                                                     |
| :---------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Allowed namespaces            | `Profile`, `Event`, `Campaign`, `Catalog`, `Recommendation` only. Any other namespace is rejected with `INVALID_WHAT`.                                     |
| Malformed Liquid              | Content that the engine cannot parse is rejected for every account.                                                                                        |
| Profile fallback required     | Every `{{ Profile.x }}` tag must carry a `default` filter, for example `{{ Profile.Name \| default: 'there' }}`.                                           |
| Profile properties must exist | Each referenced `Profile.<x>` must be a property defined for the account.                                                                                  |
| Event personalization         | `{{ Event.<prop> }}` resolves from the properties of `who.trigger.event` at display time. Each referenced property must be a known property of that event. |

Malformed Liquid is always rejected. Profile and Event property validation applies when personalization validation is enabled for the account.

**personalizationConfig**

Optional Catalog or Recommendation 2.0 block. `personalizationVersion` must be `"v2"`.

The following table lists the `personalizationConfig` fields:

| Field                                                                               | Type    | Required | Description                                                                                                                                |
| :---------------------------------------------------------------------------------- | :------ | :------- | :----------------------------------------------------------------------------------------------------------------------------------------- |
| personalizationVersion                                                              | string  | Yes      | Must be `"v2"`.                                                                                                                            |
| catalogs\[].catalogId                                                               | integer | Yes      | The catalog ID.                                                                                                                            |
| catalogs\[].alias                                                                   | string  | Yes      | Liquid key for this catalog (`{{ Catalog.<alias>.<field> }}`). Must be identifier-safe and unique across all catalogs and recommendations. |
| catalogs\[].catalogName, count, personalisationOn\[]                                | \-      | No       | Display name, item count, and the filters that pick the items.                                                                             |
| recommendations\[].strategyId                                                       | integer | Yes      | The strategy ID.                                                                                                                           |
| recommendations\[].alias                                                            | string  | Yes      | Liquid key (`{{ Recommendation.<alias>[0].<field> }}`). Same uniqueness and identifier rules.                                              |
| recommendations\[].strategyName, count\[], responseColumns\[], personalisationOn\[] | \-      | No       | Display name, item counts, returned columns, and the filters.                                                                              |
| \*.personalisationOn\[].type                                                        | string  | Yes      | Filter source: `profile`, `event`, `behaviour`, or `constant`. A `value` is required for every type except `behaviour`.                    |

# when - Display Window and Display Policy

An online campaign is never sent. `when` describes when the campaign is live (the display window) and how it competes for display (display policy).

The following table lists the `when` fields:

| Field               | Type   | Required | Description                                                                                                                          |
| :------------------ | :----- | :------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| dateTime            | object | Yes      | Trigger kind and display window. For more information, refer [dateTime](doc:create-campaign-v2-in-app#datetime).                     |
| deliveryPreferences | object | No       | Display schedule and priority. For more information, refer [deliveryPreferences](doc:create-campaign-v2-in-app#deliverypreferences). |

## dateTime

The following table lists the `dateTime` fields:

| Field | Type   | Required | Default               | Description                                                                                                                  |
| :---- | :----- | :------- | :-------------------- | :--------------------------------------------------------------------------------------------------------------------------- |
| type  | string | Yes      | \-                    | Must be `userAction`. All other values are rejected.                                                                         |
| start | object | No       | `{ "type": "now" }`   | When the campaign goes live. `{ "type": "now" }` or `{ "type": "onDateTime", "date": "YYYY-MM-DD", "time": "hh:mm AM/PM" }`. |
| end   | object | No       | `{ "type": "never" }` | When it stops. `{ "type": "never" }` or `{ "type": "onDateTime", "date": "YYYY-MM-DD", "time": "hh:mm AM/PM" }`.             |

The display window is evaluated in the account's timezone. The following fields belong to send schedules and are rejected rather than ignored: `delay`, `inactionDuration`, `timezone`, `adHocTimezone`, `userTimezoneWrapAround`, `timeSource: userProperty`.

## deliveryPreferences

Only display-policy blocks are honored. Every block is optional.

| Field           | Type    | Description                                                                                                                                                                                                                                                                                                        |
| :-------------- | :------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| displaySchedule | object  | Weekday allow-list of display windows: `{ "<WEEKDAY>": { "from": "hh:mm AM/PM", "to": "hh:mm AM/PM" } }`. Keys are `MON`, `TUE`, `WED`, `THU`, `FRI`, `SAT`, `SUN` (case-insensitive). Must name at least one weekday. A weekday not listed is not eligible for display. `from` equal to `to` means the whole day. |
| priority        | integer | Display precedence against other campaigns competing for the same trigger moment. Must be a whole number between 1 and 10. Absent means platform default.                                                                                                                                                          |

The following `deliveryPreferences` blocks are rejected with `INVALID_WHEN` rather than dropped: `doNotDisturb`, `timeToLive`, `limits`, `globalMessageLimits`, `throttle`, `cutOff`.

`advancedFrequencyCaps` is not supported for In-App at all. Accounts without the feature see `FEATURE_NOT_ENABLED` on `$.when.deliveryPreferences.advancedFrequencyCaps`, accounts with it see `INVALID_WHEN` on `$.when` (`deliveryPreferences.advancedFrequencyCaps is only supported for NativeDisplay campaigns`).

# conversionGoal

An optional top-level block that defines the conversion event tracked for the campaign's reports.

The following table lists the `conversionGoal` fields:

| Field                    | Type   | Required | Description                                                                                                                                                                    |
| :----------------------- | :----- | :------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| eventName                | string | Yes      | The conversion event. Must exist in the account.                                                                                                                               |
| revenueProperty          | string | No       | Numeric property of the conversion event to sum as revenue. Must be a known property of that event.                                                                            |
| conversionTime           | object | Yes      | Attribution window: `{ "value": <int>, "unit": "<unit>" }`. `value` must be a whole number ≥ 1. `unit` one of `minutes`, `hours`, `days`, `weeks`, `months`. Maximum 5 months. |
| filterType               | object | No       | Additional conditions on the conversion event.                                                                                                                                 |
| filterType.eventProperty | array  | No       | `[{ "propertyName", "operator", "propertyValue" }]`. `operator` defaults to `=`. `propertyValue` is required except for `exists`/`notExists`.                                  |
| filterType.firstTime     | string | No       | `"yes"`, only the user's first occurrence counts. `"no"` is a no-op.                                                                                                           |
| filterType.lastTime      | string | No       | `"yes"` or `"no"`.                                                                                                                                                             |
| filterType.timeOfDay     | array  | No       | `[["HH:mm", "HH:mm"], …]`, 24-hour from/to pairs, `from` ≤ `to`.                                                                                                               |
| filterType.dayOfWeek     | array  | No       | Weekday tokens `["MON", "TUE", …]` (case-insensitive).                                                                                                                         |
| filterType.dayOfMonth    | array  | No       | Day numbers 1–31. `LAST` is not allowed for a conversion goal.                                                                                                                 |

# Example Requests

The following are sample requests to the Create Campaign v2, In-App API:

```curl Custom HTML interstitial
curl -X POST 'https://<region>.api.clevertap.com/2/targets/create.json' \
-H 'X-CleverTap-Account-Id: ACCOUNT_ID' \
-H 'X-CleverTap-Passcode: PASSCODE' \
-H 'Content-Type: application/json' \
-d '{
    "name": "Big Sale interstitial",
    "channel": "InApp",
    "segmentType": "Live",
    "draft": false,
    "who": {
        "trigger": {
            "event": "App Launched"
        },
        "deliverTo": {
            "operatingSystems": ["Android", "iOS"],
            "deviceTypes": ["Mobile", "Tablet"]
        }
    },
    "what": {
        "messageType": "single",
        "message": {
            "type": "custom-html",
            "layout_type": "interstitial",
            "d": {
                "html": "<!DOCTYPE html><html><body><h2>Big Sale is live</h2><p>Up to 50% off, today only.</p></body></html>"
            }
        }
    },
    "when": {
        "dateTime": {
            "type": "userAction"
        },
        "deliveryPreferences": {
            "priority": 5
        }
    }
}'
```
```json Custom HTML header with trigger filter, user profile filter, and schedule
{
    "name": "Lunch deals header - Gold members",
    "channel": "InApp",
    "who": {
        "trigger": {
            "event": "Product Viewed",
            "property_filters": [
                {
                    "property": "Category",
                    "operator": "=",
                    "value": "Food"
                }
            ]
        },
        "deliverTo": {
            "operatingSystems": ["Android", "iOS"],
            "deviceTypes": ["Mobile"]
        },
        "query": {
            "all": [
                {
                    "user_property": {
                        "property": "Plan",
                        "operator": "=",
                        "value": "Gold"
                    }
                },
                {
                    "event": {
                        "event_name": "Charged",
                        "operator": "count >=",
                        "value": 1
                    }
                }
            ]
        }
    },
    "what": {
        "messageType": "single",
        "message": {
            "type": "custom-html",
            "layout_type": "header",
            "d": {
                "html": "<!DOCTYPE html><html><body><h3>Lunch deals</h3><p>Ends at 3pm.</p></body></html>"
            },
            "w": {
                "yp": 20
            }
        }
    },
    "when": {
        "dateTime": {
            "type": "userAction",
            "start": {
                "type": "now"
            },
            "end": {
                "type": "onDateTime",
                "date": "2026-07-31",
                "time": "11:59 PM"
            }
        },
        "deliveryPreferences": {
            "displaySchedule": {
                "MON": { "from": "11:00 AM", "to": "03:00 PM" },
                "TUE": { "from": "11:00 AM", "to": "03:00 PM" },
                "FRI": { "from": "11:00 AM", "to": "03:00 PM" }
            },
            "priority": 8
        }
    }
}
```
```json Basic interstitial with media and two buttons
{
    "name": "Big Sale interstitial - Basic",
    "channel": "InApp",
    "who": {
        "trigger": {
            "event": "App Launched"
        },
        "deliverTo": {
            "operatingSystems": ["Android", "iOS"],
            "deviceTypes": ["Mobile", "Tablet"]
        }
    },
    "what": {
        "messageType": "single",
        "message": {
            "type": "interstitial",
            "title": {
                "text": "Big Sale is live",
                "color": "#434761"
            },
            "message": {
                "text": "Up to 50% off, today only.",
                "color": "#434761"
            },
            "bg": "#ffffff",
            "media": {
                "url": "https://cdn.example.com/promo/hero.png"
            },
            "mediaLandscape": {
                "url": "https://cdn.example.com/promo/hero_wide.png"
            },
            "close": true,
            "buttons": [
                {
                    "text": "Order now",
                    "color": "#ffffff",
                    "bg": "#1EB858",
                    "border": "#1EB858",
                    "radius": "4",
                    "actions": {
                        "type": "url",
                        "android": "https://track.example.com/c/promo",
                        "ios": "https://track.example.com/c/promo"
                    }
                },
                {
                    "text": "Save for later",
                    "color": "#ffffff",
                    "bg": "#1EB858",
                    "border": "#1EB858",
                    "radius": "4",
                    "actions": {
                        "type": "kv",
                        "kv": [
                            { "key": "promo", "value": "SAVE20" }
                        ]
                    }
                }
            ]
        }
    },
    "when": {
        "dateTime": {
            "type": "userAction"
        },
        "deliveryPreferences": {
            "priority": 5
        }
    }
}
```
```json Basic alert-template saved as draft
{
    "name": "Enable notifications - draft",
    "channel": "InApp",
    "draft": true,
    "who": {
        "trigger": {
            "event": "App Launched",
            "first_time": true
        },
        "deliverTo": {
            "operatingSystems": ["Android", "iOS"],
            "deviceTypes": ["Mobile", "Tablet"]
        }
    },
    "what": {
        "messageType": "single",
        "message": {
            "type": "alert-template",
            "title": {
                "text": "Enable notifications"
            },
            "message": {
                "text": "Stay on top of your order updates."
            },
            "buttons": [
                {
                    "text": "Enable",
                    "actions": {
                        "type": "url",
                        "android": "app://settings/notifications",
                        "ios": "app://settings/notifications"
                    }
                },
                {
                    "text": "Not now",
                    "actions": {
                        "type": "close"
                    }
                }
            ]
        }
    },
    "when": {
        "dateTime": {
            "type": "userAction"
        }
    }
}
```
```json image-interstitial with tap target and conversion goal
{
    "name": "Hero image canvas",
    "channel": "InApp",
    "who": {
        "trigger": {
            "event": "App Launched"
        },
        "deliverTo": {
            "operatingSystems": ["Android", "iOS"],
            "deviceTypes": ["Mobile", "Tablet"]
        },
        "controlGroup": [
            { "type": "campaign", "size": 10 }
        ]
    },
    "what": {
        "messageType": "single",
        "message": {
            "type": "image-interstitial",
            "content": {
                "backgroundImage": "https://cdn.example.com/promo/hero.png",
                "altText": "Up to 50% off",
                "margin": 10,
                "actions": {
                    "type": "close"
                }
            },
            "overlay": {
                "backgroundColor": "rgba(0, 0, 0, 0.5)",
                "dismissOnTapOrClick": true
            },
            "buttons": [
                {
                    "name": "Order now",
                    "position": { "x": 30, "y": 70 },
                    "size": { "width": 40, "height": 12 },
                    "actions": {
                        "type": "url",
                        "android": "https://track.example.com/c/promo",
                        "ios": "https://track.example.com/c/promo"
                    }
                }
            ]
        }
    },
    "when": {
        "dateTime": {
            "type": "userAction"
        }
    },
    "conversionGoal": {
        "eventName": "Charged",
        "revenueProperty": "Amount",
        "conversionTime": {
            "value": 1,
            "unit": "days"
        }
    }
}
```
```json Split test
{
    "name": "Offer copy split",
    "channel": "InApp",
    "who": {
        "trigger": {
            "event": "App Launched"
        },
        "deliverTo": {
            "operatingSystems": ["Android", "iOS"],
            "deviceTypes": ["Mobile", "Tablet"]
        }
    },
    "what": {
        "messageType": "split",
        "variants": [
            {
                "name": "Variant A",
                "type": "custom-html",
                "layout_type": "interstitial",
                "d": {
                    "html": "<!DOCTYPE html><html><body><h2>Version A</h2><p>Up to 50% off.</p></body></html>"
                }
            },
            {
                "name": "Variant B",
                "type": "custom-html",
                "layout_type": "interstitial",
                "d": {
                    "html": "<!DOCTYPE html><html><body><h2>Version B</h2><p>Half price today.</p></body></html>"
                }
            }
        ],
        "distribution": {
            "Variant A": 50,
            "Variant B": 50
        }
    },
    "when": {
        "dateTime": {
            "type": "userAction"
        }
    }
}
```
```json Manual A/B mixing Custom HTML and Basic
{
    "name": "Cover A/B - HTML vs Basic",
    "channel": "InApp",
    "who": {
        "trigger": {
            "event": "App Launched"
        },
        "deliverTo": {
            "operatingSystems": ["Android", "iOS"],
            "deviceTypes": ["Mobile", "Tablet"]
        }
    },
    "what": {
        "messageType": "ab",
        "variants": [
            {
                "name": "Variant A",
                "type": "custom-html",
                "layout_type": "cover",
                "d": {
                    "html": "<!DOCTYPE html><html><body><h2>Version A</h2><p>Up to 50% off.</p></body></html>"
                }
            },
            {
                "name": "Variant B",
                "type": "cover",
                "title": { "text": "Version B" },
                "message": { "text": "Half price today." },
                "media": {
                    "url": "https://cdn.example.com/promo/hero.png"
                },
                "buttons": [
                    {
                        "text": "Order now",
                        "color": "#ffffff",
                        "bg": "#1EB858",
                        "border": "#1EB858",
                        "radius": "4",
                        "actions": {
                            "type": "url",
                            "android": "https://track.example.com/c/promo",
                            "ios": "https://track.example.com/c/promo"
                        }
                    }
                ]
            }
        ],
        "abTest": {
            "mode": "manual",
            "waitTime": 45
        }
    },
    "when": {
        "dateTime": {
            "type": "userAction"
        }
    }
}
```
```json Gamification spin-the-wheel
{
    "name": "Spin to win",
    "channel": "InApp",
    "who": {
        "trigger": {
            "event": "App Launched"
        },
        "deliverTo": {
            "operatingSystems": ["Android", "iOS"],
            "deviceTypes": ["Mobile", "Tablet"]
        }
    },
    "what": {
        "messageType": "single",
        "message": {
            "type": "custom-html",
            "layout_type": "spin-the-wheel",
            "d": {
                "html": "<!DOCTYPE html><html><body><h2>Spin to win</h2><p>One spin per customer.</p></body></html>"
            }
        }
    },
    "when": {
        "dateTime": {
            "type": "userAction"
        },
        "deliveryPreferences": {
            "priority": 5
        }
    }
}
```

# Example Response

The following are sample responses from the Create Campaign v2, In-App API:

```json Success
{
    "result": "success",
    "data": {
        "campaignId": 1490871,
        "url": "https://<dashboard-host>/<encoded-account-id>/campaigns/campaign/1490871/edit"
    },
    "status": "success"
}
```
```json Validation failure
{
    "status": "fail",
    "error": "what.message (custom-html) must have layout_type, deliverTo.operatingSystems is required (select at least one operating system)",
    "errors": [
        {
            "code": "REQUIRED_FIELD_MISSING",
            "message": "what.message (custom-html) must have layout_type",
            "path": "$.what.message.layout_type",
            "suggestions": null
        },
        {
            "code": "REQUIRED_FIELD_MISSING",
            "message": "deliverTo.operatingSystems is required (select at least one operating system)",
            "path": "$.who.deliverTo.operatingSystems",
            "suggestions": null
        }
    ]
}
```
```json Single-reason failure
{
    "result": "failure",
    "reason": "Daily API campaign creation limit reached",
    "status": "fail"
}
```

# Response Fields

The following table describes the success response fields:

| Field           | Type    | Description                                                                                                                        |
| :-------------- | :------ | :--------------------------------------------------------------------------------------------------------------------------------- |
| result          | string  | `"success"`                                                                                                                        |
| data.campaignId | integer | The created campaign ID.                                                                                                           |
| data.url        | string  | Dashboard URL where the campaign can be opened and edited. Present whenever the account's dashboard host can be resolved, omitted. |
| status          | string  | `"success"`, added by the edge for parity with other CleverTap APIs.                                                               |

When `"draft": true` is set, the campaign is created in `DRAFT` status. The response shape is unchanged. Campaigns created through this API are marked as API-created and can be edited in the dashboard.

# Error Codes

Two error body shapes are possible.

## Structured Validation Failure (400)

All validation errors in the request are collected and returned in a single response. The following fields are present:

| Field                 | Type           | Description                                                                     |
| :-------------------- | :------------- | :------------------------------------------------------------------------------ |
| status                | string         | `"fail"`                                                                        |
| error                 | string         | Semicolon-joined human-readable summary of every error.                         |
| errors\[]             | array          | Structured per-field errors.                                                    |
| errors\[].code        | string         | Stable machine-readable error code. See the table below.                        |
| errors\[].message     | string         | Human-readable description.                                                     |
| errors\[].path        | string or null | JSON path of the offending element. Null when not element-specific.             |
| errors\[].suggestions | array or null  | Closest valid values for a mistyped token. Empty or absent when not applicable. |

**Error Codes**

The following fields are present:

| Code                      | Meaning                                                                                                                                                                   |
| :------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `REQUIRED_FIELD_MISSING`  | A mandatory field is absent or blank.                                                                                                                                     |
| `INVALID_VALUE`           | Wrong type, an unrecognized discriminator token, an unknown creative key, or a value outside what the template accepts. Carries `suggestions` where a close match exists. |
| `FEATURE_NOT_ENABLED`     | The request uses a feature not enabled for this account. See Account Feature Gates below.                                                                                 |
| `UNSUPPORTED_COMBINATION` | A `segmentType` × `when.dateTime.type` × `what.messageType` combination, this channel does not support.                                                                   |
| `INVALID_WHO`             | Semantic problem in the audience.                                                                                                                                         |
| `INVALID_WHAT`            | Semantic problem in the content.                                                                                                                                          |
| `INVALID_WHEN`            | Semantic problem in the display window or delivery preferences.                                                                                                           |
| `INVALID_COMBINATION`     | An interdependency violation across `who`, `what`, and `when`.                                                                                                            |

<Callout icon="⚠️" theme="warning">
  ### **Error Path Granularity**

  Field-level validation returns precise JSON paths for each problem. Errors in schedule or delivery preference parsing report at the block root (`$.when` or `$.what`) without a sub-path, so only the first problem in that block is returned per request. `conversionGoal` reports only the first problem found in the block.
</Callout>

**Common Structured Errors**

The following table lists common structured errors:

| Code                      | Path                                         | Example Message                                                                                                                                               |
| :------------------------ | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `UNSUPPORTED_COMBINATION` | `$.segmentType`                              | `segmentType 'PBS' is not valid for the online channel InApp, it is always Live`                                                                              |
| `UNSUPPORTED_COMBINATION` | `$.when.dateTime.type`                       | `when.dateTime.type 'userInaction' is not supported with segmentType 'Live' for this channel`                                                                 |
| `UNSUPPORTED_COMBINATION` | `$.what.abTest.mode`                         | `Automated (continuous) A/B is not supported for InApp, use manual A/B (abTest.mode = manual)`                                                                |
| `REQUIRED_FIELD_MISSING`  | `$.who.trigger.event`                        | `who.trigger.event is required for online campaigns`                                                                                                          |
| `INVALID_WHO`             | `$.who.trigger.event`                        | `'Notification Sent' is a system event and cannot be used as a trigger`                                                                                       |
| `INVALID_WHO`             | `$.who.trigger.property_filters[0].operator` | `Operator '>=' is not supported for a trigger event property filter`                                                                                          |
| `REQUIRED_FIELD_MISSING`  | `$.who.deliverTo.deviceTypes`                | `deliverTo.deviceTypes is required (select at least one device type)`                                                                                         |
| `INVALID_VALUE`           | `$.who.deliverTo.operatingSystems`           | `'Windows' is not a supported operating system`                                                                                                               |
| `INVALID_WHO`             | `$.who.exclusion`                            | `who.exclusion is not supported for online campaigns`                                                                                                         |
| `INVALID_VALUE`           | `$.what.message.type`                        | `'header' is not a supported In-App template type`                                                                                                            |
| `INVALID_VALUE`           | `$.what.message.type`                        | `'advanced-builder' is not a supported In-App template type`                                                                                                  |
| `REQUIRED_FIELD_MISSING`  | `$.what.message.layout_type`                 | `what.message (custom-html) must have layout_type`                                                                                                            |
| `INVALID_VALUE`           | `$.what.message.layout_type`                 | `'banner' is not a supported In-App custom-html layout_type`                                                                                                  |
| `FEATURE_NOT_ENABLED`     | `$.what.message.layout_type`                 | `The 'scratch-the-card' gamification template is not enabled for this account.`                                                                               |
| `REQUIRED_FIELD_MISSING`  | `$.what.message.d.html`                      | `what.message (custom-html) must have d.html`                                                                                                                 |
| `INVALID_VALUE`           | `$.what.message.d.url`                       | `what.message d.url (hosted template URL) is not supported yet, supply the creative inline via d.html`                                                        |
| `INVALID_VALUE`           | `$.what.message.title`                       | `what.message title is limited to 20 characters in the editor, but 25 were supplied`                                                                          |
| `REQUIRED_FIELD_MISSING`  | `$.what.message.buttons`                     | `what.message (cover-image) needs at least 1 action button(s), the editor creates them and will not let an author remove them`                                |
| `INVALID_VALUE`           | `$.what.message.buttons`                     | `what.message (cover) accepts at most 2 button(s), but 3 were supplied`                                                                                       |
| `INVALID_VALUE`           | `$.what.message.buttons[0].radius`           | `what.message buttons[0] radius must be an even number from 0 to 24, as a string`                                                                             |
| `REQUIRED_FIELD_MISSING`  | `$.what.message.buttons[0].actions`          | `what.message buttons[0] actions ('url') must have android or ios`                                                                                            |
| `INVALID_VALUE`           | `$.what.message.media`                       | `what.message media is a 'video/mp4', this template accepts images only`                                                                                      |
| `REQUIRED_FIELD_MISSING`  | `$.what.message.content.backgroundImage`     | `what.message content must have a backgroundImage URL, it is the whole creative on this template`                                                             |
| `INVALID_VALUE`           | `$.what.message.buttons[0].position`         | `what.message buttons[0] extends past the creative: position plus size must stay within 100%`                                                                 |
| `INVALID_WHAT`            | `$.what.abTest.waitTime`                     | `abTest.waitTime (conversion window in minutes) must be positive for online A/B`                                                                              |
| `INVALID_WHAT`            | `$.what.distribution`                        | `distribution weights must sum to 100 (got 90)`                                                                                                               |
| `REQUIRED_FIELD_MISSING`  | `$.what.variants`                            | `messageOnUserProperty requires a default backup variant (a variant with value "wzrk_default"), served when a user's property value matches no other variant` |
| `INVALID_WHEN`            | `$.when`                                     | `when.dateTime.delay is not supported for an online channel, an online campaign is shown as soon as the user qualifies`                                       |
| `INVALID_WHEN`            | `$.when`                                     | `deliveryPreferences.timeToLive is not supported for an online channel, an online campaign has no send to apply it to`                                        |
| `INVALID_WHEN`            | `$.when`                                     | `deliveryPreferences.priority must be a whole number between 1 and 10`                                                                                        |
| `INVALID_WHEN`            | `$.when`                                     | `Date and time cannot be in the past : 2025-01-01 09:30 AM`                                                                                                   |
| `INVALID_WHEN`            | `$.when`                                     | `when.dateTime.end must be after when.dateTime.start`                                                                                                         |

## Single-Reason Failure

Used for failures raised after validation passes. These carry no `errors[]`, no `code`, and no `path`.

```json
{ "result": "failure", "reason": "<reason>", "status": "fail" }
```

| HTTP Status | reason                                                      | Cause                                                                                 |
| :---------- | :---------------------------------------------------------- | :------------------------------------------------------------------------------------ |
| 402         | `"Daily API campaign creation limit reached"`               | Daily API creation cap hit. The actual cap may be lower than 50,000 for your account. |
| 402         | `"Monthly API campaign creation limit reached"`             | Monthly API creation cap hit.                                                         |
| 402         | `"Campaign creation limit reached for your current plan"`   | Plan campaign limit hit.                                                              |
| 409         | `"A similar campaign was just created, please retry"`       | A near-identical campaign was created moments earlier. Retry after a short delay.     |
| 400         | `"Campaign could not be saved"`                             | Save rejected with no more specific message.                                          |
| 400         | `"Invalid request body"`                                    | Body is not a parseable JSON.                                                         |
| 501         | `"channel 'Email' is not yet available on Campaign API v2"` | Channel has no v2 pipeline.                                                           |
| 501         | `"who.prompt is not supported for InApp Live campaigns"`    | `who.prompt` sent.                                                                    |
| 500         | `"Failed to persist campaign"`                              | Validation passed, but the save failed. Safe to retry.                                |

# Account Feature Gates

The following features are only available when the corresponding account setting is enabled.

| Feature                    | Request Trigger                                                         | Behavior                                                                                                                                                                                                                        |
| :------------------------- | :---------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Gamification templates     | `custom-html` with `layout_type` `spin-the-wheel` or `scratch-the-card` | Accepted only when the gamification templates feature (`GAMIFICATION_TEMPLTES` flag) is enabled for the account. Otherwise returns `FEATURE_NOT_ENABLED` on `$.what.message.layout_type`.                                       |
| Personalization validation | Any `{{ Profile.* }}` or `{{ Event.* }}` tag                            | Profile and Event property validation applies when personalization validation is enabled for the account. Malformed Liquid is always rejected.                                                                                  |
| Super device properties    | `device_fields` rule in `who.query`                                     | Available only when super device properties is enabled. Otherwise `INVALID_WHO`.                                                                                                                                                |
| Automated (continuous) A/B | `what.abTest.mode = automated`                                          | Not supported for In-App at all. Accounts without the feature, check `FEATURE_NOT_ENABLED`, accounts with it , check `UNSUPPORTED_COMBINATION`. Both return on `$.what.abTest.mode`.                                            |
| Advance frequency caps     | `when.deliveryPreferences.advancedFrequencyCaps`                        | Not supported for In-App at all. Accounts without the feature, check `FEATURE_NOT_ENABLED`, accounts with it, check `INVALID_WHEN` (`deliveryPreferences.advancedFrequencyCaps is only supported for NativeDisplay campaigns`). |

# Cross-Block Rules

The following table lists the cross-block rules evaluated across `who`, `what`, and `when`:

| Rule                                                                                        | Rejection                                                                                                                 |
| :------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------ |
| In-App is always `Live` × `userAction`                                                      | `UNSUPPORTED_COMBINATION` on `$.segmentType` or `$.when.dateTime.type`.                                                   |
| Online A/B needs a positive conversion window                                               | `INVALID_WHAT` on `$.what.abTest.waitTime`.                                                                               |
| Automated A/B is not available for an online channel                                        | `UNSUPPORTED_COMBINATION` on `$.what.abTest.mode`.                                                                        |
| `layout_type` is only meaningful with `type: custom-html`                                   | On a Basic or canvas template, it is an unknown key.                                                                      |
| `mediaLandscape` needs landscape orientation enabled                                        | `INVALID_VALUE` on `$.what.message.mediaLandscape` unless `hasLandscape` is true explicitly or by the template's default. |
| A creative with no buttons always gets a close button                                       | `close` is forced `true` when `buttons` is empty. It cannot be switched off.                                              |
| `Event.*` personalization resolves from the trigger event                                   | Each referenced event property must exist on `who.trigger.event` when personalization validation is enabled.              |
| Send-side delivery preferences and timezone fields are meaningless for a displayed campaign | `INVALID_WHEN` on `$.when`.                                                                                               |

# Not Available for In-App

The following table lists features and fields that are not supported for In-App and are rejected if sent:

| Area                                     | Not Supported                                                                                                                                                                                                                                                                                                                                                                     |
| :--------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `segmentType`                            | `PBS`, `External Trigger`                                                                                                                                                                                                                                                                                                                                                         |
| `when.dateTime`                          | `sendNow`, `scheduleLater`, `recurring`, `userInaction`, `dateTime`, `externalTrigger`, `delay`, `inactionDuration`, `timezone`, `adHocTimezone`, `userTimezoneWrapAround`, `timeSource: userProperty`                                                                                                                                                                            |
| `when.deliveryPreferences`               | `doNotDisturb`, `timeToLive`, `limits`, `globalMessageLimits`, `throttle`, `cutOff`, `advancedFrequencyCaps`                                                                                                                                                                                                                                                                      |
| `who`                                    | `exclusion`, `inaction`, `trigger_property`, `prompt`                                                                                                                                                                                                                                                                                                                             |
| `who.query` rules                        | `events_group`, `interest`, `multi_app`, `geo_radius`, `demographic`, `technographics`, `reachability`, `subscription_groups`, event `property_filters`/`session_filters`/`geo_filters`, `count <`, `count <=`, `count !=`, `count >= 0`, absolute/predefined `when` windows, geographic `notIn`, `user_property` `>=`, `<=`, `in`, `notIn`, `notContains`, `exists`, `notExists` |
| `who.trigger.property_filters` operators | `>=`, `<=`                                                                                                                                                                                                                                                                                                                                                                        |
| `what.messageType`                       | `ab` with `mode: automated`                                                                                                                                                                                                                                                                                                                                                       |
| Template type                            | `advanced-builder`, `custom-code`, `nps-ratings`, `user-ratings`, lead-generation templates, `pip`, template-library saved IDs, the layout names `header`/`footer` used as a type                                                                                                                                                                                                 |
| `custom-html`                            | `d.url` (hosted template URL), `inlineHtml: false`, any key other than `type`, `layout_type`, `d`, `w`, `isJsEnabled`, `inlineHtml`, `w` keys other than `dk`, `sc`, `yp`, `pos`, `xp`                                                                                                                                                                                            |
| Basic family                             | `title`/`message` on `*-image` templates, text `color`, `bg`, `media` on `alert-template`, `close` on `header-template`/`footer-template`/`alert-template`, `kv` action on `alert-template`, video on any template other than `interstitial` and the three `*-image` templates, audio media, button keys outside the template's button style                                      |
| Button actions                           | `custom-code` (app functions), "request push permission". Any unrecognized action type is rejected.                                                                                                                                                                                                                                                                               |
| `image-interstitial`                     | More than 10 tap targets, pixel geometry, button text/styling, `content` keys other than `backgroundImage`, `altText`, `margin`, `actions`, `overlay` keys other than `backgroundColor`, `backgroundImage`, `backgroundRepeat`, `backgroundSize`, `dismissOnTapOrClick`                                                                                                           |
