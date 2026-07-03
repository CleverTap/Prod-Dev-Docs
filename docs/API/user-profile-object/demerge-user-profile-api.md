---
title: Demerge User Profile
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

This endpoint enables you to demerge a user profile. Demerge unlinks the identities associated with a merged user profile, splitting it back into separate standalone profiles. This reverses profile merges that occurred when multiple identities, such as email, phone, or a custom ID, were associated with the same user.

<Callout icon="📘" theme="info">
  ### **Event Data Loss**

  Demerge removes the links between identities and their associated device mappings. Events that were indexed exclusively through the demerged identity's device associations may become unreachable after the operation.
</Callout>

<Callout icon="⚠️" theme="warning">
  ### **Asynchronous Processing**

  The API returns success immediately after queuing the request. The actual demerge processing happens asynchronously, typically within 5 minutes, during the 4–7 AM window in the account's timezone. Do not assume the demerge is complete at the time of the API response.
</Callout>

For more information on request limits, refer to [API Request Limit](doc:api-request-limit).

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/demerge/profiles.json`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The following table lists the body parameters.

| Parameter  | Description                                                                                                                                                                                                                                                                                         | Required | Type             | Example Value                                           |
| :--------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------- | :--------------- | :------------------------------------------------------ |
| identities | Custom user identities to demerge. A maximum of 100 identities are allowed per request. Must be passed as an array; a single string value is not accepted and returns a 400 error. Only custom identities are supported; CleverTap-generated object IDs (`guid`) are not accepted by this endpoint. | Required | array of strings | \["client-19827239", "[abc@ct.com](mailto:abc@ct.com)"] |

The following is a sample payload.

```json
{
  "identities": ["client-19827239", "abc@ct.com"]
}
```

## Example Request

The following is a sample request to the _Demerge User Profile_ API, showing the headers needed to authenticate the request.

```curl
curl -X POST -d '{"identities":["client-19827239", "abc@ct.com"]}' "https://<region>.api.clevertap.com/1/demerge/profiles.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/demerge/profiles.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json; charset=utf-8"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "identities" => ["client-19827239", "abc@ct.com"]
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

data = '{"identities":["client-19827239", "abc@ct.com"]}'

response = requests.post('https://<region>.api.clevertap.com/1/demerge/profiles.json', headers=headers, data=data)
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
$data = '{"identities":["client-19827239", "abc@ct.com"]}';
$response = Requests::post('https://<region>.api.clevertap.com/1/demerge/profiles.json', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json; charset=utf-8'
};

var dataString = '{"identities":["client-19827239", "abc@ct.com"]}';

var options = {
    url: 'https://<region>.api.clevertap.com/1/demerge/profiles.json',
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
type Payload struct {
	Identities []string `json:"identities"`
}

data := Payload{
// fill struct
}
payloadBytes, err := json.Marshal(data)
if err != nil {
	// handle err
}
body := bytes.NewReader(payloadBytes)

req, err := http.NewRequest("POST", "https://<region>.api.clevertap.com/1/demerge/profiles.json", body)
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

The following is a sample response. A successful request returns HTTP 200 immediately after the demerge is queued. It does not indicate that processing is complete.

```json
{
  "status": "success"
}
```

## Error Codes

The following table lists the error codes returned by this endpoint:

| HTTP Status | Error                                       | Description                                                                                 | Example Error Response                                                           |
| ----------- | ------------------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| 400         | `"Invalid payload received."`               | `identities` field is missing, wrong type, or passed as a single string instead of an array | `{"status":"fail","error":"Invalid payload received.","code":400}`               |
| 400         | `"Max 100 identities allowed per request."` | `identities` array exceeds 100 items                                                        | `{"status":"fail","error":"Max 100 identities allowed per request.","code":400}` |
| 401         | `"Invalid credentials"`                     | Wrong passcode                                                                              | `{"status":"fail","error":"Invalid credentials","code":401}`                     |
| 429         | `"Too many concurrent requests"`            | Per-account throttle exceeded. Retry with exponential backoff.                              | `{"status":"fail","error":"Too many concurrent requests","code":429}`            |
| 503         | `"Server Error. Please retry later"`        | MongoDB error while queuing the request. Retry with backoff.                                | `{"status":"fail","error":"Server Error. Please retry later","code":503}`        |
| 503         | `"Please come back later"`                  | Global throttle exceeded. Retry with backoff.                                               | `{"status":"fail","error":"Please come back later","code":503}`                  |
| 200         | —                                           | Demerge request queued successfully.                                                        | `{"status":"success"}`                                                           |

To understand common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

<br />
