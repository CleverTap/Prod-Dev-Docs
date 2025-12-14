---
title: Upload Events API
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
The *Upload Events API* provides you the capability to upload user events to CleverTap. 

For example, you can use this API to store user events, such as product views and purchases. You associate these events with a specific user by using an identifier, such as an email address or a phone number. 

By storing user events in CleverTap, you can create richer segments and run targeted campaigns.

# Overview
Uploading events to CleverTap requires a POST request with a JSON payload specifying the event information. 1000 event records can be sent per API call. 

While the maximum number of event types per account is 512, some events are reserved for system events, so you may not get to use all of them. For each event type, the maximum number of properties is 256. The number of events submitted per account across those event types is unlimited. 

## Base URL
https://api.clevertap.com/1/upload

## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. To view how to get their values, refer to the [Authentication](doc:authentication) guide.
[block:parameters]
{
  "data": {
    "0-0": "X-CleverTap-Account-Id",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-1": "Your CleverTap Account ID.",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type is always set to application/json; charset=utf-8.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json; charset=utf-8\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
## Body Parameters
Events are uploaded as a JSON payload. The payload is an object keyed with “d” whose value is an array describing the event including the event name, event properties, timestamp, and user identifier.

Event keys are limited to 120 characters and property values are limited to 512 bytes.

For each event, a user identifier is required. This is the key that CleverTap uses to find the user whose profile needs to be updated. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId. If this identity is not found, a new user profile will be created. 
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "type",
    "1-0": "evtName",
    "2-0": "evtData",
    "3-0": "ts",
    "4-0": "identity",
    "5-0": "FBID",
    "6-0": "GPID",
    "7-0": "objectId",
    "0-1": "(Required) Set to event.",
    "1-1": "(Required) Name of event.",
    "2-1": "(Required) Event properties describing the event. Passed as key/value pairs.",
    "3-1": "(Optional) Date and time when the event occured, Formatted as a UNIX epoch in seconds. The format \nis $D_EPOCH. Defaults to the current timestamp if omitted.",
    "4-1": "Identity to recognize a unique user. It can be the user’s email, a phone number, or any other identifier you are using to tag your users. \nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "5-1": "Identify a unique user by Facebook ID. \n\nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "6-1": "Identify a unique user by Google Plus ID. \n\nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "7-1": "Identify a unique user by CleverTap Global Object ID.\n\nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "object",
    "3-2": "ts",
    "4-2": "string",
    "5-2": "string",
    "6-2": "string",
    "7-2": "string",
    "7-3": "“34322425\"",
    "6-3": "“34322424\"",
    "5-3": "“34322423\"",
    "4-3": "“jack@gmail.com”",
    "3-3": "1468308340",
    "2-3": "\"evtData\": \n{ \"Product name\": \"Casio Watch\", \"Category\": \"Mens Watch\" }",
    "1-3": "“Product Viewed”",
    "0-3": "\"event\""
  },
  "cols": 4,
  "rows": 8
}
[/block]
Here is an example JSON payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"d\": [\n        {\n            \"FBID\": \"34322423\",\n            \"ts\": 1468308340, // time when the event occurred in UNIX epoch value in seconds\n            \"type\": \"event\",\n            \"evtName\": \"Product viewed\",\n            \"evtData\": {\n                \"Product name\": \"Casio Chronograph Watch\",\n                \"Category\": \"Mens Watch\",\n                \"Price\": 59.99,\n                \"Currency\": \"USD\"\n            }\n        },\n        {\n            \"identity\": \"jack@gmail.com\",\n            \"ts\": 1468208340,\n            \"type\": \"event\",\n            \"evtName\": \"Charged\",\n            \"evtData\": {\n                \"Amount\": 300,\n                \"Currency\": \"USD\",\n                \"Payment mode\": \"Credit Card\",\n                \"Delivery By\": \"$D_1468322268\",     \n                \"Items\": [\n                    {\n                        \"Category\": \"books\",\n                        \"Book name\": \"The millionaire next door\",\n                        \"Quantity\": 1\n                    },\n                    {\n                        \"Category\": \"books\",\n                        \"Book name\": \"Achieving inner zen\",\n                        \"Quantity\": 4\n                    }\n                ]\n            }\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{ \"d\": [ { \"FBID\": \"34322423\", \"ts\": 1468308340, \"type\": \"event\", \"evtName\": \"Product viewed\", \"evtData\": { \"Product name\": \"Casio Chronograph Watch\", \"Category\": \"Mens Watch\", \"Price\": 59.99, \"Currency\": \"USD\" } } ] }' \"https://api.clevertap.com/1/upload\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json; charset=utf-8\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nrequire 'json'\n\nuri = URI.parse(\"https://api.clevertap.com/1/upload\")\nrequest = Net::HTTP::Post.new(uri)\nrequest.content_type = \"application/json; charset=utf-8\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\nrequest.body = JSON.dump({\n  \"d\" => [\n    {\n      \"FBID\" => \"34322423\",\n      \"ts\" => 1468308340,\n      \"type\" => \"event\",\n      \"evtName\" => \"Product viewed\",\n      \"evtData\" => {\n        \"Product name\" => \"Casio Chronograph Watch\",\n        \"Category\" => \"Mens Watch\",\n        \"Price\" => 59.99,\n        \"Currency\" => \"USD\"\n      }\n    }\n  ]\n})\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json; charset=utf-8',\n}\n\ndata = '{ \"d\": [ { \"FBID\": \"34322423\", \"ts\": 1468308340, \"type\": \"event\", \"evtName\": \"Product viewed\", \"evtData\": { \"Product name\": \"Casio Chronograph Watch\", \"Category\": \"Mens Watch\", \"Price\": 59.99, \"Currency\": \"USD\" } } ] }'\n\nresponse = requests.post('https://api.clevertap.com/1/upload', headers=headers, data=data)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json; charset=utf-8'\n);\n$data = '{ \"d\": [ { \"FBID\": \"34322423\", \"ts\": 1468308340, \"type\": \"event\", \"evtName\": \"Product viewed\", \"evtData\": { \"Product name\": \"Casio Chronograph Watch\", \"Category\": \"Mens Watch\", \"Price\": 59.99, \"Currency\": \"USD\" } } ] }';\n$response = Requests::post('https://api.clevertap.com/1/upload', $headers, $data);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json; charset=utf-8'\n};\n\nvar dataString = '{ \"d\": [ { \"FBID\": \"34322423\", \"ts\": 1468308340, \"type\": \"event\", \"evtName\": \"Product viewed\", \"evtData\": { \"Product name\": \"Casio Chronograph Watch\", \"Category\": \"Mens Watch\", \"Price\": 59.99, \"Currency\": \"USD\" } } ] }';\n\nvar options = {\n    url: 'https://api.clevertap.com/1/upload',\n    method: 'POST',\n    headers: headers,\n    body: dataString\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    }
  ]
}
[/block]
## Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"success\",\n    \"processed\": 1,\n    \"unprocessed\": []\n}",
      "language": "json"
    }
  ]
}
[/block]
## Debugging
Requests with processing errors will be returned in the API call response as shown in the example below.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\":<success, partial, fail>, \n    \"processed\":<count>, \n    \"unprocessed\": [{\"status\":\"fail\", \"code\":<error code>, \"error\":<error msg>, \"record\":<record>}]\n}",
      "language": "json"
    }
  ]
}
[/block]
To test if your data will be submitted without errors, you can add the parameter dryRun=1 to the URL. This will validate the input without submitting the data to CleverTap.

A list of errors code for this API can be found on the [Errors](doc:api-errors) page.