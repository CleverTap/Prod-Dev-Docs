---
title: External Trigger API
excerpt: Learn how to enable sending campaigns via External Trigger API.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The External Trigger campaign allows you to trigger campaign delivery based on API calls or external events. The API endpoint enables you to send messages to designated users via the External Trigger API.

<Callout icon="📘" theme="info">
  **Public Beta**

  This feature is released in Public Beta.
</Callout>

# Base URL

The base URL varies depending on the type of External Trigger campaign. Here are the example base URLs from the account in the India region, along with use cases and examples:

| Campaign Type                                     | Description                                                                                                                                                       | Example                                                                                                                                                                                     | Base URL                                                                                                               |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| Single Campaign ID targeting one or more users    | Use this endpoint when you want to trigger a single External Trigger campaign (identified by a unique `campaign_id`) for one or more users in a single API call.  | Sending a promotional offer campaign to a list of users. OTPs or transactional messages                                                                                                     | [https://in1.api.clevertap.com/1/send/externaltrigger.json](https://in1.api.clevertap.com/1/send/externaltrigger.json) |
| Multiple Campaign IDs targeting one or more users | Use this endpoint to trigger different External Trigger campaigns for different users or to send multiple campaigns to the same users within a single API request | For example, you could send a "You’ve Earned 100 Loyalty Points" campaign to high spenders and a "Refer a Friend and Get Rs. 200" campaign to low-engagement users in the same API request. | [https://in1.api.clevertap.com/2/send/externaltrigger.json](https://in1.api.clevertap.com/2/send/externaltrigger.json) |

## Region

To identify the API endpoint for your account's region, refer to [Region](doc:common-api-components#region).

# HTTP Method

`POST`

# Headers

The following headers must be included with every API request:

| Header                   | Description                                                                                                    | Required                                                      | Example            |
| ------------------------ | -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- | ------------------ |
| `X-CleverTap-Account-Id` | Your CleverTap account ID.                                                                                     | Yes                                                           | `ABCD-RSTU-1234`   |
| `X-CleverTap-Passcode`   | Passcode generated for the account to authenticate API requests.                                               | Yes                                                           | `XXXXXX`           |
| `X-CleverTap-Token`      | Token for authenticating API requests. Required for certain endpoints such as bulk or multiple campaign sends. | Required only for bulk or multiple sends else it is Optional. | `YYYYYY`           |
| `Content-Type`           | Content type of the request payload. Must be `application/json`.                                               | Yes                                                           | `application/json` |

For more information, see [Headers](doc:common-api-components#headers).

# Body Parameters

The body is uploaded as a JSON payload. A payload is an object keyed with `"ExternalTrigger"` whose value is an array including the following information: target user, campaign ID, and key-value pairs for personalization.

| Parameter         | Description                                                                                                                                                                                                                                                                                                                                                 | Required | Type                                                  | Example                                                                                                                                                    |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ----------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `to`              | Represents the user to whom the campaign is to be sent. Must contain the following identifiers per user: `email`, `identity`, or `objectId`. Multiple identifiers in a single user object are not allowed. For bulk send (up to 1000 users), use the `/2/send/externaltrigger.json`. For a single send, use`/1/send/externaltrigger.json for single send}`. | Yes      | List of strings                                       | `["johndoe@example.com"]`                                                                                                                                  |
| `campaign_id`     | Unique ID of the campaign. For sending multiple campaigns, include up to 5 campaign IDs.                                                                                                                                                                                                                                                                    | Yes      | String (single ID) or List of integers (multiple IDs) | `"1709550110"` or `[1709550110,1709550111]`                                                                                                                |
| `ExternalTrigger` | Key-value map containing the campaign message content. You can include personalized fields such as name, product details, and pricing.                                                                                                                                                                                                                      | Yes      | Map                                                   | `{ "ExternalTrigger": { "Name": "John", "productDetails": [{ "productName": "Apple Watch","productID": "APPL2209","imageURL": "https://example.jpg"}] } }` |

<Callout icon="🚧" theme="warn">
  **Note**

  CleverTap only supports **String** data type for mapped property values.
</Callout>

# Example Payloads

The following examples show how to structure request bodies for different use cases of the External Trigger API. Each payload demonstrates the minimum required fields (`to`, `campaign_id`, and `ExternalTrigger`) along with sample personalization parameters.

```json Single Campaign ID Targeting Single User
curl --location 'https://in1.api.clevertap.com/1/send/externaltrigger.json' \
--header 'X-CleverTap-Account-Id: <YOUR_ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <YOUR_PASSCODE>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "to": {
        "email": [
            "xyz@gmail.com"
        ]
    },
    "campaign_id": "1695721550",
    "ExternalTrigger": {
        "Name": "John Doe",
        "productDetails": [
            {
                "productName": "Apple Watch",
                "productID": "APPL2209",
                "imageURL": "https://example.jpg",
                "pricingDetails": [
                    {
                        "name": "Transaction Fee (10%)",
                        "value": "$37.20"
                    },
                    {
                        "name": "Payment Proc. (3%)",
                        "value": "$11.16"
                    },
                    {
                        "name": "Shipping",
                        "value": "$8.66"
                    }
                ]
            },
            {
                "productName": "iphone 14",
                "productID": "APPL1804",
                "imageURL": "https://example.jpg",
                "pricingDetails": [
                    {
                        "name": "Transaction Fee (10%)",
                        "value": "$107.20"
                    },
                    {
                        "name": "Payment Proc. (3%)",
                        "value": "$22.16"
                    },
                    {
                        "name": "Shipping",
                        "value": "$10.66"
                    }
                ]
            }
        ]
    }
}'
```
```json Single Campaign ID Targeting Multiple Users
curl --location 'https://in1.api.clevertap.com/1/send/externaltrigger.json' \
--header 'X-CleverTap-Account-Id: <YOUR_ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <YOUR_PASSCODE>' \
--header 'Content-Type: application/json' \
--header 'X-CleverTap-Token: <YOUR_TOKEN>' \
--data-raw '{
    "to": {
        "email": [
            "abc@gmail.com",
            "xyz@gmail.com"
        ]
    },
    "campaign_id": "1709550110",
    "ExternalTrigger": {
        "Name": "John Doe",
        "productDetails": [
            {
                "productName": "Apple Watch",
                "productID": "APPL2209",
                "imageURL": "http://example.jpg",
                "pricingDetails": [
                    {
                        "name": "Transaction Fee (10%)",
                        "value": "$37.20"
                    },
                    {
                        "name": "Payment Proc. (3%)",
                        "value": "$11.16"
                    },
                    {
                        "name": "Shipping",
                        "value": "$8.66"
                    }
                ]
            },
            {
                "productName": "iphone 14",
                "productID": "APPL1804",
                "imageURL": "http://example.jpg",
                "pricingDetails": [
                    {
                        "name": "Transaction Fee (10%)",
                        "value": "$107.20"
                    },
                    {
                        "name": "Payment Proc. (3%)",
                        "value": "$22.16"
                    },
                    {
                        "name": "Shipping",
                        "value": "$10.66"
                    }
                ]
            }
        ]
    }
}'
```
```json Multiple Campaign IDs Targeting One or More Users
curl --location 'https://in1.api.clevertap.com/2/send/externaltrigger.json' \
--header 'X-CleverTap-Account-Id: <YOUR_ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <YOUR_PASSCODE>' \
--header 'Content-Type: application/json' \
--header 'X-CleverTap-Token: <YOUR_TOKEN>' \
--data-raw '{
    "to": {
        "email": [
            "abc@gmail.com",
            "xyz@gmail.com"
        ],
        "identity": [
            "Qwefygji",
            "9851348"
        ],
        "objectId": [
            "Zfagw54thrt0syh9jinoga",
            "Mpgorepk6o435mp234o"
        ]
    },
    "campaign_id": [1709550110, 1709550111],
    "ExternalTrigger": {
        "Name": "John Doe",
        "productDetails": [
            {
                "productName": "Apple Watch",
                "productID": "APPL2209",
                "imageURL": "http://example.jpg",
                "pricingDetails": [
                    {
                        "name": "Transaction Fee (10%)",
                        "value": "$37.20"
                    },
                    {
                        "name": "Payment Proc. (3%)",
                        "value": "$11.16"
                    },
                    {
                        "name": "Shipping",
                        "value": "$8.66"
                    }
                ]
            },
            {
                "productName": "iphone 14",
                "productID": "APPL1804",
                "imageURL": "http://example.jpg",
                "pricingDetails": [
                    {
                        "name": "Transaction Fee (10%)",
                        "value": "$107.20"
                    },
                    {
                        "name": "Payment Proc. (3%)",
                        "value": "$22.16"
                    },
                    {
                        "name": "Shipping",
                        "value": "$10.66"
                    }
                ]
            }
        ]
    }
}'
```

# Example Request

Here is an example cURL request to the _External Trigger_ API showing the headers needed to authenticate the request from the account in the India region:

* **For Single Campaign ID targeting one or more users**

```curl
curl --location --request POST 'https://in1.api.clevertap.com/1/send/externaltrigger.json' \
--header 'X-CleverTap-Account-Id: <YOUR_ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <YOUR_PASSCODE>' \
--header 'Content-Type: application/json' \
--data-raw '{
"to": { "email": [ "<Email ID>" ] },
"campaign_id": "<campaign ID>",
"ExternalTrigger": {"<key>" : "<value>"}
}'
```
```ruby
require 'httparty'

url = 'https://in1.api.clevertap.com/1/send/externaltrigger.json' # Replace with your actual API endpoint
headers = {
  'X-CleverTap-Account-Id' => '<YOUR_ACCOUNT_ID>',
  'X-CleverTap-Passcode' => '<YOUR_PASSCODE>',
  'Content-Type' => 'application/json'
}

data = {
  "to": {
    "email": [
      "<Email ID>"
    ]
  },
  "campaign_id": "<campaign ID>",
  "ExternalTrigger": {
    "<key>": "<value>"
  }
}

response = HTTParty.post(url, headers: headers, body: data.to_json)

# Check the response
if response.code == 200
  puts "Request successful"
  puts "Response: #{response.body}"
else
  puts "Request failed with code #{response.code}"
  puts "Error: #{response.body}"
end

```
```python
import requests
import json

url = 'https://in1.api.clevertap.com/1/send/externaltrigger.json'  # Replace with your actual API endpoint
headers = {
    'X-CleverTap-Account-Id': '<YOUR_ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<YOUR_PASSCODE>',
    'Content-Type': 'application/json'
}

data = {
    "to": {
        "email": [
            "<Email ID>"
        ]
    },
    "campaign_id": "<campaign ID>",
    "ExternalTrigger": {
        "<key>": "<value>"
    }
}

response = requests.post(url, headers=headers, data=json.dumps(data))

# Check the response
if response.status_code == 200:
    print("Request successful")
    print("Response:", response.text)
else:
    print(f"Request failed with code {response.status_code}")
    print("Error:", response.text)
```
```php
<?php

$url = 'https://in1.api.clevertap.com/1/send/externaltrigger.json'; // Replace with your actual API endpoint

$headers = array(
    'X-CleverTap-Account-Id: <YOUR_ACCOUNT_ID>',
    'X-CleverTap-Passcode: <YOUR_PASSCODE>',
    'Content-Type: application/json'
);

$data = array(
    'to' => array(
        'email' => array(
            '<Email ID>'
        )
    ),
    'campaign_id' => '<campaign ID>',
    'ExternalTrigger' => array(
        '<key>' => '<value>'
    )
);

$data_json = json_encode($data);

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, 'POST');
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_json);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$response = curl_exec($ch);

if ($response === false) {
    echo 'cURL Error: ' . curl_error($ch);
} else {
    $http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    if ($http_code == 200) {
        echo 'Request successful' . PHP_EOL;
        echo 'Response: ' . $response . PHP_EOL;
    } else {
        echo 'Request failed with code ' . $http_code . PHP_EOL;
        echo 'Error: ' . $response . PHP_EOL;
    }
}

curl_close($ch);

?>

```
```javascript Node.js
const axios = require('axios');

const url = 'https://in1.api.clevertap.com/1/send/externaltrigger.json'; // Replace with your actual API endpoint

const headers = {
  'X-CleverTap-Account-Id': '<YOUR_ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<YOUR_PASSCODE>',
  'Content-Type': 'application/json',
};

const data = {
  to: {
    email: ['<Email ID>'],
  },
  campaign_id: '<campaign ID>',
  ExternalTrigger: {
    '<key>': '<value>',
  },
};

axios
  .post(url, data, { headers })
  .then((response) => {
    console.log('Request successful');
    console.log('Response:', response.data);
  })
  .catch((error) => {
    if (error.response) {
      console.log('Request failed with code', error.response.status);
      console.log('Error:', error.response.data);
    } else {
      console.error('Error:', error.message);
    }
  });

```

* **For Multiple Campaign IDs targeting one or more users**

```curl
curl --location --request POST 'https://in1.api.clevertap.com/2/send/externaltrigger.json' \
--header 'X-CleverTap-Account-Id: <YOUR_ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <YOUR_PASSCODE>' \
--header 'Content-Type: application/json' \
--data-raw '{
"to": { "email": [ "<Email ID>" ] },
"campaign_id": [<campaign ID1>,<campaign ID2>],
"ExternalTrigger": {"<key>" : "<value>"}
}'
```
```ruby
require 'httparty'
require 'json'

url = 'https://in1.api.clevertap.com/2/send/externaltrigger.json'

headers = {
  'X-CleverTap-Account-Id' => '<YOUR_ACCOUNT_ID>',
  'X-CleverTap-Passcode'   => '<YOUR_PASSCODE>',
  'Content-Type'           => 'application/json'
}

data = {
  "to" => {
    "email" => ["<Email ID>"]
  },
  "campaign_id" => ["<campaign ID1>", "<campaign ID2>"],
  "ExternalTrigger" => {
    "<key>" => "<value>"
  }
}

response = HTTParty.post(url, headers: headers, body: data.to_json)

if response.code == 200
  puts "✅ Request successful!"
  puts "Response: #{response.body}"
else
  puts "❌ Request failed with code #{response.code}"
  puts "Error: #{response.body}"
end
```
```python
import requests
import json

url = "https://in1.api.clevertap.com/2/send/externaltrigger.json"

payload = "{\n\"to\": { \"email\": [ \"<Email ID\" ] },\n\"campaign_id\": [<campaign ID1>,<campaign ID2>],\n\"ExternalTrigger\": {\"<key>\" : \"<value>\"}\n}"
headers = {
  'X-CleverTap-Account-Id': '<YOUR_ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<YOUR_PASSCODE>',
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://in1.api.clevertap.com/2/send/externaltrigger.json');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => '<YOUR_ACCOUNT_ID>',
  'X-CleverTap-Passcode' => '<YOUR_PASSCODE>',
  'Content-Type' => 'application/json'
));
$request->setBody('{\n"to": { "email": [ "<Email ID" ] },\n"campaign_id": [<campaign ID1>,<campaign ID2>],\n"ExternalTrigger": {"<key>" : "<value>"}\n}');
try {
  $response = $request->send();
  if ($response->getStatus() == 200) {
    echo $response->getBody();
  }
  else {
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  }
}
catch(HTTP_Request2_Exception $e) {
  echo 'Error: ' . $e->getMessage();
}
```
```javascript Node.js
Node js

var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://in1.api.clevertap.com/2/send/externaltrigger.json',
  'headers': {
    'X-CleverTap-Account-Id': '<YOUR_ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<YOUR_PASSCODE>',
    'Content-Type': 'application/json'
  },
  body: '{\n"to": { "email": [ "<Email ID" ] },\n"campaign_id": [<campaign ID1>,<campaign ID2>],\n"ExternalTrigger": {"<key>" : "<value>"}\n}'

};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});
```

## Example Response

The response is a JSON object containing either the success or failure status. This varies depending on the type of External Trigger campaign, as shown below:

```json Single Campaign ID Targeting One or More Users
{
    "message": "Added to queue for processing",
    "status": "success"
}
```
```json Multiple Campaign IDs Targeting One or More Users
{
   "message": "Added to queue for processing",
   "status": "partial",
   "error": "The following campaignIDs have not been added to the processing queue: [65] as they have either been stopped, or are not ready, or require approval or lack type external trigger. ",
   "code": 200
}
//The error indicates that the all the campaigns except the one with ID 65 have been added to the queue. The campaign with ID 65 is dropped.
```

## Error Codes

If there is an error in the campaign delivery, the following error codes indicate the nature of the error encountered:

| Error Code | Details                                                                                                           | Sample JSON Payload                                                                                                     | Version |
| :--------- | :---------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------- | :------ |
| 400        | Occurs when the credentials are invalid.                                                                          | `{"status":"fail","error":"Invalid Credentials","code":400}`                                                            | V1/V2   |
| 400        | Occurs when the External Trigger feature is not enabled for your account.                                         | `{"status":"fail","error":"External Trigger feature is not enabled for your account.","code":11}`                       | V1/V2   |
| 400        | Occurs when the Campaign ID is invalid.                                                                           | `{"status":"fail","error":"Invalid Campaign ID/IDs...","code":77}`                                                      | V1/V2   |
| 400        | Occurs when the Campaign ID(s) are missing in the API request.                                                    | `{"status":"fail","error":"Campaign ID(s) are missing in the API request.","code":76}`                                  | V1/V2   |
| 400        | Occurs when more than 5 campaign IDs are included in a request.                                                   | `{"status":"fail","error":"Sorry!! We don't allow more than 5 campaignIDs in one go :)","code":96}`                     | V2      |
| 400        | Occurs when the payload length exceeds 100KB.                                                                     | `{"status":"fail","error":"The payload length exceeds the permissible limit...","code":17}`                             | V1/V2   |
| 400        | Occurs when the `to` parameter is missing.                                                                        | `{"status":"fail","error":"to is a mandatory field.","code":21}`                                                        | V1/V2   |
| 400        | Occurs when at least one of Identity/Email/ObjectId is missing.                                                   | `{"status":"fail","error":"Incorrect payload: At least one of Identity/Email/ObjectId is required","code":23}`          | V1/V2   |
| 400        | Occurs when there are one or more invalid recipients.                                                             | `{"status":"fail","error":"Invalid recipients.","code":25}`                                                             | V1/V2   |
| 400        | Occurs when recipient count exceeds 1000.                                                                         | `{"status":"fail","error":"Exceeded_max_identities: Please limit the number of identities to 1000 or fewer","code":24}` | V1/V2   |
| 400        | Occurs when required JSON keys are missing.                                                                       | `{"status":"fail","error":"Required JSON payload not present for these keys [keyNames]","code":77}`                     | V1      |
| 400        | Occurs when profile not found.                                                                                    | `{"status":"fail","error":"Profile not found: {param}","code":78}`                                                      | V1      |
| 400        | Occurs when all profiles are not found.                                                                           | `{"status":"fail","error":"All Profile not found: [invalid_profiles]","code":78}`                                       | V2      |
| 400        | Occurs when request cannot be processed.                                                                          | `{"status":"fail","error":"Unexpected error, please try again","code":93}`                                              | V1/V2   |
| 400        | Occurs when the `X-CleverTap-Token` header is missing but required (for example, `/2/send/externaltrigger.json`). | `{"status":"fail","error":"Missing Token: X-CleverTap-Token is required for this request.","code":22}`                  | V2      |
| 501        | Occurs when an unexpected error occurs.                                                                           | `{"status":"fail","error":"Unexpected error, please try again","code":97}`                                              | V1/V2   |
