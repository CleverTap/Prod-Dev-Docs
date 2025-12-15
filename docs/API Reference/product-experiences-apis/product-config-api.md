---
title: Product Config API
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
The Product Config API enables you to create Product Configs. For example, you want to change the behavior of your baseball gaming app. You want to vary the ball speed and velocity based on custom properties such as at the skill level of the player in the game. 

# Create Product Config

Creating Product Config requires a POST request with a JSON payload specifying the key name and mapping.  There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.

## Base URL

[https://api.clevertap.com/1/productconfig/create](https://api.clevertap.com/1/productconfig/create)

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
        "X-CleverTap-Account-Id: ACCOUNT_ID"
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

The Product Config and its keys with the relevant segment are uploaded as a JSON payload. 


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
        Name
      </td>

      <td>
        Name of the Product config
      </td>

      <td>
        string
      </td>

      <td>
        gameConfig
      </td>
    </tr>

    <tr>
      <td>
        Description
      </td>

      <td>
        Description of the Product Config
      </td>

      <td>
        string
      </td>

      <td>
        Change the ball speed and ball velocity based on custom profile property.
      </td>
    </tr>

    <tr>
      <td>
        keys
      </td>

      <td>
        keys which you have define and want to be a part of this Feature flag. These keys should be strictly boolean. For more information, see [Keys API](doc:keys-api) API].
      </td>

      <td>
        Object
      </td>

      <td>
        "keys": &lbrace;
            "ballSpeed": [  
              &lbrace;  
                "segmentName": "New Users",  
                "segmentValue": 10  
              &rbrace;  
            ]
          &rbrace;,
      </td>
    </tr>

    <tr>
      <td>
        segmentName
      </td>

      <td>
        Name of the segment created from CleverTap dashboard. .
      </td>

      <td>
        String
      </td>

      <td>
        New Users
      </td>
    </tr>

    <tr>
      <td>
        segmentValue
      </td>

      <td>
        value for this a segment
      </td>

      <td>
        same as the keys data type
      </td>

      <td>
        10
      </td>
    </tr>

    <tr>
      <td>
        SegmentPriorityList
      </td>

      <td>
        This list specifies the priority of the segments. If a profile is qualified for multiple segments, this list determines which segment value must be sent to the users device.
      </td>

      <td>
        Object
      </td>

      <td>
        "segmentsPriorityList": [  
            &lbrace;  
              "segmentName": "Champion Users"  
            &rbrace;  
          ]
      </td>
    </tr>
  </tbody>
</Table>

 For example, you want to change the ball speed key to 10 km/hr for **New users** segment and to 20 km/hr for all " Champion users" segment,  you can use the payload below. 

Here is an example JSON payload for iOS/Android.
## Example Request


