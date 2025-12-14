---
title: Get Event Count
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
## Overview

This endpoint is used to retrieve counts for an event in a specified duration. For example, you can get the total number of charge events in the past day. 

## Base URL

Here is an example base URL from the account in the India region:  
<https://in1.api.clevertap.com/1/counts/events.json>

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload. 

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Required",
    "h-3": "Type",
    "h-4": "Example Value",
    "0-0": "event_name",
    "0-1": "The name of the event.",
    "0-2": "required",
    "0-3": "string",
    "0-4": "\"choseNewFavoriteFood\"",
    "1-0": "event_properties",
    "1-1": "The event properties that you want to filter the results on.",
    "1-2": "optional",
    "1-3": "string key/value pairs",
    "1-4": "\"event_properties\": [  \n        {  \n            \"name\": \"value\",  \n            \"operator\": \"contains\",  \n            \"value\": \"piz\"  \n        }  \n    ]",
    "2-0": "from",
    "2-1": "Start of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.",
    "2-2": "required",
    "2-3": "int",
    "2-4": "20150810",
    "3-0": "to",
    "3-1": "End of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.",
    "3-2": "required",
    "3-3": "int",
    "3-4": "20151025"
  },
  "cols": 5,
  "rows": 4,
  "align": [
    "left",
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]

Below is an example payload.

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



## Example Request

Here is an example cURL request to the _Get Event Count_ API showing the headers needed to authenticate the request from the account in the India region:

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
```javascript Node.js
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

```json
{
  "status": "success",
  "count": 7138
}
```



## Notes

The response is a JSON object containing the key status, which might be success, partial, or fail.

If the status is success, there will be a count key with an int value of the count for the specified query. If the status is fail, there will be an error key with a string value and a HTTP status code.

If the status is partial, the query has not finished processing in our system. In the response, you will receive a req_id key with a long int value that you will use to poll for the result. After the query is completed, you will either receive a success response with the count if the query was successful, or a fail response with an error string and a HTTP status code. Please wait 30 seconds between polling requests.

Here is an example response for a partial status.

```json
{
  "req_id": 384649162721759,
  "status": "partial"
}
```



After getting the req_id, you will poll the endpoint below and provide the value of req_id as a query parameter.

GET <https://in1.api.clevertap.com/1/counts/events.json?req_id=>\<your_request_id_here>

> 📘 Region
> 
> This is an example base URL from the account in the India region. To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/get-event-count-api#region).

For more information on request limit, refer to [API Request Limit](doc:api-request-limit). To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).