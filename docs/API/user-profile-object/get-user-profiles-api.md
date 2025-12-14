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
[block:api-header]
{
  "title": "Overview"
}
[/block]
There are two ways to get user profiles. The first way is to download user profiles that have performed a specific event. For example, you can download a list of users that have launched your app in the past day. The second way to download user profiles is by requesting the specific users needed. The Get User Profiles API lets you download user profiles from CleverTap. 

# Get User Profiles by Events

Getting a list of user profiles by events requires two steps:
  * With your first API call, specify the event name, date range, and batch size needed. This request returns a cursor you will use to get the list of user profile records in the [Get User Profiles Using Cursor](https://developer.clevertap.com/docs/get-user-profiles-api#section-get-user-profiles-using-cursor) section. 
  * Make a second API call using the cursor from the [Get Cursor by Specifying Events for User Profiles](https://developer.clevertap.com/docs/get-user-profiles-api#section-get-cursor-by-specifying-events-for-user-profiles) section. This call returns the first batch of user profiles and another cursor to get the second batch of user profiles. You repeat this process until the cursor is not returned in the response, which means there are no more matching user profiles.


[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The batch size denotes the maximum number of records that can be fetched in a single call. The response may vary."
}
[/block]
## Get Cursor by Specifying Events for User Profiles

In this request, you specify the event name, date range, and batch size needed. This request returns a cursor, which you will use to get the list of user profiles in the [Get User Profiles Using Cursor](https://developer.clevertap.com/docs/get-user-profiles-api#section-get-user-profiles-using-cursor) section.

### Base URL
https://api.clevertap.com/1/profiles.json

### HTTP Method
POST

### Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Refer to the [authentication guide](doc:authentication) to understand how to get their values.
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
### URL Parameter
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "0-0": "batch_size",
    "0-1": "The number of results to return in each API call. The maximum is 5000.",
    "0-2": "int",
    "0-3": "50",
    "h-3": "Example Value",
    "h-2": "Type",
    "1-1": "Optional parameter. Set to true to receive app fields in the response This will return the OS version, device make, device model, app version within the profile object.",
    "1-0": "app",
    "1-2": "boolean",
    "1-3": "true",
    "2-0": "events",
    "2-1": "Optional parameter. Includes the event summary fields in the response. The default value is true. Set to false to exclude event summary fields in the response.",
    "2-2": "boolean",
    "2-3": "true",
    "3-0": "profile",
    "3-1": "Optional parameter. Includes the custom profile properties in the response. The default value is true. Set to false to exclude custom profile properties fields in the response.",
    "3-2": "boolean",
    "3-3": "true"
  },
  "cols": 4,
  "rows": 4
}
[/block]
### Body Parameters
[block:parameters]
{
  "data": {
    "0-0": "event_name",
    "1-0": "from",
    "2-0": "to",
    "h-0": "Parameter",
    "0-1": "The event type needed. You can get both standard and custom events from this endpoint. The standard events include App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Notification Sent, Product Viewed, and UTM Visited.",
    "1-1": "Start of date range for events needed. Value specified in format YYYYMMDD.",
    "2-1": "End of date range for events needed. Value specified in format YYYYMMDD.",
    "h-2": "Type",
    "h-1": "Description",
    "0-2": "string",
    "1-2": "int",
    "2-2": "int",
    "2-3": "20171225",
    "1-3": "20171201",
    "0-3": "“App Launched”",
    "h-3": "Example Value"
  },
  "cols": 4,
  "rows": 3
}
[/block]
### Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"event_name\":\"App Launched\",\"from\":20171201,\"to\":20171225}' \"https://api.clevertap.com/1/profiles.json?batch_size=50\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"\n",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nrequire 'json'\n\nuri = URI.parse(\"https://api.clevertap.com/1/profiles.json?batch_size=50\")\nrequest = Net::HTTP::Post.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\nrequest.body = JSON.dump({\n  \"event_name\" => \"App Launched\",\n  \"from\" => 20171201,\n  \"to\" => 20171225\n})\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "#  Recommended python version 2.7 \nimport requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json',\n}\n\nparams = (\n    ('batch_size', '50'),\n)\n\ndata = '{\"event_name\":\"App Launched\",\"from\":20171201,\"to\":20171225}'\n\nresponse = requests.post('https://api.clevertap.com/1/profiles.json', headers=headers, params=params, data=data)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json'\n);\n$data = '{\"event_name\":\"App Launched\",\"from\":20171201,\"to\":20171225}';\n$response = Requests::post('https://api.clevertap.com/1/profiles.json?batch_size=50', $headers, $data);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n};\n\nvar dataString = '{\"event_name\":\"App Launched\",\"from\":20171201,\"to\":20171225}';\n\nvar options = {\n    url: 'https://api.clevertap.com/1/profiles.json?batch_size=50',\n    method: 'POST',\n    headers: headers,\n    body: dataString\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "type Payload struct {\n\tEventName string `json:\"event_name\"`\n\tFrom      int    `json:\"from\"`\n\tTo        int    `json:\"to\"`\n}\n\ndata := Payload{\n// fill struct\n}\npayloadBytes, err := json.Marshal(data)\nif err != nil {\n\t// handle err\n}\nbody := bytes.NewReader(payloadBytes)\n\nreq, err := http.NewRequest(\"POST\", \"https://api.clevertap.com/1/profiles.json?batch_size=50\", body)\nif err != nil {\n\t// handle err\n}\nreq.Header.Set(\"X-Clevertap-Account-Id\", \"ACCOUNT_ID\")\nreq.Header.Set(\"X-Clevertap-Passcode\", \"PASSCODE\")\nreq.Header.Set(\"Content-Type\", \"application/json\")\n\nresp, err := http.DefaultClient.Do(req)\nif err != nil {\n\t// handle err\n}\ndefer resp.Body.Close()",
      "language": "go"
    }
  ]
}
[/block]
### Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n \"cursor\":\"AfljfgIJBgBnamF5Kz8NegcBAwxhbCe%2Fbmhhe04BBAVlYjT4YG5reQEATQQrai57K2oue04FAUhnd38%3D\",\n \"status\":\"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]
## Get User Profiles Using Cursor

