---
title: Get Events
excerpt: Lets you download user events from CleverTap.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

Get Events API allows you to export user events in two steps. 

1. Request a cursor by specifying the event name, date range, and batch size. 
2. Page through results using the cursor until no `next_cursor` is returned.

<Callout icon="📘" theme="info">
  #### Note

  The `batch_size` parameter indicates the maximum number of records returned per call. Actual batch sizes may be smaller.
</Callout>

The following events aren't exported by this endpoint: _Notification Sent, Notification Bounce, Notification Control Group, Notification Rendered, Notification System Control Group_. Use [S3 Export](https://docs.clevertap.com/docs/data-export-to-aws-s3) or [GCP Export](https://docs.clevertap.com/docs/data-export-to-gcp) to export these events.

# Step 1: Request a Cursor

Use this endpoint to obtain a cursor for the specified event and date range. The cursor is then used in Step 2 to fetch records in batches.

## Base URL

Here is an example base URL for the India region.

``https://in1.api.clevertap.com/1/events.json``

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

The following headers are required for this request.

| Header                   | Description                       | Example            |
| ------------------------ | --------------------------------- | ------------------ |
| `X-CleverTap-Account-Id` | Your CleverTap Account ID.        | `<ACCOUNT_ID>`     |
| `X-CleverTap-Passcode`   | Your CleverTap Account Passcode.  | `<PASSCODE>`       |
| `Content-Type`           | Always set to `application/json`. | `application/json` |

For more information, refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers).

## Query Parameters

These optional query parameters belong in the URL, not the JSON body. The table below describes each parameter, along with its type, default value, and example value.

| Parameter    | Description                                                                                                                                                                                                                                                                  | Type    | Default | Example |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ------- | ------- |
| `batch_size` | Maximum number of records per batch. This value in Step 1 affects paging in Step 2. Maximum allowed value: 5000.                                                                                                                                                             | integer | 500     | `50`    |
| `app`        | Include app and device fields in the `profile` object. When set to`true`, the response adds `os_version`, `app_version`, `make`, and `model` as flat fields directly on the `profile` object, alongside fields like `name` and `email`.                                      | boolean | `false` | `true`  |
| `events`     | Include event summary fields in the `records[].events` object. When set to`false`, the `events` key is omitted entirely from the response. It is not returned as an empty object or null.                                                                                    | boolean | `true`  | `false` |
| `profile`    | Include custom profile properties in `profile.profileData`. When set to `false`, the `profileData` sub-object is omitted, but `name`, `email`, `phone`, `identity`, `all_identities`, `objectId`, and `platform` are always present in `profile` regardless of this setting. | boolean | `true`  | `false` |

## Body Parameters

These parameters must be sent in the JSON body. The table below describes each parameter along with its type, requirement status, and example value.

| Parameter    | Description                                                                                                                                                                                       | Type    | Required | Example          |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | -------- | ---------------- |
| `event_name` | Event type to export. Supports standard events (for example, _App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Product Viewed, UTM Visited_) and custom events.        | string  | Yes      | `"App Launched"` |
| `from`       | Start of the date range (inclusive) in `YYYYMMDD` format. The value must not be a future date, and `from` must be less than or equal to `to`. Violating either condition returns error code 4002. | integer | Yes      | `20171201`       |
| `to`         | End of the date range (inclusive) in `YYYYMMDD` format. Follows the same conditions as `from`.                                                                                                    | integer | Yes      | `20171225`       |

<Callout icon="📘" theme="info">
  #### Date Formats

  * Input (request body): `YYYYMMDD` (for example, `20171201`)
  * Output (`records[].ts`): `yyyyMMddHHmmSS`, expressed in the account's configured timezone (for example, `20151023140416`)
</Callout>

## Example Request

The following is the sample request:

