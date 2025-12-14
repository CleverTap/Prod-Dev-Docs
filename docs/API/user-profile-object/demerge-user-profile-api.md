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

This endpoint enables you to demerge a user profile.

> 📘 Event Data Loss
> 
> There might be instances when you may lose the event data associated with the user profile after demerging.

## Base URL

Here is an example base URL from the account in the India region:  
<https://in1.api.clevertap.com/1/demerge/profiles.json>

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload. 

| Parameter  | Description                                                                                  | Required | Type         | Example Value                     |
| :--------- | :------------------------------------------------------------------------------------------- | :------- | :----------- | :-------------------------------- |
| identities | Custom user identity defined by you. A maximum of 100 identities are allowed in one request. | optional | string/array | [“client-19827239”, “abc@ct.com”] |

Below is an example payload.

```json
{
	"identities": ["client-19827239", "abc@ct.com"]
}
```

## Example Request

Here is an example cURL request to the _Demerge User Profile_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"identities":["client-19827239", "abc@ct.com"]}'
"https://in1.api.clevertap.com/1/demerge/profiles.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/demerge/profiles.json")
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

response = requests.post('https://in1.api.clevertap.com/1/demerge/profiles.json', headers=headers, data=data)
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
$data = '{"identities":["client-19827239", "abc@ct.com"]}' ;
$response = Requests::post('https://in1.api.clevertap.com/1/demerge/profiles.json', $headers, $data);
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
    url: 'https://in1.api.clevertap.com/1/demerge/profiles.json',
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
	GUID string `json:"identities"`
}

data := Payload{
// fill struct
}
payloadBytes, err := json.Marshal(data)
if err != nil {
	// handle err
}
body := bytes.NewReader(payloadBytes)

req, err := http.NewRequest("POST", "https://in1.api.clevertap.com/1/demerge/profiles.json", body)
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

For more information on request limit, refer to [API Request Limit](doc:api-request-limit). 

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).