Using the cursor from the [Get Cursor by Specifying Events for User Profiles](https://developer.clevertap.com/docs/get-user-profiles-api#section-get-cursor-by-specifying-events-for-user-profiles) section, you will make an API call that returns the first batch of user profiles and another cursor to get the second batch of user profiles. Repeat this process until a cursor is not returned in the response, which means there are no more matching user profiles.

### Base URL
https://api.clevertap.com/1/profiles.json

### HTTP Method
GET

### Headers
The headers described below are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Refer to the [authentication guide](doc:authentication) to understand how to get their values.
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
    "2-3": "\"Content-Type: application/json\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
### URL Parameter
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "cursor",
    "0-1": "Required parameter. Specifies what set of user profiles to return.",
    "0-2": "string",
    "0-3": "ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6"
  },
  "cols": 4,
  "rows": 1
}
[/block]
### Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl \"https://api.clevertap.com/1/profiles.json?cursor=CURSOR\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\n\nuri = URI.parse(\"https://api.clevertap.com/1/profiles.json?cursor=CURSOR\")\nrequest = Net::HTTP::Get.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json',\n}\n\nparams = (\n    ('cursor', 'CURSOR'),\n)\n\nresponse = requests.get('https://api.clevertap.com/1/profiles.json', headers=headers, params=params)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json'\n);\n$response = Requests::get('https://api.clevertap.com/1/profiles.json?cursor=CURSOR', $headers);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n};\n\nvar options = {\n    url: 'https://api.clevertap.com/1/profiles.json?cursor=CURSOR',\n    headers: headers\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "req, err := http.NewRequest(\"GET\", \"https://api.clevertap.com/1/profiles.json?cursor=CURSOR\", nil)\nif err != nil {\n\t// handle err\n}\nreq.Header.Set(\"X-Clevertap-Account-Id\", \"ACCOUNT_ID\")\nreq.Header.Set(\"X-Clevertap-Passcode\", \"PASSCODE\")\nreq.Header.Set(\"Content-Type\", \"application/json\")\n\nresp, err := http.DefaultClient.Do(req)\nif err != nil {\n\t// handle err\n}\ndefer resp.Body.Close()",
      "language": "go"
    }
  ]
}
[/block]
### Example Response
If there are more user profiles available, next_cursor will be returned. The value for next_cursor is used to get the next batch of user profiles. 

User profiles will be returned in a records object. The schema for the user profile object is documented in the section below.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\",\n  \"next_cursor\": \"ZiZjNwMEBQBkaWZzY2MuO20BBQFlYmN7ZG5hewYBTQVjb2BzZmphfwEABQUra2Jmeg%3D%3D\",\n  \"records\": [\n    {\n      \"identity\": \"5555555555\",\n      \"profileData\": {\n        \"favoriteFood\": \"pizza\"\n      },\n      \"events\": {\n      \"App Launched\": {\n        \"count\": 10,\n        \"first_seen\": 1457271567,\n        \"last_seen\": 1458041215\n      },\n      \"Charged\": {\n        \"count\": 6,\n        \"first_seen\": 1457962417,\n        \"last_seen\": 1458041276\n      }\n      },\n      \"platformInfo\": [\n        {\n          \"platform\": \"iOS\",\n          \"os_version\": \"10.2\",\n          \"app_version\": \"6.1.3\",\n          \"make\": \"Apple\",\n          \"model\": \"iPhone7,2\",\n          \"push_token\": \"95f98af6ad9a5e714a56c5bf527a78cb1e3eda18d2f23bc8591437d0d8ae71a3\",\n          \"objectId\": \"-1a063854f83a4c6484285039ecff87cb\"\n        },\n        {\n          \"platform\": \"Web\",\n          \"objectId\": \"a8ffcbc9-a747-4ee3-a791-c5e58ad03097\"\n        }\n      ]\n    }\n  ]\n}",
      "language": "json"
    }
  ]
}
[/block]
If there are no more user profiles available, *next_cursor* will not be returned, and a success response will be returned.
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
## User Profile Object

