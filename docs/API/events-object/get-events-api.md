---
title: Get Events API
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
The Get Events API lets you download user events from CleverTap. 

For example, you can use this API to get a list App Launch or Purchase events. 

# Overview

Getting a list of user events requires two steps:

* With your first API call you will specify the event name, date range, and batch size needed. This request will return a cursor, which you will use to get the list of event records in step 2. 
* You will make second API call using the cursor from step 1. This call will return the first batch of events and and another cursor to get the second batch of events. You will repeat this process until the cursor is not returned in the response, which means there are no more events.

> 📘 Note
>
> The batch size denotes the maximum number of records that can be fetched in a single call. The response may vary.

# Step 1: Get Cursor by Specifying Events Needed

In this request, you will specify the event name, date range, and batch size needed. This request will return a cursor, which you will use to get the list of events in the step 2.

## Base URL

[https://api.clevertap.com/1/events.json](https://api.clevertap.com/1/events.json)

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

## URL Parameter

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
        batch\_size
      </td>

      <td>
        The number of results to return in each API call. The maximum is 5000.
      </td>

      <td>
        int
      </td>

      <td>
        50
      </td>
    </tr>

    <tr>
      <td>
        app
      </td>

      <td>
        Optional parameter. Set to true to receive app fields in the response This will return the OS version, device make, device model, app version within the profile object.
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        events
      </td>

      <td>
        Optional parameter. Includes the event summary fields in the response. The default value is true. Set to false to exclude event summary fields in the response.
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        profile
      </td>

      <td>
        Optional parameter. Includes the custom profile properties in the response. The default value is true. Set to false to exclude custom profile properties fields in the response.
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>
  </tbody>
</Table>

## Body Parameters

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
        event\_name
      </td>

      <td>
        The event type needed. You can get both standard and custom events from this endpoint. The standard events include App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Notification Sent, Product Viewed, and UTM Visited.
      </td>

      <td>
        string
      </td>

      <td>
        “App Launched”
      </td>
    </tr>

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

## Example Request

```curl
curl -X POST -d '{"event_name":"App Launched","from":20171201,"to":20171225}' "https://api.clevertap.com/1/events.json?batch_size=50" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://api.clevertap.com/1/events.json?batch_size=50")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "event_name" => "App Launched",
  "from" => 20171201,
  "to" => 20171225
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

params = (
    ('batch_size', '50'),
)

data = '{"event_name":"App Launched","from":20171201,"to":20171225}'

response = requests.post('https://api.clevertap.com/1/events.json', headers=headers, params=params, data=data)
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
$data = '{"event_name":"App Launched","from":20171201,"to":20171225}';
$response = Requests::post('https://api.clevertap.com/1/events.json?batch_size=50', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var dataString = '{"event_name":"App Launched","from":20171201,"to":20171225}';

var options = {
    url: 'https://api.clevertap.com/1/events.json?batch_size=50',
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
```go
type Payload struct {
	EventName string `json:"event_name"`
	From      int    `json:"from"`
	To        int    `json:"to"`
}

data := Payload{
// fill struct
}
payloadBytes, err := json.Marshal(data)
if err != nil {
	// handle err
}
body := bytes.NewReader(payloadBytes)

req, err := http.NewRequest("POST", "https://api.clevertap.com/1/events.json?batch_size=50", body)
if err != nil {
	// handle err
}
req.Header.Set("X-Clevertap-Account-Id", "ACCOUNT_ID")
req.Header.Set("X-Clevertap-Passcode", "PASSCODE")
req.Header.Set("Content-Type", "application/json")

resp, err := http.DefaultClient.Do(req)
if err != nil {
	// handle err
}
defer resp.Body.Close()
```

## Example Response

```json
{ "cursor" : "AfljfgIJBgBnamF5Kz8NegcBAwxhbCe%2Fbmhhe04BBAVlYjT4YG5reQEATQQrai57K2oue04FAUhnd38%3D" , "status" : "success"}
```

> 🚧 Note
>
> Currently, we do not support exporting the following events via this API:
>
> * Notification Sent
> * Notification Bounce
> * Notification  Viewed
> * Notification Control Group
> * Notification Rendered
> * Notification System Control Group 
>
> To export these events we have multiple other ways such as [S3 Export](https://docs.clevertap.com/docs/data-export-to-aws-s3), [GCP Export](https://docs.clevertap.com/docs/data-export-to-gcp), or [download from the dashboard](https://docs.clevertap.com/docs/find-people).

# Step 2: Get Events Using Cursor

Using the cursor from step 1, you will now make an API call that will return the first batch of events and and another cursor to get the second batch of events. You will repeat this process until a cursor is not returned in the response, which means there are no more events.

## Base URL

[https://api.clevertap.com/1/events.json](https://api.clevertap.com/1/events.json)

## HTTP Method

GET

## Headers

The headers described below are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

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

## URL Parameters

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
        cursor
      </td>

      <td>
        Required parameter. Specifies what set of events to return.
      </td>

      <td>
        string
      </td>

      <td>
        ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6
      </td>
    </tr>
  </tbody>
</Table>

## Example Request

```curl
curl "https://api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.clevertap.com/1/events.json?cursor=CURSOR")
request = Net::HTTP::Get.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"

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

