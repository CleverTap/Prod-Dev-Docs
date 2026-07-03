---
title: Upload Device Tokens
excerpt: ''
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

The Upload Device Tokens API enables you to add an existing device token to a CleverTap user profile. For example, you can use this API to add a GCM or APNS token to a user profile. Adding this information enables you to send push notifications to that device.

Uploading device tokens to CleverTap requires a POST request with a JSON payload specifying the device token information. A maximum of 1000 device token records can be sent per API call.

<Callout icon="📘" theme="info">
  ### **Uploading Historical Device Tokens for Android, iOS, and Web**

  Device token upload is only allowed after the user is live on the app, post SDK integration. Tokens of users prior to SDK integration cannot be fetched.
</Callout>

<Callout icon="📘" theme="info">
  ### **Supported Token Types**

  Uploading device tokens via API is available for APNS, FCM, GCM, Chrome (Web Push), WNS, and MPNS.
</Callout>

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/upload`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

Device tokens are uploaded as a JSON payload. The payload is an object keyed with `d` whose value is an array describing the device tokens.

For each device token record, `objectId` is required. This is the only supported identifier for token records. Identity, FBID, and GPID are not accepted for token uploads.

The following table lists the body parameters.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>Parameter</th>
      <th>Description</th>
      <th>Type</th>
      <th>Example Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>type</td>
      <td>Set to token. Required.</td>
      <td>string</td>
      <td>"token"</td>
    </tr>
    <tr>
      <td>tokenData</td>
      <td>Device token properties passed as key/value pairs. Required. `id` and `type` are required properties. `type` must be one of: `apns`, `fcm`, `gcm`, `chrome`, `wns`, `mpns`. For Chrome Web Push tokens, a `keys` object containing `p256dh` and `auth` is strongly recommended (see note below).</td>
      <td>object</td>
      <td>{"id": "dYfuuBm...", "type": "fcm"}</td>
    </tr>
    <tr>
      <td>objectId</td>
      <td>The CleverTap Global Object ID for the user. Required. This is the only supported identifier for token records.</td>
      <td>string</td>
      <td>"37b08803c1af4e10849f530264bac7f8"</td>
    </tr>
  </tbody>
</Table>

<Callout icon="⚠️" theme="warning">
  ###

  **Chrome Web Push encryption keys**

  For Chrome Web Push tokens, `keys.p256dh` and `keys.auth` are strongly recommended. If omitted, the token is stored but encrypted push notifications to this device will not be deliverable, as the token will not carry the required encryption material.
</Callout>

## Example Payloads

The following is a sample payload for iOS, Android, or Windows tokens.

```json
{
  "d": [
    {
      "type": "token",
      "tokenData": {
        "id": "<device_token>",
        "type": "apns"
      },
      "objectId": "<clevertap_object_id>"
    }
  ]
}
```

The following is a sample payload for a Chrome Web Push token.

```json
{
  "d": [
    {
      "type": "token",
      "tokenData": {
        "id": "<device_token>",
        "type": "chrome",
        "keys": {
          "p256dh": "<p256dh_key>",
          "auth": "<auth_key>"
        }
      },
      "objectId": "<clevertap_object_id>"
    }
  ]
}
```

The following is a sample payload for adding device tokens for multiple token types in a single call.

```json
{
  "d": [
    {
      "type": "token",
      "tokenData": {
        "id": "frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf",
        "type": "chrome",
        "keys": {
          "p256dh": "BLc4xRzKlKORKWlbdgFaBrrPK3ydWAHo4M0gs0i1oEKgPpWC5cW8OCzVrOQRv-1npXRWk8udnW3oYhIO4475rds=",
          "auth": "5I2Bu2oKdyy9CwL8QVF0NQ=="
        }
      },
      "objectId": "QBOpVJZmKilRAzfaiVS86Tovxm75lHxg"
    },
    {
      "type": "token",
      "tokenData": {
        "id": "dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO",
        "type": "fcm"
      },
      "objectId": "__gc4grg4053N1eYkW6OBH71jhFev7b1iP"
    },
    {
      "type": "token",
      "tokenData": {
        "id": "frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf",
        "type": "gcm"
      },
      "objectId": "__QlXHaDHBZxZPLEMpPo6VvwdZ7FnbGvWA"
    },
    {
      "type": "token",
      "tokenData": {
        "id": "frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf",
        "type": "apns"
      },
      "objectId": "-HwB9gZWb3RcKhXCQ222FhAhkjeYQs0Hj"
    },
    {
      "type": "token",
      "tokenData": {
        "id": "frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf",
        "type": "wns"
      },
      "objectId": "~9XrS4Dy6GsnDbdX98Ijs63kEtJDQbhJA"
    },
    {
      "type": "token",
      "tokenData": {
        "id": "frfsgvrwe:APfdsafsgdfsgghfgfgjkhfsfgdhjhbvcvnetry767456fxsasdf",
        "type": "mpns"
      },
      "objectId": "~y9MqjkDGRfbzwwH8fZI6LIXgTKaEPxHr"
    }
  ]
}
```

## Example Request

The following is a sample request to the _Upload Device Tokens_ API, showing the headers needed to authenticate the request.

```curl
curl -X POST -d '{"d": [{"type": "token","tokenData": {"id": "dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO","type": "gcm"},"objectId": "37b08803c1af4e10849f530264bac7f8"}]}' "https://<region>.api.clevertap.com/1/upload" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/upload")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json; charset=utf-8"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "d" => [
    {
      "type" => "token",
      "tokenData" => {
        "id" => "dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO",
        "type" => "gcm"
      },
      "objectId" => "37b08803c1af4e10849f530264bac7f8"
    }
  ]
})

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```
```python
import requests

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json; charset=utf-8',
}

