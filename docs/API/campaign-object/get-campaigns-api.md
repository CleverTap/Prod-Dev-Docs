---
title: Get Campaigns API
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

The Get Campaigns API lets you get a list of campaigns created using the API.

# Base URL

[https://location.api.clevertap.com/1/targets/list.json](https://location.api.clevertap.com/1/targets/list.json)

> 📘 Note
>
> Use the URL based on your location:
>
> * India - in1.api.clevertap.com
> * Singapore - sg1.api.clevertap.com
> * U.S - us1.api.clevertap.com

# HTTP Method

POST

# Headers

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

# Body Parameters

The body is uploaded as a JSON payload.\
Will return campaigns which have start time between the the date range.

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
        from
      </td>

      <td>
        Start of date range for events needed. Value specified in format YYYYMMDD.
      </td>

      <td>
        int
      </td>

      <td>
        20171201
      </td>
    </tr>

    <tr>
      <td>
        to
      </td>

      <td>
        End of date range for events needed. Value specified in format YYYYMMDD.
      </td>

      <td>
        int
      </td>

      <td>
        20171225
      </td>
    </tr>
  </tbody>
</Table>

Below is an example payload.

```json
{
    "from": 20160101,
    "to": 20160101
}
```

# Example Request

```curl
curl -X POST -d '{"from": 20160101,"to": 20160101}' "https://location.api.clevertap.com/1/targets/list.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://api.clevertap.com/1/targets/list.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "from" => 20160101,
  "to" => 20160101
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

data = '{"from": 20160101,"to": 20160101}'

response = requests.post('https://api.clevertap.com/1/targets/list.json', headers=headers, data=data)
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
$data = '{"from": 20160101,"to": 20160101}';
$response = Requests::post('https://api.clevertap.com/1/targets/list.json', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var dataString = '{"from": 20160101,"to": 20160101}';

var options = {
    url: 'https://api.clevertap.com/1/targets/list.json',
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

# Example Response

```json
{
    "status": "success",
    "targets": [
        {
            "id": 1457429935,
            "name": "My API Campaign",
            "scheduled_on": 201703081508,
            "status": "pending"
        },
        {
            "id": 1457432766,
            "name": "My API Campaign 2",
            "scheduled_on": 201603081556,
            "status": "completed"
        }
    ]
}
```
