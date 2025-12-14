---
title: markMessagesAsRead API
excerpt: Learn how to update the status of an Inbox message using API.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The `markMessagesAsRead` API updates the status of an Inbox message to "read" after a user interacts with or views it. Whenever this API is called, the Notification Viewed event is raised, and the `isRead` parameter for a particular message is set to  `true` for the user. Now, whenever the `getMessages` API is called the `isRead` parameter for the same message is updated to `true`, indicating that the message was read by the user on some device.

# Base URL

Here is an example base URL from the account in the India region:

<https://in1.api.clevertap.com/1/inbox/markMessagesAsRead>

### Region

Refer to [Region](doc:common-api-components#region) for more details.

# HTTP Method

POST

# Headers

The API headers are used while processing the API request. The below headers are required when you use CleverTap APIs:

```json
X-CleverTap-Account-Id:<encoded-account-id>
X-CleverTap-Passcode:<account-passcode>
Content-Type:application/json; charset=utf-8
```

The `X-CleverTap-Account-Id` and `X-CleverTap-Passcode` authenticate the request.

| Header                 | Description                                                 | Type   | Example Value                        |
| :--------------------- | :---------------------------------------------------------- | :----- | :----------------------------------- |
| X-CleverTap-Account-Id | Your CleverTap Account ID.                                  | string | "X-CleverTap-Account-Id: ACCOUNT_ID" |
| X-CleverTap-Passcode   | Your CleverTap Account Passcode.                            | string | "X-CleverTap-Passcode: PASSCODE"     |
| Content-Type           | The request content-type is always set to application/json. | string | "Content-Type: application/json"     |

# Body Parameters

The following are the body parameters included in the JSON response payload:

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "userId",
    "0-1": "Uniquely identifies the user.",
    "0-2": "string",
    "0-3": "[test@stockx.com](mailto:test@stockx.com)",
    "1-0": "message",
    "1-1": "Contains a list of messages read by the user. For each user, it consists of the following information: `messageId`, `wzrk_id`, and `wzrk_pivot` values. The maximum number of messages for a user can be 50.",
    "1-2": "array",
    "1-3": "{  \n  \"userId\": \"[test@stockx.com](mailto:test@stockx.com) \",  \n  \"messages\": [  \n    {  \n      \"messageId\": 2,  \n      \"isRead\": true,  \n      \"wzrk_pivot\": \"wzrk_default\",  \n      \"wzrk_id\": \"1713204790_20240415\"  \n    },  \n    {  \n      \"messageId\": 3,  \n      \"isRead\": false,  \n      \"wzrk_pivot\": \"wzrk_default\",  \n      \"wzrk_id\": \"1713204981_20240415\"  \n    }  \n  ]  \n}",
    "2-0": "messageID",
    "2-1": "Uniquely identifies the message that must be marked as read for the user.",
    "2-2": "numeric",
    "2-3": "1",
    "3-0": "wzrk_id",
    "3-1": "Indicates the internal key to identify the campaign. This parameter is required to generate click events.",
    "3-2": "string",
    "3-3": "12345679_20240130",
    "4-0": "wzrk_pivot",
    "4-1": "Indicates the internal key to identify the campaign variants. This parameter is required to generate click events.",
    "4-2": "string",
    "4-3": "wzrk_default  \n(when the default variant is rolled out to the user)",
    "5-0": "isRead",
    "5-1": "Indicates if the message is read.",
    "5-2": "string",
    "5-3": "true"
  },
  "cols": 4,
  "rows": 6,
  "align": [
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


# Example Request

Here is an example cURL request to the markMessagesAsRead API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl --location --request POST 'https://in1.api.clevertap.com/1/inbox/markMessagesAsRead' \
--header 'X-CleverTap-Account-Id: YOUR_ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE' \
--header 'Content-Type: application/json' \
--data-raw '{
  "userId": "test@stockx.com",
  "messages": [
    {
      "messageId": 2,
      "isRead": true,
      "wzrk_pivot": "wzrk_default",
      "wzrk_id": "1713204790_20240415"
    },
    {
      "messageId": 3,
      "isRead": false,
      "wzrk_pivot": "wzrk_default",
      "wzrk_id": "1713204981_20240415"
    }
  ]
}'
```
```ruby
require "uri"
require "json"
require "net/http"

