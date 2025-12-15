---
title: Subscribe
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
      slug: disassociate-api
      title: Disassociate API
---
# Overview

The Subscribe API provides the ability to set a phone number or email status as subscribed or unsubscribed. This is important so that you do not send a message to your users accidentally unless they have explicitly opted for it. There may be cases when multiple users share a phone number or email; however, once a phone number or email is marked as unsubscribed (DND), communication is stopped for all users using the specified number or email.

For example, a user opts out of receiving messages. You can pass the phone number or email address and the status as `Unsubscribe` in the Subscribe API.  The user phone number or email is unsubscribed for all users who share the same phone number or email. After the user opts to receive messages again, the phone number or email can be changed back to `Resubscribe`.

<Callout icon="📘" theme="info">
  **Note**

  To set only a specific user state as DND, set the MSG-sms flag for SMS or MSG-email flag for an email to false in the  [Upload User Profiles](https://developer.clevertap.com/docs/upload-user-profiles-api).  If the other users sharing the number or email have not opted out, they will continue to receive messages.
</Callout>

<Callout icon="📘" theme="info">
  **Note**

  From January 13, 2020 onwards, Channel Unsubscribed Event also triggers the Subscribe API.
</Callout>

Passing subscription status for phone numbers or email requires a POST request with a JSON payload specifying the phone number or email and subscription status. There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.

<Callout icon="📘" theme="info">
  **Note**

  The batch size denotes the maximum number of records that can be fetched in a single call. The response may vary.
</Callout>

## Base URL

Here is an example base URL from the account in the India region:  
[https://in1.api.clevertap.com/1/subscribe](https://in1.api.clevertap.com/1/subscribe)

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

* Add the type as email or phone number
* Add the value of the email or phone number
* Set the status to `Unsubscribe` or `Resubscribe`.

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
        Set to phone, WhatsApp, or email. Required.
      </td>

      <td>
        string
      </td>

      <td>
        “phone”  
        "email"  
        "WhatsApp"
      </td>
    </tr>

    <tr>
      <td>
        value
      </td>

      <td>
        The phone number or email address of the user
      </td>

      <td>
        string
      </td>

      <td>
        "+919213231415"  
        "[johndoe@abc.com](mailto:johndoe@abc.com)"
      </td>
    </tr>

    <tr>
      <td>
        status
      </td>

      <td>
        Pass the status of the number or email. The valid values can be `Unsubscribe` or `Resubscribe`.
      </td>

      <td>
        string
      </td>

      <td>
        `Unsubscribe` or `Resubscribe`.
      </td>
    </tr>
  </tbody>
</Table>

The following is an example JSON payload for iOS/Android.

```json
{
	"d": [
		{	
			"type": "phone",
			"value": "+919213231415",
			"status": "Unsubscribe"
		},  
		{
			"type": "phone",
			"value": "+919213231416",
			"status": "Resubscribe"
		},
    {	
			"type": "whatsapp",
			"value": "+919213233436",
			"status": "Unsubscribe"
		},
        {	
			"type": "whatsapp",
			"value": "+919213233437",
			"status": "Resubscribe"
		},
    {
			"type": "email",
			"value": "johndoe@abc.com",
			"status": "Unsubscribe"
		},
		{
			"type": "email",
			"value": "janedoe@abc.com",
			"status": "Resubscribe"
		}
	]
}
```

## Example Request

Here is an example cURL request to the _Subscribe_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl --location --request POST 'https://in1.api.clevertap.com/1/subscribe'
--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \ 
--header 'X-CleverTap-Passcode: PASSCODE' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{ "d": [ { "type": "phone", "value": "+919213231415", "status": "Unsubscribe" }, { "type": "phone", "value": "+919213231416", "status": "Resubscribe" }, { "type": "email", "value": "johndoe@abc.com", "status": "Unsubscribe" }, { "type": "email", "value": "janedoe@abc.com", "status": "Resubscribe" } ] }'
```
```ruby
require "uri"
require "net/http"
url = URI("https://in1.api.clevertap.com/1/subscribe")
https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true
request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = "application/json"
request.body = "{ \"d\": [ { \"type\": \"phone\", \"value\": \"+919213231415\", \"status\": \"Unsubscribe\" }, { \"type\": \"phone\", \"value\": \"+919213231416\", \"status\": \"Resubscribe\" }, { \"type\": \"email\", \"value\": \"johndoe@abc.com\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"janedoe@abc.com\", \"status\": \"Resubscribe\" } ] }"
response = https.request(request)
puts response.read_body
```
```python
import requests
url = "https://in1.api.clevertap.com/1/subscribe"
payload = "{ \"d\": [ { \"type\": \"phone\", \"value\": \"+919213231415\", \"status\": \"Unsubscribe\" }, { \"type\": \"phone\", \"value\": \"+919213231416\", \"status\": \"Resubscribe\" }, { \"type\": \"email\", \"value\": \"johndoe@abc.com\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"janedoe@abc.com\", \"status\": \"Resubscribe\" } ] }"
headers = {
  'X-CleverTap-Account-Id': 'ACCOUNT_ID',
  'X-CleverTap-Passcode': 'PASSCODE',
  'Content-Type': 'application/json'
}
response = requests.request("POST", url, headers=headers, data = payload)
print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://in1.api.clevertap.com/1/subscribe');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
  'X-CleverTap-Passcode' => 'PASSCODE',
  'Content-Type' => 'application/json'
));
$request->setBody('{ "d": [ { "type": "phone", "value": "+919213231415", "status": "Unsubscribe" }, { "type": "phone", "value": "+919213231416", "status": "Resubscribe" }, { "type": "email", "value": "johndoe@abc.com", "status": "Unsubscribe" }, { "type": "email", "value": "janedoe@abc.com", "status": "Resubscribe" } ] }');
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
  'url': 'https://in1.api.clevertap.com/1/subscribe',
  'headers': {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({"d":[{"type":"phone","value":"+919213231415","status":"Unsubscribe"},{"type":"phone","value":"+919213231416","status":"Resubscribe"},{"type":"email","value":"johndoe@abc.com","status":"Unsubscribe"},{"type":"email","value":"janedoe@abc.com","status":"Resubscribe"}]})
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
  url := "https://in1.api.clevertap.com/1/subscribe"
  method := "POST"
  payload := strings.NewReader("{ \"d\": [ { \"type\": \"phone\", \"value\": \"+919213231415\", \"status\": \"Unsubscribe\" }, { \"type\": \"phone\", \"value\": \"+919213231416\", \"status\": \"Resubscribe\" }, { \"type\": \"email\", \"value\": \"johndoe@abc.com\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"janedoe@abc.com\", \"status\": \"Resubscribe\" } ] }")
  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)
  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("X-CleverTap-Account-Id", "ACCOUNT_ID")
  req.Header.Add("X-CleverTap-Passcode", "PASSCODE")
  req.Header.Add("Content-Type", "application/json")
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
    "processed": 4,
    "unprocessed": []
}
```

## Debugging

Requests with processing errors are returned in the API call response, as shown in the example below.

```json
{
    "status":<success, partial, fail>, 
    "processed":<count>, 
    "unprocessed": [{"status":"fail", "code":<error code>, "error":<error msg>, "record":<record>}]
}
```

To test if your data will be submitted without errors, you can add the parameter dryRun=1 to the URL. This validates the input without submitting the data to CleverTap.

## SMS Delivery

The message delivery is decided by the phone-level and user-level subscription status. If a phone number has opted out but not the associated user profile, then the SMS is not delivered.

| Phone-level subscription (MSG-dndPhone) | User-level SMS subscription (MSG-sms) | Reachability |
| :-------------------------------------- | :------------------------------------ | :----------- |
| Subscribed                              | Subscribed                            | Reachable    |
| Subscribed                              | Unsubscribed                          | Unreachable  |
| Unsubscribed                            | Subscribed                            | Unreachable  |
| Unsubscribed                            | Unsubscribed                          | Unreachable  |

## WhatsApp Delivery

The message delivery is decided by the phone-level and user-level subscription status. If a WhatsApp number has opted out but not the associated user profile, then the WhatsApp message is not delivered.

| Phone-level subscription (MSG-dndWhatsapp) | User-level SMS subscription (MSG-Whatsapp) | Reachability |
| :----------------------------------------- | :----------------------------------------- | :----------- |
| Subscribed                                 | Subscribed                                 | Reachable    |
| Subscribed                                 | Unsubscribed                               | Unreachable  |
| Unsubscribed                               | Subscribed                                 | Unreachable  |
| Unsubscribed                               | Unsubscribed                               | Unreachable  |

## Email Delivery

The message delivery is decided by the email-level and user-level subscription status. If an email address has opted out but not the associated user profile, then the message is not delivered.

[Handling subscriptions](https://docs.clevertap.com/docs/handling-unsubscribes) will automatically trigger the [subscribe](https://developer.clevertap.com/docs/subscribe-api) and [disassociate](https://developer.clevertap.com/docs/disassociate-api) APIs leading to Email-level subscriptions instead of User-level subscriptions.

| Email-level subscription (MSG-dndEmail) | User-level Email subscription (MSG-email) | Reachability |
| :-------------------------------------- | :---------------------------------------- | :----------- |
| Subscribed                              | Subscribed                                | Reachable    |
| Subscribed                              | Unsubscribed                              | Unreachable  |
| Unsubscribed                            | Subscribed                                | Unreachable  |
| Unsubscribed                            | Unsubscribed                              | Unreachable  |
