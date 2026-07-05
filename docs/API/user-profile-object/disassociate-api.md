---
title: Disassociate a Phone Number
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: upload-user-profiles-api
      title: Upload User Profiles API
---
# Overview

The Disassociate API enables you to disconnect a phone number from a user profile. There may be cases when users change their phone numbers and those numbers are then assigned to other users after a cooling-off period.

If the user is identified by the phone number, the data of the previous user may be merged with the new user assigned to the phone number. To avoid this, run the Disassociate API and disassociate the phone number from the previous user.

For example, a user discontinues a phone number. The phone number is then assigned to a new user. You can pass the phone number in the Disassociate API. The phone number is disconnected from the previous user profile.

If you want to assign this phone number to a new user, you can use the [Upload User Profiles API](doc:upload-user-profiles-api). A new user is created for the specified phone number. This process must be repeated every time the phone number changes hands.

<Callout icon="📘" theme="info">
  ### **Note**

  You can disassociate a phone number only when it is used as an identity.
</Callout>

Passing phone numbers requires a POST request with a JSON payload specifying the phone number. There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.

<Callout icon="📘" theme="info">
  ### **Note**

  The batch size denotes the maximum number of records that can be submitted in a single call. The response may vary.
</Callout>

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/disassociate`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

Phone numbers are uploaded as a JSON payload. The payload is an object keyed with `d` whose value is an array of records.

The following table lists the body parameters.

| Parameter | Description                   | Type   | Example Value     |
| :-------- | :---------------------------- | :----- | :---------------- |
| type      | Set to `phone`. Required.     | string | `"phone"`         |
| value     | The phone number of the user. | string | `"+919213231415"` |

The following is a sample payload.

```json
{
    "d": [
        {
            "type": "phone",
            "value": "+919213231415"
        },
        {
            "type": "phone",
            "value": "+919213231416"
        }
    ]
}
```

## Example Request

The following is a sample request to the _Disassociate_ API, showing the headers needed to authenticate the request.

```curl
curl -X POST "https://<region>.api.clevertap.com/1/disassociate" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" \
-d '{"d": [{"type": "phone", "value": "+919213231415"},{"type": "phone","value": "+919213231416"}]}'
```
```ruby
require "uri"
require "net/http"

url = URI("https://<region>.api.clevertap.com/1/disassociate")
https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true
request = Net::HTTP::Post.new(url)
request["Content-Type"] = "application/json"
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request.body = "{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}"
response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://<region>.api.clevertap.com/1/disassociate"
payload = "{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}"
headers = {
  'Content-Type': 'application/json',
  'X-CleverTap-Account-Id': 'ACCOUNT_ID',
  'X-CleverTap-Passcode': 'PASSCODE'
}
response = requests.request("POST", url, headers=headers, data=payload)
print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://<region>.api.clevertap.com/1/disassociate');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Content-Type' => 'application/json',
  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
  'X-CleverTap-Passcode' => 'PASSCODE'
));
$request->setBody('{"d": [{"type": "phone", "value": "+919213231415"},{"type": "phone","value": "+919213231416"}]}');
try {
  $response = $request->send();
  if ($response->getStatus() == 200) {
    echo $response->getBody();
  } else {
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  }
} catch(HTTP_Request2_Exception $e) {
  echo 'Error: ' . $e->getMessage();
}
```
```javascript Node.js
var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://<region>.api.clevertap.com/1/disassociate',
  'headers': {
    'Content-Type': 'application/json',
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE'
  },
  body: JSON.stringify({"d":[{"type":"phone","value":"+919213231415"},{"type":"phone","value":"+919213231416"}]})
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});
```
```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() {
  url := "https://<region>.api.clevertap.com/1/disassociate"
  method := "POST"
  payload := strings.NewReader("{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}")
  client := &http.Client{}
  req, err := http.NewRequest(method, url, payload)
  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("Content-Type", "application/json")
  req.Header.Add("X-CleverTap-Account-Id", "ACCOUNT_ID")
  req.Header.Add("X-CleverTap-Passcode", "PASSCODE")
  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)
  fmt.Println(string(body))
}
```

## Example Response

The following is a sample response.

```json
{
    "status": "success",
    "processed": 2,
    "unprocessed": []
}
```

## Debugging

The response includes a `processed` count indicating how many records were successfully processed. If the count is lower than the number of records submitted, some records were invalid or could not be processed.

Unlike other upload APIs, the `unprocessed` array for this endpoint is always returned as an empty array `[]` regardless of whether individual records failed. Per-record error detail is not available in the response. To identify which records failed, compare the `processed` count against the total number of records submitted.

To test if your data submits without errors, add the parameter `dryRun=1` to the URL. This validates the input without submitting the data to CleverTap.

<Callout icon="📘" theme="info">
  ### **Note**

  If a user is identified by the phone number, the GUID can be used to identify the profile after disassociation. All event and profile updates to the user are made using the GUID until a new phone number is passed to the user.
</Callout>

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

## Error Codes

The following is the list of error codes returned by the API:

| HTTP Status | Error                                            | Description                                                                          | Example Error Response                                                                |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| 400         | `"Account credentials missing"`                  | Account ID header present but could not be parsed                                    | `{"status":"fail","error":"Account credentials missing","code":400}`                  |
| 400         | `"Payload is mandatory"`                         | Request body is missing the `d` array entirely                                       | `{"status":"fail","error":"Payload is mandatory","code":400}`                         |
| 400         | `"Batch size exceeded"`                          | More than 1000 records in the `d` array                                              | `{"status":"fail","error":"Batch size exceeded","code":400}`                          |
| 400         | `"Failed to process request."`                   | Unexpected error during request setup                                                | `{"status":"fail","error":"Failed to process request.","code":400}`                   |
| 401         | `"Invalid credentials"`                          | Account ID not found or credentials are invalid                                      | `{"status":"fail","error":"Invalid credentials","code":401}`                          |
| 403         | `"API access has been temporally suspended"`     | Account API access has been suspended                                                | `{"status":"fail","error":"API access has been temporally suspended","code":403}`     |
| 429         | `"Too many concurrent requests"`                 | Per-account concurrent request limit exceeded. Retry with exponential backoff.       | `{"status":"fail","error":"Too many concurrent requests","code":429}`                 |
| 500         | `"Failed to process request"`                    | Unexpected critical server-side failure                                              | `{"status":"fail","error":"Failed to process request","code":500}`                    |
| 503         | `"12 digit account ID mandatory."`               | `X-CleverTap-Account-Id` header is missing from the request                          | `{"status":"fail","error":"12 digit account ID mandatory.","code":503}`               |
| 503         | `"System under maintenance. Please retry later"` | Server temporarily unavailable. Retry after at least 30 seconds.                     | `{"status":"fail","error":"System under maintenance. Please retry later","code":503}` |
| 503         | `"Please come back later"`                       | Global request limit exceeded or system warming up. Retry after at least 30 seconds. | `{"status":"fail","error":"Please come back later","code":503}`                       |

<br />
