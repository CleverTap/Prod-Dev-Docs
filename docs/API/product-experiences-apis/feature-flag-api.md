---
title: Feature Flag API
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
The Feature Flag API enables you to create Feature Flag. For example, you want to activate or deactivate the dark mode based on the Feature Flag. 

# Create Feature Flag

Creating Feature Flag requires a POST request with a JSON payload specifying the key name and mapping.  There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.

## Base URL

[https://api.clevertap.com/1/featureflag/create](https://api.clevertap.com/1/featureflag/create)

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

The Feature Flag  and its keys with the relevant segment are uploaded as a JSON payload. 


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
        Name of the feature flag
      </td>

      <td>
        string
      </td>

      <td>
        dark\_mode
      </td>
    </tr>

    <tr>
      <td>
        description
      </td>

      <td>
        Description of the feature flag
      </td>

      <td>
        string
      </td>

      <td>
        enables/disables dark mode based on profile custom properties
      </td>
    </tr>

    <tr>
      <td>
        keys
      </td>

      <td>
        These are the keys that you have defined and want to be a part of this Feature flag. These keys must be strictly boolean. For more information, see [Keys API](doc:keys-api)
      </td>

      <td>
        Object
      </td>

      <td>
        "keys": \{\
            "dark": [  
              {  
                "segmentName": "New York User",  
                "segmentValue": true  
              }  
            ]\
          }
      </td>
    </tr>

    <tr>
      <td>
        segmentName
      </td>

      <td>
        name of the segment. You have created this segment via our dashboard.
      </td>

      <td>
        string
      </td>

      <td>
        New York Users
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
        same as keys data type
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        segmentsPriorityList
      </td>

      <td>
        This list specifies the priority of the segments. In case a profile gets qualified for multiple segment, this list determine which segment value should be send to the users device.
      </td>

      <td>
        Object
      </td>

      <td>
        "segmentsPriorityList": [  
            {  
              "segmentName": "New York User",  
              "percentageRollout": 80  
            }  
          ]
      </td>
    </tr>

    <tr>
      <td>
        percentageRollout
      </td>

      <td>
        Percentage rollout for a particular segment. Use this, if you want to deliver to only a percentage of users qualified for a segment. It helps you to measure the adoption of a feature.
      </td>

      <td>
        Integer
      </td>

      <td>
        20
      </td>
    </tr>
  </tbody>
</Table>
So considering the parameters given above, let's say, you want to test your dark mode feature on 20% of people staying in New York you can use the payload below. 

Here is an example JSON payload for iOS/Android. 




































































































































































































































































































































































## Example Request


```curl
curl --location --request POST 'https://api.clevertap.com/1/featureflag/create' \
--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <PassCode>' \
--header 'Content-Type: application/json' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "name": "darkMode",
  "description": "enables/disables dark mode based on profile custom properties",
  "keys": {
    "dark": [
      {
        "segmentName": "New York User",
        "segmentValue": true
      }
    ]
  },
  "segmentsPriorityList": [
    {
      "segmentName": "New York User",
      "percentageRollout": 20
    }
  ]
}'
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/featureflag/create")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "<ACCOUNT_ID>"
request["X-CleverTap-Passcode"] = "<PassCode>"
request["Content-Type"] = ["application/json", "text/plain"]
request.body = "{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 20\n    }\n  ]\n}"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/featureflag/create"

payload = "{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 20\n    }\n  ]\n}"
headers = {
  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<PassCode>',
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
$request->setUrl('https://api.clevertap.com/1/featureflag/create');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',
  'X-CleverTap-Passcode' => '<PassCode>',
  'Content-Type' => 'application/json',
  'Content-Type' => 'text/plain'
));
$request->setBody('{\n  "name": "darkMode",\n  "description": "enables/disables dark mode based on profile custom properties",\n  "keys": {\n    "dark": [\n      {\n        "segmentName": "New York User",\n        "segmentValue": true\n      }\n    ]\n  },\n  "segmentsPriorityList": [\n    {\n      "segmentName": "New York User",\n      "percentageRollout": 20\n    }\n  ]\n}');
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
  'url': 'https://api.clevertap.com/1/featureflag/create',
  'headers': {
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PassCode>',
    'Content-Type': ['application/json', 'text/plain']
  },
  body: "{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 20\n    }\n  ]\n}"

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

  url := "https://api.clevertap.com/1/featureflag/create"
  method := "POST"

  payload := strings.NewReader("{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 20\n    }\n  ]\n}")

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("X-CleverTap-Account-Id", "<ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<PassCode>")
  req.Header.Add("Content-Type", "application/json")
  req.Header.Add("Content-Type", "text/plain")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
}
```


## Example Response



























































# Update Feature Flag

Update Feature Flag details for the key mentioned in the mandatory "experiencename" URL parameter. Any update to Feature flag will be **version controlled**. 

## Base URL

[https://api.clevertap.com/1/featureflag/update?experiencename=](https://api.clevertap.com/1/featureflag/update?experiencename=)&lt;featureFlagName&gt;

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

Continuing from our example above, if you are confident of the dark mode adoption for New York users and want to roll out to a wider set of audience then you can increase the "percentageRollout" key to a higher number.

## Body Parameters

The Feature Flag and its keys with the relevant segment are uploaded as a JSON payload. 

Here is an example JSON payload for iOS/Android. We have increased the percentage of rollout to 80 percent.
## Example Request


```curl
curl --location --request POST 'https://api.clevertap.com/1/featureflag/update?experiencename=darkMode' \
--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <PassCode>' \
--header 'Content-Type: application/json' \
--header 'Content-Type: text/plain' \
--data-raw '{
  "name": "darkMode",
  "description": "enables/disables dark mode based on profile custom properties",
  "keys": {
    "dark": [
      {
        "segmentName": "New York User",
        "segmentValue": true
      }
    ]
  },
  "segmentsPriorityList": [
    {
      "segmentName": "New York User",
      "percentageRollout": 80
    }
  ]
}'
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/featureflag/update?experiencename=darkMode")

https = Net::HTTP.new(url.host, url.port);
https.use_ssl = true

request = Net::HTTP::Post.new(url)
request["X-CleverTap-Account-Id"] = "<ACCOUNT_ID>"
request["X-CleverTap-Passcode"] = "<PassCode>"
request["Content-Type"] = ["application/json", "text/plain"]
request.body = "{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 80\n    }\n  ]\n}"

response = https.request(request)
puts response.read_body
```
```python
import requests

url = "https://api.clevertap.com/1/featureflag/update?experiencename=darkMode"

payload = "{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 80\n    }\n  ]\n}"
headers = {
  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<PassCode>',
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
$request->setUrl('https://api.clevertap.com/1/featureflag/update?experiencename=darkMode');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',
  'X-CleverTap-Passcode' => '<PassCode>',
  'Content-Type' => 'application/json',
  'Content-Type' => 'text/plain'
));
$request->setBody('{\n  "name": "darkMode",\n  "description": "enables/disables dark mode based on profile custom properties",\n  "keys": {\n    "dark": [\n      {\n        "segmentName": "New York User",\n        "segmentValue": true\n      }\n    ]\n  },\n  "segmentsPriorityList": [\n    {\n      "segmentName": "New York User",\n      "percentageRollout": 80\n    }\n  ]\n}');
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
  'url': 'https://api.clevertap.com/1/featureflag/update?experiencename=darkMode',
  'headers': {
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PassCode>',
    'Content-Type': ['application/json', 'text/plain']
  },
  body: "{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 80\n    }\n  ]\n}"

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

  url := "https://api.clevertap.com/1/featureflag/update?experiencename=darkMode"
  method := "POST"

  payload := strings.NewReader("{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 80\n    }\n  ]\n}")

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("X-CleverTap-Account-Id", "<ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<PassCode>")
  req.Header.Add("Content-Type", "application/json")
  req.Header.Add("Content-Type", "text/plain")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
}
```


## Example Response



























































# Get Feature Flag

Getting Feature Flag requires a GET request with a JSON payload specifying the name and mapping.  There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000. You can specify the **optional** "experiencename" parameter to get information about a particular Feature Flag object and also specify the **optional** "version" parameter to get information about a particular version of a Feature Flag .

## Base URL

[https://api.clevertap.com/1/featureflag/get?experiencename=](https://api.clevertap.com/1/featureflag/get?experiencename=)&lt;featureFlagName&gt;\&version=&lt;versionNumber&gt;

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
curl --location --request POST 'https://api.clevertap.com/1/featureflag/get' \
--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <PASSCODE>' \
--header 'Content-Type: application/json' \
--data-raw ''
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/featureflag/get")

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

url = "https://api.clevertap.com/1/featureflag/get"

payload  = {}
headers = {
  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<PASSCODE>',
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/featureflag/get');
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
  'url': 'https://api.clevertap.com/1/featureflag/get',
  'headers': {
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PASSCODE>',
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

  url := "https://api.clevertap.com/1/featureflag/get"
  method := "POST"

  payload := strings.NewReader("")

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("X-CleverTap-Account-Id", "<ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<PASSCODE>")
  req.Header.Add("Content-Type", "application/json")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
}
```


## Example Response
















































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































# Archive a feature flag

You can archive a particular feature flag if you wish by using this API and mentioning the **mandatory** "experiencename" in the URL parameter. 

## Base URL

[https://api.clevertap.com/1/featureflag/archive?experiencename=](https://api.clevertap.com/1/featureflag/archive?experiencename=)&lt;featureFlagName&gt;

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

For example, you want to archive the featureFlag you created above you can use the following examples

## Example Request
```curl
curl --location --request POST 'https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode' \
--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <PASSCODE>' \
--header 'Content-Type: application/json' \
--data-raw ''
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode")

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

url = "https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode"

payload  = {}
headers = {
  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<PASSCODE>',
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode');
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
  'url': 'https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode',
  'headers': {
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PASSCODE>',
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

  url := "https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode"
  method := "POST"

  payload := strings.NewReader("")

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("X-CleverTap-Account-Id", "<ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<PASSCODE>")
  req.Header.Add("Content-Type", "application/json")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
}
```


## Example Response





































































































































# Activate

You can use this API to either activate an archived Feature Flag or activate a particular version of an already active Feature Flag. If you want to just activate an archived Feature Flag you do not need to pass the **version** in the URL. 

## Base URL

[https://api.clevertap.com/1/featureflag/activate?experienceName=](https://api.clevertap.com/1/featureflag/activate?experienceName=)&lt;featureFlag&gt;\&version=&lt;versionNumber&gt;

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

For example, if you want to activate a Feature Flag that you archived above you can use the following examples

## Example Request
```curl
curl --location --request POST 'https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode' \
--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \
--header 'X-CleverTap-Passcode: <PASSCODE>' \
--header 'Content-Type: application/json' \
--data-raw ''
```
```ruby
require "uri"
require "net/http"

url = URI("https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode")

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

url = "https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode"

payload  = {}
headers = {
  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
  'X-CleverTap-Passcode': '<PASSCODE>',
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))
```
```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode');
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
  'url': 'https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode',
  'headers': {
    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',
    'X-CleverTap-Passcode': '<PASSCODE>',
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

  url := "https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode"
  method := "POST"

  payload := strings.NewReader("")

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, payload)

  if err != nil {
    fmt.Println(err)
  }
  req.Header.Add("X-CleverTap-Account-Id", "<ACCOUNT_ID>")
  req.Header.Add("X-CleverTap-Passcode", "<PASSCODE>")
  req.Header.Add("Content-Type", "application/json")

  res, err := client.Do(req)
  defer res.Body.Close()
  body, err := ioutil.ReadAll(res.Body)

  fmt.Println(string(body))
}
```


## Example Response