```curl
curl --location --request POST 'https://api.clevertap.com/1/productconfig/create' 
--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' 
--header 'X-CleverTap-Passcode: <PASSCODE>' 
--header 'Content-Type: application/json' 
--header 'Content-Type: text/plain' 
--data-raw '&lbrace;
  "name": "gameConfig",
  "description": "Change the ball speed and ball velocity based on custom profile property.",
  "keys": &lbrace;
    "ballSpeed": [
      &lbrace;
        "segmentName": "New Users",
        "segmentValue": 10
      &rbrace;,
      &lbrace;
        "segmentName": "Champion Users",
        "segmentValue": 20
      &rbrace;  
    ]
  &rbrace;,
  "segmentsPriorityList": [
    &lbrace;
      "segmentName": "New Users"
    &rbrace;,
    &lbrace;
       "segmentName": "Champion Users"
    &rbrace;
  ]
&rbrace;'
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/productconfig/create")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "<ACCOUNT_ID>"
request["X-CleverTap-Passcode"] = "<PASSCODE>"
request["Content-Type"] = ["application/json", "text/plain"]
request.body = "&lbrace;\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": &lbrace;\n    \"ballSpeed\": [\n      &lbrace;\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 10\n      &rbrace;,\n      &lbrace;\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 20\n      &rbrace;  \n    ]\n  &rbrace;,\n  \"segmentsPriorityList\": [\n    &lbrace;\n      \"segmentName\": \"Champion Users\"\n    &rbrace;,\n    &lbrace;\n       \"segmentName\": \"Champion Users\"\n    &rbrace;\n  ]\n&rbrace;"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/productconfig/create"

payload = "&lbrace;\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": &lbrace;\n    \"ballSpeed\": [\n      &lbrace;\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 10\n      &rbrace;,\n      &lbrace;\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 20\n      &rbrace;  \n    ]\n  &rbrace;,\n  \"segmentsPriorityList\": [\n    &lbrace;\n      \"segmentName\": \"Champion Users\"\n    &rbrace;,\n    &lbrace;\n       \"segmentName\": \"Champion Users\"\n    &rbrace;\n  ]\n&rbrace;"
headers = &lbrace;
  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<PASSCODE>',
  'Content-Type': 'application/json',
  'Content-Type': 'text/plain'
&rbrace;

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/productconfig/create');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',
  'X-CleverTap-Passcode' => '<PASSCODE>',
  'Content-Type' => 'application/json',
  'Content-Type' => 'text/plain'
));
$request->setBody('&lbrace;\n  "name": "gameConfig",\n  "description": "Change the ball speed and ball velocity based on custom profile property.",\n  "keys": &lbrace;\n    "ballSpeed": [\n      &lbrace;\n        "segmentName": "New Users",\n        "segmentValue": 10\n      &rbrace;,\n      &lbrace;\n        "segmentName": "Champion Users",\n        "segmentValue": 20\n      &rbrace;  \n    ]\n  &rbrace;,\n  "segmentsPriorityList": [\n    &lbrace;\n      "segmentName": "New Users"\n    &rbrace;,\n    &lbrace;\n       "segmentName": " Champion Users"\n    &rbrace;\n  ]\n&rbrace;');
try &lbrace;
  $response = $request->send();
  if ($response->getStatus() == 200) &lbrace;
    echo $response->getBody();
  &rbrace;
  else &lbrace;
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  &rbrace;
&rbrace;
catch(HTTP_Request2_Exception $e) &lbrace;
  echo 'Error: ' . $e->getMessage();
&rbrace;
```
```javascript Node.js
var request = require('request');
var options = &lbrace;
  'method': 'POST',
  'url': 'https://api.clevertap.com/1/productconfig/create',
  'headers': &lbrace;
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PASSCODE>',
    'Content-Type': ['application/json', 'text/plain']
  &rbrace;,
  body: "&lbrace;\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": &lbrace;\n    \"ballSpeed\": [\n      &lbrace;\n        \"segmentName\": \"New Users\",\n        \"segmentValue\": 10\n      &rbrace;,\n      &lbrace;\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 20\n      &rbrace;  \n    ]\n  &rbrace;,\n  \"segmentsPriorityList\": [\n    &lbrace;\n      \"segmentName\": \"New Users\"\n    &rbrace;,\n    &lbrace;\n       \"segmentName\": \"Champion Users\"\n    &rbrace;\n  ]\n&rbrace;"

&rbrace;;
request(options, function (error, response) &lbrace; 
  if (error) throw new Error(error);
  console.log(response.body);
&rbrace;);
```
```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() &lbrace;

  url := "https://api.clevertap.com/1/productconfig/create"
  method := "POST"

  payload := strings.NewReader("&lbrace;\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": &lbrace;\n    \"ballSpeed\": [\n      &lbrace;\n        \"segmentName\": \"New Users\",\n        \"segmentValue\": 10\n      &rbrace;,\n      &lbrace;\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 20\n      &rbrace;  \n    ]\n  &rbrace;,\n  \"segmentsPriorityList\": [\n    &lbrace;\n      \"segmentName\": \"New Users\"\n    &rbrace;,\n    &lbrace;\n       \"segmentName\": \"Champion Users\"\n    &rbrace;\n  ]\n&rbrace;")

  client := &http.Client &lbrace;
  &rbrace;
  req, err := http.NewRequest(method, url, payload)

  if err != nil &lbrace;
    fmt.Println(err)
  &rbrace;
  req.Header.Add("X-CleverTap-Account-Id", "<ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<PASSCODE>")
  req.Header.Add("Content-Type", "application/json")
  req.Header.Add("Content-Type", "text/plain")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
&rbrace;
```


## Example Response



























































# Update Product Config

Update Feature Flag details for the key mentioned in the mandatory "experiencename" URL parameter. Any update to Feature flag will be version controlled.

## Base URL

