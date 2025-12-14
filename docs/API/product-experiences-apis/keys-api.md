---
title: Keys API
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
These are the keys that you have defined in your app code. You can map these keys from the CleverTap API and control the configuration of your app using the [Product Config](https://docs.clevertap.com/docs/product-config#section-product-config) or [Feature Flags](https://docs.clevertap.com/docs/product-config#section-feature-flags) feature.\
. The Keys API enables you to create keys for [Product Config API](doc:product-config-api)] and [Feature Flag API](doc:feature-flag-api).  For example, you want to create a configuration that changes the speed of the ball in a game. For this, you have to map the keys from inside your app to the CleverTap keys. You can then use these keys in a product config to control the app configuration. 

> 📘 Note
>
> The name of the key defined here must match the name of the key defined in the app. This name cannot be changed later. You can add up to 500 keys.

# Create Keys

Creating Keys requires a POST request with a JSON payload specifying the key details. 

## Base URL

[https://api.clevertap.com/1/keys/create](https://api.clevertap.com/1/keys/create)

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


The keys and their mapping to the relevant segment are uploaded as a JSON payload. 

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
        name
      </td>

      <td>
        Name of the Key
      </td>

      <td>
        string
      </td>

      <td>
        ball\_speed
      </td>
    </tr>

    <tr>
      <td>
        description
      </td>

      <td>
        Short description of the Key
      </td>

      <td>
        string
      </td>

      <td>
        Vary the speed of the ball.
      </td>
    </tr>

    <tr>
      <td>
        dataType
      </td>

      <td>
        data type of the key
      </td>

      <td>
        string/integer/JSON/boolean/double/float
      </td>

      <td>
        integer
      </td>
    </tr>

    <tr>
      <td>
        defaultValue
      </td>

      <td>
        default value of the key
      </td>

      <td>
        same as the dataType mentioned
      </td>

      <td>
        10
      </td>
    </tr>
  </tbody>
</Table>



































































































































## Example Request


```curl
curl --location --request POST 'https://api.clevertap.com/1/keys/create' \
--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: PASSCODE' \
--header 'Content-Type: application/json' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"name":"ball_speed",
	"description": "Vary the speed of the ball",
	"defaultValue": 10,
	"dataType": "integer"
}'
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/keys/create")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = ["application/json", "text/plain"]
request.body = "{\n\t\"name\":\"ball_speed\",\n\t\"description\": \"Vary the speed of the ball.\",\n\t\"defaultValue\": 10,\n\t\"dataType\": \"integer\"\n}"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/keys/create"

payload = "{\n\t\"name\":\"ball_speed\",\n\t\"description\": \"Vary the speed of the ball.\",\n\t\"defaultValue\": 10,\n\t\"dataType\": \"integer\"\n}"
headers = {
  'X-CleverTap-Account-Id': 'ACCOUNT_ID',
  'X-CleverTap-Passcode': 'PASSCODE',
  'Content-Type': 'application/json',
  'Content-Type': 'text/plain'
}

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/keys/create');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
  'X-CleverTap-Passcode' => 'PASSCODE',
  'Content-Type' => 'application/json',
  'Content-Type' => 'text/plain'
));
$request->setBody('{\n	"name":"ball_speed",\n	"description": "Vary the speed of the ball.",\n	"defaultValue": 10,\n	"dataType": "integer"\n}');
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
  'url': 'https://api.clevertap.com/1/keys/create',
  'headers': {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': ['application/json', 'text/plain']
  },
  body: "{\n\t\"name\":\"ball_speed\",\n\t\"description\": \"Vary the speed of the ball.\",\n\t\"defaultValue\": 10,\n\t\"dataType\": \"integer\"\n}"

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

  url := "https://api.clevertap.com/1/keys/create"
  method := "POST"

  payload := strings.NewReader("{\n	\"name\":\"ball_speed\",\n	\"description\": \"Vary the speed of the ball.\",\n	\"defaultValue\": 10,\n	\"dataType\": \"integer\"\n}")

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("X-CleverTap-Account-Id", "ACCOUNT_ID")
  req.Header.Add("X-CleverTap-Passcode", "PASSCODE")
  req.Header.Add("Content-Type", "application/json")
  req.Header.Add("Content-Type", "text/plain")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
}
```


## Example Response










































# Update Keys

Update key details for the key mentioned in the "keyName" URL parameter. Specify JSON payload with the key details in the request Body. 

## Base URL

[https://api.clevertap.com/1/keys/update?keyName=](https://api.clevertap.com/1/keys/update?keyName=)&lt;keyName&gt;

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

The keys and are uploaded as a JSON payload.
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
        description
      </td>

      <td>
        Short description of the Key
      </td>

      <td>
        string
      </td>

      <td>
        Vary the speed of the ball.
      </td>
    </tr>

    <tr>
      <td>
        dataType
      </td>

      <td>
        data type of the key
      </td>

      <td>
        string/integer/JSON/boolean/double/float
      </td>

      <td>
        integer
      </td>
    </tr>

    <tr>
      <td>
        defaultValue
      </td>

      <td>
        default value of the key
      </td>

      <td>
        same as the dataType mentioned
      </td>

      <td>
        20
      </td>
    </tr>
  </tbody>
</Table>





















































































































## Example Request


```curl
curl --location --request POST 'https://api.clevertap.com/1/keys/update?keyName=<keyName>' \
--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: PASSCODE' \
--header 'Content-Type: application/json' \
--header 'Content-Type: text/plain' \
--data-raw '{
	"description": "Vary the speed of the ball. Updated",
	"defaultValue": 20,
	"dataType": "integer"
}'
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/keys/update?keyName=<keyName>")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = ["application/json", "text/plain"]
request.body = "{\n\t\"description\": \"Vary the speed of the ball. Updated\",\n\t\"defaultValue\": 20,\n\t\"dataType\": \"integer\"\n}"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/keys/update?keyName=<keyName>"

payload = "{\n\t\"description\": \"Vary the speed of the ball. Updated\",\n\t\"defaultValue\": 20,\n\t\"dataType\": \"integer\"\n}"
headers = {
  'X-CleverTap-Account-Id': 'ACCOUNT_ID',
  'X-CleverTap-Passcode': 'PASSCODE',
  'Content-Type': 'application/json',
  'Content-Type': 'text/plain'
}

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/keys/update?keyName=<keyName>');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
  'X-CleverTap-Passcode' => 'PASSCODE',
  'Content-Type' => 'application/json',
  'Content-Type' => 'text/plain'
));
$request->setBody('{\n	"description": "Vary the speed of the ball. Updated",\n	"defaultValue": 20,\n	"dataType": "integer"\n}');
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
  'url': 'https://api.clevertap.com/1/keys/update?keyName=<keyName>',
  'headers': {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': ['application/json', 'text/plain']
  },
  body: "{\n\t\"description\": \"Vary the speed of the ball. Updated\",\n\t\"defaultValue\": 20,\n\t\"dataType\": \"integer\"\n}"

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

  url := "https://api.clevertap.com/1/keys/update?keyName=%3CkeyName%3E"
  method := "POST"

  payload := strings.NewReader("{\n	\"description\": \"Vary the speed of the ball. Updated\",\n	\"defaultValue\": 20,\n	\"dataType\": \"integer\"\n}")

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("X-CleverTap-Account-Id", "ACCOUNT_ID")
  req.Header.Add("X-CleverTap-Passcode", "PASSCODE")
  req.Header.Add("Content-Type", "application/json")
  req.Header.Add("Content-Type", "text/plain")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
}
```


## Example Response










































# Get Keys

You can get details of all the keys using this API. Add the key name in the  URL parameter to get details of a specific key.

## Base URL

[https://api.clevertap.com/1/keys/get?keyName=](https://api.clevertap.com/1/keys/get?keyName=)&lt;optional keyName&gt;

## HTTP Method

GET

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

## Example Request
```curl
curl --location --request GET 'https://api.clevertap.com/1/keys/get' \
--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: PASSCODE' \
--header 'Content-Type: application/json' \
--data-raw ''
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/keys/get")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Get.new(url)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = "application/json"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/keys/get"

payload  = {}
headers = {
  'X-CleverTap-Account-Id': 'ACCOUNT_ID',
  'X-CleverTap-Passcode': 'PASSCODE',
  'Content-Type': 'application/json'
}

response = requests.request("GET", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/keys/get');
$request->setMethod(HTTP_Request2::METHOD_GET);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
  'X-CleverTap-Passcode' => 'PASSCODE',
  'Content-Type' => 'application/json'
));
$request->setBody('');
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
  'method': 'GET',
  'url': 'https://api.clevertap.com/1/keys/get',
  'headers': {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
  }
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

  url := "https://api.clevertap.com/1/keys/get"
  method := "GET"

  payload := strings.NewReader("")

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












































































































































































































































# Archive Keys

You can archive a key using this API by specifying the key name in the URL. 

## Base URL

[https://api.clevertap.com/1/keys/archive?keyName=](https://api.clevertap.com/1/keys/archive?keyName=)&lt;keyName&gt; 

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

## Example Request
```curl
curl --location --request POST 'https://api.clevertap.com/1/keys/archive?keyName=<keyName>' \
--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: PASSCODE' \
--header 'Content-Type: application/json' \
--data-raw ''
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/keys/archive?keyName=<keyName>")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = "application/json"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/keys/archive?keyName=<keyName>"

payload  = {}
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
$request->setUrl('https://api.clevertap.com/1/keys/archive?keyName=<keyName>');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
  'X-CleverTap-Passcode' => 'PASSCODE',
  'Content-Type' => 'application/json'
));
$request->setBody('');
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
  'url': 'https://api.clevertap.com/1/keys/archive?keyName=<keyName>',
  'headers': {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
  }
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

  url := "https://api.clevertap.com/1/keys/archive?keyName=<keyName>"
  method := "POST"

  payload := strings.NewReader("")

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
































































































































# Activate Keys

You can activate any key via this API by specifying the key name in the URL.

## Base URL

[https://api.clevertap.com/1/keys/activate?keyName=](https://api.clevertap.com/1/keys/activate?keyName=)&lt;keyName&gt;

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

## Example Request
```curl
curl --location --request POST 'https://api.clevertap.com/1/keys/activate?keyName=<keyName>' \
--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: PASSCODE' \
--header 'Content-Type: application/json' \
--data-raw ''
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/keys/activate?keyName=<keyName>")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = "application/json"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/keys/activate?keyName=<keyName>"

payload  = {}
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
$request->setUrl('https://api.clevertap.com/1/keys/activate?keyName=<keyName>');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
  'X-CleverTap-Passcode' => 'PASSCODE',
  'Content-Type' => 'application/json'
));
$request->setBody('');
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
  'url': 'https://api.clevertap.com/1/keys/activate?keyName=<keyName>',
  'headers': {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
  }
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

  url := "https://api.clevertap.com/1/keys/activate?keyName=<keyName>"
  method := "POST"

  payload := strings.NewReader("")

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
































































































































> 📘 Note
>
> There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.