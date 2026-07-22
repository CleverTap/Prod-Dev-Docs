---
title: List Segments
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Use this endpoint to retrieve a paginated, filterable list of all segments in your CleverTap account.

# Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/segments/list.json`

For region-specific endpoints, refer to [Region](doc:common-api-components#region).

# HTTP Method

POST

# Headers

Refer to [Headers](doc:common-api-components#headers) for more details.

# Segment Types

The `type` filter accepts the following values. All values are case-sensitive.

The following table lists the valid segment type values.

| Value                   | Description                             |
| :---------------------- | :-------------------------------------- |
| `batch`                 | Past-behavior (batch-computed) segment. |
| `stream`                | Live-behavior (streaming) segment.      |
| `custom_list`           | Custom list segment.                    |
| `MIXPANEL`              | Mixpanel partner segment.               |
| `MPARTICLE`             | mParticle partner segment.              |
| `AMPLITUDE`             | Amplitude partner segment.              |
| `PRECOMPUTED_SEGMENT`   | Precomputed segment.                    |
| `CLEVERTAP_API_SEGMENT` | Segment created via the CleverTap API.  |
| `PARTNER_SEGMENT`       | Generic partner segment.                |

# Body Parameters

All body parameters are optional. Send an empty object `{}` to retrieve the default view: last 30 days, page 1, 100 results per page, sorted newest first.

The following table lists the body parameters:

| Parameter      | Description                                                                                                                                                    | Required/Optional | Type    | Example Value |
| :------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------- | :------ | :------------ |
| from           | Include segments created on or after this date. Must be an integer in YYYYMMDD format. Defaults to today minus 30 days.                                        | Optional          | integer | `20260101`    |
| to             | Include segments created on or before this date. Must be an integer in YYYYMMDD format. The range `from` and `to` must not exceed 365 days. Defaults to today. | Optional          | integer | `20260713`    |
| type           | Filter results by segment type. Must be one of the values in the Segment Types table above. Case-sensitive. Defaults to all types.                             | Optional          | string  | `"batch"`     |
| status         | Filter results by segment status. Accepted values: `active`, `precomputed`, `inactive`. Defaults to all statuses.                                              | Optional          | string  | `"active"`    |
| page           | The page number to retrieve. Must be 1 or greater. Defaults to 1.                                                                                              | Optional          | integer | `1`           |
| limit          | The number of results per page. Must be 1 or greater. Maximum 500. Defaults to 100.                                                                            | Optional          | integer | `50`          |
| sort_direction | Sort order for results. Accepted values: `DESC` (newest first) or `ASC`. Defaults to `DESC`.                                                                   | Optional          | string  | `"DESC"`      |

The following is a sample payload.

```json
{
    "from": 20260101,
    "to": 20260713,
    "type": "batch",
    "status": "active",
    "page": 1,
    "limit": 50,
    "sort_direction": "DESC"
}
```

# Example Request

The following is a sample request to the List Segments API, showing the headers needed to authenticate the request.

```curl
curl -X POST "https://<region>.api.clevertap.com/1/segments/list.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" \
-d '{
    "from": 20260101,
    "to": 20260713,
    "type": "batch",
    "status": "active",
    "page": 1,
    "limit": 50,
    "sort_direction": "DESC"
}'
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/segments/list.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "from" => 20260101,
  "to" => 20260713,
  "type" => "batch",
  "status" => "active",
  "page" => 1,
  "limit" => 50,
  "sort_direction" => "DESC"
})

req_options = { use_ssl: uri.scheme == "https" }
response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
puts response.read_body
```
```python
import requests

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json',
}

data = {
    "from": 20260101,
    "to": 20260713,
    "type": "batch",
    "status": "active",
    "page": 1,
    "limit": 50,
    "sort_direction": "DESC"
}

response = requests.post(
    'https://<region>.api.clevertap.com/1/segments/list.json',
    headers=headers,
    json=data
)
print(response.json())
```
```php
<?php
$headers = [
    'X-CleverTap-Account-Id: ACCOUNT_ID',
    'X-CleverTap-Passcode: PASSCODE',
    'Content-Type: application/json'
];
$data = json_encode([
    "from" => 20260101,
    "to" => 20260713,
    "type" => "batch",
    "status" => "active",
    "page" => 1,
    "limit" => 50,
    "sort_direction" => "DESC"
]);
$ch = curl_init("https://<region>.api.clevertap.com/1/segments/list.json");
curl_setopt_array($ch, [
    CURLOPT_POST => true,
    CURLOPT_HTTPHEADER => $headers,
    CURLOPT_POSTFIELDS => $data,
    CURLOPT_RETURNTRANSFER => true,
]);
echo curl_exec($ch);
curl_close($ch);
```
```javascript Node.js
const axios = require("axios");

axios.post(
    "https://<region>.api.clevertap.com/1/segments/list.json",
    {
        from: 20260101,
        to: 20260713,
        type: "batch",
        status: "active",
        page: 1,
        limit: 50,
        sort_direction: "DESC"
    },
    {
        headers: {
            "X-CleverTap-Account-Id": "ACCOUNT_ID",
            "X-CleverTap-Passcode": "PASSCODE",
            "Content-Type": "application/json"
        }
    }
).then(res => console.log(res.data))
 .catch(err => console.error(err?.response?.data || err.message));
```

# Example Response

The following is a sample success response.

```json
{
    "page": 1,
    "limit": 50,
    "total_pages": 3,
    "total_count": 128,
    "segments": [
        {
            "type": "batch",
            "id": 12345,
            "name": "High-value buyers",
            "created_on": 202605021431,
            "updated_on": 202606180905,
            "created_by": "jane@brand.com",
            "updated_by": "jane@brand.com",
            "engagement_count": 4,
            "dependent_segments_count": 1,
            "status": "active"
        }
    ]
}
```

# Response Parameters

The following table describes the response fields.

| Field                                | Type             | Description                                                                                                                                                                                          |
| :----------------------------------- | :--------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| page                                 | integer          | The page number returned, echoed from the request.                                                                                                                                                   |
| limit                                | integer          | The effective page size after the 500-record cap is applied.                                                                                                                                         |
| total_pages                          | integer          | Total number of pages for the filtered result set.                                                                                                                                                   |
| total_count                          | integer          | Total number of segments matching the filter criteria.                                                                                                                                               |
| segments                             | array of objects | Segment summaries for the current page.                                                                                                                                                              |
| segments\[].type                     | string           | Segment type as a raw enum name. See the Segment Types table for valid values. Note: this differs from the `segment.segment_type` field in Get Segment Details, which returns friendly mapped names. |
| segments\[].id                       | integer          | Unique segment ID.                                                                                                                                                                                   |
| segments\[].name                     | string           | Segment name.                                                                                                                                                                                        |
| segments\[].created_on               | integer          | Timestamp of when the segment was created, as a numeric long in `yyyyMMddHHmm` format (account timezone), for example `202605021431`. Null if not available.                                         |
| segments\[].updated_on               | integer          | Timestamp of the last update, as a numeric long in `yyyyMMddHHmm` format (account timezone), for example `202606180905`. Null if not available.                                                      |
| segments\[].created_by               | string           | Email or identifier of the user who created the segment.                                                                                                                                             |
| segments\[].updated_by               | string           | Email or identifier of the user who last updated the segment.                                                                                                                                        |
| segments\[].engagement_count         | integer          | Number of campaigns or engagements using this segment. Sourced from precomputed counters in MongoDB, not computed live per request.                                                                  |
| segments\[].dependent_segments_count | integer          | Number of other segments that depend on this segment.                                                                                                                                                |
| segments\[].status                   | string           | Segment status. One of `active`, `precomputed`, or `inactive`.                                                                                                                                       |

# Rate Limits

The following table lists the rate limits for this endpoint.

| Limit                            | Scope                                               |
| :------------------------------- | :-------------------------------------------------- |
| 3 concurrent in-flight requests  | Per account, shared across CleverTap data-push APIs |
| 60 concurrent in-flight requests | Global                                              |

<Callout icon="📘" theme="info">
  ### **Concurrency Limits**

  The concurrency limits are in-flight-request guards, not requests-per-second limits.
</Callout>

# Error Codes

The following table lists the error codes returned by this endpoint.

| HTTP Status | Error                                                                                                                       | Description                                                                                                                                 |
| :---------- | :-------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------ |
| 400         | `"Incomplete request body"`                                                                                                 | Request body could not be parsed.                                                                                                           |
| 400         | `"Invalid \"from\" date (it must be of the format YYYYMMDD)"` / `"Invalid \"to\" date (it must be of the format YYYYMMDD)"` | A date field is not an integer or not in YYYYMMDD format.                                                                                   |
| 400         | `"Invalid \"type\" (it must be a string)"` / `"Invalid \"type\" value: X"`                                                  | `type` is not a string, or the value is not a valid segment type.                                                                           |
| 400         | `"Invalid \"status\" value: must be one of active, precomputed, inactive"`                                                  | `status` is not one of the accepted values.                                                                                                 |
| 400         | `"Invalid \"status\" (it must be a string)"`                                                                                | `status` is present but not a string type.                                                                                                  |
| 400         | `"Invalid \"page\" value: must be >= 1"` / `"Invalid \"limit\" value: must be greater than 1"`                              | `page` is less than 1, or `limit` is less than 1.                                                                                           |
| 400         | `"invalid_sort_direction"`                                                                                                  | `sort_direction` is not `ASC` or `DESC`.                                                                                                    |
| 400         | `"invalid_range"`                                                                                                           | `from` is greater than `to`, or the date range exceeds 365 days.                                                                            |
| 401         | `"Invalid Credentials"`                                                                                                     | Credentials are missing or incorrect.                                                                                                       |
| 429         | `"Too many concurrent requests"`                                                                                            | Per-account concurrent request limit exceeded. Retry after a short delay.                                                                   |
| 500         | `"Internal server error"`                                                                                                   | Segment list could not be loaded from cache.                                                                                                |
| 500         | `"Failed to process request"`                                                                                               | Unexpected uncaught exception in the base servlet. Catch-all for any other server-side failure.                                             |
| 503         | `"Please come back later"`                                                                                                  | Global capacity limit reached, async timeout, or account worker not ready. All 503 conditions return this same string. Retry after a delay. |

<br />