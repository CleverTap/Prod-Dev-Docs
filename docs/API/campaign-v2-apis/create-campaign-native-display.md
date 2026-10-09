---
title: Create Campaign v2- Native Display
deprecated: false
hidden: true
link:
  new_tab: false
metadata:
  robots: index
---
# Overview

Native Display delivers creatives directly inside your app in real time. When a user fires the trigger event, the SDK receives the matching creative and renders it immediately, no notification, no delay.

Unlike push or email, Native Display campaigns are displayed, not sent. The audience is evaluated live at trigger time, and none of the standard send-side delivery controls (DND, TTL, throttling) apply.

Every request is organized into three blocks.

| Block  | What it Defines                                                                                                                      |
| :----- | :----------------------------------------------------------------------------------------------------------------------------------- |
| `who`  | The trigger event, optional event property filters, device targeting, and an optional user profile filter evaluated at trigger time. |
| `what` | The creative, template type, and message strategy (single, split, A/B, or property-based).                                           |
| `when` | The display window and optional display policy (schedule and priority).                                                              |

<Callout icon="📘" theme="info">
  ### **Native Display is always a Live Campaign**

  `segmentType` is always `Live` for Native Display. You may omit it or send `Live`. Any other value, including `PBS` or `External Trigger`, is rejected with `UNSUPPORTED_COMBINATION` on `$.segmentType`. `when.dateTime.type` must always be `userAction`, all other schedule types are rejected.
</Callout>

<Callout icon="📘" theme="info">
  ### **create.json Only**

  v2 supports `create.json` only. `list`, `stop`, `report`, and `trigger` remain v1-only under `/1/targets/*`.
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

| Limit                                       | Scope                                                                                             | On Breach                                               |
| :------------------------------------------ | :------------------------------------------------------------------------------------------------ | :------------------------------------------------------ |
| 60 concurrent in-flight requests            | Global                                                                                            | HTTP 503                                                |
| 50,000 campaign creations per day (ceiling) | Per account, all API-created campaigns (v1 + v2). The actual limit may be lower for your account. | HTTP 402, `"Daily API campaign creation limit reached"` |

# Supported Combinations

Every Native Display campaign uses `segmentType: Live` and `when.dateTime.type: userAction`. Any other `when.dateTime.type` is rejected with `UNSUPPORTED_COMBINATION` on `$.when.dateTime.type`.

The following table shows the supported `what.messageType` values for Native Display:

| messageType             | Supported                                                                                                                   |
| :---------------------- | :-------------------------------------------------------------------------------------------------------------------------- |
| `single`                | Yes                                                                                                                         |
| `split`                 | Yes                                                                                                                         |
| `ab` (manual)           | Yes                                                                                                                         |
| `ab` (automated)        | No, `UNSUPPORTED_COMBINATION` on `$.what.abTest.mode`                                                                       |
| `messageOnUserProperty` | Yes                                                                                                                         |
| `cascade`               | Feature-gated, available only on eligible accounts. Returns `FEATURE_NOT_ENABLED` on `$.what.messageType` when not enabled. |

# Request Parameters

The following table lists the top-level request fields:

