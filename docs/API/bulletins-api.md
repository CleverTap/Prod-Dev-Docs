---
title: Bulletins API
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

The Bulletins API enables you to create a bulletin after triggering a business event.

# Base URL

Here is an example base URL from the account in the India region:\
[https://in1.api.clevertap.com/1/targets/trigger.json](https://in1.api.clevertap.com/1/targets/trigger.json)

## Region

Refer to [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

# HTTP Method

`POST`

# Headers

The following headers must be included with every API request:

| Header                   | Description                                                      | Required | Example            |
| ------------------------ | ---------------------------------------------------------------- | -------- | ------------------ |
| `X-CleverTap-Account-Id` | Your CleverTap account ID.                                       | Yes      | `ABCD-RSTU-1234`   |
| `X-CleverTap-Passcode`   | Passcode generated for the account to authenticate API requests. | Yes      | `XXXXXX`           |
| `Content-Type`           | Content type of the request payload. Must be `application/json`. | Yes      | `application/json` |

For more information, see [Headers](https://developer.clevertap.com/docs/common-api-components#headers).

# Body Parameters

The body is uploaded as a JSON payload. The payload is an object describing the event, including the business event name, business event properties, timestamp (when), and user identifier (c-by).

Event keys are limited to 120 characters, and property values are limited to 512 bytes.

| Parameter        | Description                                            | Type   | Required | Example Value                                                                |
| ---------------- | ------------------------------------------------------ | ------ | -------- | ---------------------------------------------------------------------------- |
| `business_event` | The business event that triggers the Bulletin.         | string | Yes      | `"Program Released"`                                                         |
| `name`           | The name of the Bulletin.                              | string | Yes      | `"Sa Re Ga Ma episode 12"`                                                   |
| `properties`     | Event properties describing the business event.        | map    | Yes      | `{ "program_id": "234567", "language":"Hindi", "prev_program_id":"123457" }` |
| `c-by`           | Creator of the Bulletin.                               | string | Yes      | `"jdoe@clevertap.com"`                                                       |
| `when`           | Time of the event. Can be `"now"` or a UNIX timestamp. | string | Yes      | `"now"` or `1724123456`                                                      |

> 🚧 Note
>
> CleverTap only supports **String** data type for mapped property values.

# Example Payload

The following example shows how to structure the request body when raising a Bulletin:

```json
{
  "business_event": "Program Released",
  "name": "Sa Re Ga Ma episode 12",
  "properties": {
    "program_id": "234567",
    "language": "Hindi",
    "prev_program_id": "123457"
  },
  "c-by": "jdoe@clevertap.com",
  "when": "now"
}
```

# Example Request

Here are the example requests to the *Bulletins* API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST \
"https://in1.api.clevertap.com/1/targets/trigger.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" \
-d '{
  "business_event": "Program Released",
  "name": "Sa Re Ga Ma episode 12",
  "properties": {
    "program_id": "234567",
    "language": "Hindi",
    "prev_program_id": "123457"
  },
  "c-by": "jdoe@clevertap.com",
  "when": "now"
}'
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/targets/trigger.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "business_event" => "Program Released",
  "name" => "Sa Re Ga Ma episode 12",
  "properties" => { "program_id" => "234567", "language" => "Hindi","prev_program_id": "123457"},
  "c-by" => "user@example.com",
  "when" => "now"
})

req_options = { use_ssl: uri.scheme == "https" }

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```
```python
import requests
import json

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json',
}

data = {
    "business_event": "Program Released",
    "name": "Sa Re Ga Ma episode 12",
    "properties": {
        "program_id": "234567",
        "language": "Hindi",
        "prev_program_id": "123457"
    },
    "c-by": "user@example.com",
    "when": "now"
}

response = requests.post(
    'https://in1.api.clevertap.com/1/targets/trigger.json',
    headers=headers,
    data=json.dumps(data)
)

print(response.text)
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
    "business_event" => "Program Released",
    "name" => "Sa Re Ga Ma episode 12",
    "properties" => array(
        "program_id" => "234567",
        "language" => "Hindi",
        "prev_program_id" => "123457"
    ),
    "c-by" => "user@example.com",
    "when" => "now"
));

$response = Requests::post(
    'https://in1.api.clevertap.com/1/targets/trigger.json',
    $headers,
    $data
);

echo $response->body;
```
```javascript
const request = require('request');

const headers = {
  'X-CleverTap-Account-Id': 'ACCOUNT_ID',
  'X-CleverTap-Passcode': 'PASSCODE',
  'Content-Type': 'application/json'
};

const data = JSON.stringify({
  "business_event": "Program Released",
  "name": "Sa Re Ga Ma episode 12",
  "properties": {
    "program_id": "234567",
    "language": "Hindi",
    "prev_program_id": "123457"
  },
  "c-by": "user@example.com",
  "when": "now"
});

const options = {
  url: 'https://in1.api.clevertap.com/1/targets/trigger.json',
  method: 'POST',
  headers: headers,
  body: data
};

request(options, (error, response, body) => {
  if (!error && response.statusCode == 200) {
    console.log(body);
  } else {
    console.error("Error:", error || body);
  }
});
```

# Example Response

```json
{
  "status": "success",
  "targets": 1
}
```

## Notes

The response is a JSON object containing either the success or failure status.

The `targets` key returns the number of Bulletins sent for the business event that has been raised.

## Errors

The API returns an error response if the request cannot be processed. The following are the expected error cases:

| Code | Error Message                          | Description                                                                | Sample JSON Payload                                                                      |
| ---- | -------------------------------------- | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| 400  | Business Event Not Found               | Raised if the business event is not defined on the dashboard.              | `{ "when": 1724123456, "event": "order_failed", "properties": { "order_id": "12345" } }` |
| 400  | `"when" has an incorrect value - null` | Raised if the `"when"` key is missing while raising the business event.    | `{ "event": "order_placed", "properties": { "order_id": "12345" } }`                     |
| 400  | Property `product_category` missing    | Raised if a required property is missing while raising the business event. | `{ "when": 1724123456, "event": "order_placed", "properties": { "order_id": "12345" } }` |
