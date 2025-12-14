---
title: Disassociate API
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
  pages:
    - type: basic
      slug: upload-user-profiles-api
      title: Upload User Profiles API
---
The disassociate API enables you to disconnect a phone number from a user profile. There may be a case when users change their phone numbers and then the numbers are assigned to other users after the cooling-off period. 

If the user is identified by the phone number then the data of the previous user may be merged with the new user assigned to the phone number.  To avoid this scenario, run the disassociate API and disassociate the phone number assigned to the previous user.

For example, a user discontinues a phone number. The phone number is then assigned to a new user. You can pass the phone number in the Disassociate API. The phone number is disconnected from the previous user profile. 

If you want to assign this phone number to a new user, you can use the [Upload User Profiles API](doc:upload-user-profiles-api). A new user is created for the specified phone number. This process must be repeated every time the phone number changes hands.

> 📘 Note
>
> You can disassociate a phone number only when it is used as an identity.

# Overview

Passing the phone numbers requires a POST request with a JSON payload specifying the phone number. There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.

## Base URL

[https://location.api.clevertap.com/1/disassociate](https://location.api.clevertap.com/1/disassociate)

> 📘 Note
>
> Use the URL based on your location:
>
> * India - in1.api.clevertap.com
> * Singapore - sg1.api.clevertap.com
> * U.S - us1.api.clevertap.com

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
        Your CleverTap Account ID
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
        Your CleverTap Account Passcode
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

Phone numbers are uploaded as a JSON payload. A payload is an object keyed with “d” whose value is an array describing the phone numbers.

For each phone number,  type is required. The phone number is CleverTap’s user identifier, which we will use to find the user and dissociate the profile.

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
        Set to phone. Required.
      </td>

      <td>
        string
      </td>

      <td>
        “phone”
      </td>
    </tr>

    <tr>
      <td>
        value
      </td>

      <td>
        The phone number of the user
      </td>

      <td>
        string
      </td>

      <td>
        +9119244142334
      </td>
    </tr>
  </tbody>
</Table>

Here is an example JSON payload for iOS/Android.

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

```curl
curl --location --request POST 'https://location.api.clevertap.com/1/disassociate/' \
--header 'Content-Type: application/json' \
--header 'X-CleverTap-Account-Id: WWW-WWW-WWRZ' \
--header 'X-CleverTap-Passcode: ICW-QKE-YSGL' \
--data-raw '{"d": [{"type": "phone", "value": "+919213231415"},{"type": "phone","value": "+919213231416"}]}'
```
```ruby
require "uri"
require "net/http"
url = URI("https://location.api.clevertap.com/1/disassociate/")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["Content-Type"] = "application/json"
request["X-CleverTap-Account-Id"] = "WWW-WWW-WWRZ"
request["X-CleverTap-Passcode"] = "ICW-QKE-YSGL"
request.body = "{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}"
response = http.request(request)
puts response.read_body
```
```python
import requests
url = "https://location.api.clevertap.com/1/disassociate/"
payload = "{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}"
headers = {
  'Content-Type': 'application/json',
  'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',
  'X-CleverTap-Passcode': 'ICW-QKE-YSGL'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://location.api.clevertap.com/1/disassociate/');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Content-Type' => 'application/json',
  'X-CleverTap-Account-Id' => 'WWW-WWW-WWRZ',
  'X-CleverTap-Passcode' => 'ICW-QKE-YSGL'
));
$request->setBody('{"d": [{"type": "phone", "value": "+919213231415"},{"type": "phone","value": "+919213231416"}]}');
try {
  $response = $request->send();
  if ($response->getStatus() == 200) {
    echo $response->getBody();
  }
  else {
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  }
}
catch(HTTP_Request2_Exception $e) {
  echo 'Error: ' . $e->getMessage();
}
```
```javascript Node.js
var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://location.api.clevertap.com/1/disassociate/',
  'headers': {
    'Content-Type': 'application/json',
    'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',
    'X-CleverTap-Passcode': 'ICW-QKE-YSGL'
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
  url := "https://location.api.clevertap.com/1/disassociate/"
  method := "POST"
  payload := strings.NewReader("{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}")
  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)
  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("Content-Type", "application/json")
  req.Header.Add("X-CleverTap-Account-Id", "WWW-WWW-WWRZ")
  req.Header.Add("X-CleverTap-Passcode", "ICW-QKE-YSGL")
  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)
  fmt.Println(string(body))
}
```

## Example Response

```json
{
    "status": "success",
    "processed": 2,
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

> 📘 Note
>
> If a user is identified by the phone number, then the GUID can be used to identify the profile after disassociation. All the event and profile updates to the user are made using the GUID till a new phone number is passed to the user.
>
> [Handling subscriptions](https://docs.clevertap.com/docs/handling-unsubscribes) will automatically trigger the [subscribe](https://developer.clevertap.com/docs/subscribe-api) and [disassociate](https://developer.clevertap.com/docs/disassociate-api) APIs leading to Email-level subscription instead of User-level subscription.