data = '{"d": [{"type": "token","tokenData": {"id": "dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO","type": "gcm"},"objectId": "37b08803c1af4e10849f530264bac7f8"}]}'

response = requests.post('https://<region>.api.clevertap.com/1/upload', headers=headers, data=data)
```
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
    'X-CleverTap-Passcode' => 'PASSCODE',
    'Content-Type' => 'application/json; charset=utf-8'
);
$data = '{"d": [{"type": "token","tokenData": {"id": "dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO","type": "gcm"},"objectId": "37b08803c1af4e10849f530264bac7f8"}]}';
$response = Requests::post('https://<region>.api.clevertap.com/1/upload', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json; charset=utf-8'
};

var dataString = '{"d": [{"type": "token","tokenData": {"id": "dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO","type": "gcm"},"objectId": "37b08803c1af4e10849f530264bac7f8"}]}';

var options = {
    url: 'https://<region>.api.clevertap.com/1/upload',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```
```go
// Standard token upload (FCM, GCM, APNS, WNS, MPNS):
type TokenData struct {
	ID   string `json:"id"`
	Type string `json:"type"`
}

// Chrome Web Push token upload (includes encryption keys):
// type TokenData struct {
// 	ID   string `json:"id"`
// 	Type string `json:"type"`
// 	Keys struct {
// 		P256DH string `json:"p256dh"`
// 		Auth   string `json:"auth"`
// 	} `json:"keys"`
// }

type Record struct {
	Type      string    `json:"type"`
	TokenData TokenData `json:"tokenData"`
	ObjectID  string    `json:"objectId"`
}

type Payload struct {
	D []Record `json:"d"`
}

data := Payload{
// fill struct
}
payloadBytes, err := json.Marshal(data)
if err != nil {
	// handle err
}
body := bytes.NewReader(payloadBytes)

req, err := http.NewRequest("POST", "https://<region>.api.clevertap.com/1/upload", body)
if err != nil {
	// handle err
}
req.Header.Set("X-Clevertap-Account-Id", "ACCOUNT_ID")
req.Header.Set("X-Clevertap-Passcode", "PASSCODE")
req.Header.Set("Content-Type", "application/json; charset=utf-8")

resp, err := http.DefaultClient.Do(req)
if err != nil {
	// handle err
}
defer resp.Body.Close()
```