```curl
curl -X POST 'https://in1.api.clevertap.com/1/events.json?batch_size=50&app=true&events=true&profile=true' \
  -H 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \
  -H 'X-CleverTap-Passcode: <PASSCODE>' \
  -H 'Content-Type: application/json' \
  -d '{ "event_name":"App Launched", "from":20171201, "to":20171225 }'
```
```python
import requests

url = "https://in1.api.clevertap.com/1/events.json"
params = {"batch_size": 50, "app": True, "events": True, "profile": True}
headers = {
    "X-CleverTap-Account-Id": "<ACCOUNT_ID>",
    "X-CleverTap-Passcode": "<PASSCODE>",
    "Content-Type": "application/json"
}
body = {"event_name": "App Launched", "from": 20171201, "to": 20171225}

r = requests.post(url, params=params, json=body, headers=headers)
print(r.json())
```
```javascript
const axios = require("axios");

const url = "https://in1.api.clevertap.com/1/events.json";
const params = { batch_size: 50, app: true, events: true, profile: true };
const headers = {
    "X-CleverTap-Account-Id": "<ACCOUNT_ID>",
    "X-CleverTap-Passcode": "<PASSCODE>",
    "Content-Type": "application/json"
};
const data = { event_name: "App Launched", from: 20171201, to: 20171225 };

axios.post(url, data, { params, headers })
    .then(res => console.log(res.data))
    .catch(err => console.error(err.response ? err.response.data : err.message));
```
```ruby
require "net/http"
require "json"
require "uri"

uri = URI("https://in1.api.clevertap.com/1/events.json")
uri.query = URI.encode_www_form(batch_size: 50, app: true, events: true, profile: true)

req = Net::HTTP::Post.new(uri)
req["X-CleverTap-Account-Id"] = "<ACCOUNT_ID>"
req["X-CleverTap-Passcode"] = "<PASSCODE>"
req["Content-Type"] = "application/json"
req.body = { event_name: "App Launched", from: 20171201, to: 20171225 }.to_json

res = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) { |http| http.request(req) }
puts res.body
```
```php
<?php
$ch = curl_init("https://in1.api.clevertap.com/1/events.json?batch_size=50&app=true&events=true&profile=true");
curl_setopt_array($ch, [
    CURLOPT_POST => true,
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER => [
        "X-CleverTap-Account-Id: <ACCOUNT_ID>",
        "X-CleverTap-Passcode: <PASSCODE>",
        "Content-Type: application/json"
    ],
    CURLOPT_POSTFIELDS => json_encode([
        "event_name" => "App Launched",
        "from" => 20171201,
        "to" => 20171225
    ])
]);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

## Example Response

A successful Step 1 request always returns a cursor, even if the specified date range contains no events. The following is a sample successful request:

```json
{
  "status": "success",
  "cursor": "ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6"
}
```

# Step 2: Fetch Batches Using Cursor

Use the cursor received from Step 1 to fetch event records in batches. Keep calling this endpoint until the response no longer includes `next_cursor`. When there are no further batches, the `next_cursor` field is omitted entirely from the response rather than returned as null.

> 📘 **Cursor Expiry**
>
> A cursor expires 4 hours after creation or after 1 hour of inactivity, whichever comes first. A cleanup process removes expired cursors approximately every 10 minutes. Once a cursor expires, using it returns HTTP 400 with a status of `CURSOR_INVALIDATED`. There is no grace period, so you must restart from Step 1 to obtain a new cursor.

> 📘 **Zero matching records**
>
> If Step 1 returns a cursor but the specified date range contains no events, the first Step 2 call for that cursor returns `{"status": "success"}` with no `records` key. This response is identical to the end-of-pagination response, so there is no way to distinguish "no events ever existed in this range" from "all batches have already been fetched."

## Base URL

Here is an example base URL for the India region.

```
https://in1.api.clevertap.com/1/events.json
```

For more information, refer to [Region](https://developer.clevertap.com/docs/get-events-api#region).

## HTTP Method

GET

## Headers

The following headers are required for this request.

| Header                   | Description                      | Example        |
| ------------------------ | -------------------------------- | -------------- |
| `X-CleverTap-Account-Id` | Your CleverTap Account ID.       | `<ACCOUNT_ID>` |
| `X-CleverTap-Passcode`   | Your CleverTap Account Passcode. | `<PASSCODE>`   |

For more information, refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers).

## Request Parameters

The table below describes the single required parameter for this request.

| Parameter | Description                                                 | Type   | Required | Example     |
| --------- | ----------------------------------------------------------- | ------ | -------- | ----------- |
| `cursor`  | Returned from the prior response. Points to the next batch. | string | Yes      | `ZyZjfw...` |

## Example Request

The following is the sample request:

```curl cURL
curl 'https://in1.api.clevertap.com/1/events.json?cursor=CURSOR' \
  -H 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \
  -H 'X-CleverTap-Passcode: <PASSCODE>'
