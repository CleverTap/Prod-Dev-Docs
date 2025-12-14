---
title: Stop Scheduled Campaign
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

The Stop Scheduled Campaign API lets you stop a scheduled or running campaign before completion. This is useful when campaigns need to be canceled, paused, or force-stopped due to misconfiguration or business logic.

## Base URL

Here is an example base URL from the account in the India region:  
<https://in1.api.clevertap.com/1/targets/stop.json>

Refer to [Region](https://developer.clevertap.com/docs/common-api-components#region) for region-specific URLs.

# HTTP Method

POST

# Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for required authentication headers.

# Body Parameters

The body is uploaded as a JSON payload.

| Parameter | Description                                                                                                                                                                                                                                                                      | Required/Optional | Type    | Example Value |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- | ------- | ------------- |
| id        | CleverTap ID of the campaign to be stopped. Available in the **dashboard (all campaign types)** and also visible in **campaign URLs** or returned via Get Campaigns API. For more information, refer to [Get Campaigns](https://developer.clevertap.com/docs/get-campaigns-api). | Required          | Integer | `1457432766`  |

# Example Payload

The following is the sample payload:

```json
{
  "id": 1457432766
}
```

## Parameter Validation Rules

| Rule            | Details                                                     |
| --------------- | ----------------------------------------------------------- |
| Format          | Must be a positive integer.                                 |
| Minimum Length  | Campaign ID is typically a 10-digit epoch-based identifier. |
| Negative Values | Not accepted.                                               |
| Invalid ID      | Returns a `"fail"` response with error details.             |
| Already Stopped | Returns `"success"`, but no action is performed.            |

# Example Request

The following is the sample request:

```curl
curl -X POST -d '{ "id": 1457432766 }' "https://in1.api.clevertap.com/1/targets/stop.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```python
import requests

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json',
}

data = '{ "id": 1457432766 }'
response = requests.post('https://in1.api.clevertap.com/1/targets/stop.json', headers=headers, data=data)

print(response.json())
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/targets/stop.json")
request = Net::HTTP::Post.new(uri)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = "application/json"
request.body = JSON.dump({ "id" => 1457432766 })

req_options = { use_ssl: uri.scheme == "https" }

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end

puts response.code
puts response.body
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
    'https://in1.api.clevertap.com/1/targets/stop.json',
    $headers,
    $data
);

echo $response->status_code;
echo $response->body;
```
```javascript
const request = require('request');

const headers = {
  'X-CleverTap-Account-Id': 'ACCOUNT_ID',
  'X-CleverTap-Passcode': 'PASSCODE',
  'Content-Type': 'application/json'
};

const data = JSON.stringify({ id: 1457432766 });

const options = {
  url: 'https://in1.api.clevertap.com/1/targets/stop.json',
  method: 'POST',
  headers: headers,
  body: data
};

request(options, (error, response, body) => {
  if (!error && response.statusCode == 200) {
    console.log(JSON.parse(body));
  } else {
    console.error(error || body);
  }
});
```

# Example Responses

The following is the sample response:

```json
{
  "status": "success",
  "campaign_id": 1457432766,
  "state": "stopped",
  "stopped_at": "2025-07-07T12:45:00Z"
}
```

## Campaign Already Stopped

```json
{
  "status": "success",
  "campaign_id": 1457432766,
  "state": "already_stopped"
}
```

## Invalid Campaign ID

```json
{
  "status": "fail",
  "error": "Invalid Campaign ID",
  "code": 400
}
```

## Authentication Failure

```json
{
  "status": "fail",
  "error": "Invalid Credentials",
  "code": 401
}
```

# Campaign States

The **Stop Campaign API** can be used to halt a campaign depending on its current state:

| **Campaign State**  | **Description**                                                                               | **Behavior When Stop API Is Called**                                                                                      |
| ------------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Scheduled**       | The campaign is created and scheduled for a future start time but hasn’t yet begun execution. | The campaign is canceled before execution begins. Users will not receive any messages.                                    |
| **Running**         | The campaign has started and is currently delivering messages to users.                       | The campaign is stopped mid-execution. Remaining deliveries are canceled, but messages already sent will not be recalled. |
| **Completed**       | The campaign has finished executing, all deliveries have been processed.                      | The stop request is ignored. The API returns `"success"` with `"state": "already_completed"`.                             |
| **Already Stopped** | The campaign was previously stopped and is not running.                                       | The API returns `"success"` with `"state": "already_stopped"`. No further action is taken.                                |

# Use Cases

- Cancel a campaign mistakenly scheduled with incorrect targeting.
- Stop a running campaign immediately if it contains sensitive or incorrect content.
- Integrate with monitoring/alerting systems to auto-stop campaigns when anomalies are detected.

# Rate Limiting

- This API follows the same rate limits as other campaign APIs.
- It is **3 requests per second per account**. 
- If rate limits are exceeded, a `429 Too Many Requests` error may occur.

# Error Codes

When an API request fails or returns an unexpected result, CleverTap provides both an HTTP status code and a CleverTap-specific error code to help identify the issue.

- HTTP Status Code: Indicates the overall result of the HTTP request (for example, 400 for bad request, 401 for unauthorized, etc.).
- Error Code: A CleverTap-specific numeric code that provides more granular information about the exact error condition.

The following table lists the possible combinations of HTTP status and error codes for this API:

| HTTP Status | Error Code | Description                                | Example Payload                                                                  |
| ----------- | ---------- | ------------------------------------------ | -------------------------------------------------------------------------------- |
| 400         | 21         | `"id"` is missing or invalid.              | `{ "status": "fail", "error": "Invalid Campaign ID", "code": 21 }`               |
| 401         | 101        | Invalid or missing authentication headers. | `{ "status": "fail", "error": "Invalid Credentials", "code": 101 }`              |
| 404         | 73         | Campaign not found in account.             | `{ "status": "fail", "error": "Campaign not found", "code": 73 }`                |
| 409         | 74         | Campaign already stopped or completed.     | `{ "status": "success", "campaign_id": 1457432766, "state": "already_stopped" }` |
| 429         | 88         | Too many requests (rate limit exceeded).   | `{ "status": "fail", "error": "Rate limit exceeded", "code": 88 }`               |

```json
{
    "status": "success"
}
```

For more information on request limit, refer to [API Request Limit](doc:api-request-limit). To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).