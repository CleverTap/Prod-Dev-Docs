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

| Parameter   | Required | Description                                                                                                                                                                                                     | Type    | Format   | Example  |
| :---------- | :------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | :------- | :------- |
| event\_name | Yes      | Event type, standard or custom. Standard events include but are not limited to: App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Notification Sent, Product Viewed, and UTM Visited. | string  | —        | Charged  |
| from        | Yes      | Start of the date range. Must be an integer, not a string. This date is inclusive.                                                                                                                              | integer | YYYYMMDD | 20171201 |
| to          | Yes      | End of the date range. Must be an integer, not a string. This date is inclusive and must be greater than or equal to `from`.                                                                                    | integer | YYYYMMDD | 20171225 |

### Query Parameters

The following table lists the optional query parameters. Use these to control the response shape and pagination.

| Parameter   | Description                                                                                                                                                                        | Type    | Default | Example |
| :---------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | :------ | :------ |
| batch\_size | Maximum number of records to return per call. Values up to 5000 are supported.                                                                                                     | integer | —       | 5000    |
| app         | When true, includes app fields in profile platform info (OS version, device make and model, app version). When false, these fields are omitted.                                    | boolean | false   | true    |
| events      | When true, includes event summary fields inside the events object (count, first\_seen, last\_seen per event). When false, the events key is omitted entirely from profile objects. | boolean | true    | true    |
| profile     | When true, includes custom profile properties in profileData. When false, only system fields are returned.                                                                         | boolean | true    | true    |

Boolean parameters are case-insensitive. Any value other than `false` is treated as `true` (for example, `?app=true&events=false`).

<Callout icon="📘" theme="info">
  ###

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
| status | string | Always _success_ for successful requests.                   |
| cursor | string | Opaque pagination token to use in Step 2. Valid for 4 days. |

<Callout icon="⚠️" theme="warning">
  ###

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

| Field        | Type   | Required | Description                                                                                   |
| :----------- | :----- | :------- | :-------------------------------------------------------------------------------------------- |
| status       | string | Yes      | Always "success" for successful requests.                                                     |
| records      | array  | No       | Array of user profile objects. Present when data is available. See User Profile Object below. |
| next\_cursor | string | No       | Pagination token for the next page. When absent, no more records are available.               |

The following describes the pagination flow:

- Page with more data: response includes a `records` array and `next_cursor`. Use `next_cursor` to fetch the next page.
- Final page with data: response includes a `records` array and no `next_cursor`. This is the last page of data.
- No more data: response contains `{"status": "success", "records": []}` with no `next_cursor`.

Continue pagination while `next_cursor` is present in the response.

### User Profile Object

Fields that are unavailable are omitted from the response entirely, not returned as null. Always check for the existence of a field before accessing its value. Fields marked Conditional below may be absent.

System-defined fields use snake\_case (for example, `first_seen`, `last_seen`, `os_version`). User-defined custom properties in `profileData` follow your own naming convention.

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
  ###

  **Event summary fields**

  When `events=true` is set in Step 1 query parameters, each event in the `events` object contains `count`, `first_seen`, and `last_seen` fields. The `count` value represents the user's lifetime total for that event, not the count within the query date range.
</Callout>

### Platform Info Details

Each entry in the `platformInfo` array represents a device or platform where the user has interacted with your app.

The following table describes the platform info fields.

| Field        | Type   | Platforms    | Description                                                                                                                                                                               |
| :----------- | :----- | :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| platform     | string | All          | Platform type. Valid values: "iOS", "Android", "Web".                                                                                                                                     |
| objectId     | string | All          | Unique CleverTap identifier for this platform instance.                                                                                                                                   |
| push\_token  | string | iOS, Android | Device push notification token. Present only for mobile platforms with push enabled.                                                                                                      |
| app\_version | string | iOS, Android | Version of your app installed on this device (for example, 6.1.3). Present only for mobile platforms.                                                                                     |
| os\_version  | string | iOS, Android | Operating system version (for example, 17.1). Present only for mobile platforms.                                                                                                          |
| make         | string | iOS, Android | Device manufacturer (for example, "Apple", "Samsung"). Present only for mobile platforms.                                                                                                 |
| model        | string | iOS, Android | Device model identifier. iOS uses internal model IDs (for example, "iPhone15,2", not "iPhone 15 Pro"). Android uses manufacturer-specific identifiers. Present only for mobile platforms. |

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

