---
title: Get Campaigns
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

The Get Campaigns API returns a list of campaigns (created via the CleverTap dashboard or API) scheduled within the specified date range.

* Provides campaign IDs, names, scheduled start times, and status.
* Use the returned `id` with the [Get Campaign Report API](doc:get-campaign-report-api) to fetch detailed metrics.

> 📘 Use Cases
>
> * Retrieve all campaigns scheduled in a given date range.
> * Automate reporting pipelines for active campaigns.
> * Monitor campaign scheduling and delivery readiness.
> * Integrate with dashboards to track upcoming campaigns.

# Base URL

Use the following endpoint for the India region (replace with your region as required):\
[https://in1.api.clevertap.com/1/targets/list.json](https://in1.api.clevertap.com/1/targets/list.json)

Refer to [Region](https://developer.clevertap.com/docs/common-api-components#region) for the full list of endpoints by region.

# HTTP Method

POST 

# Headers

The following headers must be included with every request:

| Header                   | Description                                                      | Required | Example            |
| ------------------------ | ---------------------------------------------------------------- | -------- | ------------------ |
| `X-CleverTap-Account-Id` | Your CleverTap account ID.                                       | Yes      | `ABCD-RSTU-1234`   |
| `X-CleverTap-Passcode`   | Passcode generated for the account to authenticate API requests. | Yes      | `XXXXXX`           |
| `Content-Type`           | Content type of the request payload. Must be `application/json`. | Yes      | `application/json` |

# Body Parameters

Send a JSON payload specifying the date range. The API returns campaigns whose **scheduled start time** falls between (and including) the provided dates.

| Parameter | Description                                   | Type    | Example Value | Required |
| --------- | --------------------------------------------- | ------- | ------------- | -------- |
| `from`    | Start of the date range in `YYYYMMDD` format. | Integer | `20171201`    | Yes      |
| `to`      | End of the date range in `YYYYMMDD` format.   | Integer | `20171225`    | Yes      |

# Example Payload

The following is the sample payload:

```json
{
  "from": 20160101,
  "to": 20160101
}
```

# Example Request

The following is the sample request:

```curl
curl -X POST \
"https://in1.api.clevertap.com/1/targets/list.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" \
-d '{
  "from": 20160101,
  "to": 20160101
}'
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/targets/list.json")
request = Net::HTTP::Post.new(uri)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = "application/json"
request.body = { from: 20160101, to: 20160101 }.to_json

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```
```python
import requests

url = "https://in1.api.clevertap.com/1/targets/list.json"
headers = {
    "X-CleverTap-Account-Id": "ACCOUNT_ID",
    "X-CleverTap-Passcode": "PASSCODE",
    "Content-Type": "application/json",
}
payload = { "from": 20160101, "to": 20160101 }

response = requests.post(url, headers=headers, json=payload)
print(response.json())
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

$data = json_encode(array(
    "from" => 20160101,
    "to" => 20160101
));

$response = Requests::post(
    'https://in1.api.clevertap.com/1/targets/list.json',
    $headers,
    $data
);

echo $response->body;
?>
```
```javascript
const axios = require("axios");

const url = "https://in1.api.clevertap.com/1/targets/list.json";
const headers = {
  "X-CleverTap-Account-Id": "ACCOUNT_ID",
  "X-CleverTap-Passcode": "PASSCODE",
  "Content-Type": "application/json",
};

const data = { from: 20160101, to: 20160101 };

axios
  .post(url, data, { headers })
  .then((res) => console.log(res.data))
  .catch((err) => console.error(err.response?.data || err.message));
```

# Example Response

The following is the sample response:

```json
{
  "status": "success",
  "targets": [
    {
      "id": 1457429935,
      "name": "My API Campaign",
      "scheduled_on": 201601011508,
      "status": "pending"
    },
    {
      "id": 1457432766,
      "name": "My API Campaign 2",
      "scheduled_on": 201601011556,
      "status": "completed"
    }
  ]
}

```

# Response Parameters

The following is the list of response parameters:

| Field          | Description                                                                                                    | Type    |
| -------------- | -------------------------------------------------------------------------------------------------------------- | ------- |
| `id`           | Unique identifier for the campaign.                                                                            | Integer |
| `name`         | Name of the campaign as specified at creation.                                                                 | String  |
| `scheduled_on` | Scheduled date/time in `YYYYMMDDHHMM` (24-hour) local time.                                                    | Integer |
| `status`       | Current campaign status. Possible values: `scheduled`, `pending`, `running`, `paused`, `stopped`, `completed`. | String  |

> 📘 Date/Time Formats
>
> * **Request** (`from`, `to`): `YYYYMMDD` (e.g., `20250403`)
> * **Response** (`scheduled_on`): `YYYYMMDDHHMM` (e.g., `202504031544` -  April 3, 2025, 15:44 local time)

# Error Codes

The following is the list of error codes:

| HTTP Status | Error Code            | Description                                                                    | Example JSON Payload                                                                                                                                                       |
| ----------- | --------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **200**     | `success`             | Request succeeded. If no campaigns match the range, an empty list is returned. | `{"status":"success","targets":[]}`                                                                                                                                        |
| **400**     | `invalid_date_format` | `from` or `to` is not in `YYYYMMDD` format or not a valid date.                | `{"status":"fail","error":"Invalid \"from\" date (it must be of the format YYYYMMDD)","code":400}`                                                                         |
| **400**     | `invalid_range`       | Date range is invalid (e.g., `from` > `to`) or exceeds maximum allowed span.   | `{"status":"fail","error":"Invalid date range","code":400}`                                                                                                                |
| **401**     | `unauthorized`        | Authentication failed due to missing/invalid credentials.                      | `{"status":"fail","error":"Invalid Credentials","code":401}`                                                                                                               |
| **429**     | `rate_limit_exceeded` | Too many requests in a short period.                                           | `{"status":"fail","error":"Rate limit exceeded","code":429}`. For more information, refer to [API Request Limit](https://developer.clevertap.com/docs/api-request-limit) . |
| **500**     | `internal_error`      | Unexpected error occurred on the server.                                       | `{"status":"fail","error":"Internal server error","code":500}`                                                                                                             |

> 📘 Empty Responses
>
> A valid request may return `"status": "success"` with `"targets": []` if no campaigns are found in the given range.

# Notes

* Query manageable date ranges (≤ 31 days) to optimize performance.
* Use campaign `id` with **Get Campaign Report API** for detailed metrics.
* Rate limits apply. For details, see [API Request Limit](doc:api-request-limit).
