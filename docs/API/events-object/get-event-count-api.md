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
## Overview

This endpoint is used to retrieve counts for an event in a specified duration. For example, you can get the total number of charge events in the past day. 

## Base URL

Here is an example base URL from the account in the India region:
[https://in1.api.clevertap.com/1/counts/events.json](https://in1.api.clevertap.com/1/counts/events.json)

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload. 

| Parameter | Description | Required | Type | Example Value |
|-----------|-------------|----------|------|---------------|
| event_name | The name of the event. | required | string | "choseNewFavoriteFood" |
| event_properties | The event properties that you want to filter the results on. | optional | string key/value pairs | See example below |
| from | Start of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD. | required | int | 20150810 |
| to | End of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD. | required | int | 20151025 |

### Example event_properties

```json
"event_properties": [  
    {  
        "name": "value",  
        "operator": "contains",  
        "value": "piz"  
    }  
]
```

## Example Request

Here is an example cURL request to the *Get Event Count* API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"event_name":"choseNewFavoriteFood","event_properties":[{"name":"value","operator":"contains","value":"piz"}],"from":20150810,"to":20151025}' "https://in1.api.clevertap.com/1/counts/events.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/counts/events.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
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
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
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
    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
    'X-CleverTap-Passcode' => 'PASSCODE',
    'Content-Type' => 'application/json'
);
$data = '{"event_name":"choseNewFavoriteFood","event_properties":[{"name":"value","operator":"contains","value":"piz"}],"from":20150810,"to":20151025}';
$response = Requests::post('https://in1.api.clevertap.com/1/counts/events.json', $headers, $data);
```

```javascript
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
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

## Example Response

### Success Response

```json
{
  "status": "success",
  "count": 1434
}
```

### Partial Response

```json
{
  "status": "partial",
  "req_id": 1234567890123456789
}
```

### Error Response

```json
{
  "status": "fail",
  "error": "Error message"
}
```

## Notes

The response is a JSON object containing the key status, which might be success, partial, or fail.

If the status is success, there will be a count key with an int value of the count for the specified query. If the status is fail, there will be an error key with a string value and a HTTP status code.

If the status is partial, the query has not finished processing in our system. In the response, you will receive a req_id key with a long int value that you will use to poll for the result. After the query is completed, you will either receive a success response with the count if the query was successful, or a fail response with an error string and a HTTP status code. Please wait 30 seconds between polling requests.

After getting the req_id, you will poll the endpoint below and provide the value of req_id as a query parameter:

**GET** `https://in1.api.clevertap.com/1/counts/events.json?req_id={your_request_id_here}`

> 📘 Region
>
> This is an example base URL from the account in the India region. To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/get-event-count-api#region).

For more information on request limit, refer to [API Request Limit](doc:api-request-limit). To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).