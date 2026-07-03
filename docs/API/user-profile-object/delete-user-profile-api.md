---
title: Delete User Profile
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

This endpoint enables you to delete a user profile.

<Callout icon="📘" theme="info">
  ###

  **Per-Request Limit**

  You can pass a maximum of 100 values per `identity` array and a maximum of 100 values per `guid` array per request.
</Callout>

For more information on request limits, refer to [API Request Limit](doc:api-request-limit).

# Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/delete/profiles.json`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

# HTTP Method

POST

# Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

# Body Parameters

The body is uploaded as a JSON payload.

The following table lists the body parameters.

| Parameter | Description                                                                                                                                                                        | Required | Type                       | Example Value               |
| :-------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------- | :------------------------- | :-------------------------- |
| identity  | Custom user identity defined by you. Do not pass both `identity` and `guid` in the same request, as this returns a 400 error.                                                      | Optional | string or array of strings | \["client-19827239", "abc"] |
| guid      | The CleverTap-generated profile ID. Equivalent to `objectId` in other CleverTap API docs. Do not pass both `guid` and `identity` in the same request, as this returns a 400 error. | Optional | string or array of strings | \["ctid123", "ctid456"]     |

<Callout icon="📘" theme="info">
  ### **Note**

  For a successful request, include either `identity` or `guid` in the request body, but not both. Passing both parameters in the same request returns a 400 error.
</Callout>

The following are sample payloads showing the accepted formats.

```json
{ "identity": ["client-19827239", "abc"] }
```
```json
{ "identity": "client-19827239" }
```
```json
{ "guid": ["ctid123", "ctid456"] }
```
```json
{ "guid": "clientid123" }
```

# Example Request

The following is a sample request to the _Delete User Profile_ API, showing the headers needed to authenticate the request.

```curl
curl -X POST -d '{"guid":"df2e224d90874887b4d61153ef3a2508"}' "https://<region>.api.clevertap.com/1/delete/profiles.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/delete/profiles.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json; charset=utf-8"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "guid" => "df2e224d90874887b4d61153ef3a2508"
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

data = '{"guid":"df2e224d90874887b4d61153ef3a2508"}'

response = requests.post('https://<region>.api.clevertap.com/1/delete/profiles.json', headers=headers, data=data)
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
$data = '{"guid":"df2e224d90874887b4d61153ef3a2508"}';
$response = Requests::post('https://<region>.api.clevertap.com/1/delete/profiles.json', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json; charset=utf-8'
};

var dataString = '{"guid":"df2e224d90874887b4d61153ef3a2508"}';

var options = {
    url: 'https://<region>.api.clevertap.com/1/delete/profiles.json',
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
// To pass a single guid as a string:
type Payload struct {
	GUID string `json:"guid"`
}

// To pass multiple guids as an array, use a string slice:
// type Payload struct {
// 	GUID []string `json:"guid"`
// }

data := Payload{
// fill struct
}
payloadBytes, err := json.Marshal(data)
if err != nil {
	// handle err
}
body := bytes.NewReader(payloadBytes)

req, err := http.NewRequest("POST", "https://<region>.api.clevertap.com/1/delete/profiles.json", body)
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

# Example Response

The following is a sample response. A successful request returns HTTP 200 with a `status` of `success`. Unlike other upload APIs, this response does not include a processed count or an unprocessed list.

```json
{
  "status": "success"
}
```

# Error Codes

The following table lists the error codes returned by this endpoint.

| HTTP Status | Code or Status | Error                                                                              | Description                                             | Example Error Response                                                                                                  |
| ----------- | -------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| 400         | —              | `"Payload is mandatory"`                                                           | Empty or missing request body                           | `{"status":"fail","error":"Payload is mandatory","code":400}`                                                           |
| 400         | —              | `"Sending either identities or guids in payload is mandatory"`                     | Neither `identity` nor `guid` field present             | `{"status":"fail","error":"Sending either identities or guids in payload is mandatory","code":400}`                     |
| 400         | —              | `"Invalid payload. Empty payload is not allowed."`                                 | Both arrays present but empty                           | `{"status":"fail","error":"Invalid payload. Empty payload is not allowed.","code":400}`                                 |
| 400         | —              | `"Invalid payload. Received both guid and identity. Only one of them is allowed."` | Both `guid` and `identity` provided in the same request | `{"status":"fail","error":"Invalid payload. Received both guid and identity. Only one of them is allowed.","code":400}` |
| 400         | —              | `"Invalid payload. Max 100 guids allowed per request."`                            | `guid` array exceeds 100 items                          | `{"status":"fail","error":"Invalid payload. Max 100 guids allowed per request.","code":400}`                            |
| 400         | —              | `"Invalid payload. Max 100 identities allowed per request."`                       | `identity` array exceeds 100 items                      | `{"status":"fail","error":"Invalid payload. Max 100 identities allowed per request.","code":400}`                       |
| 503         | —              | `"Server Error. Please retry later"`                                               | MongoDB error while storing the delete request          | `{"status":"fail","error":"Server Error. Please retry later","code":503}`                                               |
| 200         | `success`      | —                                                                                  | Delete request queued successfully                      | `{"status":"success"}`                                                                                                  |

<Callout icon="❗️" theme="error">
  ### **Data Processing Rule**

  CleverTap processes deletion requests asynchronously after a 24-hour delay. Processing this request permanently removes the profile from CleverTap analytics, and the profile's event history is also excluded from all future queries. This cannot be recovered. For more information, refer to [Restoring the Deleted Data](doc:api-faqs#q-can-i-restore-the-profile-and-its-event-data-after-deleting-it).
</Callout>

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
