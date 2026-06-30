---
title: Upload Events
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

The _Upload Events API_ provides you with the capability to upload user events to CleverTap. For example, you can use this API to store user events, such as product views and purchases. You associate these events with a specific user by using an identifier, such as an email address or a phone number. By storing user events in CleverTap, you can create richer segments and run targeted campaigns.

Uploading events to CleverTap requires a POST request with a JSON payload specifying the event information. A maximum of 1000 event records can be sent per API call.

While the maximum number of event types per account is 512, some events are reserved for system events, so you may not get to use all of them. For each event, the maximum number of properties is 100 by default, and this limit is configurable per account. The number of events submitted per account across those event types is unlimited.

For more information on request limits, refer to [API Request Limit](doc:api-request-limit).

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/upload`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

Events are uploaded as a JSON payload. The payload is an object keyed with "d" whose value is an array describing the event, including the event name, event properties, timestamp, and user identifier.

Event keys are limited to 1024 characters. Property values are validated up to 1024 characters at upload time.

For each event, a user identifier is required. This is the key that CleverTap uses to find the user whose profile needs to be updated. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId. If this identity is not found, a new user profile is created.

The following table lists the body parameters:

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
        type
      </td>

      <td>
        (Required) Set to event.
      </td>

      <td>
        string
      </td>

      <td>
        "event"
      </td>
    </tr>

    <tr>
      <td>
        evtName
      </td>

      <td>
        (Required) Name of event.
      </td>

      <td>
        string
      </td>

      <td>
        "Product Viewed"
      </td>
    </tr>

    <tr>
      <td>
        evtData
      </td>

      <td>
        (Optional) Event properties describing the event. Passed as key/value pairs. If omitted or empty, the record is still processed.
      </td>

      <td>
        object
      </td>

      <td>
        "evtData":  
        \{ "Product name": "Casio Watch", "Category": "Mens Watch" }
      </td>
    </tr>

    <tr>
      <td>
        ts
      </td>

      <td>
        (Optional) Date and time when the event occurred, formatted as a UNIX epoch in seconds. Defaults to the current timestamp if omitted.
      </td>

      <td>
        integer
      </td>

      <td>
        1468308340
      </td>
    </tr>

    <tr>
      <td>
        identity
      </td>

      <td>
        Identity to recognize a unique user. It can be the user's email, a phone number, or any other identifier you are using to tag your users.  
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        "[jack@gmail.com](mailto:jack@gmail.com)"
      </td>
    </tr>

    <tr>
      <td>
        FBID
      </td>

      <td>
        Identify a unique user by Facebook ID.

        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        "34322423"
      </td>
    </tr>

    <tr>
      <td>
        GPID
      </td>

      <td>
        Identify a unique user by Google Plus ID.

        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        "34322424"
      </td>
    </tr>

    <tr>
      <td>
        objectId
      </td>

      <td>
        Identify a unique user by CleverTap Global Object ID.

        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        "34322425"
      </td>
    </tr>
  </tbody>
</Table>

# Example Request

The following is a sample request:

```curl
curl -X POST -d '{ "d": [ { "FBID": "34322423", "ts": 1468308340, "type": "event", "evtName": "Product viewed", "evtData": { "Product name": "Casio Chronograph Watch", "Category": "Mens Watch", "Price": 59.99, "Currency": "USD" } } ] }' "https://<region>.api.clevertap.com/1/upload" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/upload")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json; charset=utf-8"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "d" => [
    {
      "FBID" => "34322423",
      "ts" => 1468308340,
      "type" => "event",
      "evtName" => "Product viewed",
      "evtData" => {
        "Product name" => "Casio Chronograph Watch",
        "Category" => "Mens Watch",
        "Price" => 59.99,
        "Currency" => "USD"
      }
    }
  ]
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
    'Content-Type': 'application/json; charset=utf-8',
}

data = '{ "d": [ { "FBID": "34322423", "ts": 1468308340, "type": "event", "evtName": "Product viewed", "evtData": { "Product name": "Casio Chronograph Watch", "Category": "Mens Watch", "Price": 59.99, "Currency": "USD" } } ] }'

response = requests.post('https://<region>.api.clevertap.com/1/upload', headers=headers, data=data)
```
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
    'X-CleverTap-Passcode' => 'PASSCODE',
    'Content-Type' => 'application/json; charset=utf-8'
);
$data = '{ "d": [ { "FBID": "34322423", "ts": 1468308340, "type": "event", "evtName": "Product viewed", "evtData": { "Product name": "Casio Chronograph Watch", "Category": "Mens Watch", "Price": 59.99, "Currency": "USD" } } ] }';
$response = Requests::post('https://<region>.api.clevertap.com/1/upload', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json; charset=utf-8'
};

var dataString = '{ "d": [ { "FBID": "34322423", "ts": 1468308340, "type": "event", "evtName": "Product viewed", "evtData": { "Product name": "Casio Chronograph Watch", "Category": "Mens Watch", "Price": 59.99, "Currency": "USD" } } ] }';

var options = {
    url: 'https://<region>.api.clevertap.com/1/upload',
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

The following is a sample JSON payload:

```json
{
    "d": [
        {
            "FBID": "34322423",
            "ts": 1468308340, // time when the event occurred in UNIX epoch value in seconds
            "type": "event",
            "evtName": "Product viewed",
            "evtData": {
                "Product name": "Casio Chronograph Watch",
                "Category": "Mens Watch",
                "Price": 59.99,
                "Currency": "USD"
            }
        },
        {
            "identity": "jack@gmail.com",
            "ts": 1468208340,
            "type": "event",
            "evtName": "Charged",
            "evtData": {
                "Amount": 300,
                "Currency": "USD",
                "Payment mode": "Credit Card",
                "Delivery By": "$D_1468322268",     
                "Items": [
                    {
                        "Category": "books",
                        "Book name": "The millionaire next door",
                        "Quantity": 1
                    },
                    {
                        "Category": "books",
                        "Book name": "Achieving inner zen",
                        "Quantity": 4
                    }
                ]
            }
        }
    ]
}
```

Only the Items array in Charged events accepts a nested structure.

# Example Response

The following is a sample response:

```json
{
    "status": "success",
    "processed": 1,
    "unprocessed": []
}
```

# Debugging

Requests with processing errors are returned in the API call response, as shown in the example below.

```json
{
    "status":<success, partial, fail>, 
    "processed":<count>, 
    "unprocessed": [{"status":"fail", "code":<error code>, "error":<error msg>, "record":<record>}]
}
```

To test if your data submits without errors, you can add the parameter `dryRun=1` to the URL. This validates the input without submitting the data to CleverTap.

# Notes

* You can also use the [Data Upload Script](https://github.com/CleverTap/clevertap-data-upload) to upload the event data to the CleverTap dashboard.
* A list of error codes for this API can be found on the [Errors](doc:api-errors) page.

To understand common queries and concerns about CleverTap APIs, refer to [API FAQs](doc:api-faqs).
