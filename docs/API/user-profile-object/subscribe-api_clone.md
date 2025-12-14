---
title: Subscribe API_Clone
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
# Setting Subscription status for all users sharing the phone number or email

The Subscribe API enables you to set the status for a phone number or email as subscribed or unsubscribed. This is important so that you do not send a message to your users accidentally unless they have explicitly opted for it. There may be cases when a phone number or email is being shared by multiple users. However, once a phone number or email is marked as unsubscribed (DND), communication is stopped for all users using the specified number or email. 

For example, a user opts out of receiving messages. You can pass the phone number or email address and the status as `Unsubscribe` in the Subscribe API.  The user phone number or email is unsubscribed for all users who share the same phone number or email.

Once user opts to receive messages again, the phone number or email can be changed back to `Resubscribe`.

> 📘 Note
>
> To set only a specific user state as DND, set the MSG-sms flag for SMS or MSG-email flag for an email to false in the  [Upload User Profiles](https://developer.clevertap.com/docs/upload-user-profiles-api).  If the other users sharing the number or email have not opted out, they will continue to receive messages.

# Overview

Passing subscription status for phone numbers or email requires a POST request with a JSON payload specifying the phone number or email and subscription status. There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.

## Base URL

[https://api.clevertap.com/1/subscribe](https://api.clevertap.com/1/subscribe)

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

Add the type as email or phone number\
Add the value of the email or phone number\
Set the status to `Unsubscribe` or `Resubscribe`.

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
        Set to phone or email. Required.
      </td>

      <td>
        string
      </td>

      <td>
        “phone”\
        "email"
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
        "+919213231415"\
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

Here is an example JSON payload for iOS/Android.

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

## Example Request  \<\<@Sambit to provide>>

```curl
curl --location --request POST 'https://api.clevertap.com/1/subscribe/'
--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \ 
--header 'X-CleverTap-Passcode: PASSCODE' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{ "d": [ { "type": "phone", "value": "+919213231415", "status": "Unsubscribe" }, { "type": "phone", "value": "+919213231416", "status": "Resubscribe" }, { "type": "email", "value": "johndoe@abc.com", "status": "Unsubscribe" }, { "type": "email", "value": "janedoe@abc.com", "status": "Resubscribe" } ] }'
```
```ruby
require "uri"
require "net/http"
url = URI("https://api.clevertap.com/1/subscribe/")
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
url = "https://api.clevertap.com/1/subscribe/"
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
$request->setUrl('https://api.clevertap.com/1/subscribe/');
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
  'url': 'https://api.clevertap.com/1/subscribe/',
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
  url := "https://api.clevertap.com/1/subscribe/"
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

Requests with processing errors will be returned in the API call response as shown in the example below.

```json
{
    "status":<success, partial, fail>, 
    "processed":<count>, 
    "unprocessed": [{"status":"fail", "code":<error code>, "error":<error msg>, "record":<record>}]
}
```

To test if your data will be submitted without errors, you can add the parameter dryRun=1 to the URL. This will validate the input without submitting the data to CleverTap.

## SMS Delivery

The message delivery is decided by the phone-level and user-level subscription status. If a phone number has opted out but not the associated user profile, then the SMS is not delivered.  

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Phone-level subscription (MSG-dndPhone)
      </th>

      <th>
        User-level SMS subscription (MSG-sms)
      </th>

      <th>
        Reachability
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Subscribed
      </td>

      <td>
        Subscribed
      </td>

      <td>
        Reachable
      </td>
    </tr>

    <tr>
      <td>
        Subscribed
      </td>

      <td>
        Unsubscribed
      </td>

      <td>
        Unreachable
      </td>
    </tr>

    <tr>
      <td>
        Unsubscribed
      </td>

      <td>
        Subscribed
      </td>

      <td>
        Unreachable
      </td>
    </tr>

    <tr>
      <td>
        Unsubscribed
      </td>

      <td>
        Unsubscribed
      </td>

      <td>
        Unreachable
      </td>
    </tr>
  </tbody>
</Table>

## Email Delivery

The message delivery is decided by the email-level and user-level subscription status. If an email address has opted out but not the associated user profile, then the message is not delivered.  

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Email-level subscription (MSG-dndEmail)
      </th>

      <th>
        User-level Email subscription (MSG-email)
      </th>

      <th>
        Reachability
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Subscribed
      </td>

      <td>
        Unsubscribed
      </td>

      <td>
        Reachable
      </td>
    </tr>

    <tr>
      <td>
        Subscribed
      </td>

      <td>
        Unsubscribed
      </td>

      <td>
        Unreachable
      </td>
    </tr>

    <tr>
      <td>
        Unsubscribed
      </td>

      <td>
        Subscribed
      </td>

      <td>
        Unreachable
      </td>
    </tr>

    <tr>
      <td>
        Unsubscribed
      </td>

      <td>
        Unsubscribed
      </td>

      <td>
        Unreachable
      </td>
    </tr>
  </tbody>
</Table>
