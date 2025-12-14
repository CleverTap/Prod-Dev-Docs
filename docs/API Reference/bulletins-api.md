---
title: Bulletins API
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
# Overview

This endpoint enables you to raise a Bulletin after a business event is triggered.

## Base URL

[https://location.api.clevertap.com/1/targets/trigger.json](https://location.api.clevertap.com/1/targets/trigger.json)

> 📘 Note
>
> Use the URL based on your location:
>
> * India - in1.api.clevertap.com
> * Singapore - sg1.api.clevertap.com
> * U.S - us1.api.clevertap.com

## HTTP Method

POST

## Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Header
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        X-CleverTap-Account-Id
      </td>

      <td>
        Your CleverTap Account ID.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Account-Id: ACCOUNT\_ID"
      </td>
    </tr>

    <tr>
      <td>
        X-CleverTap-Passcode
      </td>

      <td>
        Your CleverTap Account Passcode.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Passcode: PASSCODE"
      </td>
    </tr>

    <tr>
      <td>
        Content-Type
      </td>

      <td>
        Request content-type is always set to application/json.
      </td>

      <td>
        string
      </td>

      <td>
        "Content-Type: application/json"
      </td>
    </tr>
  </tbody>
</Table>

## Body Parameters

The body is uploaded as a JSON payload. The payload is an object keyed with “d” whose value is an array describing the event including the event name, event properties, timestamp, and user identifier.

Event keys are limited to 120 characters and property values are limited to 512 bytes.

For each event, a user identifier is required. This is the key that CleverTap uses to find the user whose profile needs to be updated.

> 🚧 Note:
>
> CleverTap only supports String Data type for mapped property values.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        business\_event
      </td>

      <td>
        The business event need to trigger the Bulletin. (Required)
      </td>

      <td>
        string
      </td>

      <td>
        "Program Released"
      </td>
    </tr>

    <tr>
      <td>
        name
      </td>

      <td>
        The name of the Bulletin. (Required)
      </td>

      <td>
        string
      </td>

      <td>
        Sa Re Ga Ma episode 12
      </td>
    </tr>

    <tr>
      <td>
        properties
      </td>

      <td>
        Event properties describing the business event. (Required)
      </td>

      <td>
        string
      </td>

      <td>
        "properties" :\
        \{ "program\_id" : "234567", "language":"Hindi",  "prev\_program\_id":"123457"
      </td>
    </tr>

    <tr>
      <td>
        c-by
      </td>

      <td>
        Creator of the Bulletin. (Required)
      </td>

      <td>
        string
      </td>

      <td>
        "[jdoe@clevertap.com](mailto:jdoe@clevertap.com)"
      </td>
    </tr>
  </tbody>
</Table>

Below is an example payload.

```json
{
    "business_event" : "Program Released",
    "name" : "Sa Re Ga Ma episode 12",
    "properties" : {
        "program_id" : "234567",
        "language":"Hindi",
        "prev_program_id":"123457"
    },
    "c-by" : "jdoe@clevertap.com",
    "when":"now"
}
```

## Example Request

```curl
curl -X POST -d '{"business_event":true"}' "https://location.api.clevertap.com/1/targets/trigger.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://location.api.clevertap.com/1/targets/trigger.json")
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

response = requests.post('https://location.api.clevertap.com/1/targets/trigger.json', headers=headers, data=data)
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
$response = Requests::post('https://location.api.clevertap.com/1/targets/trigger.json', $headers, $data);
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
    url: 'https://location.api.clevertap.com/1/targets/trigger.json',
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
    "targets": 1
}
```

## Notes

The response will be a JSON object containing either the success or fail status.

The targets key returns the number of Bulletins that have been sent for the Business event that has been raised. 

## Expected errors

```json
{
    "status": "fail",
    "error": "Business Event Not Found",
    "code": 400
}
```

When the Business event raised is not defined on the dashboard.

```json
{
    "status": "fail",
    "error": "\"when\" has an incorrect value - null",
    "code": 400
}
```

The above error occurs when the "when" key is not defined while raising the business event.

```json
{
    "status": "fail",
    "error": "Propertyproduct_categorymissing",
    "code": 400
}
```

The above error occurs when a property is missing while you are raising the business event.
