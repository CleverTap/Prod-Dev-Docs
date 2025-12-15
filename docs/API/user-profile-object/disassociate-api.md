---
title: Disassociate a Phone Number
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
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

<Callout icon="📘" theme="info">
  **Note**

  You can disassociate a phone number only when it is used as an identity.
</Callout>

# Overview

Passing the phone numbers requires a POST request with a JSON payload specifying the phone number. There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.

<Callout icon="📘" theme="info">
  **Note**

  The batch size denotes the maximum number of records that can be fetched in a single call. The response may vary.
</Callout>

## Base URL

Here is an example base URL from the account in the India region:  
[https://in1.api.clevertap.com/1/disassociate](https://in1.api.clevertap.com/1/disassociate)

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

Phone numbers are uploaded as a JSON payload. A payload is an object keyed with “d” whose value is an array describing the phone numbers.

For each phone number,  type is required. The phone number is CleverTap’s user identifier, which we will use to find the user and dissociate the profile.

| Parameter | Description                  | Type   | Example Value  |
| :-------- | :--------------------------- | :----- | :------------- |
| type      | Set to phone. Required.      | string | “phone”        |
| value     | The phone number of the user | string | +9119244142334 |

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

Here is an example cURL request to the _Disassociate_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl --location --request POST 'https://in1.api.clevertap.com/1/disassociate/' \
--header 'Content-Type: application/json' \
--header 'X-CleverTap-Account-Id: <YOUR_ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <YOUR_PASSCODE>' \
--data-raw '{"d": [{"type": "phone", "value": "+919213231415"},{"type": "phone","value": "+919213231416"}]}'
```
```ruby
require "uri"
require "net/http"
url = URI("https://in1.api.clevertap.com/1/disassociate/")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["Content-Type"] = "application/json"
request["X-CleverTap-Account-Id"] = "<YOUR_ACCOUNT_ID>"
request["X-CleverTap-Passcode"] = "<YOUR_PASSCODE>"
request.body = "{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}"
response = http.request(request)
puts response.read_body
```
```python
import requests
url = "https://in1.api.clevertap.com/1/disassociate/"
payload = "{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}"
headers = {
  'Content-Type': 'application/json',
  'X-CleverTap-Account-Id': '<YOUR_ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<YOUR_PASSCODE>'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://in1.api.clevertap.com/1/disassociate/');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Content-Type' => 'application/json',
  'X-CleverTap-Account-Id' => 'YOUR_ACCOUNT_ID',
  'X-CleverTap-Passcode' => 'YOUR_PASSCODE'
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
  'url': 'https://in1.api.clevertap.com/1/disassociate/',
  'headers': {
    'Content-Type': 'application/json',
    'X-CleverTap-Account-Id': '<YOUR_ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<YOUR_PASSCODE>'
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
  url := "https://in1.api.clevertap.com/1/disassociate/"
  method := "POST"
  payload := strings.NewReader("{\"d\": [{\"type\": \"phone\", \"value\": \"+919213231415\"},{\"type\": \"phone\",\"value\": \"+919213231416\"}]}")
  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)
  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("Content-Type", "application/json")
  req.Header.Add("X-CleverTap-Account-Id", "<YOUR_ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<YOUR_PASSCODE>")
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

<Callout icon="📘" theme="info">
  **Note**

  If a user is identified by the phone number, then the GUID can be used to identify the profile after disassociation. All the event and profile updates to the user are made using the GUID till a new phone number is passed to the user.

  [Handling subscriptions](https://docs.clevertap.com/docs/handling-unsubscribes) will automatically trigger the [subscribe](https://developer.clevertap.com/docs/subscribe-api) and [disassociate](https://developer.clevertap.com/docs/disassociate-api) APIs leading to Email-level subscription instead of User-level subscription.
</Callout>

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
