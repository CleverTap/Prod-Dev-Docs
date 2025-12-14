---
title: Get Events
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
The Get Events API lets you download user events from CleverTap. For example, you can use this API to get a list of App Launch or Purchase events. 

# Overview

Getting a list of user events requires two steps:

- With your first API call you will specify the event name, date range, and batch size needed. This request will return a cursor, which you will use to get the list of event records in step 2. 
- You will make second API call using the cursor from step 1. This call will return the first batch of events and and another cursor to get the second batch of events. You will repeat this process until the cursor is not returned in the response, which means there are no more events.

> 📘 Note
> 
> The batch size denotes the maximum number of records that can be fetched in a single call. The response may vary.

# Step 1: Get Cursor by Specifying Events Needed

In this request, you will specify the event name, date range, and batch size needed. This request will return a cursor, which you will use to get the list of events in the step 2.

## Base URL

Here is an example base URL from the account in the India region:  
<https://in1.api.clevertap.com/1/events.json>

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## URL Parameter

| Parameter  | Description                                                                                                                                                                      | Type    | Example Value |
| :--------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | :------------ |
| batch_size | The number of results to return in each API call. The maximum is 5000.                                                                                                           | int     | 50            |
| app        | Optional parameter. Set to true to receive app fields in the response This will return the OS version, device make, device model, app version within the profile object.         | boolean | true          |
| events     | Optional parameter. Includes the event summary fields in the response. The default value is true. Set to false to exclude event summary fields in the response.                  | boolean | true          |
| profile    | Optional parameter. Includes the custom profile properties in the response. The default value is true. Set to false to exclude custom profile properties fields in the response. | boolean | true          |

## Body Parameters

| Parameter  | Description                                                                                                                                                                                                                     | Type   | Example Value  |
| :--------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :----- | :------------- |
| event_name | The event type needed. You can get both standard and custom events from this endpoint. The standard events include App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Product Viewed, and UTM Visited. | string | “App Launched” |
| from       | Start of date range for events needed. Value specified in format YYYYMMDD.                                                                                                                                                      | int    | 20171201       |
| to         | End of date range for events needed. Value specified in format YYYYMMDD.                                                                                                                                                        | int    | 20171225       |

## Example Request

Here is an example cURL request to the _Get Events_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"event_name":"App Launched","from":20171201,"to":20171225}' "https://in1.api.clevertap.com/1/events.json?batch_size=50" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/events.json?batch_size=50")
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

response = requests.post('https://in1.api.clevertap.com/1/events.json', headers=headers, params=params, data=data)
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
$response = Requests::post('https://in1.api.clevertap.com/1/events.json?batch_size=50', $headers, $data);
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
    url: 'https://in1.api.clevertap.com/1/events.json?batch_size=50',
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

req, err := http.NewRequest("POST", "https://in1.api.clevertap.com/1/events.json?batch_size=50", body)
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
> - Notification Sent
> - Notification Bounce
> - Notification Control Group
> - Notification Rendered
> - Notification System Control Group 
> 
> To export these events we have multiple other ways such as [S3 Export](https://docs.clevertap.com/docs/data-export-to-aws-s3) or [GCP Export](https://docs.clevertap.com/docs/data-export-to-gcp).

# Step 2: Get Events Using Cursor

Using the cursor from step 1, you will now make an API call that will return the first batch of events and and another cursor to get the second batch of events. You will repeat this process until a cursor is not returned in the response, which means there are no more events.

## Base URL

Here is an example base URL from the account in the India region:  
<https://in1.api.clevertap.com/1/events.json>

To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/get-events-api#region).

## HTTP Method

GET

## Headers

The headers described below are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

| Header                 | Description                                             | Type   | Example Value                        |
| :--------------------- | :------------------------------------------------------ | :----- | :----------------------------------- |
| X-CleverTap-Account-Id | Your CleverTap Account ID.                              | string | "X-CleverTap-Account-Id: ACCOUNT_ID" |
| X-CleverTap-Passcode   | Your CleverTap Account Passcode.                        | string | "X-CleverTap-Passcode: PASSCODE"     |
| Content-Type           | Request content-type is always set to application/json. | string | "Content-Type: application/json"     |

## URL Parameters

| Parameter | Description                                                 | Type   | Example Value                                                                  |
| :-------- | :---------------------------------------------------------- | :----- | :----------------------------------------------------------------------------- |
| cursor    | Required parameter. Specifies what set of events to return. | string | ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6 |

## Example Request

Here is an example cURL request to the _Get Events Using Cursor_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl "https://in1.api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://in1.api.clevertap.com/1/events.json?cursor=CURSOR")
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

response = requests.get('https://in1.api.clevertap.com/1/events.json?cursor=CURSOR', headers=headers)
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
$response = Requests::get('https://in1.api.clevertap.com/1/events.json?cursor=CURSOR', $headers);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var options = {
    url: 'https://in1.api.clevertap.com/1/events.json?cursor=CURSOR',
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

If there are more events available, next_cursor will be returned. The value for next_cursor is used to get the next batch of events. 

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
          "favoriteColor": "blue",
          "MSG-email": true,	// Channel preference flag (for exmaple, email messaging enabled)
          "subscription-groups": {	// Subscription group preferences for messaging channels
            "email": {
              "tech news": true,	// Subscribed to group "tech news"
              "marketing": true,	// Not subscribed to group "marketing"
              "service updates": true	// Subscribed to group "service updates"
            }
          }
        },
        "identity": "5555555555",
        "id": 33
      },
      "events": {
        "Identity Set": {
          "count": 1,
          "first_seen": 1677155829,
          "last_seen": 1677155829
        },
        "Test event": {
          "count": 2,
          "first_seen": 1677155653,
          "last_seen": 1677155726
        }
      },
      "ts": 20151023140416,
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
          "favoriteColor": "blue",
          "MSG-email": true,
          "subscription-groups": {
            "email": {
              "tech news": true,
              "marketing": true,
              "service updates": true
            }
          }
        },
        "identity": "5555555555",
        "id": 33
      },
      "events": {
        "Identity Set": {
          "count": 1,
          "first_seen": 1677155829,
          "last_seen": 1677155829
        },
        "Test event": {
          "count": 2,
          "first_seen": 1677155653,
          "last_seen": 1677155726
        }
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

If there are no more events available, next_cursor will not be returned and a success response will be returned.

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

| Key            | Description                                                                                                                                                                                                                                                     |
| :------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| objectId       | CleverTap ID of the user.                                                                                                                                                                                                                                       |
| profileData    | All custom profile properties of the user.                                                                                                                                                                                                                      |
| identity       | Custom user identity provided by you.                                                                                                                                                                                                                           |
| ts             | Event timestamp in yyyyMMddHHmmSS format.                                                                                                                                                                                                                       |
| event_props    | All event properties and their values.                                                                                                                                                                                                                          |
| session_props  | Possible values are utm_source, utm_medium, utm_campaign, session_source, and session_referrer for the event. These are only provided if they were set.                                                                                                         |
| session_source | Possible values are Direct, Search, Social, Email, and Batch. These are only provided if they were set. If unset, the source is Unavailable, and Others if there is a custom referrer (session_referrer field contains the referrer value if source is Others). |

# Notes

- You can only define single event type with each request for events.
- API errors are described on the [errors page](doc:api-errors).
- For more information on request limit, refer to [API Request Limit](doc:api-request-limit).
- To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).