| Field          | Type    | Required | Default | Description                                                                                                                                                                             |
| :------------- | :------ | :------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name           | string  | Yes      | \-      | Campaign name. Must be non-blank.                                                                                                                                                       |
| channel        | string  | Yes      | \-      | `NativeDisplay`. Case-insensitive.                                                                                                                                                      |
| segmentType    | string  | No       | `Live`  | Always `Live` for Native Display. We recommend omitting this field. If sent, must be `Live` , any other value is rejected.                                                              |
| who            | object  | Yes      | \-      | Trigger event, device targeting, optional profile filter, and control group. For more information, refer [who - Audience](doc:create-campaign-native-display#who---audience).           |
| what           | object  | Yes      | \-      | Creative and message-type strategy. For more information, refer [what - Content](doc:create-campaign-v2-in-app#what---content).                                                         |
| when           | object  | Yes      | \-      | Display window and display policy. For more information, refer [when - Display Window and Display Policy](doc:create-campaign-native-display#when---display-window-and-display-policy). |
| draft          | boolean | No       | `false` | `true` saves the campaign in `DRAFT` status instead of activating it. A draft is not shown to users until it is launched from the dashboard.                                            |
| conversionGoal | object  | No       | \-      | Conversion tracking goal. For more information, refer [conversionGoal](doc:create-campaign-native-display#conversiongoal).                                                              |

<Callout icon="📘" theme="info">
  ### **Test with Draft Mode**

  Set `"draft": true` when testing a new campaign. The campaign is saved to your account and can be reviewed in the dashboard before you make it live. Set `"draft": false` or omit the field when you are ready to activate.
</Callout>

# who - Audience

The Native Display audience is event-triggered. A user qualifies the instant they raise the trigger event, optionally filtered by the event's properties, a geo-fence, and a user profile filter evaluated at that moment. Device targeting is a separate required block.

The following table lists the `who` fields:

| Field        | Type   | Required | Description                                                                                                                                                                                          |
| :----------- | :----- | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| trigger      | object | Yes      | The trigger event and its filters. For more information, refer [who.trigger](doc:create-campaign-native-display#whotrigger).                                                                         |
| deliverTo    | object | Yes      | Operating systems and device types on which the campaign may be shown. For more information, refer [who.deliverTo](doc:create-campaign-v2-in-app#whodeliverto).                                      |
| query        | object | No       | An optional user profile filter is evaluated at trigger time against the user's profile. For more information, refer [who.query](doc:create-campaign-native-display#whoquery---user-profile-filter). |
| controlGroup | array  | No       | Control-group (holdout) settings. For more information, refer [who.controlGroup.](doc:/create-campaign-v2-native-display#whocontrolgroup)                                                            |

The following keys are rejected for Native Display with `INVALID_WHO` and the corresponding JSON path: `exclusion`, `inaction`, `trigger_property`. `prompt` returns `501`.

## who.trigger

The following table lists the `who.trigger` fields:

| Field            | Type    | Required     | Description                                                                                                                                                                                     |
| :--------------- | :------ | :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| event            | string  | Yes          | Name of the event that triggers the display. Must exist in the account. System events cannot be used.                                                                                           |
| first_time       | boolean | No (`false`) | `true` fires only on the user's first ever occurrence of the event.                                                                                                                             |
| property_filters | array   | No           | Event-property conditions on the trigger event. All entries are AND-ed. For more information, refer [Trigger Property Filters](doc:create-campaign-v2-native-display#trigger-property-filters). |
| geo_filters      | array   | No           | Geo-fence(s), the trigger event must fall inside. Non-empty when present. For more information, refer [Trigger Geo Filters](doc:create-campaign-v2-native-display#trigger-geo-filters).         |

Any other key inside `trigger` is rejected by the structural schema.

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

The following table lists the supported trigger property filter operators:

| Operator                  | Value                   | Notes                                                      |
| :------------------------ | :---------------------- | :--------------------------------------------------------- |
| `=`                       | scalar                  | Equals.                                                    |
| `!=`                      | scalar                  | Not equals.                                                |
| `in`                      | array                   | Value is one of the list. Must be a JSON array.            |
| `notIn`                   | array                   | Value is none of the list.                                 |
| `>`, `<`                  | number                  | Numeric comparison.                                        |
| `between`                 | 2-element numeric array | Inclusive range.                                           |
| `contains`, `notContains` | string                  | Substring match.                                           |
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

The following table lists the `who.deliverTo` fields:

| Field            | Allowed Values           | Type             | Required |
| :--------------- | :----------------------- | :--------------- | :------- |
| operatingSystems | `Android`, `iOS`         | array of strings | Yes      |
| deviceTypes      | `Mobile`, `Tablet`, `TV` | array of strings | Yes      |

## who.query - User Profile Filter

An optional CQL v2 rule tree is evaluated against the user's profile at trigger time. It is a group with exactly one of `any` (OR) or `all` (AND) holding an array of single-key rule objects. Groups nest up to 5 levels. Only a subset of the CQL v2 vocabulary is available here, some rules available in full segment definitions are not supported at trigger time.

The following table lists the supported user filter rules:

| Rule            | Key Fields                                                                                     | Supported Operators                                                            | Notes                                                                                                                                                                                                                                                                    |
| :-------------- | :--------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `event`         | `event_name`, `operator`, `value`, optional `when` and `occurrence`                            | `count =`, `count >`, `count >=`, `count between`                              | Lifetime count of the event on the profile. `count between` takes `[min, max]`. `count >= 0`, `count <`, `count <=`, `count !=` are rejected. `property_filters`, `session_filters`, `geo_filters` on a user filter events are rejected.                                 |
| `event + when`  | `event_name`, `when: { "kind": "relative", "mode": "last", "days": N }`, optional `occurrence` | \-                                                                             | "Did the event in the last N units." `kind` must be `relative`, `mode` must be `last`. `unit` one of `minutes`, `hours`, `days`, `weeks`, `months`, `years` (default `days`). Cannot combine with a count operator.                                                      |
| `event_did_not` | `event_name`                                                                                   | \-                                                                             | User never did the event. No `operator`, `value`, or `when` allowed.                                                                                                                                                                                                     |
| `user_property` | `property`, `operator`, `value`, `data_type`                                                   | `=`, `!=`, `>`, `<`, `contains`, `between`                                     | `>` / `<` need a numeric value, `between` a 2-element numeric array. With `data_type: "date"`: `=`, `>`, `<`, `between` on ISO `YYYY-MM-DD` strings. `>=`, `<=`, `in`, `notIn`, `notContains`, `exists`, `notExists` are not part of the user profile filter vocabulary. |
| `user_bucket`   | `operator`, `value`                                                                            | `in` only                                                                      | `value` is a non-empty array of integers 0–999.                                                                                                                                                                                                                          |
| `app_fields`    | `attribute`, `operator`, `value`                                                               | `=`, `!=`, `in`, `notIn`, `>`, `>=`, `<`, `contains`, `notContains`, `between` | `attribute` must be one of `OS Version`, `SDK Version`, `Make`, `Model`, `App Version`. `>`, `>=`, `<`, `between` apply to version attributes only. `<=` is rejected.                                                                                                    |
| `device_fields` | `attribute`, `operator`, `value`                                                               | `=`, `!=`, `in`, `notIn`, `>`, `<`                                             | `attribute` must be one of `width`, `height`, `ppi`. `>=` and `<=` are rejected. Available only when super device properties are enabled for the account.                                                                                                                |
| `geographic`    | `level`, country/state/city values                                                             | `in` only                                                                      | `notIn` is rejected for live campaigns. `state` requires `level: state`, `city` requires `level: city` plus `state`.                                                                                                                                                     |
| `segments`      | `names[]` (1–20 saved segment names), `operator`                                               | `in`, `notIn`                                                                  | Membership in saved segments by name.                                                                                                                                                                                                                                    |

The following user filter rules are not available and return `INVALID_WHO` with the rule's path: `events_group`, `interest`, `multi_app`, `geo_radius`, `demographic`, `technographics`, `reachability`, `subscription_groups`. Analytics roots (`trends`, `funnel`, `cohort`, `flow`, `pivot`) are also rejected.

## who.controlGroup

Optional array of control-group (holdout) entries. A `type` may appear at most once. `campaign` and `custom` are mutually exclusive. `system` is independent.

The following table lists the control group types:

| type       | Fields                      | Description                                                                                                                 |
| :--------- | :-------------------------- | :-------------------------------------------------------------------------------------------------------------------------- |
| `system`   | `apply` (boolean, required) | `true` holds out the account's system control group. Requires an active system control group to be defined for the account. |
| `campaign` | `size` (integer, required)  | Campaign-specific random holdout, 1–99 percent.                                                                             |
| `custom`   | `name` (string, required)   | Name of an existing, active custom control group.                                                                           |

# what - Content

The following table lists the `what` fields:

| Field                 | Type   | Required                                            | Default  | Description                                                                                                                                  |
| :-------------------- | :----- | :-------------------------------------------------- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------- |
| messageType           | string | No                                                  | `single` | `single`, `split`, `ab`, or `messageOnUserProperty`.                                                                                         |
| message               | object | Required for `single`                               | \-       | The single template object. For more information, refer [Template Object](doc:create-campaign-v2-native-display#template-object).            |
| variants              | array  | Required for `split`, `ab`, `messageOnUserProperty` | \-       | An array of template objects, each with a variant envelope key (`name` or `value`).                                                          |
| distribution          | object | Required for `split`                                | \-       | `{ "<variantName>": <percent> }`.                                                                                                            |
| abTest                | object | Required for `ab`                                   | \-       | A/B configuration. For more information, refer [messageType: ab](doc:create-campaign-v2-native-display#messagetype-single).                  |
| userProperty          | object | Required for `messageOnUserProperty`                | \-       | `{ "property": "<profile property name>" }`.                                                                                                 |
| personalizationConfig | object | No                                                  | \-       | Catalog and Recommendation 2.0 config. For more information, refer [Personalization](doc:create-campaign-v2-native-display#personalization). |

## Template Object

Each `what.message` or entry in `what.variants[]` is a Native Display template object.

The following table lists the template object fields:

| Field         | Type   | Required                                         | Description                                                                                                               |
| :------------ | :----- | :----------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------ |
| type          | string | Yes                                              | Template type. For more information, refer [Template Types](doc:create-campaign-v2-native-display#personalization) table. |
| content       | array  | Required for all types except `advanced-builder` | The creative items (slides). Item count depends on the type.                                                              |
| customKVData  | array  | No                                               | Custom key-value pairs delivered with the creative: `[{ "key": "…", "value": { "text": "…" } }]`.                         |
| builderConfig | object | Required for `advanced-builder`                  | The complete builder creative.                                                                                            |
| name          | string | Required for `split` and `ab` variants           | Unique, non-blank variant name. Used to key `distribution`.                                                               |
| value         | string | Required for `messageOnUserProperty` variants    | The profile-property value this variant targets. Unique across variants.                                                  |

## Template Types

The following table lists the supported template types and the corresponding `content[]` constraints:

| type               | content\[] items | Typical fields per item                         |
| :----------------- | :--------------- | :---------------------------------------------- |
| `simple`           | Exactly 1        | `title`, `message`, `media`, `action`           |
| `carousel`         | 2–5              | `title`, `message`, `media`, `action` per slide |
| `message-icon`     | Exactly 1        | `title`, `message`, `icon`, `action`            |
| `custom-key-value` | Exactly 1        | `title` (plus template-level `customKVData`)    |
| `simple-image`     | Exactly 1        | `media`, `action`                               |
| `carousel-image`   | 2–5              | `media`, `action` per slide                     |
| `advanced-builder` | Not used         | `builderConfig` instead of `content`            |

## Content Item Fields

Every field is optional, but a field that is present must have the correct shape. The following table lists the fields for each entry of `content[]`:

| Field                              | Accepted Shape                      | Description                                                                                                      |
| :--------------------------------- | :---------------------------------- | :--------------------------------------------------------------------------------------------------------------- |
| title                              | string or `{ "text": "…" }`         | Title text. Supports Liquid.                                                                                     |
| message                            | string or `{ "text": "…" }`         | Body text. Supports Liquid.                                                                                      |
| media                              | string or `{ "url": "…" }`          | Image URL.                                                                                                       |
| icon                               | string or `{ "url": "…" }`          | Icon URL. Used in `message-icon`.                                                                                |
| action                             | object                              | Tap action.                                                                                                      |
| action.url                         | object `{ "android": …, "ios": … }` | Per-platform deep link or URL. Must be an object. A bare string is rejected.                                     |
| action.url.android, action.url.ios | string or `{ "text": "…" }`         | The platform URL.                                                                                                |
| action.links                       | array                               | Optional array of additional link objects attached to the action. Each entry is passed through to the SDK as-is. |

String shorthand is accepted: at save time `"title": "Hi"` expands to `{ "text": "Hi" }`, `"media": "https://…"` expands to `{ "url": "https://…" }`, and `"action.url.android": "app://x"` expands to `{ "text": "app://x" }`.

## advanced-builder

For `type: advanced-builder`, supply `builderConfig` (the dashboard's Advanced Builder output) instead of `content[]`. `builderConfig` must contain at least one renderable element.

The following table lists the `builderConfig` fields:

| Field        | Type   | Description                                                                                                                                |
| :----------- | :----- | :----------------------------------------------------------------------------------------------------------------------------------------- |
| content      | object | Container definition (non-empty object).                                                                                                   |
| textElements | array  | Text elements (non-empty array).                                                                                                           |
| images       | array  | Image elements (non-empty array).                                                                                                          |
| buttons      | array  | Button elements (non-empty array), for example `{ "id", "text", "actions": { "type": "url", "android": { "text" }, "ios": { "text" } } }`. |

At least one of the four must be present and non-empty.

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

  For `split` and `ab`, the `name` you send is used only to match the `distribution` map. Variants appear in campaign reports in declaration order as Variant A, Variant B, Variant C. For `messageOnUserProperty`, variants are keyed by their `value` as sent.
</Callout>

## messageType: ab (manual only)

Variants are shown in an even split until the conversion window elapses, then the winner is selected. Native Display supports manual A/B only, automated A/B is rejected with `UNSUPPORTED_COMBINATION`.

The following table lists the `abTest` fields:

| Field                      | Type    | Required                    | Description                                                                                                                          |
| :------------------------- | :------ | :-------------------------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| mode                       | string  | No (default `manual`)       | Must be `manual`. `automated` is rejected.                                                                                           |
| waitTime                   | integer | Yes, must be greater than 0 | Conversion window in minutes. Online A/B has no test sample, the winner is decided solely from this window, and it must be positive. |
| userPercentage, sampleSize | \-      | No                          | Not used for an online channel. Ignored if sent.                                                                                     |

Variants count: 2–3. `abTest` is required when `messageType` is `ab`.

## messageType: messageOnUserProperty

Each user is shown the variant that matches their value for a chosen profile property.

| Constraint              | Value                                                                                                                    |
| :---------------------- | :----------------------------------------------------------------------------------------------------------------------- |
| `userProperty.property` | Required. Must resolve to a known profile property for the account.                                                      |
| `variants[].value`      | Required. Unique across variants.                                                                                        |
| Variants count          | 1–52                                                                                                                     |
| Backup variant          | Required. One variant must have `"value": "wzrk_default"`. Shown to users whose property value matches no other variant. |

## Personalization

**Liquid Tags**

`title`, `message`and other string-creative fields support Liquid tags.

The following table lists the Liquid tag rules:

| Rule                          | Detail                                                                                                                 |
| :---------------------------- | :--------------------------------------------------------------------------------------------------------------------- |
| Allowed namespaces            | `Profile`, `Event`, `Campaign`, `Catalog`, `Recommendation` only. Any other namespace is rejected with `INVALID_WHAT`. |
| Malformed Liquid              | Content the engine cannot parse (an unclosed `{{`, a bad filter) is rejected for every account.                        |
| Profile fallback required     | Every `{{ Profile.x }}` tag must carry a `default` filter, for example `{{ Profile.name \| default: 'there' }}`.       |
| Profile properties must exist | Each referenced `Profile.<x>` must be a property defined for the account.                                              |
| Event personalization         | `{{ Event.<prop> }}` resolves from the properties of `who.trigger.event` at display time.                              |

A malformed Liquid tag is always rejected. Profile and Event property validation applies when personalization validation is enabled for the account.

**personalizationConfig**

Optional Catalog or Recommendation 2.0 block. `personalizationVersion` must be `"v2"`.

The following table lists the `personalizationConfig` fields:

| Field                                                                               | Type    | Required | Description                                                                                                                                |
| :---------------------------------------------------------------------------------- | :------ | :------- | :----------------------------------------------------------------------------------------------------------------------------------------- |
| personalizationVersion                                                              | string  | Yes      | Must be `"v2"`.                                                                                                                            |
| catalogs\[].catalogId                                                               | integer | Yes      | The catalog ID.                                                                                                                            |
| catalogs\[].alias                                                                   | string  | Yes      | Liquid key for this catalog (`{{ Catalog.<alias>.<field> }}`). Must be identifier-safe and unique across all catalogs and recommendations. |
| catalogs\[].catalogName, count, personalizationOn\[]                                | ,       | No       | Display name, item count, and the filters that pick the items.                                                                             |
| recommendations\[].strategyId                                                       | integer | Yes      | Strategy ID                                                                                                                                |
| recommendations\[].alias                                                            | string  | Yes      | Liquid key (`{{ Recommendation.<alias>[0].<field> }}`). Same uniqueness and identifier rules as the catalog alias.                         |
| recommendations\[].strategyName, count\[], responseColumns\[], personalizationOn\[] | ,       | No       | Display name, item counts, returned columns, and the filters.                                                                              |
| personalizationOn\[].type                                                           | string  | Yes      | Filter source: `profile`, `event`, `behaviour`, or `constant`. A `value` is required for every type except `behaviour`.                    |

Catalog and strategy IDs are validated structurally at creation and resolved at display time.

# when - Display Window and Display Policy

An online campaign is never sent. `when` describes when the campaign is live (the display window) and how it competes for display (display policy).

The following table lists the `when` fields:

| Field               | Type   | Required | Description                                                                                                                                  |
| :------------------ | :----- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------- |
| dateTime            | object | Yes      | Trigger kind and display window. For more information, refer [dateTime](doc:create-campaign-v2-native-display#datetime).                     |
| deliveryPreferences | object | No       | Display schedule and priority. For more information, refer [deliveryPreferences](doc:create-campaign-v2-native-display#deliverypreferences). |

## dateTime

The following table lists the `dateTime` fields:

| Field | Type   | Required | Default               | Description                                                                                                                  |
| :---- | :----- | :------- | :-------------------- | :--------------------------------------------------------------------------------------------------------------------------- |
| type  | string | Yes      | \-                    | Must be `userAction`. All other values are rejected.                                                                         |
| start | object | No       | `{ "type": "now" }`   | When the campaign goes live. `{ "type": "now" }` or `{ "type": "onDateTime", "date": "YYYY-MM-DD", "time": "hh:mm AM/PM" }`. |
| end   | object | No       | `{ "type": "never" }` | When it stops. `{ "type": "never" }` or `{ "type": "onDateTime", "date": "YYYY-MM-DD", "time": "hh:mm AM/PM" }`.             |

The display window is evaluated in the account's timezone.

The following date and time formats apply:

| Field | Format                | Example      |
| :---- | :-------------------- | :----------- |
| date  | `YYYY-MM-DD`          | `2026-07-15` |
| time  | 12-hour `hh:mm AM/PM` | `09:30 AM`   |

The following fields belong to send schedules and are rejected rather than ignored: `delay`, `inactionDuration`, `timezone`, `adHocTimezone`, `userTimezoneWrapAround`, `timeSource: userProperty`.

## deliveryPreferences

Only display-policy blocks are honored. Every block is optional.

The following table lists the supported `deliveryPreferences` fields:

| Field                 | Type    | Description                                                                                                                                                                                                                                                                       |
| :-------------------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| displaySchedule       | object  | Weekday allow-list of display windows: `{ "<WEEKDAY>": { "from": "hh:mm AM/PM", "to": "hh:mm AM/PM" } }`.                                                                                                                                                                         |
| priority              | integer | Display precedence against other campaigns competing for the same trigger moment. Whole numbers 1–10. Absent means platform default.                                                                                                                                              |
| advancedFrequencyCaps | array   | Native Display-only. Frequency cap rules, each as `{ "count": <integer>, "interval": "day" / "week" / "month" }`. Requires the advance frequency caps feature to be enabled for the account, returns `FEATURE_NOT_ENABLED` on `$.when.deliveryPreferences.advancedFrequencyCaps`. |

**displaySchedule**

Keys are weekday tokens `MON`, `TUE`, `WED`, `THU`, `FRI`, `SAT`, `SUN` (case-insensitive). Each value is `{ "from", "to" }` in 12-hour format. A weekday that is not listed is not eligible for display. `from` equal to `to` means the whole day.

**Rejected Send-Side Blocks**

The following `deliveryPreferences` blocks have no meaning for a displayed campaign and are rejected with `INVALID_WHEN`, not silently dropped: `doNotDisturb`, `timeToLive`, `limits`, `globalMessageLimits`, `throttle`, `cutOff`.

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
| filterType.timeOfDay     | array  | No       | `[["HH:mm", "HH:mm"], …]`, 24-hour from/to pairs (`00:00`–`23:59`, from ≤ to).                                                                                                 |
| filterType.dayOfWeek     | array  | No       | Weekday tokens `["MON", "TUE", …]` (case-insensitive).                                                                                                                         |
| filterType.dayOfMonth    | array  | No       | Day numbers 1–31. `LAST` is not allowed for a conversion goal.                                                                                                                 |

# Example Requests

The following are sample requests for the Create Campaign v2 - Native Display:

```curl Single simple creative
curl -X POST 'https://<region>.api.clevertap.com/2/targets/create.json' \
-H 'X-CleverTap-Account-Id: ACCOUNT_ID' \
-H 'X-CleverTap-Passcode: PASSCODE' \
-H 'Content-Type: application/json' \
-d '{
 "name": "Home banner - Big Sale",
 "channel": "NativeDisplay",
 "segmentType": "Live",
 "draft": false,
 "who": {
 "trigger": { "event": "App Launched" },
 "deliverTo": {
 "operatingSystems": ["Android", "iOS"],
 "deviceTypes": ["Mobile", "Tablet"]
 }
 },
 "what": {
 "messageType": "single",
 "message": {
 "type": "simple",
 "content": [
 {
 "title": "Big Sale is live",
 "message": "Up to 50% off, today only",
 "media": "https://cdn.example.com/nd/hero.png",
 "action": { "url": { "android": "app://sale", "ios": "app://sale" } }
 }
 ]
 }
 },
 "when": {
 "dateTime": { "type": "userAction" },
 "deliveryPreferences": { "priority": 5 }
 }
}'
```
```json Carousel with trigger filter, user profile filter, and schedule
{
 "name": "Shoes carousel - Gold members",
 "channel": "NativeDisplay",
 "segmentType": "Live",
 "who": {
 "trigger": {
 "event": "Product Viewed",
 "property_filters": [ { "property": "Category", "operator": "=", "value": "Shoes" } ]
 },
 "deliverTo": { "operatingSystems": ["Android", "iOS"], "deviceTypes": ["Mobile"] },
 "query": {
 "all": [
 { "user_property": { "property": "Plan", "operator": "=", "value": "Gold" } },
 { "event": { "event_name": "Charged", "operator": "count >=", "value": 1 } }
 ]
 }
 },
 "what": {
 "messageType": "single",
 "message": {
 "type": "carousel",
 "content": [
 { "title": "Runners", "media": "https://cdn.example.com/nd/run.png",
 "action": { "url": { "android": "app://cat/run", "ios": "app://cat/run" } } },
 { "title": "Sneakers", "media": "https://cdn.example.com/nd/sneak.png",
 "action": { "url": { "android": "app://cat/sneak", "ios": "app://cat/sneak" } } }
 ]
 }
 },
 "when": {
 "dateTime": {
 "type": "userAction",
 "start": { "type": "now" },
 "end": { "type": "onDateTime", "date": "2026-07-31", "time": "11:59 PM" }
 },
 "deliveryPreferences": {
 "displaySchedule": {
 "MON": { "from": "09:00 AM", "to": "09:00 PM" },
 "TUE": { "from": "09:00 AM", "to": "09:00 PM" },
 "WED": { "from": "09:00 AM", "to": "09:00 PM" },
 "THU": { "from": "09:00 AM", "to": "09:00 PM" },
 "FRI": { "from": "09:00 AM", "to": "09:00 PM" }
 },
 "priority": 8
 }
 }
}
```
```json Split test
{
 "name": "Offer copy split",
 "channel": "NativeDisplay",
 "who": {
 "trigger": { "event": "App Launched" },
 "deliverTo": { "operatingSystems": ["Android", "iOS"], "deviceTypes": ["Mobile", "Tablet"] }
 },
 "what": {
 "messageType": "split",
 "variants": [
 { "name": "Variant A", "type": "simple",
 "content": [ { "title": "Offer A", "message": "Flat 40% off",
 "action": { "url": { "android": "app://offer/a", "ios": "app://offer/a" } } } ] },
 { "name": "Variant B", "type": "simple",
 "content": [ { "title": "Offer B", "message": "Buy one get one",
 "action": { "url": { "android": "app://offer/b", "ios": "app://offer/b" } } } ] }
 ],
 "distribution": { "Variant A": 50, "Variant B": 50 }
 },
 "when": { "dateTime": { "type": "userAction" } }
}
```
```json Manual A/B with conversion goal
{
 "name": "Headline A/B",
 "channel": "NativeDisplay",
 "who": {
 "trigger": { "event": "App Launched" },
 "deliverTo": { "operatingSystems": ["Android", "iOS"], "deviceTypes": ["Mobile", "Tablet"] },
 "controlGroup": [ { "type": "campaign", "size": 10 } ]
 },
 "what": {
 "messageType": "ab",
 "variants": [
 { "name": "Variant A", "type": "simple",
 "content": [ { "title": "Headline A", "message": "Version A copy",
 "action": { "url": { "android": "app://ab/a", "ios": "app://ab/a" } } } ] },
 { "name": "Variant B", "type": "simple",
 "content": [ { "title": "Headline B", "message": "Version B copy",
 "action": { "url": { "android": "app://ab/b", "ios": "app://ab/b" } } } ] }
 ],
 "abTest": { "mode": "manual", "waitTime": 45 }
 },
 "when": { "dateTime": { "type": "userAction" } },
 "conversionGoal": {
 "eventName": "Charged",
 "revenueProperty": "Amount",
 "conversionTime": { "value": 1, "unit": "days" }
 }
}
```
```json messageOnUserProperty with Liquid
{
 "name": "Tiered welcome",
 "channel": "NativeDisplay",
 "who": {
 "trigger": { "event": "App Launched" },
 "deliverTo": { "operatingSystems": ["Android", "iOS"], "deviceTypes": ["Mobile", "Tablet"] }
 },
 "what": {
 "messageType": "messageOnUserProperty",
 "userProperty": { "property": "Customer Type" },
 "variants": [
 { "value": "wzrk_default", "type": "simple",
 "content": [ { "title": "Welcome {{ Profile.Name | default: 'there' }}",
 "message": "Discover what's new",
 "action": { "url": { "android": "app://home", "ios": "app://home" } } } ] },
 { "value": "gold", "type": "simple",
 "content": [ { "title": "Hello Gold member", "message": "Your exclusive perks are ready",
 "action": { "url": { "android": "app://gold", "ios": "app://gold" } } } ] }
 ]
 },
 "when": { "dateTime": { "type": "userAction" } }
}
```
```json Custom key-value saved as draft
{
 "name": "Home config - draft",
 "channel": "NativeDisplay",
 "draft": true,
 "who": {
 "trigger": { "event": "App Launched", "first_time": true },
 "deliverTo": { "operatingSystems": ["Android"], "deviceTypes": ["Mobile", "Tablet", "TV"] }
 },
 "what": {
 "messageType": "single",
 "message": {
 "type": "custom-key-value",
 "content": [ { "title": "Config push" } ],
 "customKVData": [
 { "key": "promo_code", "value": { "text": "SAVE50" } },
 { "key": "banner_id", "value": { "text": "home_top" } }
 ]
 }
 },
 "when": { "dateTime": { "type": "userAction" } }
}
```
```json Advanced Builder
{
 "name": "Builder CTA",
 "channel": "NativeDisplay",
 "who": {
 "trigger": { "event": "App Launched" },
 "deliverTo": { "operatingSystems": ["Android", "iOS"], "deviceTypes": ["Mobile", "Tablet"] }
 },
 "what": {
 "messageType": "single",
 "message": {
 "type": "advanced-builder",
 "builderConfig": {
 "buttons": [
 { "id": "btn1", "text": "Shop now",
 "actions": { "type": "url", "android": { "text": "app://sale" }, "ios": { "text": "app://sale" } } }
 ]
 }
 }
 },
 "when": { "dateTime": { "type": "userAction" } }
}
```

# Example Response

The following are sample responses from the Create Campaign v2, Native Display API:

```json Success
{
 "result": "success",
 "data": { "campaignId": 1490871 },
 "status": "success"
}
```
```json Validation Failure
{
 "status": "fail",
 "error": "who.trigger.event is required for online campaigns, deliverTo.operatingSystems is required (select at least one operating system)",
 "errors": [
 { "code": "REQUIRED_FIELD_MISSING",
 "message": "who.trigger.event is required for online campaigns",
 "path": "$.who.trigger.event", "suggestions": null },
 { "code": "REQUIRED_FIELD_MISSING",
 "message": "deliverTo.operatingSystems is required (select at least one operating system)",
 "path": "$.who.deliverTo.operatingSystems", "suggestions": null }
 ]
}
```
```json Single-Reason Failure
{ "result": "failure", "reason": "Daily API campaign creation limit reached", "status": "fail" }
```

# Response Fields

The following table describes the success response fields:

| Field           | Type    | Description                                                                                                         |
| :-------------- | :------ | :------------------------------------------------------------------------------------------------------------------ |
| result          | string  | `"success"`                                                                                                         |
| data.campaignId | integer | The created campaign ID.                                                                                            |
| data.url        | string  | Dashboard URL where the campaign can be opened and edited. Present when the dashboard URL can be resolved, omitted. |
| status          | string  | `"success"`                                                                                                         |

When `"draft": true` is set, the campaign is created in `DRAFT` status. The response shape is unchanged. Campaigns created through this API are marked as API-created and can be edited in the dashboard.

# Error Codes

Two error body shapes are possible.

## Structured Validation Failure (400)

All validation errors in the request are collected and returned in a single response. The following fields are present:

| Field                 | Type           | Description                                                                                            |
| :-------------------- | :------------- | :----------------------------------------------------------------------------------------------------- |
| status                | string         | `"fail"`                                                                                               |
| error                 | string         | Semicolon-joined human-readable summary of every error.                                                |
| errors\[]             | array          | Structured per-field errors.                                                                           |
| errors\[].code        | string         | Stable machine-readable error code.                                                                    |
| errors\[].message     | string         | Human-readable description.                                                                            |
| errors\[].path        | string or null | JSON path of the offending element, for example `$.who.trigger.event`. Null when not element-specific. |
| errors\[].suggestions | array or null  | Closest valid values for a mistyped token. Empty or absent when not applicable.                        |

**Error codes**

The following table lists the error codes:

| Code                      | Meaning                                                                                                                                                                                                           |
| :------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `REQUIRED_FIELD_MISSING`  | A mandatory field is absent or blank.                                                                                                                                                                             |
| `INVALID_VALUE`           | Wrong type, or an unrecognized discriminator token. Carries `suggestions` where a close match exists.                                                                                                             |
| `FEATURE_NOT_ENABLED`     | The request uses a feature not enabled for this account. For more information, refer[ Account Feature Gates](doc:create-campaign-v2-in-app#account-feature-gates).                                                |
| `UNSUPPORTED_COMBINATION` | A `segmentType` × `when.dateTime.type` × `what.messageType` combination this channel does not support, for example, a non-`Live` `segmentType`, a non-`userAction` trigger, or automated A/B.                     |
| `INVALID_WHO`             | Semantic problem in the audience, an unresolvable event, property, or segment name, a restricted trigger event, an unsupported user profile filter rule or operator, a bad geo filter, or a disallowed `who` key. |
| `INVALID_WHAT`            | Semantic problem in the content, bad distribution, invalid A/B config, unknown pivot property, a disallowed Liquid tag namespace, malformed `personalizationConfig`, or variant parsing.                          |
| `INVALID_WHEN`            | Semantic problem in the display window or delivery preferences.                                                                                                                                                   |
| `INVALID_COMBINATION`     | An interdependency violation across `who`, `what`, and `when`.                                                                                                                                                    |

<Callout icon="⚠️" theme="warning">
  ### **Error Path Granularity**

  Not every error carries a precise path. Field-level validation returns precise JSON paths for each problem. Errors in schedule or delivery preference parsing report at the block root (`$.when` or `$.what`) without a sub-path, so only the first problem in that block is returned per request. `conversionGoal` reports only the first problem found in the block.
</Callout>

**Common Structured Errors**

The following table lists common structured errors:

| Code                      | Path                                         | Example Message                                                                                        |
| :------------------------ | :------------------------------------------- | :----------------------------------------------------------------------------------------------------- |
| `UNSUPPORTED_COMBINATION` | `$.segmentType`                              | `segmentType 'PBS' is not valid for the online channel NativeDisplay, it is always Live`               |
| `UNSUPPORTED_COMBINATION` | `$.when.dateTime.type`                       | `when.dateTime.type 'userInaction' is not supported with segmentType 'Live' for this channel`          |
| `UNSUPPORTED_COMBINATION` | `$.what.abTest.mode`                         | `Automated (continuous) A/B is not supported for NativeDisplay, use manual A/B (abTest.mode = manual)` |
| `REQUIRED_FIELD_MISSING`  | `$.who.trigger.event`                        | `who.trigger.event is required for online campaigns`                                                   |
| `INVALID_WHO`             | `$.who.trigger.event`                        | `'Notification Sent' is a system event and cannot be used as a trigger`                                |
| `INVALID_WHO`             | `$.who.trigger.property_filters[0].operator` | `Operator '>=' is not supported for a trigger event property filter`                                   |
| `REQUIRED_FIELD_MISSING`  | `$.who.deliverTo.deviceTypes`                | `deliverTo.deviceTypes is required (select at least one device type)`                                  |
| `INVALID_VALUE`           | `$.what.message.type`                        | `'banner' is not a supported Native Display template type`                                             |
| `INVALID_VALUE`           | `$.what.message.content.action.url`          | `what.message action.url must be an object`                                                            |
| `INVALID_WHAT`            | `$.what.abTest.waitTime`                     | `abTest.waitTime (conversion window in minutes) must be positive for online A/B`                       |
| `INVALID_WHAT`            | `$.what.distribution`                        | `distribution weights must sum to 100 (got 90)`                                                        |
| `REQUIRED_FIELD_MISSING`  | `$.what.variants`                            | `messageOnUserProperty requires a default backup variant (a variant with value "wzrk_default")`        |
| `INVALID_WHEN`            | `$.when`                                     | `deliveryPreferences.timeToLive is not supported for an online channel`                                |
| `INVALID_WHEN`            | `$.when`                                     | `when.dateTime.end must be after when.dateTime.start`                                                  |

Unrecognized tokens carry a _did you mean_ suggestion for: `channel`, `segmentType`, `when.dateTime.type`, `what.messageType`, `abTest.mode`, `when.dateTime.start.type`/`end.type`, weekday tokens, and `deliveryPreferences` keys.

## Single-Reason Failure

Used for failures raised after validation passes. These carry no `errors[]`, no `code`, and no `path`.

```json
{ "result": "failure", "reason": "<reason>", "status": "fail" }
```

The following table lists the single-reason failure cases:

| HTTP Status | reason                                                           | Cause                                                                                |
| :---------- | :--------------------------------------------------------------- | :----------------------------------------------------------------------------------- |
| 402         | `"Daily API campaign creation limit reached"`                    | Daily API creation cap hit. The actual cap on your account may be lower than 50,000. |
| 402         | `"Monthly API campaign creation limit reached"`                  | Monthly API creation cap hit.                                                        |
| 400         | `"Campaign creation limit reached for your current plan"`        | Plan campaign limit hit.                                                             |
| 409         | `"A similar campaign was just created, please retry"`            | A near-identical campaign was created moments earlier. Retry after a short delay.    |
| 400         | `"Campaign could not be saved"`                                  | Save rejected with no more specific message.                                         |
| 400         | `"Invalid request body"`                                         | Body is not a parseable JSON.                                                        |
| 501         | `"channel 'Email' is not yet available on Campaign API v2"`      | Channel has no v2 pipeline.                                                          |
| 501         | `"who.prompt is not supported for NativeDisplay Live campaigns"` | `who.prompt` sent.                                                                   |
| 500         | `"Failed to persist campaign"`                                   | Validation passed, but the save failed. Safe to retry.                               |

# Account Feature Gates

The following features are only available when the corresponding account setting is enabled. Using one while disabled returns `FEATURE_NOT_ENABLED` with the offending path.

The following table lists the account feature gates:

| Feature                    | Request Trigger                              | Behavior                                                                                                                                                                                        |
| :------------------------- | :------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Personalization validation | Any `{{ Profile.* }}` or `{{ Event.* }}` tag | Profile and Event property validation applies when personalization validation is enabled for the account. A malformed Liquid tag is always rejected.                                            |
| Super device properties    | `device_fields` rule in `who.query`          | Available only when super device properties are enabled for the account. Otherwise `INVALID_WHO`.                                                                                               |
| Automated (continuous) A/B | `what.abTest.mode = automated`               | Not supported for Native Display. Accounts without the feature, check `FEATURE_NOT_ENABLED`, accounts with it, check  `UNSUPPORTED_COMBINATION`. Both return the error on `$.what.abTest.mode`. |

# Cross-Block Rules

The following rules span `who`, `what`, and `when` are evaluated together. The following table lists the cross-block rules:

| Rule                                                                                        | Rejection                                                                                                    |
| :------------------------------------------------------------------------------------------ | :----------------------------------------------------------------------------------------------------------- |
| Native Display is always `Live` × `userAction`                                              | `UNSUPPORTED_COMBINATION` on `$.segmentType` or `$.when.dateTime.type`.                                      |
| Online A/B needs a positive conversion window                                               | `INVALID_WHAT` on `$.what.abTest.waitTime`.                                                                  |
| Automated A/B is not available for an online channel                                        | `UNSUPPORTED_COMBINATION` on `$.what.abTest.mode`.                                                           |
| `Event.*` personalization resolves from the trigger event                                   | Each referenced event property must exist on `who.trigger.event` when personalization validation is enabled. |
| Send-side delivery preferences and timezone fields are meaningless for a displayed campaign | `INVALID_WHEN` on `$.when` with the messages listed in the error codes table.                                |

# Not Available for Native Display

The following table lists features and fields that are not supported for Native Display and are rejected if sent:

| Area                                     | Not Supported                                                                                                                                                                                                                                                                                                                                                                     |
| :--------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `segmentType`                            | `PBS`, `External Trigger`                                                                                                                                                                                                                                                                                                                                                         |
| `when.dateTime`                          | `sendNow`, `scheduleLater`, `recurring`, `userInaction`, `dateTime`, `externalTrigger`, `delay`, `inactionDuration`, `timezone`, `adHocTimezone`, `userTimezoneWrapAround`, `timeSource: userProperty`                                                                                                                                                                            |
| `when.deliveryPreferences`               | `doNotDisturb`, `timeToLive`, `limits`, `globalMessageLimits`, `throttle`, `cutOff`                                                                                                                                                                                                                                                                                               |
| `who`                                    | `exclusion`, `inaction`, `trigger_property`, `prompt`                                                                                                                                                                                                                                                                                                                             |
| `who.query` rules                        | `events_group`, `interest`, `multi_app`, `geo_radius`, `demographic`, `technographics`, `reachability`, `subscription_groups`, event `property_filters`/`session_filters`/`geo_filters`, `count <`, `count <=`, `count !=`, `count >= 0`, absolute/predefined `when` windows, geographic `notIn`, `user_property` `>=`, `<=`, `in`, `notIn`, `notContains`, `exists`, `notExists` |
| `who.trigger.property_filters` operators | `>=`, `<=`                                                                                                                                                                                                                                                                                                                                                                        |
| `what`                                   | `ab` with `mode: automated`, `platformSpecific` (Push-only)                                                                                                                                                                                                                                                                                                                       |
