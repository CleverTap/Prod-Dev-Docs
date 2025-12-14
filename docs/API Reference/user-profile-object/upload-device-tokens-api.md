---
title: Upload Device Tokens API
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
[block:callout]
{
  "type": "info",
  "title": "Note: Uploading historical device tokens for Android/iOS/Web.",
  "body": "Device token upload is only allowed after the user is live on the app, post SDK integration by the app. We cannot fetch tokens of users prior to SDK integration."
}
[/block]
The Upload Device Tokens API enables developers to add an existing device token to a CleverTap user profile. 

For example, you can use this API to add a GCM or APNS token to a user profile. Adding this information will let you send push notifications to that device. 

# Overview
Uploading device tokens to CleverTap requires a POST request with a JSON payload specifying the device token information. 100 device token records can be sent per API call. 

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
Device tokens are uploaded as a JSON payload. The payload is an object keyed with “d” whose value is an array describing the device tokens.

For each device token, objectId is required. objectId is CleverTap’s user identifier, which we will use to find the user and add the device token to their profile.


[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "type",
    "1-0": "tokenData",
    "2-0": "objectId",
    "0-1": "Set to token. Required.",
    "1-1": "Device token properties. Passed as key/value pairs. Required.\nId and type are required properties of tokenData. They are passed as strings.",
    "2-1": "Identify a unique user by CleverTap Global Object ID.\nYou have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.",
    "0-2": "string",
    "1-2": "object",
    "2-2": "string",
    "0-3": "“token”",
    "1-3": "{\n    \"d\": [\n                {\n            \"type\": \"token\",\n            \"tokenData\": {\n                \"id\": \"dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO\",\n                \"type\": \"fcm\"\n            },\n            \"objectId\": \"__gc4grg4053N1eYkW6OBH71jhFev7b1iP\"\n        },\n        {\n            \"type\": \"token\",\n            \"tokenData\": {\n                \"id\": \"frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf\",\n                \"type\": \"gcm\"\n            }\n    ]\n}",
    "2-3": "“34322425\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
Here is an example JSON payload for iOS/Android/Windows.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"d\": [\n        {\n            \"type\": \"token\",\n            \"tokenData\": {\n                \"id\": <Device Token>,\n                \"type\": apns/gcm/fcm/wns/mpns\n            },\n            \"objectId\": <ClevertapId/GUID>\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]
Here is an example JSON payload for Chrome.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"type\": \"token\",\n    \"tokenData\": {\n        \"id\": <DeviceToken>,\n        \"type\": \"chrome\",\n        \"keys\": {\n            \"p256dh\": <>,\n            \"auth\": <>\n        }\n    },\n    \"objectId\": <ClevertapId/GUID>\n}\n",
      "language": "json"
    }
  ]
}
[/block]
Here is an example JSON payload for adding device tokens for multiple OS types.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"d\": [\n        {\n            \"type\": \"token\",\n            \"tokenData\": {\n                \"id\": \"frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf\",\n                \"type\": \"chrome\",\n                \"keys\": {\n                    \"p256dh\": \"BLc4xRzKlKORKWlbdgFaBrrPK3ydWAHo4M0gs0i1oEKgPpWC5cW8OCzVrOQRv-1npXRWk8udnW3oYhIO4475rds=\",\n                    \"auth\": \"5I2Bu2oKdyy9CwL8QVF0NQ==\"\n                }\n            },\n            \"objectId\": \"QBOpVJZmKilRAzfaiVS86Tovxm75lHxg\"\n        },\n        {\n            \"type\": \"token\",\n            \"tokenData\": {\n                \"id\": \"dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO\",\n                \"type\": \"fcm\"\n            },\n            \"objectId\": \"__gc4grg4053N1eYkW6OBH71jhFev7b1iP\"\n        },\n        {\n            \"type\": \"token\",\n            \"tokenData\": {\n                \"id\": \"frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf\",\n                \"type\": \"gcm\"\n            },\n            \"objectId\": \"__QlXHaDHBZxZPLEMpPo6VvwdZ7FnbGvWA\"\n        },\n        {\n            \"type\": \"token\",\n            \"tokenData\": {\n                \"id\": \"frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf\",\n                \"type\": \"apns\"\n            },\n            \"objectId\": \"-HwB9gZWb3RcKhXCQ222FhAhkjeYQs0Hj\"\n        },\n        {\n            \"type\": \"token\",\n            \"tokenData\": {\n                \"id\": \"frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf\",\n                \"type\": \"wns\"\n            },\n            \"objectId\": \"~9XrS4Dy6GsnDbdX98Ijs63kEtJDQbhJA\"\n        },\n        {\n            \"type\": \"token\",\n            \"tokenData\": {\n                \"id\": \"frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf\",\n                \"type\": \"mpns\"\n            },\n            \"objectId\": \"~y9MqjkDGRfbzwwH8fZI6LIXgTKaEPxHr\"\n        }\n    ]\n}",
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
      "code": "curl -X POST -d '{\"d\": [{\"type\": \"token\",\"tokenData\": {\"id\": \"dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO\",\"type\": \"gcm\"},\"objectId\": \"37b08803c1af4e10849f530264bac7f8\"}]}' \"https://api.clevertap.com/1/upload\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json; charset=utf-8\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nrequire 'json'\n\nuri = URI.parse(\"https://api.clevertap.com/1/upload\")\nrequest = Net::HTTP::Post.new(uri)\nrequest.content_type = \"application/json; charset=utf-8\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\nrequest.body = JSON.dump({\n  \"d\" => [\n    {\n      \"type\" => \"token\",\n      \"tokenData\" => {\n        \"id\" => \"dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO\",\n        \"type\" => \"gcm\"\n      },\n      \"objectId\" => \"37b08803c1af4e10849f530264bac7f8\"\n    }\n  ]\n})\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json; charset=utf-8',\n}\n\ndata = '{\"d\": [{\"type\": \"token\",\"tokenData\": {\"id\": \"dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO\",\"type\": \"gcm\"},\"objectId\": \"37b08803c1af4e10849f530264bac7f8\"}]}'\n\nresponse = requests.post('https://api.clevertap.com/1/upload', headers=headers, data=data)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json; charset=utf-8'\n);\n$data = '{\"d\": [{\"type\": \"token\",\"tokenData\": {\"id\": \"dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO\",\"type\": \"gcm\"},\"objectId\": \"37b08803c1af4e10849f530264bac7f8\"}]}';\n$response = Requests::post('https://api.clevertap.com/1/upload', $headers, $data);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json; charset=utf-8'\n};\n\nvar dataString = '{\"d\": [{\"type\": \"token\",\"tokenData\": {\"id\": \"dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO\",\"type\": \"gcm\"},\"objectId\": \"37b08803c1af4e10849f530264bac7f8\"}]}';\n\nvar options = {\n    url: 'https://api.clevertap.com/1/upload',\n    method: 'POST',\n    headers: headers,\n    body: dataString\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "type Payload struct {\n\tD []struct {\n\t\tType      string `json:\"type\"`\n\t\tTokenData struct {\n\t\t\tID   string `json:\"id\"`\n\t\t\tType string `json:\"type\"`\n\t\t} `json:\"tokenData\"`\n\t\tObjectID string `json:\"objectId\"`\n\t} `json:\"d\"`\n}\n\ndata := Payload{\n// fill struct\n}\npayloadBytes, err := json.Marshal(data)\nif err != nil {\n\t// handle err\n}\nbody := bytes.NewReader(payloadBytes)\n\nreq, err := http.NewRequest(\"POST\", \"https://api.clevertap.com/1/upload\", body)\nif err != nil {\n\t// handle err\n}\nreq.Header.Set(\"X-Clevertap-Account-Id\", \"ACCOUNT_ID\")\nreq.Header.Set(\"X-Clevertap-Passcode\", \"PASSCODE\")\nreq.Header.Set(\"Content-Type\", \"application/json; charset=utf-8\")\n\nresp, err := http.DefaultClient.Do(req)\nif err != nil {\n\t// handle err\n}\ndefer resp.Body.Close()",
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
A list of errors code for this API can be found on the [errors page](doc:api-errors).

## Notes
  * A device token can be updated against a CleverTap Id (objectId), but not against an email ID or a custom-defined user identity. If given profile already has a token, it will not be updated and an error will be reported into Live Error Stream.
  * Concurrent requests are limited to 3 per account. Requests that exceed the limit will return a 429 HTTP response code.