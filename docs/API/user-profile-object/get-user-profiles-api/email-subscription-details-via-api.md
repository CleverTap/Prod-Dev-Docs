---
title: Get User Profiles
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The Get User Profiles API enables you to fetch detailed information about user profiles, including profile properties, events, and platform data. You can use it to retrieve either a single user profile or multiple profiles using cursor-based pagination.

There are two ways to get user profiles:

* **[By event](doc:subscription-api#get-user-profiles-by-events)**: Download profiles of users who have performed a specific event. For example, you can fetch a list of users who launched your app in the past day.
* **[By ID](doc:subscription-api#download-user-profiles-by-id)**: Request specific user profiles directly.

All endpoints in the Get User Profiles API require a region-specific base URL. Use the same region endpoint consistently across the entire workflow, including Get Cursor, Get Using Cursor, and Download by ID.

# Authentication

All requests must include the following headers:

| Header                 | Description                                                 |
| ---------------------- | ----------------------------------------------------------- |
| X-CleverTap-Account-Id | Your CleverTap Account ID.                                  |
| X-CleverTap-Passcode   | CleverTap Account Passcode associated with your Account ID. |
| Content-Type           | Must be set to `application/json`.                          |

# Get User Profiles by Events

Getting a list of user profiles by events requires two steps:

* With your first API call, specify the event name, date range, and batch size needed. This request returns a cursor you will use to get the list of user profile records in the [Get User Profiles Using Cursor](doc:get-user-profiles-api#get-cursor-by-specifying-events-for-user-profiles) section.
* Make a second API call using the cursor from the [Get Cursor by Specifying Events for User Profiles](doc:subscription-api#get-cursor-by-specifying-events-for-user-profiles) section. This call returns the first batch of user profiles and another cursor to get the second batch of user profiles. You repeat this process until the cursor is not returned in the response, meaning no more matching user profiles exist.

> 📘 Batch Size
>
> The batch size denotes the maximum number of records that can be fetched in a single call. The batch size should be provided in multiples of 23 (23, 46, 69....) as there is a total of 23 memory buckets available in CleverTap's data storage mechanism.

## Get Cursor by Specifying Events for User Profiles

This API returns a cursor that you can use to fetch user profiles that performed specific events.

### Base URL

Here is the sample base URL from the account in the India region:

[https://in1.api.clevertap.com/1/profiles.json](https://\<region>.api.clevertap.com/1/profiles.json)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

### HTTP Method

POST

### Query Parameters

| Parameter  | Description                                                                                                                                                                   | Type    | Example Value |
| :--------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | :------------ |
| batch_size | The number of results to return in each API call. The maximum is 5000.                                                                                                        | int     | 50            |
| app        | Optional parameter. Set to true to receive app fields in the response. This will return the OS version, device make, device model, and app version within the profile object. | boolean | true          |
| events     | Optional parameter. Includes the event summary fields in the response. The default value is true. Set to false to exclude event summary fields in the response.               | boolean | true          |
| profile    | Optional parameter. The response includes the custom profile properties. The default value is true. Set to false to exclude custom profile properties fields in the response. | boolean | true          |

### Body Parameters

| Parameter  | Description                                                                                                                                                                                                                                        | Type   |                |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | :------------- |
| event_name | The event type needed. You can get both standard and custom events from this endpoint. The standard events include App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Notification Sent, Product Viewed, and UTM Visited. | string | “App Launched” |
| from       | Start of date range for events needed. Value specified in format YYYYMMDD.                                                                                                                                                                         | int    | 20171201       |
| to         | End of date range for events needed. Value specified in format YYYYMMDD.                                                                                                                                                                           | int    | 20171225       |

### Sample Request

Here is an example cURL request to the Get User Profiles API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"event_name":"App Launched","from":20171201,"to":20171225}' "https://in1.api.clevertap.com/1/profiles.json?batch_size=50" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/profiles.json?batch_size=50")
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

response = requests.post('https://in1.api.clevertap.com/1/profiles.json', headers=headers, params=params, data=data)
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
$response = Requests::post('https://in1.api.clevertap.com/1/profiles.json?batch_size=50', $headers, $data);
```
```node Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var dataString = '{"event_name":"App Launched","from":20171201,"to":20171225}';

var options = {
    url: 'https://in1.api.clevertap.com/1/profiles.json?batch_size=50',
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

req, err := http.NewRequest("POST", "https://in1.api.clevertap.com/1/profiles.json?batch_size=50", body)
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

### Sample Response

## Get User Profiles Using Cursor

Using the cursor from the [Get Cursor by Specifying Events for User Profiles](doc:get-user-profiles-api#get-cursor-by-specifying-events-for-user-profiles) section, you will make an API call that returns the first batch of user profiles and another cursor to get the second batch of user profiles. Repeat this process until a cursor is not returned in the response, which means there are no more matching user profiles.

### Base URL

Here is the sample base URL from the account in the India region:

[https://in1.api.clevertap.com/1/profiles.json?cursor=\<cursor>](https://in1.api.clevertap.com/1/profiles.json?cursor=\<cursor>)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

### HTTP Method

GET

### Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

### Query Parameter

| Parameter | Parameter                                                          | Type   | Example Value                                                                  |
| :-------- | :----------------------------------------------------------------- | :----- | :----------------------------------------------------------------------------- |
| cursor    | Required parameter. Specifies what set of user profiles to return. | string | ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6 |

### Sample Request

Here is an example cURL request to the Get User Profiles Using Cursor API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl "https://in1.api.clevertap.com/1/profiles.json?cursor=CURSOR" \
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
```node Node.js
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

### Sample Response

If additional user profiles are available, next_cursor will be returned. The value for next_cursor is used to get the next batch of user profiles.

User profiles will be returned in a records object. The schema for the user profile object is documented in the section below.

If no more user profiles are available, `next_cursor` will not be returned, and a success response will be returned.

### User Profile Object

The following is the schema for the user profile object:

| Key          | Description                                                                                                                                                                                                 |
| :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| email        | User’s email address.                                                                                                                                                                                       |
| profileData  | User’s custom profile properties.                                                                                                                                                                           |
| identity     | Custom user identity provided by you.                                                                                                                                                                       |
| name         | User’s name.                                                                                                                                                                                                |
| platformInfo | Array containing all platform names with the corresponding `objectId` (CleverTap ID), `phone number`, `push_token`, `app_version`, `os_version`, `make` (device make), `model` (device model) if available. |

# Download User Profiles by ID

You can download a specific user profile by passing any of the following three identifiers: email, identity, and objectId.

### Base URL

[https://in1.api.clevertap.com/1/profile.json?\<parameter>=\<value>](https://in1.api.clevertap.com/1/profile.json?\<parameter>=\<value>)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

### HTTP Method

GET

### Query Parameters

Pass any one of the following parameters:

| Parameter | Description                          | Type   | Example Value                      |
| --------- | ------------------------------------ | ------ | ---------------------------------- |
| email     | User’s email address.                | string | `jack@gmail.com`                   |
| identity  | Custom user identity defined by you. | string | `5555555555`                       |
| objectId  | CleverTap ID.                        | string | `1a063854f83a4c6484285039ecff87cb` |

### Sample Request

Here is an example cURL request to the Get User Profiles by ID API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl "https://in1.api.clevertap.com/1/profile.json?email=jack@gmail.com" \
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
```node Node.js
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

### Sample Response

The user profile returns in a record object. The schema for the user profile object is as shown below.

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

<br />

# Additional Resources

* [API Errors](doc:api-errors)
* [API Request Limit](doc:api-request-limit)
* [API FAQs](doc:api-faqs)