## Example Response

The following is a sample response.

```json
{
    "status": "success",
    "processed": 1,
    "unprocessed": []
}
```

## Debugging

Requests with processing errors are returned in the API call response. The following is a sample error response structure.

```json
{
    "status": "success | partial | fail",
    "processed": <count>,
    "unprocessed": [{"status": "fail", "code": <error code>, "error": <error message>, "record": <record>}]
}
```

To test if your data submits without errors, add the parameter `dryRun=1` to the URL. This validates the input without submitting the data to CleverTap.

For more information on request limits, refer to [API Request Limit](doc:api-request-limit). To understand common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).&#x20;

## Error Codes

The following table lists the error codes returned by this API:<br />

| HTTP Status | Error                                            | Description                                                                           | Example Error Response                                                                               |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| 400         | `"Payload is mandatory"`                         | Empty request body                                                                    | `{"status":"fail","error":"Payload is mandatory","code":400}`                                        |
| 400         | `"Malformed request"`                            | JSON parse failure                                                                    | `{"status":"fail","error":"Malformed request","code":400}`                                           |
| 401         | `"Invalid credentials"`                          | Wrong or missing passcode                                                             | `{"status":"fail","error":"Invalid credentials","code":401}`                                         |
| 403         | `"API access has been temporally suspended"`     | Account API access suspended                                                          | `{"status":"fail","error":"API access has been temporally suspended","code":403}`                    |
| 429         | `"Too many concurrent requests"`                 | Per-account concurrent request limit exceeded. Retry with exponential backoff.        | `{"status":"fail","error":"Too many concurrent requests","code":429}`                                |
| 503         | `"12 digit account ID mandatory."`               | Missing or invalid `X-CleverTap-Account-Id` header                                    | `{"status":"fail","error":"12 digit account ID mandatory.","code":503}`                              |
| 503         | `"Please come back later"`                       | Global throttle limit exceeded. Retry with backoff.                                   | `{"status":"fail","error":"Please come back later","code":503}`                                      |
| 200         | `"Data is not of type Event, Profile or Token."` | `type` field is not `"token"`, `"event"`, or `"profile"`                              | `{"status":"fail","code":524,"error":"Data is not of type Event, Profile or Token.","record":{...}}` |
| 200         | `"Invalid 'objectId'."`                          | `objectId` missing or empty                                                           | `{"status":"fail","code":530,"error":"Invalid 'objectId'.","record":{...}}`                          |
| 200         | `"Invalid token data."`                          | `tokenData` is null or has fewer than 2 fields                                        | `{"status":"fail","code":531,"error":"Invalid token data.","record":{...}}`                          |
| 200         | `"Token 'id' is not present."`                   | `tokenData.id` missing or empty                                                       | `{"status":"fail","code":532,"error":"Token 'id' is not present.","record":{...}}`                   |
| 200         | `"Token 'type' is not present."`                 | `tokenData.type` missing or empty                                                     | `{"status":"fail","code":533,"error":"Token 'type' is not present.","record":{...}}`                 |
| 200         | `"Token 'type' is not valid for 'objectId'."`    | Token type doesn't match the device platform (for example, `fcm` for an iOS objectId) | `{"status":"fail","code":534,"error":"Token 'type' is not valid for 'objectId'.","record":{...}}`    |
| 200         | `"Invalid token type"`                           | `tokenData.type` is not one of: `apns`, `gcm`, `fcm`, `chrome`, `wns`, `mpns`         | `{"status":"fail","code":535,"error":"Invalid token type","record":{...}}`                           |

<br />
