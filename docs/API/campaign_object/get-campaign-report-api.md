---
title: Get Campaign Report
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

The Get Campaign Report API retrieves metrics and metadata for a specific campaign.

* Supports **all campaign types** (Push, Email, SMS, In-App, Web, etc.).
* You must provide the `id` of the campaign.
* If the campaign has not **completed**, the API returns an error indicating its status.

<Callout icon="📘" theme="info">
  **Use Cases**

  * Integrate campaign reports into analytics dashboards.
  * Track delivery, click, and conversion metrics.
  * Monitor campaign health and optimize performance.
  * Automate reporting workflows.
</Callout>

# Base URL

The sample base URL for the India region:  
[https://in1.api.clevertap.com/1/targets/result.json](https://in1.api.clevertap.com/1/targets/result.json)

Refer to [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Authentication

Authentication is required using the account ID and passcode headers.

# Headers

| Header                   | Description                                                      | Required | Example            |
| ------------------------ | ---------------------------------------------------------------- | -------- | ------------------ |
| `X-CleverTap-Account-Id` | Your CleverTap account ID.                                       | Yes      | `ABCD-RSTU-1234`   |
| `X-CleverTap-Passcode`   | Passcode generated for the account to authenticate API requests. | Yes      | `XXXXXX`           |
| `Content-Type`           | Content type of the request payload. Must be `application/json`. | Yes      | `application/json` |

# HTTP Method

POST

# Body Parameters

The following table lists the body parameters:

| Parameter | Description                                                                                                                                                                                                                     | Type    | Example Value | Required |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ------------- | -------- |
| `id`      | The CleverTap campaign ID of the campaign whose report you want. Applies to all campaign types. You can find this ID in the **CleverTap Dashboard** under each campaign, or via the [Get Campaigns API](doc:get-campaigns-api). | Integer | `1457432766`  | Yes      |

## Sample Payload

The following is the sample payload:

```json
{
  "id": 1457432766
}
```

# Example Request

The following is the sample request:

```curl
curl -X POST \
"https://in1.api.clevertap.com/1/targets/result.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" \
-d '{
  "id": 1457432766
}'
```
```ruby
require 'net/http'
require 'json'
require 'uri'

uri = URI("https://in1.api.clevertap.com/1/targets/result.json")
request = Net::HTTP::Post.new(uri)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = "application/json"
request.body = { id: 1457432766 }.to_json

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```
```python
import requests

url = "https://in1.api.clevertap.com/1/targets/result.json"
headers = {
    "X-CleverTap-Account-Id": "ACCOUNT_ID",
    "X-CleverTap-Passcode": "PASSCODE",
    "Content-Type": "application/json",
}
payload = { "id": 1457432766 }

response = requests.post(url, headers=headers, json=payload)
if response.status_code == 200:
    print("Report:", response.json())
else:
    print("Error:", response.text)
```
```javascript
const axios = require("axios");

const url = "https://in1.api.clevertap.com/1/targets/result.json";
const headers = {
  "X-CleverTap-Account-Id": "ACCOUNT_ID",
  "X-CleverTap-Passcode": "PASSCODE",
  "Content-Type": "application/json",
};
const data = { id: 1457432766 };

axios.post(url, data, { headers })
  .then((res) => console.log("Report:", res.data))
  .catch((err) => console.error("Error:", err.response?.data || err.message));
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

$data = json_encode(array("id" => 1457432766));

$response = Requests::post(
  'https://in1.api.clevertap.com/1/targets/result.json',
  $headers,
  $data
);

echo $response->body;
?>
```

# Example Response

The following are the sample responses:

```json
{
  "status": "success",
  "result": {
    "sent": 82,
    "clicked": 28
  }
}
```

## Response Parameters

The following are the response parameters:

| Field     | Description                                                            | Type    |
| --------- | ---------------------------------------------------------------------- | ------- |
| `sent`    | Number of messages attempted to be sent.                               | Integer |
| `clicked` | Number of users who clicked the message (push, email, SMS with links). | Integer |

<Callout icon="📘" theme="info">
  **Notes**

  * Metrics are integers and cannot be negative.
  * Reports are **finalized only after campaign completion**.
  * For recurring campaigns, reports are available **per instance**.
  * If no data is available, fields may be omitted or set to `null`.
</Callout>

# Error Codes

The following are the error codes:

| HTTP Status | Error Code               | Description                                                                                                                                                                              | Example JSON Payload                                                      |
| ----------- | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **200**     | `success`                | Request successful and metrics returned.                                                                                                                                                 | `{"status":"success","result":{"sent":82}}`                               |
| **400**     | `invalid_payload`        | Malformed request body or missing `id`.                                                                                                                                                  | `{"status":"fail","error":"Missing campaign id","code":400}`              |
| **400**     | `invalid_id_format`      | The campaign ID is not a valid integer.                                                                                                                                                  | `{"status":"fail","error":"Invalid campaign id format","code":400}`       |
| **401**     | `unauthorized`           | Authentication failed due to missing/invalid credentials.                                                                                                                                | `{"status":"fail","error":"Invalid Credentials","code":401}`              |
| **403**     | `forbidden`              | The account does not have permission to use this API.                                                                                                                                    | `{"status":"fail","error":"Forbidden","code":403}`                        |
| **404**     | `report_not_found`       | Campaign ID does not exist or no report available.                                                                                                                                       | `{"status":"fail","error":"Report not found","code":404}`                 |
| **409**     | `campaign_not_completed` | The campaign has not completed, so report cannot be generated.                                                                                                                           | `{"status":"fail","error":"This target hasn't been completed""code":404}` |
| **429**     | `rate_limit_exceeded`    | Too many requests made in a short time. Limit: 60 requests/min per campaign. For more information, refer to [API Request Limit](https://developer.clevertap.com/docs/api-request-limit). | `{"status":"fail","error":"Rate limit exceeded","code":429}`              |
| **500**     | `internal_error`         | Unexpected error occurred on the server.                                                                                                                                                 | `{"status":"fail","error":"Internal server error","code":500}`            |

# Limitations

* Only one campaign ID can be requested per call.
* Bulk report retrieval is not supported.
* Recommended polling interval: **once every 1–5 minutes** per campaign.
* Reports are updated in near real-time but finalized only after completion.
