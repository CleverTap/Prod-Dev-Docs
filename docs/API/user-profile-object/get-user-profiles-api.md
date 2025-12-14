---
title: Get User Profiles API
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
## Overview

There are two ways to get user profiles. The first way is to download user profiles that have performed a specific event. For example, you can download a list of users that have launched your app in the past day. The second way to download user profiles is by requesting the specific users needed. The Get User Profiles API lets you download user profiles from CleverTap. 

# Get User Profiles by Events

Getting a list of user profiles by events requires two steps:

* With your first API call, specify the event name, date range, and batch size needed. This request returns a cursor you will use to get the list of user profile records in the [Get User Profiles Using Cursor](https://developer.clevertap.com/docs/get-user-profiles-api#section-get-user-profiles-using-cursor) section. 
* Make a second API call using the cursor from the [Get Cursor by Specifying Events for User Profiles](https://developer.clevertap.com/docs/get-user-profiles-api#section-get-cursor-by-specifying-events-for-user-profiles) section. This call returns the first batch of user profiles and another cursor to get the second batch of user profiles. You repeat this process until the cursor is not returned in the response, which means there are no more matching user profiles.

> 📘 Note
>
> The batch size denotes the maximum number of records that can be fetched in a single call. The response may vary.

## Get Cursor by Specifying Events for User Profiles

In this request, you specify the event name, date range, and batch size needed. This request returns a cursor, which you will use to get the list of user profiles in the [Get User Profiles Using Cursor](https://developer.clevertap.com/docs/get-user-profiles-api#section-get-user-profiles-using-cursor) section.

### Base URL

[https://api.clevertap.com/1/profiles.json](https://api.clevertap.com/1/profiles.json)

### HTTP Method

POST

### Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Refer to the [authentication guide](doc:authentication) to understand how to get their values.

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

### URL Parameter

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

### Body Parameters

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

### Example Request

```curl
curl -X POST -d '{"event_name":"App Launched","from":20171201,"to":20171225}' "https://api.clevertap.com/1/profiles.json?batch_size=50" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://api.clevertap.com/1/profiles.json?batch_size=50")
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
#  Recommended python version 2.7 
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

response = requests.post('https://api.clevertap.com/1/profiles.json', headers=headers, params=params, data=data)
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
$response = Requests::post('https://api.clevertap.com/1/profiles.json?batch_size=50', $headers, $data);
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
    url: 'https://api.clevertap.com/1/profiles.json?batch_size=50',
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

req, err := http.NewRequest("POST", "https://api.clevertap.com/1/profiles.json?batch_size=50", body)
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

### Example Response

```json
{
 "cursor":"AfljfgIJBgBnamF5Kz8NegcBAwxhbCe%2Fbmhhe04BBAVlYjT4YG5reQEATQQrai57K2oue04FAUhnd38%3D",
 "status":"success"
}
```

## Get User Profiles Using Cursor

Using the cursor from the [Get Cursor by Specifying Events for User Profiles](https://developer.clevertap.com/docs/get-user-profiles-api#section-get-cursor-by-specifying-events-for-user-profiles) section, you will make an API call that returns the first batch of user profiles and another cursor to get the second batch of user profiles. Repeat this process until a cursor is not returned in the response, which means there are no more matching user profiles.

### Base URL

[https://api.clevertap.com/1/profiles.json](https://api.clevertap.com/1/profiles.json)

### HTTP Method

GET

### Headers

The headers described below are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Refer to the [authentication guide](doc:authentication) to understand how to get their values.

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

### URL Parameter

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
        Required parameter. Specifies what set of user profiles to return.
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

### Example Request

```curl
curl "https://api.clevertap.com/1/profiles.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.clevertap.com/1/profiles.json?cursor=CURSOR")
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

params = (
    ('cursor', 'CURSOR'),
)

response = requests.get('https://api.clevertap.com/1/profiles.json', headers=headers, params=params)
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
$response = Requests::get('https://api.clevertap.com/1/profiles.json?cursor=CURSOR', $headers);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var options = {
    url: 'https://api.clevertap.com/1/profiles.json?cursor=CURSOR',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```
```go
req, err := http.NewRequest("GET", "https://api.clevertap.com/1/profiles.json?cursor=CURSOR", nil)
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

### Example Response

If there are more user profiles available, next\_cursor will be returned. The value for next\_cursor is used to get the next batch of user profiles. 

User profiles will be returned in a records object. The schema for the user profile object is documented in the section below.

```json
{
  "status": "success",
  "next_cursor": "ZiZjNwMEBQBkaWZzY2MuO20BBQFlYmN7ZG5hewYBTQVjb2BzZmphfwEABQUra2Jmeg%3D%3D",
  "records": [
    {
      "identity": "5555555555",
      "profileData": {
        "favoriteFood": "pizza"
      },
      "events": {
      "App Launched": {
        "count": 10,
        "first_seen": 1457271567,
        "last_seen": 1458041215
      },
      "Charged": {
        "count": 6,
        "first_seen": 1457962417,
        "last_seen": 1458041276
      }
      },
      "platformInfo": [
        {
          "platform": "iOS",
          "os_version": "10.2",
          "app_version": "6.1.3",
          "make": "Apple",
          "model": "iPhone7,2",
          "push_token": "95f98af6ad9a5e714a56c5bf527a78cb1e3eda18d2f23bc8591437d0d8ae71a3",
          "objectId": "-1a063854f83a4c6484285039ecff87cb"
        },
        {
          "platform": "Web",
          "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097"
        }
      ]
    }
  ]
}
```

If there are no more user profiles available, *next\_cursor* will not be returned, and a success response will be returned.

```json
{ "status" : "success"}
```

## User Profile Object

The following is the schema for the user profile object.

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
        email
      </td>

      <td>
        User’s email address.
      </td>
    </tr>

    <tr>
      <td>
        profileData
      </td>

      <td>
        User’s custom profile properties.
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
        name
      </td>

      <td>
        User’s name.
      </td>
    </tr>

    <tr>
      <td>
        platformInfo
      </td>

      <td>
        Array containing all platform names with the corresponding objectId (CleverTap ID), phone number, push\_token, app\_version, os\_version, make (device make), model (device model) if available.
      </td>
    </tr>
  </tbody>
</Table>

## Download User Profiles by ID

You can download user profiles by requesting the specific user needed. You can also retrieve a user’s profile and all the events performed by a specific user. Supported identity values are email, a custom identity, or the CleverTap objectId.

### Base URL

[https://api.clevertap.com/1/profile.json](https://api.clevertap.com/1/profile.json)

### HTTP Method

GET

### Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Refer to the [authentication guide](doc:authentication) to understand how to get their values.

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

### URL Parameters

One of the three parameters documented below needs to be included in your request to specify the user profile needed.

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
        email
      </td>

      <td>
        User’s email address.
      </td>

      <td>
        string
      </td>

      <td>
        “[jack@gmail.com](mailto:jack@gmail.com)”
      </td>
    </tr>

    <tr>
      <td>
        identity
      </td>

      <td>
        Custom user identity defined by you.
      </td>

      <td>
        string
      </td>

      <td>
        “5555555555”
      </td>
    </tr>

    <tr>
      <td>
        objectId
      </td>

      <td>
        CleverTap ID.
      </td>

      <td>
        string
      </td>

      <td>
        “1a063854f83a4c6484285039ecff87cb”
      </td>
    </tr>
  </tbody>
</Table>

### Example Request

```curl
curl "https://api.clevertap.com/1/profile.json?email=jack@gmail.com" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.clevertap.com/1/profile.json?email=jack@gmail.com")
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

params = (
    ('email', 'jack@gmail.com'),
)

response = requests.get('https://api.clevertap.com/1/profile.json', headers=headers, params=params)
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
$response = Requests::get('https://api.clevertap.com/1/profile.json?email=jack@gmail.com', $headers);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var options = {
    url: 'https://api.clevertap.com/1/profile.json?email=jack@gmail.com',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```
```go
req, err := http.NewRequest("GET", "https://api.clevertap.com/1/profile.json?email=jack@gmail.com", nil)
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

### Example Response

The user profile returns in a record object. The schema for the user profile object is documented below.

```json
{
  "status": "success",
  "record": {
    "email": "jack@gmail.com",
    "profileData": {
      "Last Score": 308,
      "High Score": 308,
      "Replayed": true
    },
    "events": {
      "App Launched": {
        "count": 10,
        "first_seen": 1457271567,
        "last_seen": 1458041215
      },
      "Charged": {
        "count": 6,
        "first_seen": 1457962417,
        "last_seen": 1458041276
      }
    },
    "platformInfo": [
      {
        "platform": "iOS",
        "os_version": "10.2",
        "app_version": "6.1.3",
        "make": "Apple",
        "model": "iPhone7,2",
        "push_token": "95f98af6ad9a5e714a56c5bf527a78cb1e3eda18d2f23bc8591437d0d8ae71a3",
        "objectId": "-1a063854f83a4c6484285039ecff87cb"
      },
      {
        "platform": "Web",
        "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097"
      }
    ]
  }
}
```

### Additional Notes

* API errors are described on [this page](doc:api-errors).
* Requests are limited to three (3) concurrent requests. If you exceed this limit, a 429 error returns. Try again after 10 minutes.
