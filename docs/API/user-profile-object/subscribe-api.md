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

The Subscribe API provides the ability to set a phone number or email status as subscribed or unsubscribed. This is important so that you do not send a message to your users unless they have explicitly opted in. There may be cases when multiple users share a phone number or email; however, once a phone number or email is marked as unsubscribed (DND), communication stops for all users using the specified number or email.

For example, a user opts out of receiving messages. You can pass the phone number or email address and the status as `Unsubscribe` in the Subscribe API. The user phone number or email is unsubscribed for all users who share the same phone number or email. After the user opts to receive messages again, the phone number or email can be changed back to `Resubscribe`.

<Callout icon="📘" theme="info">
  ### **Note**

  To set only a specific user's state as DND, set the `MSG-sms` flag for SMS or `MSG-email` flag for email to false in the [Upload User Profiles API](https://developer.clevertap.com/docs/upload-user-profiles-api). If the other users sharing the number or email have not opted out, they continue to receive messages.
</Callout>

<Callout icon="📘" theme="info">
  ### **Note**

  From January 13, 2020 onwards, the Channel Unsubscribed Event also triggers the Subscribe API.
</Callout>

Passing subscription status for phone numbers or email requires a POST request with a JSON payload specifying the phone number or email and subscription status. There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.

<Callout icon="📘" theme="info">
  ### **Note**

  The batch size denotes the maximum number of records that can be submitted in a single call. The response may vary.
</Callout>

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/subscribe`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The following table lists the body parameters. All parameters are required for each record. Both `type` and `status` are case-insensitive, so values such as `"PHONE"` and `"unsubscribe"` are accepted.

| Parameter | Description                                                                                | Type   | Example Value                          |
| :-------- | :----------------------------------------------------------------------------------------- | :----- | :------------------------------------- |
| type      | The channel type. Accepted values: `phone`, `email`, `whatsapp`. Required.                 | string | `"phone"`, `"email"`, `"whatsapp"`     |
| value     | The phone number or email address of the user. Required.                                   | string | `"+919213231415"`, `"johndoe@abc.com"` |
| status    | The subscription status to set. Accepted values: `Unsubscribe` or `Resubscribe`. Required. | string | `"Unsubscribe"` or `"Resubscribe"`     |

The following is a sample JSON payload.

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

The following is a sample request to the _Subscribe_ API, showing the headers needed to authenticate the request.

```curl
curl -X POST "https://<region>.api.clevertap.com/1/subscribe" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json" \
-d '{ "d": [ { "type": "phone", "value": "+919213231415", "status": "Unsubscribe" }, { "type": "phone", "value": "+919213231416", "status": "Resubscribe" }, { "type": "whatsapp", "value": "+919213233436", "status": "Unsubscribe" }, { "type": "email", "value": "johndoe@abc.com", "status": "Unsubscribe" }, { "type": "email", "value": "janedoe@abc.com", "status": "Resubscribe" } ] }'
```
```ruby
require "uri"
require "net/http"

