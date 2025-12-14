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
[block:api-header]
{
  "title": "Overview"
}
[/block]
The GET schema APIs provide the capability to get the latest schema via API so that you have the latest changes.  

#GET Events Schema API
You can get all the events from the schema. This API will get you all the system events and custom events along with their meta and properties.

##Base URL
Use the following base URL:

https://[location].api.clevertap.com/getSchema?type=events
[block:callout]
{
  "type": "info",
  "title": "Location-based URL",
  "body": "Use the URL based on your location:\n  * India - in1.api.clevertap.com\n  * Singapore - sg1.api.clevertap.com\n  * U.S - us1.api.clevertap.com"
}
[/block]
###HTTP Method
GET

####Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please refer to the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "X-CleverTap-Account-Id",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "0-1": "Your CleverTap Account ID",
    "1-1": "Your CleverTap Account Passcode",
    "2-1": "Request content-type is always set to application/json; charset=utf-8.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json; charset=utf-8\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
### Body Parameters
You do not need to provide body parameters. 

#### Example Request  
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request POST 'https://[location].api.clevertap.com/getSchema?type=events\n--header 'X-CleverTap-Account-Id: RRP-XXT-CCWY' \\\n--header 'X-CleverTap-Passcode: 1c10a905d0fa4c168a8c9d818acf0a90'",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\nurl = URI(\"https://[location].api.clevertap.com/getschema?type=events/\")\nhttp = Net::HTTP.new(url.host, url.port);\nrequest = Net::HTTP::Post.new(url)\nrequest[\"Content-Type\"] = \"application/json\"\nrequest[\"X-CleverTap-Account-Id\"] = \"WWW-WWW-WWRZ\"\nrequest[\"X-CleverTap-Passcode\"] = \"ICW-QKE-YSGL\"\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    },
    {
      "code": "import requests\nurl = \"https://[location].api.clevertap.com/getschema?type=events\"\nheaders = {\n  'Content-Type': 'application/json',\n  'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',\n  'X-CleverTap-Passcode': 'ICW-QKE-YSGL'\n}\nresponse = requests.request(\"POST\", url, headers=headers)\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://[location].api.clevertap.com/getschema?type=events');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'Content-Type' => 'application/json',\n  'X-CleverTap-Account-Id' => 'WWW-WWW-WWRZ',\n  'X-CleverTap-Passcode' => 'ICW-QKE-YSGL'\n));\n\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://[location].api.clevertap.com/getschema?type=events',\n  'headers': {\n    'Content-Type': 'application/json',\n    'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',\n    'X-CleverTap-Passcode': 'ICW-QKE-YSGL'\n  };\n\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\nfunc main() {\n  url := \"https://[location].api.clevertap.com/getschema?type=events\"\n  method := \"POST\"\n  \n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"Content-Type\", \"application/json\")\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"WWW-WWW-WWRZ\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"ICW-QKE-YSGL\")\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n  fmt.Println(string(body))\n}",
      "language": "go"
    }
  ]
}
[/block]
####Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"success\",\n    \"events\": [\n        {\n            \"name\": \"Product Viewed\",\n            \"type\": \"Defined\",\n            \"status\": \"Active\",\n            \"createdOn\": 1528867959,\n            \"drp\": 1116,\n            \"eventCount\": {\n                \"currentMonth\": 6,\n                \"previousMonth\": 16\n            },\n            \"dataPoints\": 266,\n            \"properties\": [\n                {\n                    \"name\": \"product_price\",\n                    \"type\": \"Defined\",\n                    \"status\": \"Active\",\n                    \"required\": false,\n                    \"dataType\": \"Mixed\",\n                    \"dataTypeFallback\": \"Allow property\",\n                    \"dataPoints\": 0\n                }\n             \n              ]\n        }\n      ]\n}\n",
      "language": "json"
    }
  ]
}
[/block]
#GET User Properties Schema API 
You can get the schema for all the user properties. 

##Base URL
Use the following base URL:
https://[location].api.clevertap.com/getSchema?type=userproperty
[block:callout]
{
  "type": "info",
  "title": "Location-based URL",
  "body": "Use the URL based on your location:\n  * India - in1.api.clevertap.com\n  * Singapore - sg1.api.clevertap.com\n  * U.S - us1.api.clevertap.com"
}
[/block]
###HTTP Method
POST

#### Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please refer to the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "X-CleverTap-Account-Id",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "0-1": "Your CleverTap Account ID",
    "1-1": "Your CleverTap Account Passcode",
    "2-1": "Request content-type is always set to application/json; charset=utf-8.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json; charset=utf-8\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
### Body Parameters
You do not need to provide body parameters. 

#### Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request GET 'https://[location]/getSchema?type=userProperties' \\\n--header 'X-CleverTap-Account-Id: XRR-EEE-RRP' \\\n--header 'X-CleverTap-Passcode: 1c10a905d0fa4c168a8c9d818acf0a90'",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\nurl = URI(\"https://[location]/getSchema?type=userProperties\")\nhttp = Net::HTTP.new(url.host, url.port);\nrequest = Net::HTTP::Post.new(url)\nrequest[\"Content-Type\"] = \"application/json\"\nrequest[\"X-CleverTap-Account-Id\"] = \"WWW-WWW-WWRZ\"\nrequest[\"X-CleverTap-Passcode\"] = \"ICW-QKE-YSGL\"\nresponse = http.request(request)\nputs response.read_body",
      "language": "ruby"
    },
    {
      "code": "import requests\nurl = \"https://[location]/getSchema?type=userProperties\"\nheaders = {\n  'Content-Type': 'application/json',\n  'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',\n  'X-CleverTap-Passcode': 'ICW-QKE-YSGL'\n}\nresponse = requests.request(\"POST\", url, headers=headers)\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://[location]/getSchema?type=userProperties');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'Content-Type' => 'application/json',\n  'X-CleverTap-Account-Id' => 'WWW-WWW-WWRZ',\n  'X-CleverTap-Passcode' => 'ICW-QKE-YSGL'\n));\n\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://[location]/getSchema?type=userProperties',\n  'headers': {\n    'Content-Type': 'application/json',\n    'X-CleverTap-Account-Id': 'WWW-WWW-WWRZ',\n    'X-CleverTap-Passcode': 'ICW-QKE-YSGL'\n  };\n\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\nfunc main() {\n  url := \"https://[location]/getSchema?type=userProperties\"\n  method := \"POST\"\n  \n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"Content-Type\", \"application/json\")\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"WWW-WWW-WWRZ\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"ICW-QKE-YSGL\")\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n  fmt.Println(string(body))\n}",
      "language": "go"
    }
  ]
}
[/block]
### Example Response 
[block:code]
{
  "codes": [
    {
      "code": " {\n    \"status\": \"success\",\n    \"userProperties\": [\n        {\n            \"name\": \"testkey3\",\n            \"type\": \"Defined\",\n            \"status\": \"Discarded\",\n            \"dataType\": \"Mixed\",\n            \"dataTypeFallback\": \"Allow property\",\n            \"createdOn\": 1528878023\n        },\n        {\n            \"name\": \"keywords4\",\n            \"type\": \"Defined\",\n            \"status\": \"Discarded\",\n            \"dataType\": \"Mixed\",\n            \"dataTypeFallback\": \"Allow property\",\n            \"createdOn\": 1528878023\n        }\n      ]\n }\n",
      "language": "json"
    }
  ]
}
[/block]