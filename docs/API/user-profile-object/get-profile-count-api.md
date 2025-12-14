---
title: Get Profile Count API
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
This endpoint enables you to get the total number of profiles that match an event query.

## Base URL
https://api.clevertap.com/1/counts/profiles.json

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
## Body Parameters
The body is uploaded as a JSON payload. 

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Required",
    "h-3": "Type",
    "h-4": "Example Value",
    "0-0": "event_name",
    "1-0": "event_properties",
    "2-0": "from",
    "3-0": "to",
    "3-1": "End of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.",
    "2-1": "Start of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.",
    "1-1": "The event properties that you want to filter the results on.",
    "0-1": "The name of the event.",
    "0-2": "required",
    "1-2": "optional",
    "2-2": "required",
    "3-2": "required",
    "0-3": "string",
    "1-3": "string key/value pairs",
    "2-3": "int",
    "3-3": "int",
    "0-4": "\"choseNewFavoriteFood\"",
    "1-4": "\"event_properties\": [\n        {\n            \"name\": \"value\",\n            \"operator\": \"contains\",\n            \"value\": \"piz\"\n        }\n    ]",
    "2-4": "20150810",
    "3-4": "20151025"
  },
  "cols": 5,
  "rows": 4
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"event_name\": \"choseNewFavoriteFood\",\n    \"event_properties\": [\n        {\n            \"name\": \"value\",\n            \"operator\": \"contains\",\n            \"value\": \"piz\"\n        }\n    ],\n    \"from\": 20150810,\n    \"to\": 20151025\n}\n",
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
      "code": "curl -X POST -d '{\"event_name\":\"choseNewFavoriteFood\",\"event_properties\":[{\"name\":\"value\",\"operator\":\"contains\",\"value\":\"piz\"}],\"from\":20150810,\"to\":20151025}' \"https://api.clevertap.com/1/counts/profiles.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"\n",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nrequire 'json'\n\nuri = URI.parse(\"https://api.clevertap.com/1/counts/profiles.json\")\nrequest = Net::HTTP::Post.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\nrequest.body = JSON.dump({\n  \"event_name\" => \"choseNewFavoriteFood\",\n  \"event_properties\" => [\n    {\n      \"name\" => \"value\",\n      \"operator\" => \"contains\",\n      \"value\" => \"piz\"\n    }\n  ],\n  \"from\" => 20150810,\n  \"to\" => 20151025\n})\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json',\n}\n\ndata = '{\"event_name\":\"choseNewFavoriteFood\",\"event_properties\":[{\"name\":\"value\",\"operator\":\"contains\",\"value\":\"piz\"}],\"from\":20150810,\"to\":20151025}'\n\nresponse = requests.post('https://api.clevertap.com/1/counts/profiles.json', headers=headers, data=data)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json'\n);\n$data = '{\"event_name\":\"choseNewFavoriteFood\",\"event_properties\":[{\"name\":\"value\",\"operator\":\"contains\",\"value\":\"piz\"}],\"from\":20150810,\"to\":20151025}';\n$response = Requests::post('https://api.clevertap.com/1/counts/profiles.json', $headers, $data);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n};\n\nvar dataString = '{\"event_name\":\"choseNewFavoriteFood\",\"event_properties\":[{\"name\":\"value\",\"operator\":\"contains\",\"value\":\"piz\"}],\"from\":20150810,\"to\":20151025}';\n\nvar options = {\n    url: 'https://api.clevertap.com/1/counts/profiles.json',\n    method: 'POST',\n    headers: headers,\n    body: dataString\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "type Payload struct {\n\tEventName       string `json:\"event_name\"`\n\tEventProperties []struct {\n\t\tName     string `json:\"name\"`\n\t\tOperator string `json:\"operator\"`\n\t\tValue    string `json:\"value\"`\n\t} `json:\"event_properties\"`\n\tFrom int `json:\"from\"`\n\tTo   int `json:\"to\"`\n}\n\ndata := Payload{\n// fill struct\n}\npayloadBytes, err := json.Marshal(data)\nif err != nil {\n\t// handle err\n}\nbody := bytes.NewReader(payloadBytes)\n\nreq, err := http.NewRequest(\"POST\", \"https://api.clevertap.com/1/counts/profiles.json\", body)\nif err != nil {\n\t// handle err\n}\nreq.Header.Set(\"X-Clevertap-Account-Id\", \"ACCOUNT_ID\")\nreq.Header.Set(\"X-Clevertap-Passcode\", \"PASSCODE\")\nreq.Header.Set(\"Content-Type\", \"application/json\")\n\nresp, err := http.DefaultClient.Do(req)\nif err != nil {\n\t// handle err\n}\ndefer resp.Body.Close()",
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
      "code": "{\n  \"status\": \"success\",\n  \"count\": 7138\n}",
      "language": "json"
    }
  ]
}
[/block]
## Note
The response is a JSON object containing the key status, which might be success, partial, or fail.

If the status is success, there will be a count key with an int value of the count for the specified query. If the status is fail, there will be an error key with a string value and a HTTP status code.

If the status is partial, the query has not finished processing in our system. In the response, you will receive a req_id key with a long int value that you will use to poll for the result. After the query is completed, you will either receive a success response with the count if the query was successful, or a fail response with an error string and a HTTP status code. Please wait 30 seconds between polling requests.

Here is an example response for a partial status.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"req_id\": 384649162721759,\n  \"status\": \"partial\"\n}",
      "language": "json"
    }
  ]
}
[/block]
After getting the req_id, you will poll the endpoint below and provide the value of req_id as a query parameter.

GET https://api.clevertap.com/1/counts/profiles.json?req_id=<your_request_id_here>