| HTTP | When it happens                                                                                                                        | Resolution                                                                                                                                                      | Example JSON                                                                                                              |
| :--- | :------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------ |
| 200  | Request completed successfully.                                                                                                        | Parse response data according to the endpoint schema.                                                                                                           | `{"status":"success","cursor":"..."}`                                                                                     |
| 400  | Malformed JSON, invalid date format, `from` greater than `to`, missing cursor, expired cursor, or multiple lookup parameters provided. | Check JSON syntax, verify dates are integers in YYYYMMDD format, ensure `from` is less than or equal to `to`, and use only one of email, identity, or objectId. | `{"status":"fail","error":"...","code":400}` — for example: `{"status":"fail","error":"Payload is mandatory","code":400}` |
| 401  | Wrong or missing auth headers.                                                                                                         | Verify `X-CleverTap-Account-Id` and `X-CleverTap-Passcode` are correct.                                                                                         | `{"status":"fail","error":"...","code":401}`                                                                              |
| 403  | Account lacks permission to access the profiles endpoint.                                                                              | Contact CleverTap support to enable API access for your account.                                                                                                | `{"status":"fail","error":"...","code":403}`                                                                              |
| 404  | No profile matches the given email, identity, or objectId.                                                                             | Verify the identifier is correct, and the user exists in your CleverTap account.                                                                                | `{"status":"fail","error":"...","code":404}`                                                                              |
| 429  | Too many requests in a short time.                                                                                                     | Implement exponential backoff (1s, 2s, 4s, 8s, 16s, maximum 5 retries). Wait before retrying.                                                                   | `{"status":"fail","error":"...","code":429}`                                                                              |
| 500  | Unexpected server error.                                                                                                               | Retry with exponential backoff. Contact support if the issue persists.                                                                                          | `{"status":"fail","error":"...","code":500}`                                                                              |
| 503  | System busy or account worker not ready.                                                                                               | Retry with exponential backoff. Contact support if the issue persists.                                                                                          | `{"status":"fail","error":"...","code":503}`                                                                              |

The following is an example of a malformed JSON request body that results in a 400 error: `{"event_name": "Test", from: 20240101}` — the field name `from` is missing quotes.

## Notes and Best Practices

The following are guidelines for working with this API.

- Consistency: Use the same region base URL for Step 1, Step 2, and by-ID requests (for example, `https://in1.api.clevertap.com`).
- Date formats: Input dates are YYYYMMDD integers, not strings. Response timestamps such as `first_seen` and `last_seen` are UNIX seconds, not milliseconds.
- Pagination: Continue calling Step 2  `next_cursor` until it is absent from the response. The final page includes `records` no `next_cursor`. The subsequent request returns `{"status": "success", "records": []}`.
- Rate limits: Rate limits are account-specific. Contact CleverTap support for your account's limits. Implement exponential backoff (1s, 2s, 4s, 8s, 16s, maximum 5 retries) when you receive 429 errors.
- Request timeouts: Set a 60-second timeout on all API requests. On timeout, retry with the same cursor; the cursor retains its position and does not reset.
- URL encoding: Always URL-encode email, identity, and objectId values in query strings. Common characters requiring encoding: `+` (%2B), `@` (%40), `#` (%23), `&` (%26), space (%20).
- Cursor handling: Cursors are opaque tokens valid for 4 days. Do not decode, modify, or store them. Use cursors immediately for pagination. Each cursor is single-threaded; using the same cursor from multiple processes simultaneously may cause unrecoverable failures, requiring a restart from Step 1.
- Error handling: Implement retry logic with exponential backoff for 429, 500, and 503 errors. Parse error responses to extract the error message for logging and debugging.
- Field presence: Fields that are unavailable are omitted from responses entirely, not returned as null. Always check whether a field exists before accessing its value in your code.

