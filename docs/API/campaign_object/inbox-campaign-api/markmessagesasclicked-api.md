---
title: markMessagesAsClicked API
excerpt: Learn how to track clicks on links embedded in the Inbox message using API.
deprecated: false
hidden: true
link:
  new_tab: false
metadata:
  title: ''
  description: ''
  robots: index
---
# Overview

The `markMessagesAsClicked` API raises a _Notification Clicked_ event for the users on the CleverTap dashboard whenever they click any link in the Inbox message.

# Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/1/inbox/markMessagesAsClicked](https://in1.api.clevertap.com/1/inbox/markMessagesAsClicked)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

# HTTP Method

POST

# Headers

The following headers are required when you use CleverTap APIs:

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

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        userId
      </td>

      <td>
        Uniquely identifies the user.
      </td>

      <td>
        string
      </td>

      <td>
        [test@stockx.com](mailto:test@stockx.com)
      </td>
    </tr>

    <tr>
      <td>
        messages
      </td>

      <td>
        Contains a list of messages read by the user. For each user, it consists of the following information: `messageId`, `wzrk_id`, and `wzrk_pivot` values. The maximum number of message IDs for a user is 100.
      </td>

      <td>
        array
      </td>

      <td>
        <code>
          {'{'}

          "messageId": 1,
          "wzrk\_id": "12345679\_20240130",
          "wzrk\_pivot": "wzrk\_default"
          {'}'},{'{'}
          "messageId": 2,
          "wzrk\_id": "12345679\_20240131",
          "wzrk\_pivot": "wzrk\_default"

          {'}'}
        </code>
      </td>
    </tr>

    <tr>
      <td>
        messageID
      </td>

      <td>
        Uniquely identifies the message that must be marked as read for the user.
      </td>

      <td>
        numeric
      </td>

      <td>
        1
      </td>
    </tr>

    <tr>
      <td>
        wzrk_id
      </td>

      <td>
        Indicates the internal key to identify the campaign. This parameter is required to generate click events.
      </td>

      <td>
        string
      </td>

      <td>
        12345679_20240130
      </td>
    </tr>

    <tr>
      <td>
        wzrk_pivot
      </td>

      <td>
        Indicates the internal key to identify the campaign variants. This parameter is required to generate click events.
      </td>

      <td>
        string
      </td>

      <td>
        wzrk_default<br />
        (when the default variant is rolled out to the user)
      </td>
    </tr>
  </tbody>
</Table>

# Example Request

Here is an example cURL request to the markMessagesAsRead API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl --location --request POST 'https://in1.api.clevertap.com/1/inbox/markMessagesAsClicked' \
--header 'X-CleverTap-Account-Id: YOUR_ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE' \
--header 'Content-Type: application/json' \
--data-raw '{
  "userId": "test@stockx.com",
  "messages": [
    {
      "messageId": 1,
      "wzrk_id": "12345679_20240130",
      "wzrk_pivot": "wzrk_default"
    },
    {
      "messageId": 2,
      "wzrk_id": "12345679_20240131",
      "wzrk_pivot": "wzrk_default"
    }
  ]
}'
```
```ruby
require "uri"
require "json"
require "net/http"

url = URI("https://in1.api.clevertap.com/1/inbox/markMessagesAsClicked")

https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true

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
      "messageId": 8
    },
    {
      "wzrk_id": "1711612209_20240328",
      "wzrk_pivot": "wzrk_default",
      "messageId": 9
    }
  ]
})

response = https.request(request)
puts response.read_body
```
```python
import requests
import json

url = "https://in1.api.clevertap.com/1/inbox/markMessagesAsClicked"

payload = json.dumps({
  "userId": "stockx1@test.com",
  "messages": [
    {
      "wzrk_id": "1711383026_20240325",
      "wzrk_pivot": "wzrk_default",
      "messageId": 8
    },
    {
      "wzrk_id": "1711612209_20240328",
      "wzrk_pivot": "wzrk_default",
      "messageId": 9
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
  CURLOPT_URL => 'https://in1.api.clevertap.com/1/inbox/markMessagesAsClicked',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => '',
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 0,
  CURLOPT_FOLLOWLOCATION => true,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => 'POST',
  CURLOPT_POSTFIELDS =>'{"userId": "stockx1@test.com", "messages": [{"wzrk_id":"1711383026_20240325","wzrk_pivot":"wzrk_default", "messageId":8},{"wzrk_id":"1711612209_20240328","wzrk_pivot":"wzrk_default", "messageId":9}]}',
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
  'url': 'https://in1.api.clevertap.com/1/inbox/markMessagesAsClicked',
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
        "messageId": 8
      },
      {
        "wzrk_id": "1711612209_20240328",
        "wzrk_pivot": "wzrk_default",
        "messageId": 9
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
  "error": "Error message"
}
```

For more information about different errors you might encounter, refer to [Inbox API Errors](doc:inbox-api-errors).