```
```python
import requests

url = "https://in1.api.clevertap.com/1/events.json"
params = {"cursor": "CURSOR"}
headers = {
    "X-CleverTap-Account-Id": "<ACCOUNT_ID>",
    "X-CleverTap-Passcode": "<PASSCODE>"
}
r = requests.get(url, params=params, headers=headers)
print(r.json())
```
```javascript
const axios = require("axios");

const url = "https://in1.api.clevertap.com/1/events.json";
const params = { cursor: "CURSOR" };
const headers = {
    "X-CleverTap-Account-Id": "<ACCOUNT_ID>",
    "X-CleverTap-Passcode": "<PASSCODE>"
};

axios.get(url, { params, headers })
    .then(res => console.log(res.data))
    .catch(err => console.error(err.response ? err.response.data : err.message));
```
```ruby
require "net/http"
require "uri"

uri = URI("https://in1.api.clevertap.com/1/events.json")
uri.query = URI.encode_www_form(cursor: "CURSOR")

req = Net::HTTP::Get.new(uri)
req["X-CleverTap-Account-Id"] = "<ACCOUNT_ID>"
req["X-CleverTap-Passcode"] = "<PASSCODE>"

res = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) { |http| http.request(req) }
puts res.body
```
```php
<?php
$url = "https://in1.api.clevertap.com/1/events.json?cursor=CURSOR";
$ch = curl_init($url);
curl_setopt_array($ch, [
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER => [
        "X-CleverTap-Account-Id: <ACCOUNT_ID>",
        "X-CleverTap-Passcode: <PASSCODE>"
    ]
]);
$response = curl_exec($ch);
curl_close($ch);
echo $response;
```

## Example Response (with more data)

If more events are available, the response includes `next_cursor`, as shown below.

```json
{
  "status": "success",
  "next_cursor": "ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6",
  "records": [
    {
      "profile": {
        "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097",
        "platform": "Web",
        "email": "peter@foo.com",
        "profileData": { "favoriteColor": "blue" },
        "identity": "5555555555"
      },
      "events": {
        "Identity Set": { "count": 1, "first_seen": 1677155829, "last_seen": 1677155829 },
        "Test event": { "count": 2, "first_seen": 1677155653, "last_seen": 1677155726 }
      },
      "ts": 20151023140416,
      "event_props": { "value": "pizza" },
      "session_props": {
        "utm_source": "Foo",
        "utm_medium": "Bar",
        "utm_campaign": "FooBar",
        "session_source": "Direct"
      }
    }
  ]
}
```

If there are no more events, the response omits `next_cursor`, as shown below.

```json
{ "status": "success" }
```

If the data is not yet ready, the response indicates that the request is still in progress, as shown below.

```json
{
  "status": "fail",
  "error": "Request still in progress, please retry later",
  "code": 2
}
```

# Event Object Schema

Use this schema to parse objects in `records[]`. The table below describes each key, its type, and its description.

| Key                        | Type    | Description                                                                                                                                                                                                                                                                                      |
| -------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `profile`                  | object  | User identity and custom profile properties. When `app=true`, this object also includes `os_version`, `app_version`, `make`, and `model` as flat fields.                                                                                                                                         |
| `profile.objectId`         | string  | CleverTap Global Object ID for the user. This is the user identifier returned by this API; there is no separate numeric `profile.id` field.                                                                                                                                                      |
| `profile.platform`         | string  | Source platform (for example, `Web`, `Android`, `iOS`).                                                                                                                                                                                                                                          |
| `profile.email`            | string  | User email, if available.                                                                                                                                                                                                                                                                        |
| `profile.profileData`      | object  | Custom profile properties (key-value map). Omitted when `profile=false`.                                                                                                                                                                                                                         |
| `profile.identity`         | string  | Your custom identity for the user, if set.                                                                                                                                                                                                                                                       |
| `events`                   | object  | Event summary keyed by event name: `{ "<eventName>": { "count": integer, "first_seen": integer, "last_seen": integer } }`. Omitted entirely when `events=false`.                                                                                                                                 |
| `events.<name>.count`      | integer | Occurrence count within the requested window.                                                                                                                                                                                                                                                    |
| `events.<name>.first_seen` | integer | Unix timestamp (seconds) when first seen.                                                                                                                                                                                                                                                        |
| `events.<name>.last_seen`  | integer | Unix timestamp (seconds) when last seen.                                                                                                                                                                                                                                                         |
| `ts`                       | integer | A 14-digit integer in `yyyyMMddHHmmSS` format, expressed in the account's configured timezone, not UTC or the server or browser timezone. For example, `20241215142305` represents December 15, 2024, 14:23:05 in account time. This value is not a Unix timestamp and does not follow ISO 8601. |
| `event_props`              | object  | Event properties as a flat key-value map. Values are scalars only: string, integer, float, a date string in `yyyyMMdd` format, or a JSON array of strings for multi-value properties. Values are never nested objects or nested arrays.                                                          |
| `session_props`            | object  | UTM and session metadata, for example `utm_source`, `utm_medium`, `utm_campaign`, `session_source`, `session_referrer`.                                                                                                                                                                          |
| `session_source`           | string  | Always one of `Direct`, `Search`, `Social`, `Email`, `Batch`, `Others`, or `Unavailable`. If a custom referrer is present, this value is `Others` and the actual referrer appears in `session_referrer`. If unavailable, the value is `Unavailable`.                                             |

# Error Codes

The following table lists the error codes returned by this API, along with their HTTP status codes, descriptions, and example error responses.

| HTTP Status | Code or Status       | Error                 | Description                                                                                                                         | Example Error Response                                                                      |
| ----------- | -------------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| 401         | 401                  | `invalid_credentials` | Authentication failure. Invalid or missing credentials.                                                                             | `{ "status": "fail", "error": "Invalid credentials", "code": 401 }`                         |
| 400         | 4001                 | `invalid_event_name`  | Event type not recognized.                                                                                                          | `{ "status": "fail", "error": "Invalid event_name provided", "code": 4001 }`                |
| 400         | 4002                 | `invalid_date_range`  | Invalid date range (bad format, `from` greater than `to`, or a future date).                                                        | `{ "status": "fail", "error": "Invalid date range format YYYYMMDD", "code": 4002 }`         |
| 400         | 4003                 | `invalid_batch_size`  | `batch_size` too large or not an integer.                                                                                           | `{ "status": "fail", "error": "batch_size must be an integer <= 5000", "code": 4003 }`      |
| 400         | `CURSOR_INVALIDATED` | `invalid_cursor`      | Cursor expired (4 hours from creation or 1 hour of inactivity) or otherwise invalid. There is no grace period; restart from Step 1. | `{ "status": "fail", "error": "Invalid cursor", "code": 4004 }`                             |
| 202         | 2                    | `in_progress`         | Request still processing, retry later.                                                                                              | `{ "status": "fail", "error": "Request still in progress, please retry later", "code": 2 }` |
| 429         | 429                  | `rate_limit_exceeded` | API rate limit exceeded.                                                                                                            | `{ "status": "fail", "error": "Rate limit exceeded. Try again later", "code": 429 }`        |
| 500         | 500                  | `internal_error`      | Internal server error.                                                                                                              | `{ "status": "fail", "error": "Internal server error. Please retry later", "code": 500 }`   |

# Notes

This section summarizes key behaviors of the export flow.

* One event per request: each Step 1 request targets a single `event_name`.
* Response evolution: Step 1 returns `{ cursor, status }`. Step 2 returns `{ status, records, next_cursor? }`.
* For endpoint limits, see [API Request Limit](doc:api-request-limit).
* For standard errors, see [API Errors](doc:api-errors).

<br />
