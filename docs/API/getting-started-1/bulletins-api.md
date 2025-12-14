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
https://location.api.clevertap.com/1/targets/trigger.json


[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Use the URL based on your location:\n  * India - in1.api.clevertap.com\n  * Singapore - sg1.api.clevertap.com\n  * U.S - us1.api.clevertap.com"
}
[/block]
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
## Body Parameters
The body is uploaded as a JSON payload. The payload is an object keyed with “d” whose value is an array describing the event including the event name, event properties, timestamp, and user identifier.

Event keys are limited to 120 characters and property values are limited to 512 bytes.

For each event, a user identifier is required. This is the key that CleverTap uses to find the user whose profile needs to be updated.
[block:callout]
{
  "type": "warning",
  "body": "CleverTap only supports String Data type for mapped property values.",
  "title": "Note:"
}
[/block]

[block:parameters]
{
  "data": {
    "0-0": "business_event",
    "h-0": "Parameter",
    "h-1": "Description",
    "0-1": "The business event need to trigger the Bulletin. (Required)",
    "0-2": "string",
    "h-2": "Type",
    "h-3": "Example Value",
    "h-4": "Example Value",
    "0-3": "\"Program Released\"",
    "0-4": "\"Program Released\"",
    "1-0": "name",
    "1-1": "The name of the Bulletin. (Required)",
    "1-3": "Sa Re Ga Ma episode 12",
    "2-0": "properties",
    "3-0": "c-by",
    "1-2": "string",
    "2-1": "Event properties describing the business event. (Required)",
    "2-3": "\"properties\" : \n{ \"program_id\" : \"234567\", \"language\":\"Hindi\",  \"prev_program_id\":\"123457\"",
    "2-4": "\"properties\" : \n{ \"program_id\" : \"234567\", \"language\":\"Hindi\",  \"prev_program_id\":\"123457\"",
    "1-4": "Sa Re Ga Ma episode 12",
    "3-1": "Creator of the Bulletin. (Required)",
    "2-2": "string",
    "3-2": "string",
    "3-3": "\"jdoe@clevertap.com\""
  },
  "cols": 4,
  "rows": 4
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"business_event\" : \"Program Released\",\n    \"name\" : \"Sa Re Ga Ma episode 12\",\n    \"properties\" : {\n        \"program_id\" : \"234567\",\n        \"language\":\"Hindi\",\n        \"prev_program_id\":\"123457\"\n    },\n    \"c-by\" : \"jdoe@clevertap.com\",\n    \"when\":\"now\"\n}\n",
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
      "code": "curl -X POST -d '{\"business_event\":true\"}' \"https://location.api.clevertap.com/1/targets/trigger.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nrequire 'json'\n\nuri = URI.parse(\"https://location.api.clevertap.com/1/targets/trigger.json\")\nrequest = Net::HTTP::Post.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\nrequest.body = JSON.dump({\n  \"user_type\" => true\n  \n})\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby",
      "name": null
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json',\n}\n\ndata = '{\"user_type\":true}'\n\nresponse = requests.post('https://location.api.clevertap.com/1/targets/trigger.json', headers=headers, data=data)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json'\n);\n$data = '{\"user_type\":true}';\n$response = Requests::post('https://location.api.clevertap.com/1/targets/trigger.json', $headers, $data);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n};\n\nvar dataString = '{\"user_type\":true}';\n\nvar options = {\n    url: 'https://location.api.clevertap.com/1/targets/trigger.json',\n    method: 'POST',\n    headers: headers,\n    body: dataString\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
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
      "code": "{\n    \"status\": \"success\",\n    \"targets\": 1\n}",
      "language": "json"
    }
  ]
}
[/block]
## Notes
The response will be a JSON object containing either the success or fail status.

The targets key returns the number of Bulletins that have been sent for the Business event that has been raised. 

##Expected errors
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"fail\",\n    \"error\": \"Business Event Not Found\",\n    \"code\": 400\n}",
      "language": "json"
    }
  ]
}
[/block]
When the Business event raised is not defined on the dashboard.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"fail\",\n    \"error\": \"\\\"when\\\" has an incorrect value - null\",\n    \"code\": 400\n}",
      "language": "json"
    }
  ]
}
[/block]
The above error occurs when the "when" key is not defined while raising the business event.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"fail\",\n    \"error\": \"Propertyproduct_categorymissing\",\n    \"code\": 400\n}",
      "language": "json"
    }
  ]
}
[/block]
The above error occurs when a property is missing while you are raising the business event.