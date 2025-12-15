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
  **GUIDs Limit**

  You can pass a maximum of 100 IDs or globally unique identifiers (GUIDs) per request.
</Callout>

For more information on request limit, refer to [API Request Limit](doc:api-request-limit).

## Base URL

Here is an example base URL from the account in the India region:  
[https://in1.api.clevertap.com/1/delete/profiles.json](https://in1.api.clevertap.com/1/delete/profiles.json)

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload.

| Parameter | Description                          | Required | Type         | Example Value              |
| :-------- | :----------------------------------- | :------- | :----------- | :------------------------- |
| identity  | Custom user identity defined by you. | optional | string/array | ["client-19827239", "abc"] |
| guid      | CleverTap ID.                        | optional | string/array | ["ctid123", "ctid456"]     |

<Callout icon="📘" theme="info">
  **Note**

  For a successful API request, make sure one of the optional parameters, either `identity` or `guid` is present in the body parameter.
</Callout>

Below is an example payload.

```json
{
	"identity": ["client-19827239", "abc"]
}
OR
{
	"identity": "client-19827239"
}
OR 
{
	"guid": ["ctid123", "ctid456"]
}
OR
{
	"guid": "clientid123"
}
```

## Example Request

Here is an example cURL request to the _Delete User Profile_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"guid":"df2e224d90874887b4d61153ef3a2508"}' "https://in1.api.clevertap.com/1/delete/profiles.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/delete/profiles.json")
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

response = requests.post('https://in1.api.clevertap.com/1/delete/profiles.json', headers=headers, data=data)
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
$response = Requests::post('https://in1.api.clevertap.com/1/delete/profiles.json', $headers, $data);
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
    url: 'https://in1.api.clevertap.com/1/delete/profiles.json',
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
	GUID string `json:"guid"`
}

data := Payload{
// fill struct
}
payloadBytes, err := json.Marshal(data)
if err != nil {
	// handle err
}
body := bytes.NewReader(payloadBytes)

req, err := http.NewRequest("POST", "https://in1.api.clevertap.com/1/delete/profiles.json", body)
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

```json
{
  "status": "success"
}
```

# FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

<Callout icon="❗️" theme="error">
  **Data Processing Rule**

  Once a request is accepted, CleverTap processes the data during non-business hours. Processing this request leads to the permanent deletion of profiles along with their event data that can not be recovered. For more information, refer to [Restoring the Delete Data](doc:api-faqs#q-can-i-restore-the-profile-and-its-event-data-after-deleting-it).
</Callout>