response = requests.get('https://api.clevertap.com/1/events.json?cursor=CURSOR', headers=headers)
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
$response = Requests::get('https://api.clevertap.com/1/events.json?cursor=CURSOR', $headers);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var options = {
    url: 'https://api.clevertap.com/1/events.json?cursor=CURSOR',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```

## Example Response

If there are more events available, next\_cursor will be returned. The value for next\_cursor is used to get the next batch of events. 

Events will be returned in a records object. The schema for the Event object is provided in the next section.

```json
{
    "status": "success",
    "next_cursor": "ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6",
    "records": [
        {
            "profile": {
                "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097",
                "platform": "Web",
                "email": "peter@foo.com",
                "profileData": {
                    "favoriteColor": "blue"
                },
                "identity": "5555555555",
                "id": 33
            },
            "ts": 20151023140416,   // yyyyMMddHHmmSS format
            "event_props": {
                "value": "pizza"
            },
            "session_props": {
                "utm_source": "Foo",
                "utm_medium": "Bar",
                "utm_campaign": "FooBar",
                "session_source": "Direct"
            }
        },
        {
            "profile": {
                "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097",
                "platform": "Web",
                "email": "peter@foo.com",
                "profileData": {
                    "favoriteColor": "blue"
                },
                "identity": "5555555555",
                "id": 33
            },
            "ts": 20151024121636,
            "event_props": {
                "value": "pizza"
            },
            "session_props": {
                "session_source": "Others",
                "session_referrer": "http://example.com"
            }
        }
    ]
}
```

If there are no more events available, next\_cursor will not be returned and a success response will be returned.

```json
{ "status" : "success"}
```

If the data for the cursor is not ready, you'll get the following response :

```json
{
   "status": "fail",
   "error": "Request still in progress, please retry later",
   "code": 2
}
```

# Event Object

Here is the schema for the Events object.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Key
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        objectId
      </td>

      <td>
        CleverTap ID of the user.
      </td>
    </tr>

    <tr>
      <td>
        profileData
      </td>

      <td>
        All custom profile properties of the user.
      </td>
    </tr>

    <tr>
      <td>
        identity
      </td>

      <td>
        Custom user identity provided by you.
      </td>
    </tr>

    <tr>
      <td>
        ts
      </td>

      <td>
        Event timestamp in yyyyMMddHHmmSS format.
      </td>
    </tr>

    <tr>
      <td>
        event\_props
      </td>

      <td>
        All event properties and their values.
      </td>
    </tr>

    <tr>
      <td>
        session\_props
      </td>

      <td>
        Possible values are utm\_source, utm\_medium, utm\_campaign, session\_source, and session\_referrer for the event. These are only provided if they were set.
      </td>
    </tr>

    <tr>
      <td>
        session\_source
      </td>

      <td>
        Possible values are Direct, Search, Social, Email, and Batch. These are only provided if they were set. If unset, the source is Unavailable, and Others if there is a custom referrer (session\_referrer field contains the referrer value if source is Others).
      </td>
    </tr>
  </tbody>
</Table>

# Notes

* You can only define single event type with each request for events.
* API errors are described on the [errors page](doc:api-errors).