url = URI("https://<region>.api.clevertap.com/1/subscribe")
https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true
request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = "application/json"
request.body = "{ \"d\": [ { \"type\": \"phone\", \"value\": \"+919213231415\", \"status\": \"Unsubscribe\" }, { \"type\": \"phone\", \"value\": \"+919213231416\", \"status\": \"Resubscribe\" }, { \"type\": \"whatsapp\", \"value\": \"+919213233436\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"johndoe@abc.com\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"janedoe@abc.com\", \"status\": \"Resubscribe\" } ] }"
response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://<region>.api.clevertap.com/1/subscribe"
payload = "{ \"d\": [ { \"type\": \"phone\", \"value\": \"+919213231415\", \"status\": \"Unsubscribe\" }, { \"type\": \"phone\", \"value\": \"+919213231416\", \"status\": \"Resubscribe\" }, { \"type\": \"whatsapp\", \"value\": \"+919213233436\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"johndoe@abc.com\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"janedoe@abc.com\", \"status\": \"Resubscribe\" } ] }"
headers = {
  'X-CleverTap-Account-Id': 'ACCOUNT_ID',
  'X-CleverTap-Passcode': 'PASSCODE',
  'Content-Type': 'application/json'
}
response = requests.request("POST", url, headers=headers, data=payload)
print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://<region>.api.clevertap.com/1/subscribe');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
  'X-CleverTap-Passcode' => 'PASSCODE',
  'Content-Type' => 'application/json'
));
$request->setBody('{ "d": [ { "type": "phone", "value": "+919213231415", "status": "Unsubscribe" }, { "type": "phone", "value": "+919213231416", "status": "Resubscribe" }, { "type": "whatsapp", "value": "+919213233436", "status": "Unsubscribe" }, { "type": "email", "value": "johndoe@abc.com", "status": "Unsubscribe" }, { "type": "email", "value": "janedoe@abc.com", "status": "Resubscribe" } ] }');
try {
  $response = $request->send();
  if ($response->getStatus() == 200) {
    echo $response->getBody();
  } else {
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  }
} catch(HTTP_Request2_Exception $e) {
  echo 'Error: ' . $e->getMessage();
}
```
```javascript Node.js
var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://<region>.api.clevertap.com/1/subscribe',
  'headers': {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({"d":[{"type":"phone","value":"+919213231415","status":"Unsubscribe"},{"type":"phone","value":"+919213231416","status":"Resubscribe"},{"type":"whatsapp","value":"+919213233436","status":"Unsubscribe"},{"type":"email","value":"johndoe@abc.com","status":"Unsubscribe"},{"type":"email","value":"janedoe@abc.com","status":"Resubscribe"}]})
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
  url := "https://<region>.api.clevertap.com/1/subscribe"
  method := "POST"
  payload := strings.NewReader("{ \"d\": [ { \"type\": \"phone\", \"value\": \"+919213231415\", \"status\": \"Unsubscribe\" }, { \"type\": \"phone\", \"value\": \"+919213231416\", \"status\": \"Resubscribe\" }, { \"type\": \"whatsapp\", \"value\": \"+919213233436\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"johndoe@abc.com\", \"status\": \"Unsubscribe\" }, { \"type\": \"email\", \"value\": \"janedoe@abc.com\", \"status\": \"Resubscribe\" } ] }")
  client := &http.Client{}
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

The following is a sample response.

```json
{
    "status": "success",
    "processed": 5,
    "unprocessed": []
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

## SMS Delivery

The message delivery is decided by the phone-level and user-level subscription status. If a phone number has opted out but not the associated user profile, the SMS is not delivered.

The following table shows the reachability matrix for SMS.

| Phone-level subscription (MSG-dndPhone) | User-level SMS subscription (MSG-sms) | Reachability |
| :-------------------------------------- | :------------------------------------ | :----------- |
| Subscribed                              | Subscribed                            | Reachable    |
| Subscribed                              | Unsubscribed                          | Unreachable  |
| Unsubscribed                            | Subscribed                            | Unreachable  |
| Unsubscribed                            | Unsubscribed                          | Unreachable  |

## WhatsApp Delivery

The message delivery is decided by the phone-level and user-level subscription status. If a WhatsApp number has opted out but not the associated user profile, the WhatsApp message is not delivered.

The following table shows the reachability matrix for WhatsApp.

| Phone-level subscription (MSG-dndWhatsApp) | User-level WhatsApp subscription (MSG-whatsapp) | Reachability |
| :----------------------------------------- | :---------------------------------------------- | :----------- |
| Subscribed                                 | Subscribed                                      | Reachable    |
| Subscribed                                 | Unsubscribed                                    | Unreachable  |
| Unsubscribed                               | Subscribed                                      | Unreachable  |
| Unsubscribed                               | Unsubscribed                                    | Unreachable  |

## Email Delivery

The message delivery is decided by the email-level and user-level subscription status. If an email address has opted out but not the associated user profile, the message is not delivered.

[Handling subscriptions](https://docs.clevertap.com/docs/handling-unsubscribes) automatically triggers the [subscribe](https://developer.clevertap.com/docs/subscribe-api) and [disassociate](https://developer.clevertap.com/docs/disassociate-api) APIs, leading to email-level subscriptions instead of user-level subscriptions.

The following table shows the reachability matrix for Email.

| Email-level subscription (MSG-dndEmail) | User-level Email subscription (MSG-email) | Reachability |
| :-------------------------------------- | :---------------------------------------- | :----------- |
| Subscribed                              | Subscribed                                | Reachable    |
| Subscribed                              | Unsubscribed                              | Unreachable  |
| Unsubscribed                            | Subscribed                                | Unreachable  |
| Unsubscribed                            | Unsubscribed                              | Unreachable  |

## Error Codes

The following is the list of error codes returned by the API:

| HTTP Status | Error                                            | Description                                                                          | Example Error Response                                                                |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| 400         | `"Account credentials missing"`                  | Account ID header present but could not be parsed                                    | `{"status":"fail","error":"Account credentials missing","code":400}`                  |
| 400         | `"Malformed request"`                            | Request body is not valid JSON                                                       | `{"status":"fail","error":"Malformed request","code":400}`                            |
| 400         | `"Payload is mandatory"`                         | Request body is missing the `d` array entirely                                       | `{"status":"fail","error":"Payload is mandatory","code":400}`                         |
| 400         | `"Batch size exceeded"`                          | More than 1000 records in the `d` array                                              | `{"status":"fail","error":"Batch size exceeded","code":400}`                          |
| 400         | `"Failed to process request."`                   | Unexpected error during request setup                                                | `{"status":"fail","error":"Failed to process request.","code":400}`                   |
| 401         | `"Invalid credentials"`                          | Account ID not found or credentials are invalid                                      | `{"status":"fail","error":"Invalid credentials","code":401}`                          |
| 403         | `"API access has been temporally suspended"`     | Account API access has been suspended                                                | `{"status":"fail","error":"API access has been temporally suspended","code":403}`     |
| 429         | `"Too many concurrent requests"`                 | Per-account concurrent request limit exceeded. Retry with exponential backoff.       | `{"status":"fail","error":"Too many concurrent requests","code":429}`                 |
| 500         | `"Failed to process request"`                    | Unexpected critical server-side failure                                              | `{"status":"fail","error":"Failed to process request","code":500}`                    |
| 503         | `"12 digit account ID mandatory."`               | `X-CleverTap-Account-Id` header is missing from the request                          | `{"status":"fail","error":"12 digit account ID mandatory.","code":503}`               |
| 503         | `"System under maintenance. Please retry later"` | Server temporarily unavailable. Retry after at least 30 seconds.                     | `{"status":"fail","error":"System under maintenance. Please retry later","code":503}` |
| 503         | `"Please come back later"`                       | Global request limit exceeded or system warming up. Retry after at least 30 seconds. | `{"status":"fail","error":"Please come back later","code":503}`                       |

<br />
