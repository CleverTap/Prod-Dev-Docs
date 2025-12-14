---
title: Demerge User Profile API
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
This endpoint enables you to demerge a user profile.

## Base URL
https://location.api.clevertap.com/1/demerge/profiles.json
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
    "0-1": "Your CleverTap Account ID.",
    "0-2": "string",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "2-0": "Content-Type",
    "2-1": "Request content-type is always set to application/json.",
    "2-2": "string",
    "2-3": "\"Content-Type: application/json\"",
    "1-0": "X-CleverTap-Passcode",
    "1-1": "Your CleverTap Account Passcode.",
    "1-2": "string",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\""
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
    "0-0": "identities",
    "0-1": "Custom user identity defined by you.",
    "0-2": "optional",
    "0-3": "string/array",
    "0-4": "[“client-19827239”, “abc@ct.com”]"
  },
  "cols": 5,
  "rows": 1
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"identities\": [\"client-19827239\", \"abc@ct.com\"]\n}",
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
      "code": "curl -X POST -d '{\"identities\":[\"client-19827239\", \"abc@ct.com\"]}'\n\"https://location.api.clevertap.com/1/demerge/profiles.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json; charset=utf-8\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nrequire 'json'\n\nuri = URI.parse(\"https://location.api.clevertap.com/1/demerge/profiles.json\")\nrequest = Net::HTTP::Post.new(uri)\nrequest.content_type = \"application/json; charset=utf-8\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\nrequest.body = JSON.dump({\n  \"guid\" => \"df2e224d90874887b4d61153ef3a2508\"\n})\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json; charset=utf-8',\n}\n\ndata = '{\"identities\":[\"client-19827239\", \"abc@ct.com\"]}'\n\nresponse = requests.post('https://location.api.clevertap.com/1/demerge/profiles.json', headers=headers, data=data)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json; charset=utf-8'\n);\n$data = '{\"identities\":[\"client-19827239\", \"abc@ct.com\"]}' ;\n$response = Requests::post('https://location.api.clevertap.com/1/demerge/profiles.json', $headers, $data);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json; charset=utf-8'\n};\n\nvar dataString = '{\"identities\":[\"client-19827239\", \"abc@ct.com\"]}';\n\nvar options = {\n    url: 'https://location.api.clevertap.com/1/demerge/profiles.json',\n    method: 'POST',\n    headers: headers,\n    body: dataString\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "type Payload struct {\n\tGUID string `json:\"guid\"`\n}\n\ndata := Payload{\n// fill struct\n}\npayloadBytes, err := json.Marshal(data)\nif err != nil {\n\t// handle err\n}\nbody := bytes.NewReader(payloadBytes)\n\nreq, err := http.NewRequest(\"POST\", \"https://location.api.clevertap.com/1/delete/profiles.json\", body)\nif err != nil {\n\t// handle err\n}\nreq.Header.Set(\"X-Clevertap-Account-Id\", \"ACCOUNT_ID\")\nreq.Header.Set(\"X-Clevertap-Passcode\", \"PASSCODE\")\nreq.Header.Set(\"Content-Type\", \"application/json; charset=utf-8\")\n\nresp, err := http.DefaultClient.Do(req)\nif err != nil {\n\t// handle err\n}\ndefer resp.Body.Close()",
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
      "code": "{\n  \"status\": \"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]