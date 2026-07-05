---
title: Real-Time Counts
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

This endpoint retrieves a real-time count of active users in your app over the past five minutes. The results can be split by `user_type`, `session_source`, `browser`, `os`, and `device` for active and new users.

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/now.json`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload. An empty payload `{}` returns a real-time count of all active users with no breakdown. Set any of the parameters below to `true` to include a breakdown by that dimension.

The following table lists the body parameters.

| Parameter       | Description                                                                    | Required | Type    | Example Value |
| :-------------- | :----------------------------------------------------------------------------- | :------- | :------ | :------------ |
| user\_type      | When `true `, returns a breakdown of active users by type (new vs. returning). | Optional | boolean | `true`        |
| session\_source | When `true`, returns a breakdown of active users by session source.            | Optional | boolean | `true`        |
| browser         | When `true`, returns a breakdown of active users by browser.                   | Optional | boolean | `true`        |
| os              | When `true`, returns a breakdown of active users by operating system.          | Optional | boolean | `true`        |
| device          | When `true`, returns a breakdown of active users by device type.               | Optional | boolean | `true`        |

The following is a sample payload requesting multiple breakdowns at once.

```json
{
    "user_type": true,
    "session_source": true,
    "browser": true,
    "os": true,
    "device": true
}
```

## Example Request

The following is a sample request to the _Real-Time Counts_ API, showing the headers needed to authenticate the request.

```curl
curl -X POST -d '{"user_type":true}' "https://<region>.api.clevertap.com/1/now.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/now.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "user_type" => true
})

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

data = '{"user_type":true}'

response = requests.post('https://<region>.api.clevertap.com/1/now.json', headers=headers, data=data)
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
$data = '{"user_type":true}';
$response = Requests::post('https://<region>.api.clevertap.com/1/now.json', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var dataString = '{"user_type":true}';

var options = {
    url: 'https://<region>.api.clevertap.com/1/now.json',
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

## Example Response

The following is a sample response with no breakdown parameters set (empty payload).

```json
{
  "status": "success",
  "count": 214
}
```

The following is a sample response when all breakdown parameters are set to `true`.

```json
{
  "status": "success",
  "count": 214,
  "user_type": {
    "new": 58,
    "returning": 156
  },
  "session_source": {
    "organic": 120,
    "paid": 94
  },
  "browser": {
    "Chrome": 98,
    "Safari": 72,
    "Firefox": 44
  },
  "os": {
    "Android": 130,
    "iOS": 84
  },
  "device": {
    "mobile": 170,
    "tablet": 44
  }
}
```

Each breakdown key is only present in the response when its corresponding parameter is set to `true` in the request.

## Response Fields

The following table describes the response fields.

| Field           | Type    | Description                                                                                                                             |
| :-------------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------------- |
| status          | string  | `"success"` or `"fail"`.                                                                                                                |
| count           | integer | Total number of active users in the past 5 minutes. Always present on success.                                                          |
| user\_type      | object  | Breakdown of active users by type (for example, `"new"`, `"returning"`). Present only when `user_type: true` was passed in the request. |
| session\_source | object  | Breakdown of active users by session source. Present only when `session_source: true` was passed in the request.                        |
| browser         | object  | Breakdown of active users by browser. Present only when `browser: true` was passed in the request.                                      |
| os              | object  | Breakdown of active users by operating system. Present only when `os: true` was passed in the request.                                  |
| device          | object  | Breakdown of active users by device type. Present only when `device: true` was passed in the request.                                   |
| error           | string  | Error message. Present only when `status` is `"fail"`.                                                                                  |

For more information on request limits, refer to [API Request Limit](doc:api-request-limit). To understand common queries and concerns about CleverTap APIs, refer to the [API FAQs](doc:api-faqs).

## Error Codes

The following is the list of error codes returned by the API:

| HTTP Status | Error                                        | Description                                                                           | Example Error Response                                                            |
| ----------- | -------------------------------------------- | ------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| 400         | `"Incomplete request body"`                  | Request body could not be read due to an I/O error                                    | `{"status":"fail","error":"Incomplete request body","code":400}`                  |
| 400         | `"Failed to process request."`               | Unexpected error during request setup                                                 | `{"status":"fail","error":"Failed to process request.","code":400}`               |
| 400         | `"Error Processing, Please try again later"` | Async processing failure                                                              | `{"status":"fail","error":"Error Processing, Please try again later","code":400}` |
| 401         | `"Invalid credentials"`                      | Account ID not found or passcode is incorrect                                         | `{"status":"fail","error":"Invalid credentials","code":401}`                      |
| 403         | `"API access has been temporally suspended"` | Account's API access has been suspended                                               | `{"status":"fail","error":"API access has been temporally suspended","code":403}` |
| 429         | `"Too many concurrent requests"`             | Per-account concurrent request limit exceeded. Retry after 120 seconds.               | `{"status":"fail","error":"Too many concurrent requests","code":429}`             |
| 500         | `"Please try again"`                         | Dashboard request timed out or failed after 3 retry attempts                          | `{"status":"fail","error":"Please try again","code":500}`                         |
| 500         | `"Failed to process request"`                | Unexpected critical server-side failure                                               | `{"status":"fail","error":"Failed to process request","code":500}`                |
| 503         | `"12 digit account ID mandatory."`           | `X-CleverTap-Account-Id` header is missing from the request                           | `{"status":"fail","error":"12 digit account ID mandatory.","code":503}`           |
| 503         | `"Please come back later"`                   | Server at global capacity limit, warming up, or async timeout. Retry after 2 minutes. | `{"status":"fail","error":"Please come back later","code":503}`                   |

<br />
