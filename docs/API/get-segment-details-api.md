---
title: Get Segment Details API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Use this endpoint to retrieve the metadata and membership trend for a single segment.

# Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/segments/get.json`

For region-specific endpoints, refer to [Region](doc:common-api-components#region).

# HTTP Method

POST

# Headers

Refer to [Headers](doc:common-api-components#api-headers) for more details.

# Body Parameters

The following table lists the body parameters.

| Parameter   | Description                                                                                                                                       | Type    | Required/Optional | Example Value |
| :---------- | :------------------------------------------------------------------------------------------------------------------------------------------------ | :------ | :---------------- | :------------ |
| segment_id  | The ID of the segment to fetch.                                                                                                                   | integer | Required          | `12345`       |
| from        | Start of the membership trend window. Must be an integer in YYYYMMDD format. Must be less than or equal to `to`. Defaults to today minus 30 days. | integer | Optional          | `20260601`    |
| to          | End of the membership trend window. Must be an integer in YYYYMMDD format. Defaults to today.                                                     | integer | Optional          | `20260713`    |
| granularity | The bucket size for the membership trend data. Accepted values: `day`, `week`, `month`. Defaults to `day`.                                        | string  | Optional          | `"week"`      |

The following is a sample payload.

```json
{
    "segment_id": 12345,
    "from": 20260601,
    "to": 20260713,
    "granularity": "week"
}
```

# Example Request

The following is a sample request to the Get Segment Details API, showing the headers needed to authenticate the request.

```curl
curl -X POST "https://<region>.api.clevertap.com/1/segments/get.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" \
-d '{
    "segment_id": 12345,
    "from": 20260601,
    "to": 20260713,
    "granularity": "week"
}'
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/segments/get.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "segment_id" => 12345,
  "from" => 20260601,
  "to" => 20260713,
  "granularity" => "week"
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
    "segment_id": 12345,
    "from": 20260601,
    "to": 20260713,
    "granularity": "week"
}

response = requests.post(
    'https://<region>.api.clevertap.com/1/segments/get.json',
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
    "segment_id" => 12345,
    "from" => 20260601,
    "to" => 20260713,
    "granularity" => "week"
]);
$ch = curl_init("https://<region>.api.clevertap.com/1/segments/get.json");
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
    "https://<region>.api.clevertap.com/1/segments/get.json",
    {
        segment_id: 12345,
        from: 20260601,
        to: 20260713,
        granularity: "week"
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
    "segment": {
        "id": 12345,
        "name": "High-value buyers",
        "segment_type": "past_behavior",
        "status": "active",
        "definition": "...segment definition string...",
        "created_at": 202605021431,
        "updated_at": 202606180905,
        "created_by": "jane@brand.com",
        "updated_by": "jane@brand.com"
    },
    "trend": {
        "from": "2026-06-01",
        "to": "2026-07-13",
        "granularity": "week",
        "data": [
            { "date": "2026-06-01", "user_count": 10432 },
            { "date": "2026-06-08", "user_count": 10890 }
        ]
    }
}
```

# Response Parameters

The following table describes the response fields.

| Field                | Type             | Description                                                                                                                                                                                                              |
| :------------------- | :--------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| segment              | object           | Object containing the complete metadata for the requested segment, including its type, status, definition, creation details, and audit information.                                                                      |
| segment.id           | integer          | The unique numeric identifier for this segment. This matches the `segment_id` value passed in the request.                                                                                                               |
| segment.name         | string           | The display name of the segment as configured in the CleverTap dashboard.                                                                                                                                                |
| segment.segment_type | string           | Human-readable label describing how the segment is computed.                                                                                                                                                             |
| segment.status       | string           | The current operational status of the segment. `active` means the segment is live and in use. `inactive` means the segment is disabled. `precomputed` means the segment uses a precomputed membership list.              |
| segment.definition   | string           | Segment definition string passed through verbatim from the data store. Describes the rules or criteria used to compute segment membership. Null if not set or not available.                                             |
| segment.created_at   | integer          | Timestamp of when the segment was created, as a numeric long in `yyyyMMddHHmm` format (account timezone), for example `202605021431`. Null if not available.                                                             |
| segment.updated_at   | integer          | Timestamp of when the segment was last updated, as a numeric long in `yyyyMMddHHmm` format (account timezone), for example `202606180905`. Null if not available.                                                        |
| segment.created_by   | string           | Email address or identifier of the user who created this segment. Null if not available.                                                                                                                                 |
| segment.updated_by   | string           | Email address or identifier of the user who most recently modified this segment. Null if not available.                                                                                                                  |
| trend                | object           | Object containing the membership trend time series for the requested date window and granularity.                                                                                                                        |
| trend.from           | string           | The resolved start date of the trend window in `YYYY-MM-DD` format. Reflects the actual start date used, defaulting to today minus 30 days if not specified in the request.                                              |
| trend.to             | string           | The resolved end date of the trend window in `YYYY-MM-DD` format. Reflects the actual end date used, defaulting to today if not specified in the request.                                                                |
| trend.granularity    | string           | The bucket size used to group trend data points, echoed from the request. One of `day`, `week`, or `month`.                                                                                                              |
| trend.data           | array of objects | Array of data points representing segment membership over time. Each entry contains `date` (string, `YYYY-MM-DD`, the start of the bucket) and `user_count` (integer, the number of users in the segment at that point). |

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

| HTTP Status | Error                                                                                                 | Description                                                                                                                                   | Example Error Response                                                                                                             |
| :---------- | :---------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| 400         | `"Incomplete request body"`                                                                           | Request body could not be parsed.                                                                                                             | `{"status":"fail","error":"Incomplete request body","code":400}`                                                                   |
| 400         | `"\"segment_id\" is required"`                                                                        | `segment_id` is missing from the request body.                                                                                                | `{"status":"fail","error":"\"segment_id\" is required","code":400}`                                                                |
| 400         | `"\"segment_id\" must be an integer"`                                                                 | `segment_id` is present but not an integer.                                                                                                   | `{"status":"fail","error":"\"segment_id\" must be an integer","code":400}`                                                         |
| 400         | `"\"from\" must be an integer in YYYYMMDD format"` / `"\"to\" must be an integer in YYYYMMDD format"` | A date field is not an integer or not in YYYYMMDD format.                                                                                     | `{"status":"fail","error":"\"from\" must be an integer in YYYYMMDD format","code":400}`                                            |
| 400         | `"\"granularity\" must be one of: \"day\", \"week\", \"month\""`                                      | `granularity` is not one of the accepted values.                                                                                              | `{"status":"fail","error":"\"granularity\" must be one of: \"day\", \"week\", \"month\"","code":400}`                              |
| 400         | `"\"from\" must be less than or equal to \"to\""`                                                     | `from` is greater than `to`. The response also includes `"additional": {"error_code": "invalid_range"}`.                                      | `{"status":"fail","error":"\"from\" must be less than or equal to \"to\"","code":400,"additional":{"error_code":"invalid_range"}}` |
| 400         | `"Date range must not exceed 365 days"`                                                               | The date range between `from` and `to` meets or exceeds 365 days. The response also includes `"additional": {"error_code": "invalid_range"}`. | `{"status":"fail","error":"Date range must not exceed 365 days","code":400,"additional":{"error_code":"invalid_range"}}`           |
| 401         | `"Invalid Credentials"`                                                                               | Credentials are missing or incorrect.                                                                                                         | `{"status":"fail","error":"Invalid Credentials","code":401}`                                                                       |
| 404         | `"Segment not found"`                                                                                 | No segment exists with the given `segment_id`.                                                                                                | `{"status":"fail","error":"Segment not found","code":404}`                                                                         |
| 429         | `"Too many concurrent requests"`                                                                      | Per-account concurrent request limit exceeded. Retry after a short delay.                                                                     | `{"status":"fail","error":"Too many concurrent requests","code":429}`                                                              |
| 503         | `"Please come back later"`                                                                            | Global capacity limit reached. Retry after a delay.                                                                                           | `{"status":"fail","error":"Please come back later","code":503}`                                                                    |
