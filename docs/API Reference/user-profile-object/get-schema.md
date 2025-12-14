---
title: GET Schema
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
The GET schema APIs allow you to get the latest schema via API so that you have the latest changes.  You can GET the schema for events or user profiles.

# Get Events Schema API

## Overview

You can GET all the events from the schema. This API will get you all the system events and custom events along with their meta and properties.

### Base URL

https\://\[location].api.clevertap.com/getSchema?type=events

> 📘 Note
>
> Use the URL based on your location:
>
> * India - in1.api.clevertap.com
> * Singapore - sg1.api.clevertap.com
> * U.S - us1.api.clevertap.com

### HTTP Method

GET

### Headers

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

### Body Parameters

You do not need to provide body parameters. 

### Example Request

```curl
curl --location --request POST 'https://[location].api.clevertap.com/getSchema?type=events
--header 'X-CleverTap-Account-Id: RRP-XXT-CCWY' \
--header 'X-CleverTap-Passcode: 1c10a905d0fa4c168a8c9d818acf0a90'
```
```ruby
require "uri"
require "net/http"
url = URI("https://[location].api.clevertap.com/getschema?type=events/")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["Content-Type"] = "application/json"
request["X-CleverTap-Account-Id"] = "WWW-WWW-WWRZ"
request["X-CleverTap-Passcode"] = "ICW-QKE-YSGL"
response = http.request(request)
puts response.read_body
```
```python
import requests
url = "https://[location].api.clevertap.com/getschema?type=events"
headers = {
  'Content-Type': 'application/json',
  'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',
  'X-CleverTap-Passcode': 'ICW-QKE-YSGL'
}
response = requests.request("POST", url, headers=headers)
print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://[location].api.clevertap.com/getschema?type=events');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Content-Type' => 'application/json',
  'X-CleverTap-Account-Id' => 'WWW-WWW-WWRZ',
  'X-CleverTap-Passcode' => 'ICW-QKE-YSGL'
));

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
  'url': 'https://[location].api.clevertap.com/getschema?type=events',
  'headers': {
    'Content-Type': 'application/json',
    'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',
    'X-CleverTap-Passcode': 'ICW-QKE-YSGL'
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
  url := "https://[location].api.clevertap.com/getschema?type=events"
  method := "POST"
  
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
    "events": [
        {
            "name": "Product Viewed",
            "type": "Defined",
            "status": "Active",
            "createdOn": 1528867959,
            "drp": 1116,
            "eventCount": {
                "currentMonth": 6,
                "previousMonth": 16
            },
            "dataPoints": 266,
            "properties": [
                {
                    "name": "product_price",
                    "type": "Defined",
                    "status": "Active",
                    "required": false,
                    "dataType": "Mixed",
                    "dataTypeFallback": "Allow property",
                    "dataPoints": 0
                }
             
              ]
        }
      ]
}
```

# GET User Properties Schema API 

## Overview

You can GET the schema for all the user properties. 

### Base URL

https\://\[location].api.clevertap.com/getSchema?type=userproperty

> 📘 Note
>
> Use the URL based on your location:
>
> * India - in1.api.clevertap.com
> * Singapore - sg1.api.clevertap.com
> * U.S - us1.api.clevertap.com

### HTTP Method

POST

### Headers

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

### Body Parameters

You do not need to provide body parameters. 

### Example Request

```curl
curl --location --request GET 'https://[location]/getSchema?type=userProperties' \
--header 'X-CleverTap-Account-Id: XRR-EEE-RRP' \
--header 'X-CleverTap-Passcode: 1c10a905d0fa4c168a8c9d818acf0a90'
```
```ruby
require "uri"
require "net/http"
url = URI("https://[location]/getSchema?type=userProperties")
http = Net::HTTP.new(url.host, url.port);
request = Net::HTTP::Post.new(url)
request["Content-Type"] = "application/json"
request["X-CleverTap-Account-Id"] = "WWW-WWW-WWRZ"
request["X-CleverTap-Passcode"] = "ICW-QKE-YSGL"
response = http.request(request)
puts response.read_body
```
```python
import requests
url = "https://[location]/getSchema?type=userProperties"
headers = {
  'Content-Type': 'application/json',
  'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',
  'X-CleverTap-Passcode': 'ICW-QKE-YSGL'
}
response = requests.request("POST", url, headers=headers)
print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://[location]/getSchema?type=userProperties');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Content-Type' => 'application/json',
  'X-CleverTap-Account-Id' => 'WWW-WWW-WWRZ',
  'X-CleverTap-Passcode' => 'ICW-QKE-YSGL'
));

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
  'url': 'https://[location]/getSchema?type=userProperties',
  'headers': {
    'Content-Type': 'application/json',
    'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',
    'X-CleverTap-Passcode': 'ICW-QKE-YSGL'
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
  url := "https://[location]/getSchema?type=userProperties"
  method := "POST"
  
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

### Example Response

```json
{
    "status": "success",
    "userProperties": [
        {
            "name": "testkey3",
            "type": "Defined",
            "status": "Discarded",
            "dataType": "Mixed",
            "dataTypeFallback": "Allow property",
            "createdOn": 1528878023
        },
        {
            "name": "keywords4",
            "type": "Defined",
            "status": "Discarded",
            "dataType": "Mixed",
            "dataTypeFallback": "Allow property",
            "createdOn": 1528878023
        }
      ]
 }
```
