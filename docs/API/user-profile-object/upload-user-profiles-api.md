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
## Overview

Use this API to create or update user profiles in CleverTap. With this API, you can update user profile information, such as gender, phone number, and customer type.

Uploading user profiles to CleverTap requires a POST request with a JSON payload specifying the user profile information. A maximum of 1000 user profile records can be sent per API call.

<Callout icon="📘" theme="info">
  ### **Concurrent Requests**

  Concurrent requests to the _Upload User Profiles_ API are limited to 3 per account by default. Requests that exceed the limit will return a 429 HTTP response code.
</Callout>

For more information on request limits, refer to [API Request Limit](doc:api-request-limit).

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/upload`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## Account-level Passcode for APIs

You can have multiple account-level passcodes rather than a single account passcode used across different partners. This offers better security when using CleverTap APIs.

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

User profile attribute keys are limited to 1024 characters, and their values are limited to 1024 characters. The maximum number of custom user profile keys is 256.

For each user profile, a user identifier is required. CleverTap uses this key to find the user whose profile needs to be updated. Set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId. A new user profile is created if the identity is not found.

The following table lists the body parameters.

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>Parameter</th>
      <th>Description</th>
      <th>Type</th>
      <th>Example Value</th>
      <th>Required/Optional</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>d</td>
      <td>Array of user profile records to upload. Maximum 1000 records per call.</td>
      <td>array</td>
      <td>—</td>
      <td>Required</td>
    </tr>
    <tr>
      <td>type</td>
      <td>Set to profile.</td>
      <td>string</td>
      <td>"profile"</td>
      <td>Required</td>
    </tr>
    <tr>
      <td>profileData</td>
      <td>Object containing the profile properties to set or update. Accepted format for profileData["Phone"]: +[country code][phone number]. Accepted values for profileData["Gender"]: "M" or "F".</td>
      <td>object</td>
      <td>"profileData": {"Name": "Jack Montana", "Email": "jack@gmail.com", "Phone": "+14155551234", "Gender": "M"}</td>
      <td>Required</td>
    </tr>
    <tr>
      <td>identity</td>
      <td>Identity to recognize a unique user. It can be the user's email address, a phone number, or any other identifier you use to tag your users. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.</td>
      <td>string</td>
      <td>"jack@gmail.com"</td>
      <td>Optional</td>
    </tr>
    <tr>
      <td>FBID</td>
      <td>Identify a unique user by Facebook ID. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.</td>
      <td>string</td>
      <td>"34322423"</td>
      <td>Optional</td>
    </tr>
    <tr>
      <td>GPID</td>
      <td>Identify a unique user by Google Plus ID. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.</td>
      <td>string</td>
      <td>"34322424"</td>
      <td>Optional</td>
    </tr>
    <tr>
      <td>objectId</td>
      <td>Identify a unique user by CleverTap Global Object ID. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.</td>
      <td>string</td>
      <td>"25b08803c1af4e00839f530264dac6f8"</td>
      <td>Optional</td>
    </tr>
  </tbody>
</Table>

## Example Payloads

The following examples show the supported operations. Each operation must be sent as a separate record. The examples below are mutually exclusive and cannot be combined in a single `profileData` object.

**Create or update a profile**

The following is a sample payload for creating or updating a user profile.

```json
{
  "d": [
    {
      "identity": "1189549",
      "type": "profile",
      "profileData": {
        "Name": "Jack Montana",
        "Email": "jack@gmail.com",
        "Phone": "+14155551234",
        "Gender": "M",
        "DOB": "$D_1487576752",
        "Customer Type": "Platinum"
      }
    }
  ]
}
```

**Delete a profile property**

The following is a sample payload for deleting a profile property value using the `$delete` operator.

```json
{
  "d": [
    {
      "identity": "1189549",
      "type": "profile",
      "profileData": {
        "Name": {"$delete": 1},
        "Gender": {"$delete": 1}
      }
    }
  ]
}
```

<Callout icon="⚠️" theme="warning">
  ###

  **$delete scope**

  The `$delete` operator is supported only for `Name` and `Gender` via this API. Using `$delete` on other properties via the Upload User Profiles API may fail silently or be rejected.
</Callout>

<mark>FLAG: Confirm with engineering whether $delete on arbitrary custom properties is supported via this API. Code review (BaseUploadServlet.java) confirmed $delete validation only for Name and Gender. Until confirmed, the doc restricts this to Name and Gender only.</mark>

**Set messaging and subscription preferences**

The following is a sample payload for setting messaging preferences and DND flags.

```json
{
  "d": [
    {
      "identity": "1189549",
      "type": "profile",
      "profileData": {
        "MSG-sms": false,
        "MSG-email": true,
        "MSG-whatsapp": true,
        "MSG-dndWhatsApp": true,
        "MSG-dndPhone": true,
        "MSG-dndEmail": true
      }
    }
  ]
}
```

<Callout icon="⚠️" theme="warning">
  ### **MSG-email vs MSG-dndEmail**

  These two flags have different scopes:

  - `MSG-email: false` opts out this specific profile from the email channel.
  - `MSG-dndEmail: true` sets a platform-wide DND and unsubscribes all profiles sharing the same email address.

  The same distinction applies to `MSG-dndPhone` and `MSG-dndWhatsApp`. Use DND flags with care, as their effect extends beyond the individual profile.
</Callout>

**Unsubscribe from Subscription Groups**

The following is a sample payload for unsubscribing a profile from specific subscription groups.

```json
{
  "d": [
    {
      "identity": "1189549",
      "type": "profile",
      "profileData": {
        "category-unsubscribe": {
          "email": ["Newsletters", "Promotions"]
        }
      }
    }
  ]
}
```

**Add or Remove Multi-Value Properties**

The following are sample payloads for adding and removing values from a multi-value property. Send each operation as a separate record.

```json
{
  "d": [
    {
      "identity": "1189549",
      "type": "profile",
      "profileData": {
        "Custom Multi Value": {"$add": ["shoes", "bags"]}
      }
    }
  ]
}
```

```json
{
  "d": [
    {
      "identity": "1189549",
      "type": "profile",
      "profileData": {
        "Custom Multi Value": {"$remove": ["shoes", "bags"]}
      }
    }
  ]
}
```

**Increment or Decrement Numeric Properties**

The following is a sample payload for incrementing or decrementing numeric profile properties.

```json
{
  "d": [
    {
      "identity": "1189549",
      "type": "profile",
      "profileData": {
        "My Number 1": {"$incr": 10},
        "My Number 2": {"$decr": 30}
      }
    }
  ]
}
```

<Callout icon="📘" theme="info">
  ### **Increment and Decrement**

  The property must have an existing value to increment or decrement a numerical user property. If the property does not exist or if the value is null, pass the property key and add a value before using the `$incr` or `$decr` operator.
</Callout>

## Example Request

The following is a sample request to the _Upload User Profiles_ API, showing the headers needed to authenticate the request.

```curl
curl -X POST -d '{"d":[{"objectId":"25b08803c1af4e00839f530264dac6f8","type":"profile","profileData":{"Name": "Jack Test"}}]}' "https://<region>.api.clevertap.com/1/upload" \
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
$data = '{"d":[{"objectId":"25b08803c1af4e00839f530264dac6f8","type":"profile","profileData":{"Name": "Jack Test"}}]}';
$response = Requests::post('https://<region>.api.clevertap.com/1/upload', $headers, $data);
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