For more information on request limits, refer to [API Request Limit](doc:api-request-limit). To understand common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

# Error Codes

The following table lists the error codes returned by this API:

**Step 1: POST&#x20;**`/1/profiles.json`**&#x20;- HTTP-level errors**

| HTTP Status | Error                                                               | Description                                        | Example Error Response                                                                                   |
| ----------- | ------------------------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| 400         | `"Account ID missing"`                                              | `X-CleverTap-Account-Id` header absent or empty    | `{"status":"fail","error":"Account ID missing","code":400}`                                              |
| 400         | `"Account blocked"`                                                 | Account not found or suspended                     | `{"status":"fail","error":"Account blocked","code":400}`                                                 |
| 400         | `"Invalid credentials"`                                             | Wrong passcode                                     | `{"status":"fail","error":"Invalid credentials","code":400}`                                             |
| 400         | `"No query specified"`                                              | Neither `query` nor `enc_query` parameter provided | `{"status":"fail","error":"No query specified","code":400}`                                              |
| 400         | `"Invalid query specified"`                                         | Query is malformed or fails decompression          | `{"status":"fail","error":"Invalid query specified","code":400}`                                         |
| 400         | `"Illegal version"`                                                 | Unsupported query version                          | `{"status":"fail","error":"Illegal version","code":400}`                                                 |
| 403         | `"API access has been temporarily suspended"`                       | Account-level API access suspended                 | `{"status":"fail","error":"API access has been temporarily suspended","code":403}`                       |
| 429         | `"Too many requests. Please try later or contact customer support"` | API disabled for this account via dynamic config   | `{"status":"fail","error":"Too many requests. Please try later or contact customer support","code":429}` |
| 503         | `"Please come back later"`                                          | Account not in cache or EventStore unavailable     | `{"status":"fail","error":"Please come back later","code":503}`                                          |

**Step 2: GET&#x20;**`/1/profiles.json`**&#x20;- response-body errors (all return HTTP 200)**

| HTTP Status | Error                                                   | Description                                                            | Example Error Response                                                            |
| ----------- | ------------------------------------------------------- | ---------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| 200         | `"Too many requests"`                                   | Account-level throttling. Retry with backoff.                          | `{"status":"fail","error":"Too many requests"}`                                   |
| 200         | `"Service unavailable. Please retry later"`             | System-level throttling. Retry with backoff.                           | `{"status":"fail","error":"Service unavailable. Please retry later"}`             |
| 200         | `"System busy, please retry later"`                     | EventStore temporarily unavailable. Retry with backoff.                | `{"status":"fail","error":"System busy, please retry later"}`                     |
| 200         | `"Incorrect Usage"`                                     | Malformed cursor (host count mismatch). Restart from Step 1.           | `{"status":"fail","error":"Incorrect Usage"}`                                     |
| 200         | `"Incorrect Usage, stale cursor"`                       | Out-of-order or concurrent cursor use. Restart from Step 1.            | `{"status":"fail","error":"Incorrect Usage, stale cursor"}`                       |
| 200         | `"Cursor invalidated. Cannot export using this cursor"` | Cursor permanently invalidated after error. Restart from Step 1.       | `{"status":"fail","error":"Cursor invalidated. Cannot export using this cursor"}` |
| 200         | `"Error processing"`                                    | Unexpected server error. Retry; contact support if the issue persists. | `{"status":"fail","error":"Error processing"}`                                    |

**Get User Profile by ID -&#x20;**`/1/profile.json`

| HTTP Status | Error | Description                                                                                                       | Example Error Response                       |
| ----------- | ----- | ----------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| 200         | —     | Profile found.                                                                                                    | `{"status":"success","record":{...}}`        |
| 200         | —     | Profile not found. The `record` field is `null`<br />This endpoint does not return HTTP 404 for missing profiles. | `{"status":"success","record":null}`         |
| 503         | —     | Missing or invalid lookup parameter. No `guid`, `email`, or `identity` provided.                                  | `{"status":"fail","error":"...","code":503}` |
| 503         | —     | EventStore temporarily unavailable. Retry with backoff.                                                           | `{"status":"fail","error":"...","code":503}` |

<br />
