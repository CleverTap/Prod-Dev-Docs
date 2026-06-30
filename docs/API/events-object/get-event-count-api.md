---
title: Get Event Count
excerpt: >-
  Retrieve counts for events in a specified duration using the CleverTap Get
  Event Count API endpoint.
deprecated: false
hidden: false
link:
  new_tab: false
metadata:
  title: ''
  description: ''
  robots: index
---
# Overview

This endpoint retrieves counts for an event in a specified duration. For example, you can get the total number of charge events in the past day.

# Base URL

Here is an example base URL from the account in the India region: [https://in1.api.clevertap.com/1/counts/events.json](https://in1.api.clevertap.com/1/counts/events.json)

## Region

Refer to [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

# HTTP Method

The initial query uses the POST method. Polling for results uses the GET method on the same endpoint path. See the Notes section below for details on polling.

# Headers

The headers required depend on the request type. The initial POST request requires all headers described in [Headers](https://developer.clevertap.com/docs/common-api-components#headers), including `Content-Type`.

The GET polling request requires only `X-CleverTap-Account-Id` and `X-CleverTap-Passcode`; `Content-Type` is not required for GET requests.

# Body Parameters

The body is uploaded as a JSON payload. The request body must be valid JSON. Unexpected fields are ignored. An empty or malformed JSON body returns HTTP 400 with a validation error.

The table below describes each body parameter along with its requirement status, type, and example value.

| Parameter        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Required | Type                   | Example Value          |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ---------------------- | ---------------------- |
| event_name       | The name of the event. Event names contain only alphanumeric characters, underscores, and hyphens, with a maximum length of 255 characters. Special characters and spaces are not allowed.                                                                                                                                                                                                                                                                                                                                                                                                                                          | required | string                 | "choseNewFavoriteFood" |
| event_properties | The event properties that you want to filter the results on. Each filter supports `name`, `operator`, and `value`. Supported `operator` values: `equals`, `contains`, `not_contains`, `in`, `not_in`, `gt`, `gte`, `lt`, `lte`, `exists`, `not_exists`. Multiple filters combine with AND logic, so all filters must match. An empty array or an omitted field applies no filtering. Value types vary by operator: `equals`, `contains`, and `not_contains` expect a string; `in` and `not_in` expect an array; `gt`, `gte`, `lt`, and `lte` expect a number or date string; `exists` and `not_exists` don't require a value field. | optional | string key/value pairs | See example below      |
| from             | Start of date range within which users should have performed the event you specified in event_name. Input values are formatted as integers in YYYYMMDD format, with exactly 8 digits and leading zeros for single-digit months and days (for example, January 5, 2024 is 20240105). The value of from must be less than or equal to. Invalid date ordering returns HTTP 400.                                                                                                                                                                                                                                                        | required | int                    | 20150810               |
| to               | End of date range within which users should have performed the event you specified in event_name. Follows the same date format and ordering rules as from.                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | required | int                    | 20151025               |

# Example Request

Here is an example cURL request to the _Get Event Count_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"event_name":"choseNewFavoriteFood","event_properties":[{"name":"value","operator":"contains","value":"piz"}],"from":20150810,"to":20151025}' "https://in1.api.clevertap.com/1/counts/events.json" \
-H "X-CleverTap-Account-Id: <ACCOUNT_ID>" \
-H "X-CleverTap-Passcode: <PASSCODE>" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/counts/events.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "<ACCOUNT_ID>"
request["X-Clevertap-Passcode"] = "<PASSCODE>"
request.body = JSON.dump({
  "event_name" => "choseNewFavoriteFood",
  "event_properties" => [
    {
      "name" => "value",
      "operator" => "contains",
      "value" => "piz"
    }
  ],
  "from" => 20150810,
  "to" => 20151025
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
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PASSCODE>',
    'Content-Type': 'application/json',
}

data = '{"event_name":"choseNewFavoriteFood","event_properties":[{"name":"value","operator":"contains","value":"piz"}],"from":20150810,"to":20151025}'

response = requests.post('https://in1.api.clevertap.com/1/counts/events.json', headers=headers, data=data)
```
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',
    'X-CleverTap-Passcode' => '<PASSCODE>',
    'Content-Type' => 'application/json'
);
$data = '{"event_name":"choseNewFavoriteFood","event_properties":[{"name":"value","operator":"contains","value":"piz"}],"from":20150810,"to":20151025}';
$response = Requests::post('https://in1.api.clevertap.com/1/counts/events.json', $headers, $data);
```
```javascript
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PASSCODE>',
    'Content-Type': 'application/json'
};

var dataString = '{"event_name":"choseNewFavoriteFood","event_properties":[{"name":"value","operator":"contains","value":"piz"}],"from":20150810,"to":20151025}';

var options = {
    url: 'https://in1.api.clevertap.com/1/counts/events.json',
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

# Example Payload

The following is the sample payload:

```json
{
    "event_name": "choseNewFavoriteFood",
    "event_properties": [
        {
            "name": "value",
            "operator": "contains",
            "value": "piz"
        }
    ],
    "from": 20150810,
    "to": 20151025
}
```

# Example Response

## Success Response

```json
{
  "status": "success",
  "count": 7138
}
```

## Partial Response

```json
{
  "status": "partial",
  "req_id": 1234567890123456789
}
```

## Error Response

```json
{
  "status": "fail",
  "error": "Error message"
}
```

# Notes

The response is a JSON object with a status key that can be success, partial, or fail.

If the status is success, the response includes a count key with an int value of the count for the specified query. If the status is fail, the response includes an error key with a string value and an HTTP status code.

If the status is partial, the query has not finished processing in the system. The response includes a req_id key with a long int value that you use to poll for the result. After the query completes, you receive either a success response with the count or a failure response with an error string and an HTTP status code. Wait 30 seconds between polling requests.

After getting the req_id, poll the endpoint below using the GET method, and provide the value of req_id as a query parameter. This polling request requires only the `X-CleverTap-Account-Id` and `X-CleverTap-Passcode` headers; `Content-Type` is not required since there's no request body.

**GET** `https://in1.api.clevertap.com/1/counts/events.json?req_id={your_request_id_here}`

<Callout icon="📘" theme="info">
  **Region**

  This is an example base URL from the account in the India region. To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/get-event-count-api#region).
</Callout>

For more information on the request limit, refer to [API Request Limit](doc:api-request-limit). To understand common queries and concerns about CleverTap APIs, refer to [API FAQs](doc:api-faqs).

# Error Codes

<br />

| HTTP Status | Code or Status | Error                                                   | Description                                                                                                 | Example Error Response                                                               |
| ----------- | -------------- | ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| 400         | fail           | Account credentials missing                             | Account-Id or Passcode header is present but cannot be parsed                                               | \{"status":"fail","error":"Account credentials missing"}                             |
| 400         | fail           | Payload is mandatory                                    | Request body is absent, empty, or not valid JSON                                                            | \{"status":"fail","error":"Payload is mandatory"}                                    |
| 400         | fail           | (query validation error string)                         | Body is valid JSON but the query fails semantic validation; the exact validation error is returned verbatim | \{"status":"fail","error":"[unknown event property]"}                                |
| 400         | fail           | Invalid request ID                                      | Polling req_id is expired or not found                                                                      | \{"status":"fail","error":"Invalid request ID"}                                      |
| 400         | fail           | Invalid query                                           | Polling request has no req_id, or an unhandled exception occurred during processing                         | \{"status":"fail","error":"Invalid query"}                                           |
| 400         | fail           | Error Processing, Please try again later                | Async processing failure (e.g. async context error mid-flight)                                              | \{"status":"fail","error":"Error Processing, Please try again later"}                |
| 401         | fail           | Invalid Credentials                                     | Account-Id or Passcode header value is incorrect                                                            | \{"status":"fail","error":"Invalid Credentials"}                                     |
| 429         | fail           | Too many concurrent requests                            | Per-account concurrency limit exceeded (infrastructure throttle)                                            | \{"status":"fail","error":"Too many concurrent requests"}                            |
| 429         | fail           | Invalid query blocking this because of too many retries | Request blocked by an internal per-account rate-limit control (LaunchDarkly-managed)                        | \{"status":"fail","error":"Invalid query blocking this because of too many retries"} |
| 500         | fail           | Server error                                            | Event Store returned an internal server error                                                               | \{"status":"fail","error":"Server error"}                                            |
| 503         | fail           | 12 digit account ID mandatory.                          | Account-Id header is missing from the request                                                               | \{"status":"fail","error":"12 digit account ID mandatory."}                          |
| 503         | fail           | Please come back later                                  | Global request limit exceeded, or async processing timed out                                                | \{"status":"fail","error":"Please come back later"}                                  |
| 503         | fail           | System under maintenance. Please retry later            | Event Store is unreachable or reports itself as down                                                        | \{"status":"fail","error":"System under maintenance. Please retry later"}            |

<br />
