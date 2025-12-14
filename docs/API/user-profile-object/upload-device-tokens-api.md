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
> 📘 Note: Uploading historical device tokens for Android/iOS/Web.
>
> Device token upload is only allowed after the user is live on the app, post SDK integration by the app. We cannot fetch tokens of users prior to SDK integration.

The Upload Device Tokens API enables developers to add an existing device token to a CleverTap user profile. 

For example, you can use this API to add a GCM or APNS token to a user profile. Adding this information will let you send push notifications to that device. 

# Overview

Uploading device tokens to CleverTap requires a POST request with a JSON payload specifying the device token information. 100 device token records can be sent per API call. 

## Base URL

[https://api.clevertap.com/1/upload](https://api.clevertap.com/1/upload)

## HTTP Method

POST

## Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Header
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
        X-CleverTap-Account-Id
      </td>

      <td>
        Your CleverTap Account ID.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Account-Id: ACCOUNT\_ID"
      </td>
    </tr>

    <tr>
      <td>
        X-CleverTap-Passcode
      </td>

      <td>
        Your CleverTap Account Passcode.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Passcode: PASSCODE"
      </td>
    </tr>

    <tr>
      <td>
        Content-Type
      </td>

      <td>
        Request content-type is always set to application/json; charset=utf-8.
      </td>

      <td>
        string
      </td>

      <td>
        "Content-Type: application/json; charset=utf-8"
      </td>
    </tr>
  </tbody>
</Table>

## Body Parameters

Device tokens are uploaded as a JSON payload. The payload is an object keyed with “d” whose value is an array describing the device tokens.

For each device token, objectId is required. objectId is CleverTap’s user identifier, which we will use to find the user and add the device token to their profile.


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
        type
      </td>

      <td>
        Set to token. Required.
      </td>

      <td>
        string
      </td>

      <td>
        “token”
      </td>
    </tr>

    <tr>
      <td>
        tokenData
      </td>

      <td>
        Device token properties. Passed as key/value pairs. Required.\
        Id and type are required properties of tokenData. They are passed as strings.
      </td>

      <td>
        object
      </td>

      <td>
        {`
            "d": [  
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
                    }  
            ]  
        `}
      </td>
    </tr>

    <tr>
      <td>
        objectId
      </td>

      <td>
        Identify a unique user by CleverTap Global Object ID.\
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “34322425"
      </td>
    </tr>
  </tbody>
</Table>

Here is an example JSON payload for iOS/Android/Windows.





























































































































































































































































Here is an example JSON payload for Chrome.
Here is an example JSON payload for adding device tokens for multiple OS types.




















































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































## Example Request


```curl
curl -X POST -d '{"d": [{"type": "token","tokenData": {"id": "dYfuuBmHLGI:APA91bEfAq1b6NAmz0uhUbVxwSLqgKF25zDb7vhgajzS-bOAEUKekH_jbzO5oU1Qip-ZLvDpwKccxAxVNjC3rUUJnFTKDUiBcF9AtuG03_PRfjoUxLKHMR9qykK22SiubrLirNzQtNnO","type": "gcm"},"objectId": "37b08803c1af4e10849f530264bac7f8"}]}' "https://api.clevertap.com/1/upload" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://api.clevertap.com/1/upload")
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

response = requests.post('https://api.clevertap.com/1/upload', headers=headers, data=data)
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
$response = Requests::post('https://api.clevertap.com/1/upload', $headers, $data);
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
    url: 'https://api.clevertap.com/1/upload',
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
	D []struct {
		Type      string `json:"type"`
		TokenData struct {
			ID   string `json:"id"`
			Type string `json:"type"`
		} `json:"tokenData"`
		ObjectID string `json:"objectId"`
	} `json:"d"`
}

data := Payload{
// fill struct
}
payloadBytes, err := json.Marshal(data)
if err != nil {
	// handle err
}
body := bytes.NewReader(payloadBytes)

req, err := http.NewRequest("POST", "https://api.clevertap.com/1/upload", body)
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





















































































## Debugging

Requests with processing errors will be returned in the API call response as shown in the example below.
























































































































































































To test if your data will be submitted without errors, you can add the parameter dryRun=1 to the URL. This will validate the input without submitting the data to CleverTap.\
A list of errors code for this API can be found on the [errors page](doc:api-errors).

## Notes

* A device token can be updated against a CleverTap Id (objectId), but not against an email ID or a custom-defined user identity. If given profile already has a token, it will not be updated and an error will be reported into Live Error Stream.
* Concurrent requests are limited to 3 per account. Requests that exceed the limit will return a 429 HTTP response code.