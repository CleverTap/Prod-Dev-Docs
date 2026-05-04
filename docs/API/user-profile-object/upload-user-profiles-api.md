---
title: Upload User Profiles
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

Use this API to create or update user profiles in CleverTap. With this API, you can update user profile information such as gender, phone, and customer type.

Uploading user profiles to CleverTap requires a POST request with a JSON payload specifying the user profile information. A maximum of 1000 user profile records can be sent per API call.

<Callout icon="📘" theme="info">
  **Concurrent Requests**

  Concurrent requests to the _Upload User Profiles_ API are limited to 15 per account. Requests that exceed the limit will return a 429 HTTP response code.
</Callout>

For more information on request limit, refer to [API Request Limit](doc:api-request-limit).

## Base URL

Here is an example base URL for an account in the India region:  
[https://in1.api.clevertap.com/1/upload](https://in1.api.clevertap.com/1/upload)

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## Account-level Passcode for APIs

You can have multiple account-level passcodes rather than having a single account passcode used across different partners. This offers better security when using CleverTap APIs.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

User profile attribute keys are limited to 120 bytes, and their values are limited to 512 bytes. The maximum number of custom user profile keys is 256.

For each user profile, a user identifier is required. This is the key that CleverTap uses to find the user whose profile needs to be updated. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId. A new user profile will be created if this identity is not found.

<Table align={["left","left","left","left","left"]}>
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

      <th>
        Required/Optional
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        type
      </td>

      <td>
        Set to profile.
      </td>

      <td>
        string
      </td>

      <td>
        “profile”
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        profileData
      </td>

      <td>
        <ul><li>Accepted format for profileData[“Phone”]: +[country code][phone number].</li>\
        <li> Accepted values for profileData["Gender"]: "M" or "F".</li></ul>
      </td>

      <td>
        object
      </td>

      <td>
        "profileData": \{  
        "Name": "Jack Montana",  
        "Email": "[jack@gmail.com](mailto:jack@gmail.com)",  
        "Phone": "+14155551234",  
        "Gender": "M",  
        "Employed": "Y",  
        "Education": "Graduate",  
        "Married": "Y",  
        "MSG-sms": false,  
        "Customer Type": "Platinum"  
        }
      </td>

      <td>
        Required
      </td>
    </tr>

    <tr>
      <td>
        identity
      </td>

      <td>
        Identity to recognize a unique user. It can be the user’s email address, a phone number, or any other identifier you use to tag your users.  
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “[jack@gmail.com](mailto:jack@gmail.com)”
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        FBID
      </td>

      <td>
        Identify a unique user by Facebook ID.  
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “34322423"
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        objectId
      </td>

      <td>
        Identify a unique user by CleverTap Global Object ID.  
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “34322425"
      </td>

      <td>
        Optional
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
        "MSG-dndWhatsApp": true, 
        "MSG-dndPhone": true, 
        "MSG-dndEmail": true, //called when all profiles sharing the same email must be unsubscribed
        "category-unsubscribe": {
        	"email": ["Newsletters", "Promotions"]
        },//called when profile needs to be unsubscribed from multiple subscription groups
        "category-resubscribe": {
        	"email": ["Football", "Movies"]
        },//called when profile needs to be subscribed from multiple subscription groups
        "DOB": "$D_1487576752",// Pass the date format value as Epoch in seconds.
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
> The property must have an existing value to increment or decrement a numerical user property. If the property does not exist or if the value is NULL, please pass the property key or add the value before using the incr or decr operator.

## Example Request

Here are the sample requests to the _Upload User Profiles_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"d":[{"objectId":"25b08803c1af4e00839f530264dac6f8","type":"profile","profileData":{"Name": "Jack Test"}}]}' "https://in1.api.clevertap.com/1/upload" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/upload")
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

response = requests.post('https://in1.api.clevertap.com/1/upload', headers=headers, data=data)
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
$response = Requests::post('https://in1.api.clevertap.com/1/upload', $headers, $data);
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
    url: 'https://in1.api.clevertap.com/1/upload',
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

req, err := http.NewRequest("POST", "https://in1.api.clevertap.com/1/upload", body)
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

The following is the example response for Upload User Profiles API:

```json
{
    "status": "success",
    "processed": 1,
    "unprocessed": []
}
```

## Delete User Property Values

You can delete user property values using the`$delete` operator as shown in the following example:

```json
{"d":
    [
        {
            "identity":"test_user",
            "type":"profile",
            "profileData":
                {
                    "testAttribute":{"$delete":"user_property_value"}
                }
        }
    ]
}
```

## Debugging

Requests with processing errors will be returned in the API call response, as shown in the example below.

```json
{
    "status":<success, partial, fail>, 
    "processed":<count>, 
    "unprocessed": [{"status":"fail", "code":<error code>, "error":<error msg>, "record":<record>}]
}
```

To test if your data will be submitted without errors, you can add the parameter `dryRun=1` to the URL. This will validate the input without submitting the data to CleverTap.

For more information on API error codes, refer to [this page](doc:api-errors).

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
