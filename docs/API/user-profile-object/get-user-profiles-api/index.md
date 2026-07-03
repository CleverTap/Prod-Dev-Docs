---
title: Get User Profiles
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
# Overview

Use the Get User Profiles API to fetch detailed user profile information, profile properties, event summaries, and platform metadata. You can retrieve profiles by [event ](doc:get-user-profiles-api#get-user-profiles-by-events) using a cursor-based flow or by [user ID](doc:get-user-profiles-api#download-user-profiles-by-id). Use the same region-based URL across the entire workflow.

# Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/profiles.json`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region). The following table shows common region prefixes.

| Region    | Base URL                        |
| :-------- | :------------------------------ |
| India     | `https://in1.api.clevertap.com` |
| US        | `https://us1.api.clevertap.com` |
| Singapore | `https://sg1.api.clevertap.com` |
| Europe    | `https://eu1.api.clevertap.com` |

# Authentication

All requests require authentication headers. POST requests also require `Content-Type: application/json`. Do not include `Content-Type` on GET requests.

The following table lists the required headers.

| Header                 | Required  | Description                                                                                   |
| :--------------------- | :-------- | :-------------------------------------------------------------------------------------------- |
| X-CleverTap-Account-Id | Yes       | Your CleverTap Account ID (for example, TEST-ABC-123).                                        |
| X-CleverTap-Passcode   | Yes       | The CleverTap Account Passcode is associated with your Account ID.                            |
| Content-Type           | POST only | `application/json`. Required only on POST requests in Step 1. Do not include on GET requests. |

# Get User Profiles by Event

This two-step flow lets you pull profiles of users who performed a specific event in a date range. Step 1 submits the event and date filter via POST (a request body is required for filtering) and returns a cursor. Step 2 uses GET requests with that cursor to page through the matching profiles.

## Step 1: Get a Cursor

This step returns a cursor to page through profiles that match your event and date filters.

### Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/profiles.json`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

### HTTP Method

POST

### Request Body

The following table lists the required body parameters. Provide the event name and date window to filter users.

| Parameter  | Required | Description                                                                                                                                                                                                     | Type    | Format   | Example  |
| :--------- | :------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | :------- | :------- |
| event_name | Yes      | Event type, standard or custom. Standard events include but are not limited to: App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Notification Sent, Product Viewed, and UTM Visited. | string  | —        | Charged  |
| from       | Yes      | Start of the date range. Must be an integer, not a string. This date is inclusive.                                                                                                                              | integer | YYYYMMDD | 20171201 |
| to         | Yes      | End of the date range. Must be an integer, not a string. This date is inclusive and must be greater than or equal to `from`.                                                                                    | integer | YYYYMMDD | 20171225 |

### Query Parameters

The following table lists the optional query parameters. Use these to control the response shape and pagination.

| Parameter  | Description                                                                                                                                                                      | Type    | Default | Example |
| :--------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | :------ | :------ |
| batch_size | Maximum number of records to return per call. Values up to 5000 are supported.                                                                                                   | integer | —       | 5000    |
| app        | When true, includes app fields in profile platform info (OS version, device make and model, app version). When false, these fields are omitted.                                  | boolean | false   | true    |
| events     | When true, includes event summary fields inside the events object (count, first_seen, last_seen per event). When false, the events key is omitted entirely from profile objects. | boolean | true    | true    |
| profile    | When true, includes custom profile properties in profileData. When false, only system fields are returned.                                                                       | boolean | true    | true    |

Boolean parameters are case-insensitive. Any value other than `false` is treated as `true` (for example, `?app=true&events=false`).

<Callout icon="📘" theme="info">
  **Batch size**

  Choose a size that balances throughput and memory. Values up to 5000 are supported.
</Callout>

#### Example Request

The following is a sample Step 1 request showing the headers required to authenticate it:

```curl
curl -X POST 'https://<region>.api.clevertap.com/1/profiles.json?batch_size=5000&app=true&events=true&profile=true' \
  -H 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \
  -H 'X-CleverTap-Passcode: <PASSCODE>' \
  -H 'Content-Type: application/json' \
  -d '{ "event_name": "App Launched", "from": 20171201, "to": 20171225 }'
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/profiles.json?batch_size=5000&app=true&events=true&profile=true")
req = Net::HTTP::Post.new(uri)
req["X-CleverTap-Account-Id"] = "<ACCOUNT_ID>"
req["X-CleverTap-Passcode"]   = "<PASSCODE>"
req["Content-Type"]           = "application/json"
req.body = { event_name: "App Launched", from: 20171201, to: 20171225 }.to_json
res = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) { |http| http.request(req) }
puts res.body
```
```python
import requests

url = "https://<region>.api.clevertap.com/1/profiles.json"
params = {"batch_size": 5000, "app": "true", "events": "true", "profile": "true"}
headers = {
  "X-CleverTap-Account-Id": "<ACCOUNT_ID>",
  "X-CleverTap-Passcode": "<PASSCODE>",
  "Content-Type": "application/json"
}
payload = {"event_name": "App Launched", "from": 20171201, "to": 20171225}
r = requests.post(url, params=params, headers=headers, json=payload, timeout=60)
print(r.json())
```
```javascript Node.js
const axios = require("axios");

axios.post(
  "https://<region>.api.clevertap.com/1/profiles.json",
  { event_name: "App Launched", from: 20171201, to: 20171225 },
  {
    params: { batch_size: 5000, app: true, events: true, profile: true },
    headers: {
      "X-CleverTap-Account-Id": "<ACCOUNT_ID>",
      "X-CleverTap-Passcode": "<PASSCODE>",
      "Content-Type": "application/json",
    },
    timeout: 60000
  }
).then(res => console.log(res.data)).catch(err => {
  console.error(err?.response?.data || err.message);
});
```
```php
<?php
$ch = curl_init("https://<region>.api.clevertap.com/1/profiles.json?batch_size=5000&app=true&events=true&profile=true");
curl_setopt_array($ch, [
  CURLOPT_POST => true,
  CURLOPT_HTTPHEADER => [
    "X-CleverTap-Account-Id: <ACCOUNT_ID>",
    "X-CleverTap-Passcode: <PASSCODE>",
    "Content-Type: application/json"
  ],
  CURLOPT_POSTFIELDS => json_encode([
    "event_name" => "App Launched",
    "from" => 20171201,
    "to" => 20171225
  ]),
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_TIMEOUT => 60
]);
echo curl_exec($ch);
curl_close($ch);
```

Set a 60-second request timeout for all API calls to handle long response times.

#### Example Response

The following is a sample response for Step 1. A successful request returns HTTP 200.

```json
{
  "cursor": "AfljfgIJBgBnamF5Kz8NegcBAwxhbCe%2Fbmhhe04BBAVlYjT4YG5reQEATQQrai57K2oue04FAUhnd38%3D",
  "status": "success"
}
```

The following table describes the cursor response schema.

| Field  | Type   | Description                                                 |
| :----- | :----- | :---------------------------------------------------------- |
| status | string | Always "success" for successful requests.                   |
| cursor | string | Opaque pagination token to use in Step 2. Valid for 4 days. |

<mark>FLAG: Cursor TTL of 4 days is confirmed from source code. Verify this value can be stated publicly before publish, or align with the preferred externally-communicated value.</mark>

<Callout icon="⚠️" theme="warning">
  **Cursors are opaque tokens**

  Do not attempt to decode, modify, or store cursors long-term. Use them only for immediate pagination. Cursors expire after 4 days. Each cursor is single-threaded; using the same cursor from multiple processes simultaneously may cause out-of-order requests that fail unrecoverably, requiring you to restart from Step 1.
</Callout>

## Step 2: Get Profiles Using Cursor

Use the cursor from Step 1 to page through results. If `next_cursor` is present in the response, request again with that value until it stops appearing.

Your Step 1 query parameters (`app`, `events`, `profile`) are stored server-side and associated with the cursor. Do not pass these parameters again in Step 2 requests. Only the `cursor` parameter is needed.

### Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/profiles.json?cursor=<CURSOR>`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

### HTTP Method

GET

### Headers

Use the same authentication headers as Step 1. Do not include `Content-Type` on GET requests.

### Query Parameter

The following table lists the required query parameters for Step 2:

| Parameter | Required | Description                                                    | Type   | Example                                                          |
| :-------- | :------- | :------------------------------------------------------------- | :----- | :--------------------------------------------------------------- |
| cursor    | Yes      | Opaque token from Step 1 or from the previous Step 2 response. | string | ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQ... |

### Example Request

The following is a sample request for Step 2:

```curl
curl 'https://<region>.api.clevertap.com/1/profiles.json?cursor=<CURSOR>' \
  -H 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \
  -H 'X-CleverTap-Passcode: <PASSCODE>'
```
```python
import requests

r = requests.get(
  "https://<region>.api.clevertap.com/1/profiles.json",
  params={"cursor": "<CURSOR>"},
  headers={
    "X-CleverTap-Account-Id": "<ACCOUNT_ID>",
    "X-CleverTap-Passcode": "<PASSCODE>"
  },
  timeout=60
)
print(r.json())
```

The pattern for Ruby, JavaScript, and PHP is identical to Step 1: make a GET request with only the `cursor` query parameter and the two auth headers. Omit `Content-Type`.

If a request times out, retry with the same cursor. The cursor retains its position and does not reset on timeout.

### Example Response

The following is a sample response for Step 2. A successful request returns HTTP 200.

```json
{
  "status": "success",
  "next_cursor": "ZiZjNwMEBQBkaWZzY2MuO20BBQFlYmN7ZG5lewYBTQVjb2BzZmphfwEABQUra2Jmeg%3D%3D",
  "records": [
    {
      "identity": "5555555555",
      "profileData": {
        "favoriteFood": "pizza",
        "MSG-email": true,
        "subscription-groups": {
          "email": {
            "tech news": true,
            "marketing": false,
            "service updates": true
          }
        }
      },
      "events": {
        "App Launched": {
          "count": 10,
          "first_seen": 1701388800,
          "last_seen": 1704067200
        },
        "Charged": {
          "count": 6,
          "first_seen": 1701475200,
          "last_seen": 1704153600
        }
      },
      "platformInfo": [
        {
          "platform": "iOS",
          "os_version": "17.1",
          "app_version": "6.1.3",
          "make": "Apple",
          "model": "iPhone15,2",
          "push_token": "95f98af6ad9a5e7...a3",
          "objectId": "-1a063854f83a4c6484285039ecff87cb"
        },
        { "platform": "Web", "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097" }
      ]
    },
    {
      "identity": "6666666666",
      "events": {
        "App Launched": {
          "count": 2,
          "first_seen": 1703980800,
          "last_seen": 1704067200
        }
      },
      "platformInfo": []
    }
  ]
}
```

### Pagination Response Schema

The following table describes the pagination response schema.

| Field       | Type   | Required | Description                                                                                   |
| :---------- | :----- | :------- | :-------------------------------------------------------------------------------------------- |
| status      | string | Yes      | Always "success" for successful requests.                                                     |
| records     | array  | No       | Array of user profile objects. Present when data is available. See User Profile Object below. |
| next_cursor | string | No       | Pagination token for the next page. When absent, no more records are available.               |

The following describes the pagination flow:

* Page with more data: response includes a `records` array and `next_cursor`. Use `next_cursor` to fetch the next page.
* Final page with data: response includes a `records` array and no `next_cursor`. This is the last page of data.
* No more data: response contains `{"status": "success", "records": []}` with no `next_cursor`.

Continue pagination while `next_cursor` is present in the response.

### User Profile Object

Fields that are unavailable are omitted from the response entirely, not returned as null. Always check for the existence of a field before accessing its value. Fields marked Conditional below may be absent.

System-defined fields use snake_case (for example, `first_seen`, `last_seen`, `os_version`). User-defined custom properties in `profileData` follow your own naming convention.

The following table describes the user profile object schema.

| Key          | Required    | Description                                                                                                                                                                                                                                                                                                                                                                 |
| :----------- | :---------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| email        | Conditional | User's email address. Omitted if not set.                                                                                                                                                                                                                                                                                                                                   |
| identity     | Conditional | The unique identifier you assigned to this user via the SDK (for example, user ID, phone number, email, or UUID). Omitted if not set.                                                                                                                                                                                                                                       |
| name         | Conditional | User's name. Omitted if not set.                                                                                                                                                                                                                                                                                                                                            |
| profileData  | Conditional | Object containing custom profile properties. Omitted when the user has no custom properties. May include channel preference flags (for example, `MSG-email: true`) and subscription groups under the `subscription-groups` key. Subscription groups are keyed by channel type (Email, SMS, Push, WhatsApp) with nested group names mapped to opt-in status (true or false). |
| events       | Conditional | Object keyed by event name. Each event contains: `count` (integer, total events across user lifetime, not limited to the query date range), `first_seen` (UNIX timestamp in seconds), `last_seen` (UNIX timestamp in seconds). Present when `events=true` was used in Step 1; omitted when `events=false`.                                                                  |
| platformInfo | Yes         | An array of platform entries. Always present, and may be an empty array `[]` if the user has no registered platforms. Each entry contains: `platform` (string), `objectId` (unique CleverTap identifier for that platform instance). App-specific fields appear only for mobile platforms: `push_token`, `app_version`, `os_version`, `make`, `model`.                      |

<Callout icon="📘" theme="info">
  **Event summary fields**

  When `events=true` is set in Step 1 query parameters, each event in the `events` object contains `count`, `first_seen`, and `last_seen` fields. The `count` value represents the user's lifetime total for that event, not the count within the query date range.
</Callout>

#### Platform Info Details

Each entry in the `platformInfo` array represents a device or platform where the user has interacted with your app.

The following table describes the platform info fields.

| Field       | Type   | Platforms    | Description                                                                                                                                                                               |
| :---------- | :----- | :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| platform    | string | All          | Platform type. Valid values: "iOS", "Android", "Web".                                                                                                                                     |
| objectId    | string | All          | Unique CleverTap identifier for this platform instance.                                                                                                                                   |
| push_token  | string | iOS, Android | Device push notification token. Present only for mobile platforms with push enabled.                                                                                                      |
| app_version | string | iOS, Android | Version of your app installed on this device (for example, 6.1.3). Present only for mobile platforms.                                                                                     |
| os_version  | string | iOS, Android | Operating system version (for example, 17.1). Present only for mobile platforms.                                                                                                          |
| make        | string | iOS, Android | Device manufacturer (for example, "Apple", "Samsung"). Present only for mobile platforms.                                                                                                 |
| model       | string | iOS, Android | Device model identifier. iOS uses internal model IDs (for example, "iPhone15,2", not "iPhone 15 Pro"). Android uses manufacturer-specific identifiers. Present only for mobile platforms. |

Platforms appear in no guaranteed order. Always check the `platform` field explicitly rather than relying on array position. Web platforms include only `platform` and `objectId` fields.

## Download User Profile by ID

Use this endpoint to fetch a single user profile by email, identity, or objectId.

### Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/profile.json`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

### HTTP Method

GET

### Query Parameters

Always URL-encode parameter values. The following characters require encoding: `+` (%2B), `@` (%40), `#` (%23), `&` (%26), space (%20), `/` (%2F).

Provide exactly one of the following parameters per request. If multiple parameters are provided, the API returns a 400 error.

The following table lists the accepted query parameters.

| Parameter | Description                                                   | Type   | Example (URL-encoded)            |
| :-------- | :------------------------------------------------------------ | :----- | :------------------------------- |
| email     | User's email address.                                         | string | jack%2Bvip%40gmail.com           |
| identity  | The unique identifier you assigned to the user.               | string | 5555555555                       |
| objectId  | Unique CleverTap identifier for a specific platform instance. | string | 1a063854f83a4c6484285039ecff87cb |

### Example Request

The following is a sample request to the Download User Profile by ID endpoint.

```curl
# email with '+' and '@' must be URL-encoded
curl 'https://<region>.api.clevertap.com/1/profile.json?email=jack%2Bvip%40gmail.com' \
  -H 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \
  -H 'X-CleverTap-Passcode: <PASSCODE>'
```

### Example Response

The following is a sample response. A successful request returns HTTP 200.

```json
{
  "status": "success",
  "record": {
    "email": "jack+vip@gmail.com",
    "identity": "5555555555",
    "profileData": {
      "High Score": 200,
      "Favorite Food": "Pizza",
      "MSG-email": true,
      "subscription-groups": {
        "email": {
          "tech news": true,
          "marketing": false,
          "service updates": true
        }
      }
    },
    "events": {
      "App Launched": { "count": 10, "first_seen": 1701388800, "last_seen": 1704067200 }
    },
    "platformInfo": [
      {
        "platform": "iOS",
        "os_version": "17.1",
        "app_version": "1.2.3",
        "make": "Apple",
        "model": "iPhone14,5",
        "push_token": "abcdef12345",
        "objectId": "1a063854f83a4c6484285039ecff87cb"
      }
    ]
  }
}
```

The following table describes the single profile response schema.

| Field  | Type   | Required | Description                                                                         |
| :----- | :----- | :------- | :---------------------------------------------------------------------------------- |
| status | string | Yes      | Always "success" for successful requests.                                           |
| record | object | Yes      | User profile object with the same structure described in User Profile Object above. |

All successful responses across all endpoints in this doc include `status: "success"` plus endpoint-specific data fields (`cursor`, `records` and `next_cursor`, or `record`).

# Error Handling

The following table describes the error response schema. All error responses follow this structure:

| Field  | Type    | Description                                                  |
| :----- | :------ | :----------------------------------------------------------- |
| status | string  | Always "fail" for error responses.                           |
| error  | string  | Human-readable error message.                                |
| code   | integer | HTTP status code, also returned in the HTTP response header. |

The following table lists the error codes returned by all endpoints in this doc. The error message strings in the Example JSON column are illustrative; the API returns free-form messages, and exact wording may vary.

| HTTP | When it happens                                                                                                                        | Resolution                                                                                                                                                      | Example JSON                                                                                                                                                                                                                   |
| :--- | :------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200  | Request completed successfully.                                                                                                        | Parse response data according to the endpoint schema.                                                                                                           | `{"status":"success","cursor":"..."}`                                                                                                                                                                                          |
| 400  | Malformed JSON, invalid date format, `from` greater than `to`, missing cursor, expired cursor, or multiple lookup parameters provided. | Check JSON syntax, verify dates are integers in YYYYMMDD format, ensure `from` is less than or equal to `to`, and use only one of email, identity, or objectId. | `{"status":"fail","error":"...","code":400}` — for example: `{"status":"fail","error":"Payload is mandatory","code":400}`                                                                                                      |
| 401  | Wrong or missing auth headers.                                                                                                         | Verify `X-CleverTap-Account-Id` and `X-CleverTap-Passcode` are correct.                                                                                         | `{"status":"fail","error":"...","code":401}`                                                                                                                                                                                   |
| 403  | Account lacks permission to access the profiles endpoint.                                                                              | Contact CleverTap support to enable API access for your account.                                                                                                | `{"status":"fail","error":"...","code":403}`                                                                                                                                                                                   |
| 404  | No profile matches the given email, identity, or objectId.                                                                             | Verify the identifier is correct and the user exists in your CleverTap account.                                                                                 | `{"status":"fail","error":"...","code":404}`<mark> FLAG: 404 behavior for /1/profile.json not confirmed in code. Verify whether the API returns 404 or a different response when a profile is not found before publish.</mark> |
| 429  | Too many requests in a short time.                                                                                                     | Implement exponential backoff (1s, 2s, 4s, 8s, 16s, maximum 5 retries). Wait before retrying.                                                                   | `{"status":"fail","error":"...","code":429}`                                                                                                                                                                                   |
| 500  | Unexpected server error.                                                                                                               | Retry with exponential backoff. Contact support if the issue persists.                                                                                          | `{"status":"fail","error":"...","code":500}`                                                                                                                                                                                   |
| 503  | System busy or account worker not ready.                                                                                               | Retry with exponential backoff. Contact support if the issue persists.                                                                                          | `{"status":"fail","error":"...","code":503}`                                                                                                                                                                                   |

The following is an example of a malformed JSON request body that results in a 400 error: `{"event_name": "Test", from: 20240101}` — the field name `from` is missing quotes.

## Notes and Best Practices

The following are guidelines for working with this API.

* Consistency: Use the same region base URL for Step 1, Step 2, and by-ID requests (for example, `https://in1.api.clevertap.com`).
* Date formats: Input dates are YYYYMMDD integers, not strings. Response timestamps such as `first_seen` and `last_seen` are UNIX seconds, not milliseconds.
* Pagination: Continue calling Step 2 with `next_cursor` until it is absent from the response. The final page includes `records` and no `next_cursor`. The subsequent request returns `{"status": "success", "records": []}`.
* Rate limits: Rate limits are account-specific. Contact CleverTap support for your account's limits. Implement exponential backoff (1s, 2s, 4s, 8s, 16s, maximum 5 retries) when you receive 429 errors.
* Request timeouts: Set a 60-second timeout on all API requests. On timeout, retry with the same cursor; the cursor retains its position and does not reset.
* GET headers: Do not include `Content-Type` on GET requests (Step 2 and Download by ID).
* URL encoding: Always URL-encode email, identity, and objectId values in query strings. Common characters requiring encoding: `+` (%2B), `@` (%40), `#` (%23), `&` (%26), space (%20).
* Cursor handling: Cursors are opaque tokens valid for 4 days. Do not decode, modify, or store them. Use cursors immediately for pagination. Each cursor is single-threaded; using the same cursor from multiple processes simultaneously may cause unrecoverable failures, requiring a restart from Step 1.
* Error handling: Implement retry logic with exponential backoff for 429, 500, and 503 errors. Parse error responses to extract the error message for logging and debugging.
* Field presence: Fields that are unavailable are omitted from responses entirely, not returned as null. Always check for the existence of a field before accessing its value in your code.

For more information on request limits, refer to [API Request Limit](doc:api-request-limit). To understand common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

***

## Open flags

1. **SDK docs link**: Placeholder note. Add the actual URL before publish.
2. **Cursor TTL (4 days)**: Confirmed from source code. Verify whether this value can be stated publicly, or align with the preferred externally-communicated value.
3. **Push token character limits** (iOS 64-hex, Android 4096-char): Need confirmation from the mobile SDK team before stating in the doc.
4. **404 on by-ID lookup**: Not confirmed in code. Verify whether `/1/profile.json` returns 404 or a different response when a profile is not found.
5. **11 valid filter operators (Issue #4)**: Confirmed from source code by the reviewer but the list was not provided. Add an operator reference table once the list is confirmed.
6. **Webhooks/streaming (Issue #55)**: Not added. No confirmed targets for the links.

## Technical claims added or corrected, and where they came from

**Must-fix corrections from GPT review:**

* Pagination final-page response corrected from `{"status":"success"}` to `{"status":"success","records":[]}`, sourced from GPT review Must Fix #5/#39.
* `profileData` changed from Required:Yes to Conditional, and description updated to state the field is absent when no custom properties exist (not an empty `{}`), sourced from Must Fix #41.
* Second sample record in Step 2 response corrected: `profileData` key removed entirely (previously showed `{}`), sourced from Must Fix #41.
* By-ID sample response model fixed from `"iPhone 12"` (marketing name) to `"iPhone14,5"` (internal identifier), sourced from Must Fix #44.
* Cursor TTL changed from "24 hours" to "4 days" across all three occurrences, sourced from Must Fix #19/#33.
* 503 row added to error table for "system busy / account worker not ready," sourced from Must Fix.
* 404 and 422 rows: 404 retained with flag, 422 row removed as it could not be confirmed in code, sourced from Must Fix.
* All error message strings changed to `{"status":"fail","error":"...","code":N}` format with an illustrative-only note, sourced from Must Fix.
* "600 requests/minute for standard plans" removed and replaced with "account-specific, contact CleverTap support," sourced from Must Fix #24.
* Step 2 note corrected: "cursor encodes query params" changed to "params stored server-side and associated with the cursor," sourced from Must Fix #26.

**Accept items newly incorporated:**

* Region table added to Base URL section, sourced from Issue #17.
* CORS restriction added to security callout, sourced from Issue #54 (confirmed in Accept list).
* Boolean parameter note updated: case-insensitive, any non-"false" treated as true, sourced from Issue #36.
* Cursor retry-on-timeout note added: same cursor retains position, sourced from Issue #58.
* Issue #4 operators flagged as pending, sourced from Accept list.

**Rejected items not added:**

* 90-day date range limit (Issue #37): no such limit exists in code.
* "600 requests/minute" (Issue #24): fabricated, removed.
* `profileData: {}` example (Issue #41): field is absent, not empty object.
* "24 hours" cursor TTL (Issues #19/#33): wrong; corrected to 4 days.

# Overview

The Get User Profiles API enables you to fetch detailed information about user profiles, including profile properties, events, and platform data. You can use it to retrieve either a single user profile or multiple profiles using cursor-based pagination.

There are two ways to get user profiles:

* **[By event](doc:get-user-profiles-api#get-user-profiles-by-events)**: Download profiles of users who have performed a specific event. For example, you can fetch a list of users who launched your app in the past day.
* **[By ID](doc:get-user-profiles-api#download-user-profiles-by-id)**: Request specific user profiles directly.

All endpoints in the Get User Profiles API require a region-specific base URL. Use the same region endpoint consistently across the entire workflow, including Get Cursor, Get Using Cursor, and Download by ID.

# Authentication

All requests must include the following headers:

| Header                 | Description                                                 |
| ---------------------- | ----------------------------------------------------------- |
| X-CleverTap-Account-Id | Your CleverTap Account ID.                                  |
| X-CleverTap-Passcode   | CleverTap Account Passcode associated with your Account ID. |
| Content-Type           | Must be set to `application/json`.                          |

# Get User Profiles by Events

Getting a list of user profiles by events requires two steps:

* With your first API call, specify the event name, date range, and batch size needed. This request returns a cursor you will use to get the list of user profile records in the [Get User Profiles Using Cursor](doc:get-user-profiles-api#get-user-profiles-using-cursor) section.
* Make a second API call using the cursor from the [Get Cursor by Specifying Events for User Profiles](doc:get-user-profiles-api#get-cursor-by-specifying-events-for-user-profiles) section. This call returns the first batch of user profiles and another cursor to get the second batch of user profiles. You repeat this process until the cursor is not returned in the response, meaning no more matching user profiles exist.

<Callout icon="📘" theme="info">
  **Batch Size**

  The batch size denotes the maximum number of records that can be fetched in a single call. The batch size should be provided in multiples of 23 (23, 46, 69....) as there is a total of 23 memory buckets available in CleverTap's data storage mechanism.
</Callout>

## Get Cursor by Specifying Events for User Profiles

This API returns a cursor that you can use to fetch user profiles that performed specific events.

### Base URL

Here is the sample base URL from the account in the India region:

[https://in1.api.clevertap.com/1/profiles.json](https://\<region>.api.clevertap.com/1/profiles.json)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

### HTTP Method

POST

### Query Parameters

| Parameter  | Description                                                                                                                                                                   | Type    | Example Value |
| :--------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | :------------ |
| batch_size | The number of results to return in each API call. The maximum is 5000.                                                                                                        | int     | 50            |
| app        | Optional parameter. Set to true to receive app fields in the response. This will return the OS version, device make, device model, and app version within the profile object. | boolean | true          |
| events     | Optional parameter. Includes the event summary fields in the response. The default value is true. Set to false to exclude event summary fields in the response.               | boolean | true          |
| profile    | Optional parameter. The response includes the custom profile properties. The default value is true. Set to false to exclude custom profile properties fields in the response. | boolean | true          |

### Body Parameters

| Parameter  | Description                                                                                                                                                                                                                                        | Type   |                |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | :------------- |
| event_name | The event type needed. You can get both standard and custom events from this endpoint. The standard events include App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Notification Sent, Product Viewed, and UTM Visited. | string | “App Launched” |
| from       | Start of date range for events needed. Value specified in format YYYYMMDD.                                                                                                                                                                         | int    | 20171201       |
| to         | End of date range for events needed. Value specified in format YYYYMMDD.                                                                                                                                                                           | int    | 20171225       |

```json
{
  "event_name": "Charged",
  "from": 1622505600,
  "to": 1625097600
}
```

### Sample Request

Here is an example cURL request to the Get User Profiles API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"event_name":"App Launched","from":20171201,"to":20171225}' "https://in1.api.clevertap.com/1/profiles.json?batch_size=50" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/profiles.json?batch_size=50")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "event_name" => "App Launched",
  "from" => 20171201,
  "to" => 20171225
})

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```
```python
#  Recommended python version 2.7 
import requests

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json',
}

params = (
    ('batch_size', '50'),
)

data = '{"event_name":"App Launched","from":20171201,"to":20171225}'

response = requests.post('https://in1.api.clevertap.com/1/profiles.json', headers=headers, params=params, data=data)
```
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
    'X-CleverTap-Passcode' => 'PASSCODE',
    'Content-Type' => 'application/json'
);
$data = '{"event_name":"App Launched","from":20171201,"to":20171225}';
$response = Requests::post('https://in1.api.clevertap.com/1/profiles.json?batch_size=50', $headers, $data);
```
```node Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var dataString = '{"event_name":"App Launched","from":20171201,"to":20171225}';

var options = {
    url: 'https://in1.api.clevertap.com/1/profiles.json?batch_size=50',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```
```go
type Payload struct {
	EventName string `json:"event_name"`
	From      int    `json:"from"`
	To        int    `json:"to"`
}

data := Payload{
// fill struct
}
payloadBytes, err := json.Marshal(data)
if err != nil {
	// handle err
}
body := bytes.NewReader(payloadBytes)

req, err := http.NewRequest("POST", "https://in1.api.clevertap.com/1/profiles.json?batch_size=50", body)
if err != nil {
	// handle err
}
req.Header.Set("X-Clevertap-Account-Id", "ACCOUNT_ID")
req.Header.Set("X-Clevertap-Passcode", "PASSCODE")
req.Header.Set("Content-Type", "application/json")

resp, err := http.DefaultClient.Do(req)
if err != nil {
	// handle err
}
defer resp.Body.Close()
```

### Sample Response

```json
{
 "cursor":"AfljfgIJBgBnamF5Kz8NegcBAwxhbCe%2Fbmhhe04BBAVlYjT4YG5reQEATQQrai57K2oue04FAUhnd38%3D",
 "status":"success"
}
```

## Get User Profiles Using Cursor

Using the cursor from the [Get Cursor by Specifying Events for User Profiles](doc:get-user-profiles-api#get-cursor-by-specifying-events-for-user-profiles) section, you will make an API call that returns the first batch of user profiles and another cursor to get the second batch of user profiles. Repeat this process until a cursor is not returned in the response, which means there are no more matching user profiles.

### Base URL

Here is the sample base URL from the account in the India region:

[https://in1.api.clevertap.com/1/profiles.json?cursor=\<cursor>](https://in1.api.clevertap.com/1/profiles.json?cursor=\<cursor>)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

### HTTP Method

GET

### Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

### Query Parameter

| Parameter | Parameter                                                          | Type   | Example Value                                                                  |
| :-------- | :----------------------------------------------------------------- | :----- | :----------------------------------------------------------------------------- |
| cursor    | Required parameter. Specifies what set of user profiles to return. | string | ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6 |

### Sample Request

Here is an example cURL request to the Get User Profiles Using Cursor API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl "https://in1.api.clevertap.com/1/profiles.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.clevertap.com/1/profiles.json?cursor=CURSOR")
request = Net::HTTP::Get.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```
```python
import requests

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json',
}

params = (
    ('cursor', 'CURSOR'),
)

response = requests.get('https://api.clevertap.com/1/profiles.json', headers=headers, params=params)
```
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
    'X-CleverTap-Passcode' => 'PASSCODE',
    'Content-Type' => 'application/json'
);
$response = Requests::get('https://api.clevertap.com/1/profiles.json?cursor=CURSOR', $headers);
```
```node Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var options = {
    url: 'https://api.clevertap.com/1/profiles.json?cursor=CURSOR',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```
```go
req, err := http.NewRequest("GET", "https://api.clevertap.com/1/profiles.json?cursor=CURSOR", nil)
if err != nil {
	// handle err
}
req.Header.Set("X-Clevertap-Account-Id", "ACCOUNT_ID")
req.Header.Set("X-Clevertap-Passcode", "PASSCODE")
req.Header.Set("Content-Type", "application/json")

resp, err := http.DefaultClient.Do(req)
if err != nil {
	// handle err
}
defer resp.Body.Close()
```

### Sample Response

If there are more user profiles available, next_cursor will be returned. The value for next_cursor is used to get the next batch of user profiles.

User profiles will be returned in a records object. The schema for the user profile object is documented in the section below.

```json
{
  "status": "success",
  "next_cursor": "ZiZjNwMEBQBkaWZzY2MuO20BBQFlYmN7ZG5hewYBTQVjb2BzZmphfwEABQUra2Jmeg%3D%3D",
  "records": [
    {
      "identity": "5555555555",
      "profileData": {
        "favoriteFood": "pizza"
      },

      "events": {
      "App Launched": {
        "count": 10,
        "first_seen": 1457271567,
        "last_seen": 1458041215
      },
      "Charged": {
        "count": 6,
        "first_seen": 1457962417,
        "last_seen": 1458041276
      }
      },

      "platformInfo": [
        {
          "platform": "iOS",
          "os_version": "10.2",
          "app_version": "6.1.3",
          "make": "Apple",
          "model": "iPhone7,2",
          "push_token": "95f98af6ad9a5e714a56c5bf527a78cb1e3eda18d2f23bc8591437d0d8ae71a3",
          "objectId": "-1a063854f83a4c6484285039ecff87cb"
        },
        {
          "platform": "Web",
          "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097"
        }

      ]

    }

  ]

}
```

If no more user profiles are available, `next_cursor` will not be returned, and a success response will be returned.

```json
{ "status" : "success"}
```

### User Profile Object

The following is the schema for the user profile object:

| Key          | Description                                                                                                                                                                                                 |
| :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| email        | User’s email address.                                                                                                                                                                                       |
| profileData  | User’s custom profile properties.                                                                                                                                                                           |
| identity     | Custom user identity provided by you.                                                                                                                                                                       |
| name         | User’s name.                                                                                                                                                                                                |
| platformInfo | Array containing all platform names with the corresponding `objectId` (CleverTap ID), `phone number`, `push_token`, `app_version`, `os_version`, `make` (device make), `model` (device model) if available. |

# Download User Profiles by ID

You can download a specific user profile by passing any of the following three identifiers: email, identity, or objectId.

### Base URL

[https://in1.api.clevertap.com/1/profile.json?\<parameter>=\<value>](https://in1.api.clevertap.com/1/profile.json?\<parameter>=\<value>)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

### HTTP Method

GET

### Query Parameters

Pass any one of the following parameters:

| Parameter | Description                          | Type   | Example Value                      |
| --------- | ------------------------------------ | ------ | ---------------------------------- |
| email     | User’s email address.                | string | `jack@gmail.com`                   |
| identity  | Custom user identity defined by you. | string | `5555555555`                       |
| objectId  | CleverTap ID.                        | string | `1a063854f83a4c6484285039ecff87cb` |

### Sample Request

Here is an example cURL request to the Get User Profiles by ID API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl "https://in1.api.clevertap.com/1/profile.json?email=jack@gmail.com" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.clevertap.com/1/profile.json?email=jack@gmail.com")
request = Net::HTTP::Get.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```
```python
import requests

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json',
}

params = (
    ('email', 'jack@gmail.com'),
)

response = requests.get('https://api.clevertap.com/1/profile.json', headers=headers, params=params)
```
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
    'X-CleverTap-Passcode' => 'PASSCODE',
    'Content-Type' => 'application/json'
);
$response = Requests::get('https://api.clevertap.com/1/profile.json?email=jack@gmail.com', $headers);
```
```node Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var options = {
    url: 'https://api.clevertap.com/1/profile.json?email=jack@gmail.com',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```
```go
req, err := http.NewRequest("GET", "https://api.clevertap.com/1/profile.json?email=jack@gmail.com", nil)
if err != nil {
	// handle err
}
req.Header.Set("X-Clevertap-Account-Id", "ACCOUNT_ID")
req.Header.Set("X-Clevertap-Passcode", "PASSCODE")
req.Header.Set("Content-Type", "application/json")

resp, err := http.DefaultClient.Do(req)
if err != nil {
	// handle err
}
defer resp.Body.Close()
```

### Sample Response

The user profile returns in a record object. The schema for the user profile object is as shown below.

```json
{

  "status": "success",

  "record": {

    "email": "jack@gmail.com",

    "profileData": {

      "Last Score": 308,

      "High Score": 308,

      "Replayed": true

    },

    "events": {

      "App Launched": {

        "count": 10,

        "first_seen": 1457271567,

        "last_seen": 1458041215

      },

      "Charged": {

        "count": 6,

        "first_seen": 1457962417,

        "last_seen": 1458041276

      }

    },

    "platformInfo": [

      {

        "platform": "iOS",

        "os_version": "10.2",

        "app_version": "6.1.3",

        "make": "Apple",

        "model": "iPhone7,2",

        "push_token": "95f98af6ad9a5e714a56c5bf527a78cb1e3eda18d2f23bc8591437d0d8ae71a3",

        "objectId": "-1a063854f83a4c6484285039ecff87cb"

      },

      {

        "platform": "Web",

        "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097"

      }

    ]

  }

}
```

# Additional Resources

* [API Errors](doc:api-errors)
* [API Request Limit](doc:api-request-limit)
* [API FAQs](doc:api-faqs)
