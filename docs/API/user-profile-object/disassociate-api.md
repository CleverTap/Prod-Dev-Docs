---
title: Disassociate API
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
  pages:
    - type: basic
      slug: upload-user-profiles-api
      title: Upload User Profiles API
---
The disassociate API enables you to disconnect a phone number from a user profile. There may be a case when users change their phone numbers and then the numbers are assigned to other users after the cooling-off period. 

If the user is identified by the phone number then the data of the previous user may be merged with the new user assigned to the phone number.  To avoid this scenario, run the disassociate API and disassociate the phone number assigned to the previous user.

For example, a user discontinues a phone number. The phone number is then assigned to a new user. You can pass the phone number in the Disassociate API. The phone number is disconnected from the previous user profile. 

If you want to assign this phone number to a new user, you can use the [Upload User Profiles API](doc:upload-user-profiles-api). A new user is created for the specified phone number. This process must be repeated every time the phone number changes hands.

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "You can disassociate a phone number only when it is used as an identity."
}
[/block]
# Overview
Passing the phone numbers requires a POST request with a JSON payload specifying the phone number. There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.


[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The batch size denotes the maximum number of records that can be fetched in a single call. The response may vary."
}
[/block]
## Base URL
https://location.api.clevertap.com/1/disassociate
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
    "0-1": "Your CleverTap Account ID",
    "1-1": "Your CleverTap Account Passcode",
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
Phone numbers are uploaded as a JSON payload. A payload is an object keyed with “d” whose value is an array describing the phone numbers.

For each phone number,  type is required. The phone number is CleverTap’s user identifier, which we will use to find the user and dissociate the profile.


[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "type",
    "1-0": "value",
    "0-1": "Set to phone. Required.",
    "1-1": "The phone number of the user",
    "0-2": "string",
    "1-2": "string",
    "0-3": "“phone”",
    "1-3": "+9119244142334"
  },
  "cols": 4,
  "rows": 2
}
[/block]
Here is an example JSON payload for iOS/Android.
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"d\": [\n\t\t{\n\t\t\t\"type\": \"phone\",\n\t\t\t\"value\": \"+919213231415\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"phone\",\n\t\t\t\"value\": \"+919213231416\"\n\t\t}\n\t]\n}\n",
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
      "code": "curl --location --request POST 'https://location.api.clevertap.com/1/disassociate/' \\\n--header 'Content-Type: application/json' \\\n--header 'X-CleverTap-Account-Id: WWW-WWW-WWRZ' \\\n--header 'X-CleverTap-Passcode: ICW-QKE-YSGL' \\\n--data-raw '{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}'",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\nurl = URI(\"https://location.api.clevertap.com/1/disassociate/\")\nhttp = Net::HTTP.new(url.host, url.port);\nrequest = Net::HTTP::Post.new(url)\nrequest[\"Content-Type\"] = \"application/json\"\nrequest[\"X-CleverTap-Account-Id\"] = \"WWW-WWW-WWRZ\"\nrequest[\"X-CleverTap-Passcode\"] = \"ICW-QKE-YSGL\"\nrequest.body = \"{\\\"d\\\": [{\\\"type\\\": \\\"phone\\\", \\\"value\\\": \\\"+919213231415\\\"},{\\\"type\\\": \\\"phone\\\",\\\"value\\\": \\\"+919213231416\\\"}]}\"\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    },
    {
      "code": "import requests\nurl = \"https://location.api.clevertap.com/1/disassociate/\"\npayload = \"{\\\"d\\\": [{\\\"type\\\": \\\"phone\\\", \\\"value\\\": \\\"+919213231415\\\"},{\\\"type\\\": \\\"phone\\\",\\\"value\\\": \\\"+919213231416\\\"}]}\"\nheaders = {\n  'Content-Type': 'application/json',\n  'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',\n  'X-CleverTap-Passcode': 'ICW-QKE-YSGL'\n}\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://location.api.clevertap.com/1/disassociate/');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'Content-Type' => 'application/json',\n  'X-CleverTap-Account-Id' => 'WWW-WWW-WWRZ',\n  'X-CleverTap-Passcode' => 'ICW-QKE-YSGL'\n));\n$request->setBody('{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://location.api.clevertap.com/1/disassociate/',\n  'headers': {\n    'Content-Type': 'application/json',\n    'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',\n    'X-CleverTap-Passcode': 'ICW-QKE-YSGL'\n  },\n  body: JSON.stringify({\"d\":[{\"type\":\"phone\",\"value\":\"+919213231415\"},{\"type\":\"phone\",\"value\":\"+919213231416\"}]})\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\nfunc main() {\n  url := \"https://location.api.clevertap.com/1/disassociate/\"\n  method := \"POST\"\n  payload := strings.NewReader(\"{\\\"d\\\": [{\\\"type\\\": \\\"phone\\\", \\\"value\\\": \\\"+919213231415\\\"},{\\\"type\\\": \\\"phone\\\",\\\"value\\\": \\\"+919213231416\\\"}]}\")\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"Content-Type\", \"application/json\")\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"WWW-WWW-WWRZ\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"ICW-QKE-YSGL\")\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n  fmt.Println(string(body))\n}",
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
      "code": "{\n    \"status\": \"success\",\n    \"processed\": 2,\n    \"unprocessed\": []\n}",
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



[block:callout]
{
  "type": "info",
  "body": "If a user is identified by the phone number, then the GUID can be used to identify the profile after disassociation. All the event and profile updates to the user are made using the GUID till a new phone number is passed to the user.\n\n[Handling subscriptions](https://docs.clevertap.com/docs/handling-unsubscribes) will automatically trigger the [subscribe](https://developer.clevertap.com/docs/subscribe-api) and [disassociate](https://developer.clevertap.com/docs/disassociate-api) APIs leading to Email-level subscription instead of User-level subscription.",
  "title": "Note"
}
[/block]