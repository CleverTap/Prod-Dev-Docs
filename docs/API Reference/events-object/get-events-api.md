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

# Step 1: Get Cursor by Specifying Events Needed

In this request, you will specify the event name, date range, and batch size needed. This request will return a cursor, which you will use to get the list of events in the step 2.

## Base URL
https://api.clevertap.com/1/events.json

## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "X-CleverTap-Account-Id",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "0-1": "Your CleverTap Account ID.",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type is always set to application/json.",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
## URL Parameter
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "0-0": "batch_size",
    "h-1": "Description",
    "0-1": "The number of results to return in each API call. The maximum is 5000.",
    "h-2": "Type",
    "0-2": "int",
    "0-3": "50",
    "h-3": "Example Value",
    "1-0": "app",
    "1-1": "Optional parameter. Set to true to receive app fields in the response This will return the OS version, device make, device model, app version within the profile object.",
    "1-2": "boolean",
    "1-3": "true",
    "2-0": "events",
    "3-0": "profile",
    "2-1": "Optional parameter. Includes the event summary fields in the response. The default value is true. Set to false to exclude event summary fields in the response.",
    "3-1": "Optional parameter. Includes the custom profile properties in the response. The default value is true. Set to false to exclude custom profile properties fields in the response.",
    "2-2": "boolean",
    "3-2": "boolean",
    "2-3": "true",
    "3-3": "true"
  },
  "cols": 4,
  "rows": 4
}
[/block]
## Body Parameters
[block:parameters]
{
  "data": {
    "0-0": "event_name",
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-1": "The event type needed. You can get both standard and custom events from this endpoint. The standard events include App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Notification Sent, Product Viewed, and UTM Visited.",
    "1-0": "from",
    "2-0": "to",
    "1-1": "Start of date range for events needed. Value specified in format YYYYMMDD.",
    "2-1": "End of date range for events needed. Value specified in format YYYYMMDD.",
    "0-2": "string",
    "1-2": "int",
    "2-2": "int",
    "0-3": "“App Launched”",
    "1-3": "20171201",
    "2-3": "20171225"
  },
  "cols": 4,
  "rows": 3
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"event_name\":\"App Launched\",\"from\":20171201,\"to\":20171225}' \"https://api.clevertap.com/1/events.json?batch_size=50\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nrequire 'json'\n\nuri = URI.parse(\"https://api.clevertap.com/1/events.json?batch_size=50\")\nrequest = Net::HTTP::Post.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\nrequest.body = JSON.dump({\n  \"event_name\" => \"App Launched\",\n  \"from\" => 20171201,\n  \"to\" => 20171225\n})\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json',\n}\n\nparams = (\n    ('batch_size', '50'),\n)\n\ndata = '{\"event_name\":\"App Launched\",\"from\":20171201,\"to\":20171225}'\n\nresponse = requests.post('https://api.clevertap.com/1/events.json', headers=headers, params=params, data=data)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json'\n);\n$data = '{\"event_name\":\"App Launched\",\"from\":20171201,\"to\":20171225}';\n$response = Requests::post('https://api.clevertap.com/1/events.json?batch_size=50', $headers, $data);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n};\n\nvar dataString = '{\"event_name\":\"App Launched\",\"from\":20171201,\"to\":20171225}';\n\nvar options = {\n    url: 'https://api.clevertap.com/1/events.json?batch_size=50',\n    method: 'POST',\n    headers: headers,\n    body: dataString\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "type Payload struct {\n\tEventName string `json:\"event_name\"`\n\tFrom      int    `json:\"from\"`\n\tTo        int    `json:\"to\"`\n}\n\ndata := Payload{\n// fill struct\n}\npayloadBytes, err := json.Marshal(data)\nif err != nil {\n\t// handle err\n}\nbody := bytes.NewReader(payloadBytes)\n\nreq, err := http.NewRequest(\"POST\", \"https://api.clevertap.com/1/events.json?batch_size=50\", body)\nif err != nil {\n\t// handle err\n}\nreq.Header.Set(\"X-Clevertap-Account-Id\", \"ACCOUNT_ID\")\nreq.Header.Set(\"X-Clevertap-Passcode\", \"PASSCODE\")\nreq.Header.Set(\"Content-Type\", \"application/json\")\n\nresp, err := http.DefaultClient.Do(req)\nif err != nil {\n\t// handle err\n}\ndefer resp.Body.Close()",
      "language": "go"
    }
  ]
}
[/block]
## Example Response
[block:code]
{
  "codes": [
    {
      "code": "{ \"cursor\" : \"AfljfgIJBgBnamF5Kz8NegcBAwxhbCe%2Fbmhhe04BBAVlYjT4YG5reQEATQQrai57K2oue04FAUhnd38%3D\" , \"status\" : \"success\"}",
      "language": "json"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "Currently, we do not support exporting the following events via this API:\n\n  * Notification Sent\n  * Notification Bounce\n  * Notification  Viewed\n  * Notification Control Group\n  * Notification Rendered\n  * Notification System Control Group \n\nTo export these events we have multiple other ways such as [S3 Export](https://docs.clevertap.com/docs/data-export-to-aws-s3), [GCP Export](https://docs.clevertap.com/docs/data-export-to-gcp), or [download from the dashboard](https://docs.clevertap.com/docs/find-people).",
  "title": "Note"
}
[/block]
# Step 2: Get Events Using Cursor
Using the cursor from step 1, you will now make an API call that will return the first batch of events and and another cursor to get the second batch of events. You will repeat this process until a cursor is not returned in the response, which means there are no more events.

## Base URL
https://api.clevertap.com/1/events.json

## HTTP Method
GET

## Headers
The headers described below are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "X-CleverTap-Account-Id",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "0-1": "Your CleverTap Account ID.",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type is always set to application/json.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
## URL Parameters
[block:parameters]
{
  "data": {
    "0-0": "cursor",
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-1": "Required parameter. Specifies what set of events to return.",
    "0-2": "string",
    "0-3": "ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6"
  },
  "cols": 4,
  "rows": 1
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl \"https://api.clevertap.com/1/events.json?cursor=CURSOR\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\n\nuri = URI.parse(\"https://api.clevertap.com/1/events.json?cursor=CURSOR\")\nrequest = Net::HTTP::Get.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json',\n}\n\nresponse = requests.get('https://api.clevertap.com/1/events.json?cursor=CURSOR', headers=headers)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json'\n);\n$response = Requests::get('https://api.clevertap.com/1/events.json?cursor=CURSOR', $headers);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n};\n\nvar options = {\n    url: 'https://api.clevertap.com/1/events.json?cursor=CURSOR',\n    headers: headers\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    }
  ]
}
[/block]
## Example Response
If there are more events available, next_cursor will be returned. The value for next_cursor is used to get the next batch of events. 

Events will be returned in a records object. The schema for the Event object is provided in the next section.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"success\",\n    \"previous_cursor\": \"ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF/Z21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMARl6\",\n    \"next_cursor\": \"ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6\",\n    \"records\": [\n        {\n            \"profile\": {\n                \"objectId\": \"a8ffcbc9-a747-4ee3-a791-c5e58ad03097\",\n                \"platform\": \"Web\",\n                \"email\": \"peter@foo.com\",\n                \"profileData\": {\n                    \"favoriteColor\": \"blue\"\n                },\n                \"identity\": \"5555555555\",\n                \"id\": 33\n            },\n            \"ts\": 20151023140416,   // yyyyMMddHHmmSS format\n            \"event_props\": {\n                \"value\": \"pizza\"\n            },\n            \"session_props\": {\n                \"utm_source\": \"Foo\",\n                \"utm_medium\": \"Bar\",\n                \"utm_campaign\": \"FooBar\",\n                \"session_source\": \"Direct\"\n            }\n        },\n        {\n            \"profile\": {\n                \"objectId\": \"a8ffcbc9-a747-4ee3-a791-c5e58ad03097\",\n                \"platform\": \"Web\",\n                \"email\": \"peter@foo.com\",\n                \"profileData\": {\n                    \"favoriteColor\": \"blue\"\n                },\n                \"identity\": \"5555555555\",\n                \"id\": 33\n            },\n            \"ts\": 20151024121636,\n            \"event_props\": {\n                \"value\": \"pizza\"\n            },\n            \"session_props\": {\n                \"session_source\": \"Others\",\n                \"session_referrer\": \"http://example.com\"\n            }\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]
If there are no more events available, next_cursor will not be returned and a success response will be returned.
[block:code]
{
  "codes": [
    {
      "code": "{ \"status\" : \"success\"}",
      "language": "json"
    }
  ]
}
[/block]
If the data for the cursor is not ready, you'll get the following response :
[block:code]
{
  "codes": [
    {
      "code": "{\n   \"status\": \"fail\",\n   \"error\": \"Request still in progress, please retry later\",\n   \"code\": 2\n}",
      "language": "json"
    }
  ]
}
[/block]
# Event Object
Here is the schema for the Events object.
[block:parameters]
{
  "data": {
    "h-0": "Key",
    "h-1": "Description",
    "0-0": "objectId",
    "1-0": "profileData",
    "2-0": "identity",
    "3-0": "ts",
    "4-0": "event_props",
    "5-0": "session_props",
    "6-0": "session_source",
    "6-1": "Possible values are Direct, Search, Social, Email, and Batch. These are only provided if they were set. If unset, the source is Unavailable, and Others if there is a custom referrer (session_referrer field contains the referrer value if source is Others).",
    "5-1": "Possible values are utm_source, utm_medium, utm_campaign, session_source, and session_referrer for the event. These are only provided if they were set.",
    "4-1": "All event properties and their values.",
    "3-1": "Event timestamp in yyyyMMddHHmmSS format.",
    "2-1": "Custom user identity provided by you.",
    "1-1": "All custom profile properties of the user.",
    "0-1": "CleverTap ID of the user."
  },
  "cols": 2,
  "rows": 7
}
[/block]
# Notes
  * You can only define single event type with each request for events.
  * API errors are described on the [errors page](doc:api-errors).