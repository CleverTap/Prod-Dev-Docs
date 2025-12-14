---
title: Upload User Profiles API
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
The Upload User Profiles API enables you to create or update user profiles in CleverTap.\
For example, you can use this API to update a user profile to include information, such as gender, phone, and customer type.

# Overview

Uploading user profiles to CleverTap requires a POST request with a JSON payload specifying the user profile information. 100 user profiles records can be sent per API call. 

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

User profile attribute keys are limited to 120 bytes and their values are limited to 512 bytes. The maximum number of custom user profile keys is 256.

For each user profile, a user identifier is required. This is the key that CleverTap uses to find the user whose profile needs to be updated. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId. If this identity is not found a new user profile will be created. 

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
        Set to profile. Required.
      </td>

      <td>
        string
      </td>

      <td>
        “profile”
      </td>
    </tr>

    <tr>
      <td>
        profileData
      </td>

      <td>
        User profile properties. Passed as key/value pairs. Required.\
        profileData[“Phone”] should be formatted as +[country code][phone number].
      </td>

      <td>
        object
      </td>

      <td>
        "profileData": \{\
                "Name": "Jack Montana",\
                "Email": "[jack@gmail.com](mailto:jack@gmail.com)",\
                "Phone": "+14155551234",\
                "Gender": "M",\
                "Employed": "Y",\
                "Education": "Graduate",\
                "Married": "Y",\
                "MSG-sms": false,\
                "Customer Type": "Platinum"\
          }
      </td>
    </tr>

    <tr>
      <td>
        ts
      </td>

      <td>
        Date and time when the event occurred, Formatted as a UNIX epoch in seconds. Defaults to the current timestamp if omitted. Optional.
      </td>

      <td>
        ts
      </td>

      <td>
        1468308340
      </td>
    </tr>

    <tr>
      <td>
        identity
      </td>

      <td>
        Identity to recognize a unique user. It can be the user’s email, a phone number, or any other identifier that you are using to tag your users.\
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “[jack@gmail.com](mailto:jack@gmail.com)”
      </td>
    </tr>

    <tr>
      <td>
        FBID
      </td>

      <td>
        Identify a unique user by Facebook ID.\
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “34322423"
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

Here is an example JSON payload.

```json
{
  "d": [
    {
      "identity": "1189549",  
      "ts": 1468308340,       
      "type": "profile",
      "profileData": {
        "Name": "Jack Montana",
        "Name": {"$delete" : 1}, //this will delete the value of name
        "Email": "jack@gmail.com",
        "Phone": "+14155551234",
        "Gender": "M",
        "Gender": {"$delete" : 1}, //this will delete the value of gender
        "MSG-sms": false,
        "MSG-email":true,//called when email must be subscribed  
        "MSG-whatsapp": true,
        "MSG-dndPhone": true, 
        "MSG-dndEmail": true, //called when all profiles sharing the same email must be unsubscribed
        "category-unsubscribe": {
        	"email": ["Newsletters", "Promotions"]
        },//called when profile needs to be unsubscribed from multiple subscription groups
        "category-resubscribe": {
        	"email": ["Football", "Movies"]
        },//called when profile needs to be subscribed from multiple subscription groups
        "DOB": "$D_1487576752",    
        "Customer Type": "Platinum",
        "Custom Multi Value":{"$remove":["shoes","bags"]}, // To remove Multi Value properties
        "Custom Multi Value":{"$add":["shoes","bags"]},  //To add Multi Value properties
        "My Number 1": {"$incr" : 10}, //To increment integer data
        "My Number 2": {"$decr" : 30} //To decrement integer data
      }
    }
  ]
}
//$delete applicable only to name and gender fields
```

> 📘 Note
>
> In order to increment/decrement a numerical user property, the property needs have an existing value. If the property does not exist or if the value is NULL, please pass the property key OR add the value before using the incr/decr operator

## Example Request

```curl
curl -X POST -d '{"d":[{"objectId":"25b08803c1af4e00839f530264dac6f8","type":"profile","profileData":{"Name": "Jack Test"}}]}' "https://api.clevertap.com/1/upload" \
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
      "objectId" => "25b08803c1af4e00839f530264dac6f8",
      "type" => "profile",
      "profileData" => {
        "Name" => "Jack Test"
      }
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

data = '{"d":[{"objectId":"25b08803c1af4e00839f530264dac6f8","type":"profile","profileData":{"Name": "Jack Test"}}]}'

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
$data = '{"d":[{"objectId":"25b08803c1af4e00839f530264dac6f8","type":"profile","profileData":{"Name": "Jack Test"}}]}';
$response = Requests::post('https://api.clevertap.com/1/upload', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json; charset=utf-8'
};

var dataString = '{"d":[{"objectId":"25b08803c1af4e00839f530264dac6f8","type":"profile","profileData":{"Name": "Jack Test"}}]}';

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
		ObjectID    string `json:"objectId"`
		Type        string `json:"type"`
		ProfileData struct {
			Name string `json:"Name"`
		} `json:"profileData"`
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

```json
{
    "status": "success",
    "processed": 1,
    "unprocessed": []
}
```

## Debugging

Requests with processing errors will be returned in the API call response as shown in the example below.

```json
{
    "status":<success, partial, fail>, 
    "processed":<count>, 
    "unprocessed": [{"status":"fail", "code":<error code>, "error":<error msg>, "record":<record>}]
}
```

To test if your data will be submitted without errors, you can add the parameter dryRun=1 to the URL. This will validate the input without submitting the data to CleverTap.

A list of errors code for this API can be found on [this page](doc:api-errors).
