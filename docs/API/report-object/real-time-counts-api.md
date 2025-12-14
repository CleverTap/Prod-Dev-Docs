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

This endpoint enables you to get a real-time count of active users in the past five minutes in your app. The results can be spliced by user_type, session_source, browser, os, and device for active and new users.

## Base URL

Here is an example base URL from the account in the India region:  
<https://in1.api.clevertap.com/1/now.json>

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload. An empty payload {} will return a real-time count of active users.

| Parameter | Description                                             | Required | Type    | Example Value |
| :-------- | :------------------------------------------------------ | :------- | :------ | :------------ |
| user_type | If set to true, will return the split of users by type. | optional | boolean | true          |

Below is an example payload.

```json
{
	"user_type": true
}
```

## Example Request

Here is an example cURL request to the _Real-Time Counts_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"user_type":true}' "https://in1.api.clevertap.com/1/now.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/now.json")
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

response = requests.post('https://in1.api.clevertap.com/1/now.json', headers=headers, data=data)
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
$response = Requests::post('https://in1.api.clevertap.com/1/now.json', $headers, $data);
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
    url: 'https://in1.api.clevertap.com/1/now.json',
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

```json
{
  "status": "success",
  "count": 214
}
```

Here is an example response if you set user_type parameter to true.

```json
{
  "status": "success",
  "count": 214,
  "user_type": {
    "new": 58
  }
}
```

## Notes

The response will be a JSON object containing either the success or fail status.

If no payload was provided and the request was successful, the response will include a count of active users in real-time.

```json
{
  "status": "success",
  "count": 214
}
```

For more information on request limit, refer to [API Request Limit](doc:api-request-limit).

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).