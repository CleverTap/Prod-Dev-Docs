---
title: Subscribe API
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
      slug: disassociate-api
      title: Disassociate API
---
# Overview
The Subscribe API provides the ability to set a phone number or email status as subscribed or unsubscribed. This is important so that you do not send a message to your users accidentally unless they have explicitly opted for it. There may be cases when multiple users share a phone number or email; however, once a phone number or email is marked as unsubscribed (DND), communication is stopped for all users using the specified number or email. 

For example, a user opts out of receiving messages. You can pass the phone number or email address and the status as `Unsubscribe` in the Subscribe API.  The user phone number or email is unsubscribed for all users who share the same phone number or email. After the user opts to receive messages again, the phone number or email can be changed back to `Resubscribe`.
[block:callout]
{
  "type": "info",
  "body": "To set only a specific user state as DND, set the MSG-sms flag for SMS or MSG-email flag for an email to false in the  [Upload User Profiles](https://developer.clevertap.com/docs/upload-user-profiles-api).  If the other users sharing the number or email have not opted out, they will continue to receive messages.",
  "title": "Note"
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "From January 13, 2020 onwards, Channel Unsubscribed Event also triggers the Subscribe API."
}
[/block]

Passing subscription status for phone numbers or email requires a POST request with a JSON payload specifying the phone number or email and subscription status. There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.

