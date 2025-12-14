---
title: Upload User Profiles API_callback_WIP
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
The Upload User Profiles API enables you to create or update user profiles in CleverTap. 
For example, you can use this API to update a user profile to include information, such as gender, phone, and customer type.

# Overview
Uploading user profiles to CleverTap requires a POST request with a JSON payload specifying the user profile information. 100 user profiles records can be sent per API call. 

## Base URL
https://api.clevertap.com/1/upload

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
User profile attribute keys are limited to 120 bytes and their values are limited to 512 bytes. The maximum number of custom user profile keys is 256.

For each user profile, a user identifier is required. This is the key that CleverTap uses to find the user whose profile needs to be updated. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId. If this identity is not found a new user profile will be created. 
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "type",
    "1-0": "profileData",
    "0-1": "Set to profile. Required.",
    "1-1": "User profile properties. Passed as key/value pairs. Required.\nprofileData[“Phone”] should be formatted as +[country code][phone number].",
    "0-2": "string",
    "1-2": "object",
    "0-3": "“profile”",
    "1-3": "\"profileData\": {\n        \"Name\": \"Jack Montana\",\n        \"Email\": \"jack@gmail.com\",\n        \"Phone\": \"+14155551234\",\n        \"Gender\": \"M\",\n        \"Employed\": \"Y\",\n        \"Education\": \"Graduate\",\n        \"Married\": \"Y\",\n        \"MSG-sms\": false,\n        \"Customer Type\": \"Platinum\"\n  }",
    "2-0": "ts",
    "3-0": "identity",
    "2-1": "Date and time when the event occurred, Formatted as a UNIX epoch in seconds. Defaults to the current timestamp if omitted. Optional.",
    "2-2": "ts",
    "2-3": "1468308340",
    "3-1": "Identity to recognize a unique user. It can be the user’s email, a phone number, or any other identifier that you are using to tag your users. \nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "3-2": "string",
    "3-3": "“jack@gmail.com”",
    "4-0": "FBID",
    "5-0": "objectId",
    "5-1": "Identify a unique user by CleverTap Global Object ID.\nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "4-1": "Identify a unique user by Facebook ID. \nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "4-3": "“34322423\"",
    "5-3": "“34322425\"",
    "4-2": "string",
    "5-2": "string"
  },
  "cols": 4,
  "rows": 6
}
[/block]
Here is an example JSON payload. {@harsh, please check}
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"d\": [\n    {\n      \"identity\": \"1189549\",  \n      \"ts\": 1468308340,       \n      \"type\": \"profile\",\n      \"profileData\": {\n        \"Name\": \"Jack Montana\",\n        \"Name\": {\"$delete\" : 1}, //this will delete the value of name\n        \"Email\": \"jack@gmail.com\",\n        \"Phone\": \"+14155551234\",\n        \"Gender\": \"M\",\n        \"Gender\": {\"$delete\" : 1}, //this will delete the value of gender\n        \"MSG-sms\": false,\n        \"MSG-email\":true,//called when email must be subscribed  \n        \"MSG-whatsapp\": true,\n        \"MSG-dndPhone\": true, \n        \"MSG-dndEmail\": true, //called when all profiles sharing the same email must be unsubscribed\n        \"category-unsubscribe\": {\n        \t\"email\": [\"Newsletters\", \"Promotions\"]\n        },//called when profile needs to be unsubscribed from multiple subscription groups\n        \"category-resubscribe\": {\n        \t\"email\": [\"Football\", \"Movies\"]\n        },//called when profile needs to be subscribed from multiple subscription groups\n        \"DOB\": \"$D_1487576752\",    \n        \"Customer Type\": \"Platinum\",\n        \"Custom Multi Value\":{\"$remove\":[\"shoes\",\"bags\"]}, // To remove Multi Value properties\n        \"Custom Multi Value\":{\"$add\":[\"shoes\",\"bags\"]},  //To add Multi Value properties\n        \"My Number 1\": {\"$incr\" : 10}, //To increment integer data\n        \"My Number 2\": {\"$decr\" : 30} //To decrement integer data\n      }\n    }\n  ]\n}\n//$delete applicable only to name and gender fields",
      "language": "json"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "In order to increment/decrement a numerical user property, the property needs have an existing value. If the property does not exist or if the value is NULL, please pass the property key OR add the value before using the incr/decr operator",
  "title": "Note"
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"d\":[{\"objectId\":\"25b08803c1af4e00839f530264dac6f8\",\"type\":\"profile\",\"profileData\":{\"Name\": \"Jack Test\"}}]}' \"https://api.clevertap.com/1/upload\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json; charset=utf-8\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nrequire 'json'\n\nuri = URI.parse(\"https://api.clevertap.com/1/upload\")\nrequest = Net::HTTP::Post.new(uri)\nrequest.content_type = \"application/json; charset=utf-8\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\nrequest.body = JSON.dump({\n  \"d\" => [\n    {\n      \"objectId\" => \"25b08803c1af4e00839f530264dac6f8\",\n      \"type\" => \"profile\",\n      \"profileData\" => {\n        \"Name\" => \"Jack Test\"\n      }\n    }\n  ]\n})\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json; charset=utf-8',\n}\n\ndata = '{\"d\":[{\"objectId\":\"25b08803c1af4e00839f530264dac6f8\",\"type\":\"profile\",\"profileData\":{\"Name\": \"Jack Test\"}}]}'\n\nresponse = requests.post('https://api.clevertap.com/1/upload', headers=headers, data=data)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json; charset=utf-8'\n);\n$data = '{\"d\":[{\"objectId\":\"25b08803c1af4e00839f530264dac6f8\",\"type\":\"profile\",\"profileData\":{\"Name\": \"Jack Test\"}}]}';\n$response = Requests::post('https://api.clevertap.com/1/upload', $headers, $data);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json; charset=utf-8'\n};\n\nvar dataString = '{\"d\":[{\"objectId\":\"25b08803c1af4e00839f530264dac6f8\",\"type\":\"profile\",\"profileData\":{\"Name\": \"Jack Test\"}}]}';\n\nvar options = {\n    url: 'https://api.clevertap.com/1/upload',\n    method: 'POST',\n    headers: headers,\n    body: dataString\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "type Payload struct {\n\tD []struct {\n\t\tObjectID    string `json:\"objectId\"`\n\t\tType        string `json:\"type\"`\n\t\tProfileData struct {\n\t\t\tName string `json:\"Name\"`\n\t\t} `json:\"profileData\"`\n\t} `json:\"d\"`\n}\n\ndata := Payload{\n// fill struct\n}\npayloadBytes, err := json.Marshal(data)\nif err != nil {\n\t// handle err\n}\nbody := bytes.NewReader(payloadBytes)\n\nreq, err := http.NewRequest(\"POST\", \"https://api.clevertap.com/1/upload\", body)\nif err != nil {\n\t// handle err\n}\nreq.Header.Set(\"X-Clevertap-Account-Id\", \"ACCOUNT_ID\")\nreq.Header.Set(\"X-Clevertap-Passcode\", \"PASSCODE\")\nreq.Header.Set(\"Content-Type\", \"application/json; charset=utf-8\")\n\nresp, err := http.DefaultClient.Do(req)\nif err != nil {\n\t// handle err\n}\ndefer resp.Body.Close()",
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

A list of errors code for this API can be found on [this page](doc:api-errors).