The following is the schema for the user profile object.
[block:parameters]
{
  "data": {
    "h-0": "Key",
    "h-1": "Description",
    "0-0": "email",
    "1-0": "profileData",
    "2-0": "identity",
    "3-0": "name",
    "4-0": "platformInfo",
    "0-1": "User’s email address.",
    "1-1": "User’s custom profile properties.",
    "2-1": "Custom user identity provided by you.",
    "3-1": "User’s name.",
    "4-1": "Array containing all platform names with the corresponding objectId (CleverTap ID), phone number, push_token, app_version, os_version, make (device make), model (device model) if available."
  },
  "cols": 2,
  "rows": 5
}
[/block]
## Download User Profiles by ID

You can download user profiles by requesting the specific user needed. You can also retrieve a user’s profile and all the events performed by a specific user. Supported identity values are email, a custom identity, or the CleverTap objectId.

### Base URL
https://api.clevertap.com/1/profile.json

### HTTP Method
GET

### Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Refer to the [authentication guide](doc:authentication) to understand how to get their values.
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
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type is always set to application/json.",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
### URL Parameters
One of the three parameters documented below needs to be included in your request to specify the user profile needed.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "email",
    "1-0": "identity",
    "2-0": "objectId",
    "0-1": "User’s email address.",
    "1-1": "Custom user identity defined by you.",
    "2-1": "CleverTap ID.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "“jack@gmail.com”",
    "1-3": "“5555555555”",
    "2-3": "“1a063854f83a4c6484285039ecff87cb”"
  },
  "cols": 4,
  "rows": 3
}
[/block]
### Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl \"https://api.clevertap.com/1/profile.json?email=jack@gmail.com\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\n\nuri = URI.parse(\"https://api.clevertap.com/1/profile.json?email=jack@gmail.com\")\nrequest = Net::HTTP::Get.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json',\n}\n\nparams = (\n    ('email', 'jack@gmail.com'),\n)\n\nresponse = requests.get('https://api.clevertap.com/1/profile.json', headers=headers, params=params)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json'\n);\n$response = Requests::get('https://api.clevertap.com/1/profile.json?email=jack@gmail.com', $headers);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n};\n\nvar options = {\n    url: 'https://api.clevertap.com/1/profile.json?email=jack@gmail.com',\n    headers: headers\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "req, err := http.NewRequest(\"GET\", \"https://api.clevertap.com/1/profile.json?email=jack@gmail.com\", nil)\nif err != nil {\n\t// handle err\n}\nreq.Header.Set(\"X-Clevertap-Account-Id\", \"ACCOUNT_ID\")\nreq.Header.Set(\"X-Clevertap-Passcode\", \"PASSCODE\")\nreq.Header.Set(\"Content-Type\", \"application/json\")\n\nresp, err := http.DefaultClient.Do(req)\nif err != nil {\n\t// handle err\n}\ndefer resp.Body.Close()",
      "language": "go"
    }
  ]
}
[/block]
### Example Response
The user profile returns in a record object. The schema for the user profile object is documented below.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\",\n  \"record\": {\n    \"email\": \"jack@gmail.com\",\n    \"profileData\": {\n      \"Last Score\": 308,\n      \"High Score\": 308,\n      \"Replayed\": true\n    },\n    \"events\": {\n      \"App Launched\": {\n        \"count\": 10,\n        \"first_seen\": 1457271567,\n        \"last_seen\": 1458041215\n      },\n      \"Charged\": {\n        \"count\": 6,\n        \"first_seen\": 1457962417,\n        \"last_seen\": 1458041276\n      }\n    },\n    \"platformInfo\": [\n      {\n        \"platform\": \"iOS\",\n        \"os_version\": \"10.2\",\n        \"app_version\": \"6.1.3\",\n        \"make\": \"Apple\",\n        \"model\": \"iPhone7,2\",\n        \"push_token\": \"95f98af6ad9a5e714a56c5bf527a78cb1e3eda18d2f23bc8591437d0d8ae71a3\",\n        \"objectId\": \"-1a063854f83a4c6484285039ecff87cb\"\n      },\n      {\n        \"platform\": \"Web\",\n        \"objectId\": \"a8ffcbc9-a747-4ee3-a791-c5e58ad03097\"\n      }\n    ]\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]
### Additional Notes
  * API errors are described on [this page](doc:api-errors).
  * Requests are limited to three (3) concurrent requests. If you exceed this limit, a 429 error returns. Try again after 10 minutes.