[https://api.clevertap.com/1/productConfig/update?experiencename=](https://api.clevertap.com/1/productConfig/update?experiencename=)&lt;featureFlagName&gt;

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
        "X-CleverTap-Account-Id: ACCOUNT_ID"
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

Now for example, if you want to decrease the ball speed for Champion Users from 20 to 15 km/hr then you can use the following payload:

Here is an example JSON payload for iOS/Android.






















































































































































































































































































































































































































































































































## Example Request
```curl
curl --location --request POST 'https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig' 
--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' 
--header 'X-CleverTap-Passcode: <PASSCODE>' 
--header 'Content-Type: application/json' 
--header 'Content-Type: text/plain' 
--data-raw '&lbrace;
  "name": "gameConfig",
  "description": "Change the ball speed and ball velocity based on custom profile property.",
  "keys": &lbrace;
    "ballSpeed": [
      &lbrace;
        "segmentName": "New Users",
        "segmentValue": 10
      &rbrace;,
      &lbrace;
        "segmentName": "Champion Users",
        "segmentValue": 15
      &rbrace;  
    ]
  &rbrace;,
  "segmentsPriorityList": [
    &lbrace;
      "segmentName": "New Users"
    &rbrace;,
    &lbrace;
       "segmentName": "Champion Users"
    &rbrace;
  ]
&rbrace;'
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "<ACCOUNT_ID>"
request["X-CleverTap-Passcode"] = "<PASSCODE>"
request["Content-Type"] = ["application/json", "text/plain"]
request.body = "&lbrace;\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": &lbrace;\n    \"ballSpeed\": [\n      &lbrace;\n        \"segmentName\": \”New Users\",\n        \"segmentValue\": 10\n      &rbrace;,\n      &lbrace;\n        \"segmentName\": \”Champion Users\",\n        \"segmentValue\": 15\n      &rbrace;  \n    ]\n  &rbrace;,\n  \"segmentsPriorityList\": [\n    &lbrace;\n      \"segmentName\": \”New Users\"\n    &rbrace;,\n    &lbrace;\n       \"segmentName\": \”Champion Users\"\n    &rbrace;\n  ]\n&rbrace;"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig"

payload = "&lbrace;\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": &lbrace;\n    \"ballSpeed\": [\n      &lbrace;\n        \"segmentName\": \"New Users\",\n        \"segmentValue\": 10\n      &rbrace;,\n      &lbrace;\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 15\n      &rbrace;  \n    ]\n  &rbrace;,\n  \"segmentsPriorityList\": [\n    &lbrace;\n      \"segmentName\": \"New Users\"\n    &rbrace;,\n    &lbrace;\n       \"segmentName\": \"Champion Users\"\n    &rbrace;\n  ]\n&rbrace;"
headers = &lbrace;
  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<PASSCODE>',
  'Content-Type': 'application/json',
  'Content-Type': 'text/plain'
&rbrace;

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',
  'X-CleverTap-Passcode' => '<PASSCODE>',
  'Content-Type' => 'application/json',
  'Content-Type' => 'text/plain'
));
$request->setBody('&lbrace;\n  "name": "gameConfig",\n  "description": "Change the ball speed and ball velocity based on custom profile property.",\n  "keys": &lbrace;\n    "ballSpeed": [\n      &lbrace;\n        "segmentName": "New Users",\n        "segmentValue": 10\n      &rbrace;,\n      &lbrace;\n        "segmentName": "Champion Users",\n        "segmentValue": 15\n      &rbrace;  \n    ]\n  &rbrace;,\n  "segmentsPriorityList": [\n    &lbrace;\n      "segmentName": "New Users"\n    &rbrace;,\n    &lbrace;\n       "segmentName": "Champion Users"\n    &rbrace;\n  ]\n&rbrace;');
try &lbrace;
  $response = $request->send();
  if ($response->getStatus() == 200) &lbrace;
    echo $response->getBody();
  &rbrace;
  else &lbrace;
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  &rbrace;
&rbrace;
catch(HTTP_Request2_Exception $e) &lbrace;
  echo 'Error: ' . $e->getMessage();
&rbrace;
```
```javascript Node.js
var request = require('request');
var options = &lbrace;
  'method': 'POST',
  'url': 'https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig',
  'headers': &lbrace;
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PASSCODE>',
    'Content-Type': ['application/json', 'text/plain']
  &rbrace;,
  body: "&lbrace;\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": &lbrace;\n    \"ballSpeed\": [\n      &lbrace;\n        \"segmentName\": \"New Users\",\n        \"segmentValue\": 10\n      &rbrace;,\n      &lbrace;\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 15\n      &rbrace;  \n    ]\n  &rbrace;,\n  \"segmentsPriorityList\": [\n    &lbrace;\n      \"segmentName\": \"New Users\"\n    &rbrace;,\n    &lbrace;\n       \"segmentName\": \"Champion Users\"\n    &rbrace;\n  ]\n&rbrace;"

&rbrace;;
request(options, function (error, response) &lbrace; 
  if (error) throw new Error(error);
  console.log(response.body);
&rbrace;);
```
```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() &lbrace;

  url := "https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig"
  method := "POST"

  payload := strings.NewReader("&lbrace;\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": &lbrace;\n    \"ballSpeed\": [\n      &lbrace;\n        \"segmentName\": \"New Users\",\n        \"segmentValue\": 10\n      &rbrace;,\n      &lbrace;\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 15\n      &rbrace;  \n    ]\n  &rbrace;,\n  \"segmentsPriorityList\": [\n    &lbrace;\n      \"segmentName\": \"New Users\"\n    &rbrace;,\n    &lbrace;\n       \"segmentName\": \"Champion Users\"\n    &rbrace;\n  ]\n&rbrace;")

  client := &http.Client &lbrace;
  &rbrace;
  req, err := http.NewRequest(method, url, payload)

  if err != nil &lbrace;
    fmt.Println(err)
  &rbrace;
  req.Header.Add("X-CleverTap-Account-Id", "<ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<PASSCODE>")
  req.Header.Add("Content-Type", "application/json")
  req.Header.Add("Content-Type", "text/plain")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
&rbrace;
```


## Example Response



























































# Get Product Config

Getting Product Config requires a GET request with a JSON payload specifying the name and mapping. There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000. You can specify the optional "experiencename" parameter to get information about a particular Product Config object and also specify the optional "version" parameter to get information about a particular version of a Product Config .

## Base URL

[https://api.clevertap.com/1/productconfig/get?experiencename=](https://api.clevertap.com/1/productconfig/get?experiencename=)&lt;productconfig&gt;\&version=&lt;versionNumber&gt;

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
        "X-CleverTap-Account-Id: ACCOUNT_ID"
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

Here is an example JSON payload for iOS/Android. 

## Example Request
```curl
curl --location --request POST 'https://api.clevertap.com/1/productconfig/get' 
--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' 
--header 'X-CleverTap-Passcode: <PASSCODE>' 
--header 'Content-Type: application/json' 
--data-raw ''
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/productconfig/get")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "<ACCOUNT_ID>"
request["X-CleverTap-Passcode"] = "<PASSCODE>"
request["Content-Type"] = "application/json"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/productconfig/get"

payload  = &lbrace;&rbrace;
headers = &lbrace;
  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<PASSCODE>',
  'Content-Type': 'application/json'
&rbrace;

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/productconfig/get');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',
  'X-CleverTap-Passcode' => '<PASSCODE>',
  'Content-Type' => 'application/json'
));
$request->setBody('');
try &lbrace;
  $response = $request->send();
  if ($response->getStatus() == 200) &lbrace;
    echo $response->getBody();
  &rbrace;
  else &lbrace;
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  &rbrace;
&rbrace;
catch(HTTP_Request2_Exception $e) &lbrace;
  echo 'Error: ' . $e->getMessage();
&rbrace;
```
```javascript Node.js
var request = require('request');
var options = &lbrace;
  'method': 'POST',
  'url': 'https://api.clevertap.com/1/productconfig/get',
  'headers': &lbrace;
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PASSCODE>',
    'Content-Type': 'application/json'
  &rbrace;
&rbrace;;
request(options, function (error, response) &lbrace; 
  if (error) throw new Error(error);
  console.log(response.body);
&rbrace;);
```
```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() &lbrace;

  url := "https://api.clevertap.com/1/productconfig/get"
  method := "POST"

  payload := strings.NewReader("")

  client := &http.Client &lbrace;
  &rbrace;
  req, err := http.NewRequest(method, url, payload)

  if err != nil &lbrace;
    fmt.Println(err)
  &rbrace;
  req.Header.Add("X-CleverTap-Account-Id", "<ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<PASSCODE>")
  req.Header.Add("Content-Type", "application/json")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
&rbrace;
```


## Example Response

















































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































# Archive Product Config

You can archive any Product Config using this API and mentioning the mandatory "experiencename" in the URL parameter.

## Base URL

[https://api.clevertap.com/1/productconfig/archive?experiencename=](https://api.clevertap.com/1/productconfig/archive?experiencename=)&lt;productConfig&gt;

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
        "X-CleverTap-Account-Id: ACCOUNT_ID"
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

See the following examples:

## Example Request


```curl
curl --location --request GET 'https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig' 
--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' 
--header 'X-CleverTap-Passcode: <PASSCODE>' 
--header 'Content-Type: application/json' 
--data-raw ''
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Get.new(url)
request["X-CleverTap-Account-Id"] = "<ACCOUNT_ID>"
request["X-CleverTap-Passcode"] = "<PASSCODE>"
request["Content-Type"] = "application/json"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig"

payload  = &lbrace;&rbrace;
headers = &lbrace;
  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<PASSCODE>',
  'Content-Type': 'application/json'
&rbrace;

response = requests.request("GET", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig');
$request->setMethod(HTTP_Request2::METHOD_GET);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',
  'X-CleverTap-Passcode' => '<PASSCODE>',
  'Content-Type' => 'application/json'
));
$request->setBody('');
try &lbrace;
  $response = $request->send();
  if ($response->getStatus() == 200) &lbrace;
    echo $response->getBody();
  &rbrace;
  else &lbrace;
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  &rbrace;
&rbrace;
catch(HTTP_Request2_Exception $e) &lbrace;
  echo 'Error: ' . $e->getMessage();
&rbrace;
```
```javascript Node.js
var request = require('request');
var options = &lbrace;
  'method': 'GET',
  'url': 'https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig',
  'headers': &lbrace;
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PASSCODE>',
    'Content-Type': 'application/json'
  &rbrace;
&rbrace;;
request(options, function (error, response) &lbrace; 
  if (error) throw new Error(error);
  console.log(response.body);
&rbrace;);
```
```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() &lbrace;

  url := "https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig"
  method := "GET"

  payload := strings.NewReader("")

  client := &http.Client &lbrace;
  &rbrace;
  req, err := http.NewRequest(method, url, payload)

  if err != nil &lbrace;
    fmt.Println(err)
  &rbrace;
  req.Header.Add("X-CleverTap-Account-Id", "<ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<PASSCODE>")
  req.Header.Add("Content-Type", "application/json")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
&rbrace;
```


## Example Response





































































































































# Activate Product Config

You can use this API to either activate an archived Product Config or activate a particular version of an already active Product Config. If you want to just activate an archived Product Config you don't need to pass the version in the URL.

## Base URL

[https://api.clevertap.com/1/productconfig/activate?experienceName=](https://api.clevertap.com/1/productconfig/activate?experienceName=)&lt;productconfig&gt;\&version=&lt;versionNumber&gt;

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
        "X-CleverTap-Account-Id: ACCOUNT_ID"
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

For example, you want to activate the product config you archived above you can use the following examples

Here is an example JSON payload for iOS/Android. 

## Example Request
```curl
curl --location --request GET 'https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig' 
--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' 
--header 'X-CleverTap-Passcode: <PASSCODE>' 
--header 'Content-Type: application/json' 
--data-raw ''
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Get.new(url)
request["X-CleverTap-Account-Id"] = "<ACCOUNT_ID>"
request["X-CleverTap-Passcode"] = "<PASSCODE>"
request["Content-Type"] = "application/json"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig"

payload  = &lbrace;&rbrace;
headers = &lbrace;
  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<PASSCODE>',
  'Content-Type': 'application/json'
&rbrace;

response = requests.request("GET", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig');
$request->setMethod(HTTP_Request2::METHOD_GET);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',
  'X-CleverTap-Passcode' => '<PASSCODE>',
  'Content-Type' => 'application/json'
));
$request->setBody('');
try &lbrace;
  $response = $request->send();
  if ($response->getStatus() == 200) &lbrace;
    echo $response->getBody();
  &rbrace;
  else &lbrace;
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  &rbrace;
&rbrace;
catch(HTTP_Request2_Exception $e) &lbrace;
  echo 'Error: ' . $e->getMessage();
&rbrace;
```
```javascript Node.js
var request = require('request');
var options = &lbrace;
  'method': 'GET',
  'url': 'https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig',
  'headers': &lbrace;
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PASSCODE>',
    'Content-Type': 'application/json'
  &rbrace;
&rbrace;;
request(options, function (error, response) &lbrace; 
  if (error) throw new Error(error);
  console.log(response.body);
&rbrace;);
```
```go
package main

import (
  "fmt"
  "strings"
  "net/http"
  "io/ioutil"
)

func main() &lbrace;

  url := "https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig"
  method := "GET"

  payload := strings.NewReader("")

  client := &http.Client &lbrace;
  &rbrace;
  req, err := http.NewRequest(method, url, payload)

  if err != nil &lbrace;
    fmt.Println(err)
  &rbrace;
  req.Header.Add("X-CleverTap-Account-Id", "<ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<PASSCODE>")
  req.Header.Add("Content-Type", "application/json")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
&rbrace;
```


## Example Response