## Base URL
https://location.api.clevertap.com/1/subscribe
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
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please refer to the [authentication guide](doc:authentication) to view how to get their values.
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
  * Add the type as email or phone number
  * Add the value of the email or phone number
  * Set the status to `Unsubscribe` or `Resubscribe`. 
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "type",
    "1-0": "value",
    "2-0": "status",
    "0-1": "Set to phone or email. Required.",
    "1-1": "The phone number or email address of the user",
    "2-1": "Pass the status of the number or email. The valid values can be `Unsubscribe` or `Resubscribe`.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "“phone”\n\"email\"",
    "1-3": "\"+919213231415\"\n\"johndoe@abc.com\"",
    "2-3": "`Unsubscribe` or `Resubscribe`."
  },
  "cols": 4,
  "rows": 3
}
[/block]
The following is an example JSON payload for iOS/Android.
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"d\": [\n\t\t{\t\n\t\t\t\"type\": \"phone\",\n\t\t\t\"value\": \"+919213231415\",\n\t\t\t\"status\": \"Unsubscribe\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"phone\",\n\t\t\t\"value\": \"+919213231416\",\n\t\t\t\"status\": \"Resubscribe\"\n\t\t},\n    {\n\t\t\t\"type\": \"email\",\n\t\t\t\"value\": \"johndoe@abc.com\",\n\t\t\t\"status\": \"Unsubscribe\"\n\t\t},\n\t\t{\n\t\t\t\"type\": \"email\",\n\t\t\t\"value\": \"janedoe@abc.com\",\n\t\t\t\"status\": \"Resubscribe\"\n\t\t}\n\t]\n}",
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
      "code": "curl --location --request POST 'https://location.api.clevertap.com/1/subscribe/'\n--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \\ \n--header 'X-CleverTap-Passcode: PASSCODE' \\ \n--header 'Content-Type: application/json' \\ \n--data-raw '{ \"d\": [ { \"type\": \"phone\", \"value\": \"+919213231415\", \"status\": \"Unsubscribe\" }, { \"type\": \"phone\", \"value\": \"+919213231416\", \"status\": \"Resubscribe\" }, { \"type\": \"email\", \"value\": \"johndoe@abc.com\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"janedoe@abc.com\", \"status\": \"Resubscribe\" } ] }'\n",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\nurl = URI(\"https://location.api.clevertap.com/1/subscribe/\")\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-CleverTap-Passcode\"] = \"PASSCODE\"\nrequest[\"Content-Type\"] = \"application/json\"\nrequest.body = \"{ \\\"d\\\": [ { \\\"type\\\": \\\"phone\\\", \\\"value\\\": \\\"+919213231415\\\", \\\"status\\\": \\\"Unsubscribe\\\" }, { \\\"type\\\": \\\"phone\\\", \\\"value\\\": \\\"+919213231416\\\", \\\"status\\\": \\\"Resubscribe\\\" }, { \\\"type\\\": \\\"email\\\", \\\"value\\\": \\\"johndoe@abc.com\\\", \\\"status\\\": \\\"Unsubscribe\\\" }, { \\\"type\\\": \\\"email\\\", \\\"value\\\": \\\"janedoe@abc.com\\\", \\\"status\\\": \\\"Resubscribe\\\" } ] }\"\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\nurl = \"https://location.api.clevertap.com/1/subscribe/\"\npayload = \"{ \\\"d\\\": [ { \\\"type\\\": \\\"phone\\\", \\\"value\\\": \\\"+919213231415\\\", \\\"status\\\": \\\"Unsubscribe\\\" }, { \\\"type\\\": \\\"phone\\\", \\\"value\\\": \\\"+919213231416\\\", \\\"status\\\": \\\"Resubscribe\\\" }, { \\\"type\\\": \\\"email\\\", \\\"value\\\": \\\"johndoe@abc.com\\\", \\\"status\\\": \\\"Unsubscribe\\\" }, { \\\"type\\\": \\\"email\\\", \\\"value\\\": \\\"janedoe@abc.com\\\", \\\"status\\\": \\\"Resubscribe\\\" } ] }\"\nheaders = {\n  'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n  'X-CleverTap-Passcode': 'PASSCODE',\n  'Content-Type': 'application/json'\n}\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://location.api.clevertap.com/1/subscribe/');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n  'X-CleverTap-Passcode' => 'PASSCODE',\n  'Content-Type' => 'application/json'\n));\n$request->setBody('{ \"d\": [ { \"type\": \"phone\", \"value\": \"+919213231415\", \"status\": \"Unsubscribe\" }, { \"type\": \"phone\", \"value\": \"+919213231416\", \"status\": \"Resubscribe\" }, { \"type\": \"email\", \"value\": \"johndoe@abc.com\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"janedoe@abc.com\", \"status\": \"Resubscribe\" } ] }');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}\n",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://location.api.clevertap.com/1/subscribe/',\n  'headers': {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n  },\n  body: JSON.stringify({\"d\":[{\"type\":\"phone\",\"value\":\"+919213231415\",\"status\":\"Unsubscribe\"},{\"type\":\"phone\",\"value\":\"+919213231416\",\"status\":\"Resubscribe\"},{\"type\":\"email\",\"value\":\"johndoe@abc.com\",\"status\":\"Unsubscribe\"},{\"type\":\"email\",\"value\":\"janedoe@abc.com\",\"status\":\"Resubscribe\"}]})\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\nfunc main() {\n  url := \"https://location.api.clevertap.com/1/subscribe/\"\n  method := \"POST\"\n  payload := strings.NewReader(\"{ \\\"d\\\": [ { \\\"type\\\": \\\"phone\\\", \\\"value\\\": \\\"+919213231415\\\", \\\"status\\\": \\\"Unsubscribe\\\" }, { \\\"type\\\": \\\"phone\\\", \\\"value\\\": \\\"+919213231416\\\", \\\"status\\\": \\\"Resubscribe\\\" }, { \\\"type\\\": \\\"email\\\", \\\"value\\\": \\\"johndoe@abc.com\\\", \\\"status\\\": \\\"Unsubscribe\\\" }, { \\\"type\\\": \\\"email\\\", \\\"value\\\": \\\"janedoe@abc.com\\\", \\\"status\\\": \\\"Resubscribe\\\" } ] }\")\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"ACCOUNT_ID\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"PASSCODE\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n  fmt.Println(string(body))\n}\n",
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
      "code": "{\n    \"status\": \"success\",\n    \"processed\": 4,\n    \"unprocessed\": []\n}",
      "language": "json"
    }
  ]
}
[/block]
## Debugging 
Requests with processing errors are returned in the API call response as shown in the example below.
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
To test if your data will be submitted without errors, you can add the parameter dryRun=1 to the URL. This validates the input without submitting the data to CleverTap.
[block:api-header]
{
  "title": "SMS Delivery"
}
[/block]
The message delivery is decided by the phone-level and user-level subscription status. If a phone number has opted out but not the associated user profile, then the SMS is not delivered.  
[block:parameters]
{
  "data": {
    "h-0": "Phone-level subscription (MSG-dndPhone)",
    "h-1": "User-level SMS subscription (MSG-sms)",
    "h-2": "Reachability",
    "0-0": "Subscribed",
    "0-1": "Subscribed",
    "0-2": "Reachable",
    "1-0": "Subscribed",
    "1-1": "Unsubscribed",
    "1-2": "Unreachable",
    "2-0": "Unsubscribed",
    "2-1": "Subscribed",
    "2-2": "Unreachable",
    "3-0": "Unsubscribed",
    "3-1": "Unsubscribed",
    "3-2": "Unreachable"
  },
  "cols": 3,
  "rows": 4
}
[/block]

[block:api-header]
{
  "title": "Email Delivery"
}
[/block]
The message delivery is decided by the email-level and user-level subscription status. If an email address has opted out but not the associated user profile, then the message is not delivered.  

[Handling subscriptions](https://docs.clevertap.com/docs/handling-unsubscribes) will automatically trigger the [subscribe](https://developer.clevertap.com/docs/subscribe-api) and [disassociate](https://developer.clevertap.com/docs/disassociate-api) APIs leading to Email-level subscription instead of User-level subscription.
[block:parameters]
{
  "data": {
    "h-0": "Email-level subscription (MSG-dndEmail)",
    "h-1": "User-level Email subscription (MSG-email)",
    "h-2": "Reachability",
    "0-0": "Subscribed",
    "1-0": "Subscribed",
    "2-0": "Unsubscribed",
    "3-0": "Unsubscribed",
    "0-1": "Subscribed",
    "1-1": "Unsubscribed",
    "2-1": "Subscribed",
    "3-1": "Unsubscribed",
    "0-2": "Reachable",
    "1-2": "Unreachable",
    "2-2": "Unreachable",
    "3-2": "Unreachable"
  },
  "cols": 3,
  "rows": 4
}
[/block]