url = URI("https://in1.api.clevertap.com/1/inbox/markAsRead")

http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "YOUR_ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "YOUR_ACCOUNT_PASSCODE"
request["X-CleverTap-Token"] = "YOUR_ACCOUNT_TOKEN"
request["Content-Type"] = "application/json"
request.body = JSON.dump({
  "userId": "stockx1@test.com",
  "messages": [
    {
      "wzrk_id": "1711383026_20240325",
      "wzrk_pivot": "wzrk_default",
      "messageId": 8,
      "isRead": false
    },
    {
      "wzrk_id": "1711612209_20240328",
      "wzrk_pivot": "wzrk_default",
      "messageId": 9,
      "isRead": false
    }
  ]
})

response = http.request(request)
puts response.read_body
```
```python
import requests
import json

url = "https://in1.api.clevertap.com/1/inbox/markAsRead"

payload = json.dumps({
  "userId": "stockx1@test.com",
  "messages": [
    {
      "wzrk_id": "1711383026_20240325",
      "wzrk_pivot": "wzrk_default",
      "messageId": 8,
      "isRead": False
    },
    {
      "wzrk_id": "1711612209_20240328",
      "wzrk_pivot": "wzrk_default",
      "messageId": 9,
      "isRead": False
    }
  ]
})
headers = {
  'X-CleverTap-Account-Id': 'YOUR_ACCOUNT_ID',
  'X-CleverTap-Passcode': 'YOUR_ACCOUNT_PASSCODE',
  'X-CleverTap-Token': 'YOUR_ACCOUNT_TOKEN',
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
```
```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://in1.api.clevertap.com/1/inbox/markAsRead',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'POST',
  CURLOPT_POSTFIELDS =>'{
  "userId": "stockx1@test.com",
  "messages": [
    {
      "wzrk_id": "1711383026_20240325",
      "wzrk_pivot": "wzrk_default",
      "messageId": 8,
      "isRead": false
    },
    {
      "wzrk_id": "1711612209_20240328",
      "wzrk_pivot": "wzrk_default",
      "messageId": 9,
      "isRead": false
    }
  ]
}',
  CURLOPT_HTTPHEADER => array(
    'X-CleverTap-Account-Id: YOUR_ACCOUNT_ID',
    'X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE',
    'X-CleverTap-Token: YOUR_ACCOUNT_TOKEN',
    'Content-Type: application/json'
  ),
));

$response = curl_exec($curl);

curl_close($curl);
echo $response;
```
```node
var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://in1.api.clevertap.com/1/inbox/markAsRead',
  'headers': {
    'X-CleverTap-Account-Id': 'YOUR_ACCOUNT_ID',
    'X-CleverTap-Passcode': 'YOUR_ACCOUNT_PASSCODE',
    'X-CleverTap-Token': 'YOUR_ACCOUNT_TOKEN',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    "userId": "stockx1@test.com",
    "messages": [
      {
        "wzrk_id": "1711383026_20240325",
        "wzrk_pivot": "wzrk_default",
        "messageId": 8,
        "isRead": false
      },
      {
        "wzrk_id": "1711612209_20240328",
        "wzrk_pivot": "wzrk_default",
        "messageId": 9,
        "isRead": false
      }
    ]
  })

};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});
```

# Example Response

The following response is received when the request is processed successfully:

```json
{
  "status": "success"
}
```

If there is an error in processing the request, the following response is received:

```json
{
    "status": "fail",
    "error": "<errordescription>",
    "code": <errorcode>
}
```

For more information about different errors you might encounter, refer to [Inbox API Errors](doc:inbox-api-errors).