## Delete User Property Values

You can delete user property values using the `$delete` operator. The `$delete` operator is supported only for `Name` and `Gender`. The value must be an integer `1`.

The following is a sample payload for deleting a property value.

```json
{
  "d": [
    {
      "identity": "test_user",
      "type": "profile",
      "profileData": {
        "Name": {"$delete": 1}
      }
    }
  ]
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

For more information on API error codes, refer to [this page](doc:api-errors). To understand common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

# Error Codes

The following table lists the error codes returned by this API:

| HTTP Status | Error                                                         | Description                                  | Example Error Response                                                                |
| ----------- | ------------------------------------------------------------- | -------------------------------------------- | ------------------------------------------------------------------------------------- |
| 400         | `"Payload is mandatory"`                                      | Empty or missing request body                | `{"status":"fail","error":"Payload is mandatory","code":400}`                         |
| 400         | `"Malformed request"`                                         | JSON cannot be parsed                        | `{"status":"fail","error":"Malformed request","code":400}`                            |
| 400         | `"Maximum 1000 records allowed in one request."`              | Batch size exceeds 1000                      | `{"status":"fail","error":"Maximum 1000 records allowed in one request.","code":400}` |
| 401         | `"Invalid credentials"`                                       | Wrong passcode                               | `{"status":"fail","error":"Invalid credentials","code":401}`                          |
| 503         | `"12 digit account ID mandatory."`                            | Account ID header absent                     | `{"status":"fail","error":"12 digit account ID mandatory.","code":503}`               |
| 503         | `"Please come back later"`                                    | Account not in cache                         | `{"status":"fail","error":"Please come back later","code":503}`                       |
| 200         | `"'identity' or 'fbId' or 'gpId' or 'objectId' is necessary"` | No valid user identifier provided            | `{"status":"fail","code":523,"error":"...","record":{...}}`                           |
| 200         | `"Data is not of type Event, Profile or Token."`              | `type` field missing or unrecognized         | `{"status":"fail","code":524,"error":"...","record":{...}}`                           |
| 200         | `"Timestamp must be in Unix Epoch format."`                   | Invalid or missing `ts` field                | `{"status":"fail","code":525,"error":"...","record":{...}}`                           |
| 200         | `"Record is too old, please use historic batch upload API."`  | Timestamp too far in the past                | `{"status":"fail","code":527,"error":"...","record":{...}}`                           |
| 200         | `"Email is not valid"`                                        | Email missing `@` or malformed               | `{"status":"fail","code":515,"error":"...","record":{...}}`                           |
| 200         | `"Phone number is not valid"`                                 | Phone doesn't start with `+` or is too short | `{"status":"fail","code":516,"error":"...","record":{...}}`                           |
| 200         | `"Employed needs to be Y/N"`                                  | Invalid value for `Employed` field           | `{"status":"fail","code":517,"error":"...","record":{...}}`                           |
| 200         | `"Education needs to be School/College/Graduate."`            | Invalid value for `Education` field          | `{"status":"fail","code":518,"error":"...","record":{...}}`                           |
| 200         | `"Married needs to be Y/N"`                                   | Invalid value for `Married` field            | `{"status":"fail","code":519,"error":"...","record":{...}}`                           |
| 200         | `"Age is invalid"`                                            | Age \< 0, > 120, or non-numeric              | `{"status":"fail","code":520,"error":"...","record":{...}}`                           |
| 200         | `"Invalid Name update operation"`                             | Invalid operator used on `Name` field        | `{"status":"fail","code":536,"error":"...","record":{...}}`                           |
| 200         | `"Invalid gender update operation"`                           | Invalid operator used on `Gender` field      | `{"status":"fail","code":537,"error":"...","record":{...}}`                           |
| 200         | `"509 - Event name is mandatory"`                             | `evtName` missing on event-type record       | `{"status":"fail","code":509,"error":"...","record":{...}}`                           |
| 200         | `"513 - Trying to raise a restricted system event"`           | Attempt to upload a reserved event           | `{"status":"fail","code":513,"error":"...","record":{...}}`                           |
| 200         | `"512 - Only Items can have a nested structure..."`           | Nested object used in non-items field        | `{"status":"fail","code":512,"error":"...","record":{...}}`                           |
| 200         | `"522 - Cannot have more than N properties."`                 | Event exceeds property limit (default: 100)  | `{"status":"fail","code":522,"error":"...","record":{...}}`                           |

<br />
