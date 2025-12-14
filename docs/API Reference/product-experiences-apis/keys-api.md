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
These are the keys that you have defined in your app code. You can map these keys from the CleverTap API and control the configuration of your app using the [Product Config](https://docs.clevertap.com/docs/product-config#section-product-config) or [Feature Flags](https://docs.clevertap.com/docs/product-config#section-feature-flags) feature. 
. The Keys API enables you to create keys for [Product Config API](doc:product-config-api)] and [Feature Flag API](doc:feature-flag-api).  For example, you want to create a configuration that changes the speed of the ball in a game. For this, you have to map the keys from inside your app to the CleverTap keys. You can then use these keys in a product config to control the app configuration. 

[block:callout]
{
  "type": "info",
  "body": "The name of the key defined here must match the name of the key defined in the app. This name cannot be changed later. You can add up to 500 keys.",
  "title": "Note"
}
[/block]
# Create Keys
Creating Keys requires a POST request with a JSON payload specifying the key details. 

## Base URL
https://api.clevertap.com/1/keys/create

## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
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
## Body Parameters
The keys and their mapping to the relevant segment are uploaded as a JSON payload. 

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "name",
    "0-1": "Name of the Key",
    "0-2": "string",
    "0-3": "ball_speed",
    "1-0": "description",
    "1-2": "string",
    "1-1": "Short description of the Key",
    "1-3": "Vary the speed of the ball.",
    "2-0": "dataType",
    "3-0": "defaultValue",
    "2-1": "data type of the key",
    "2-2": "string/integer/JSON/boolean/double/float",
    "2-3": "integer",
    "3-1": "default value of the key",
    "3-2": "same as the dataType mentioned",
    "3-3": "10"
  },
  "cols": 4,
  "rows": 4
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"name\": \"ball_speed\",\n  \"description\": \"Vary the speed of the ball\",\n  \"dataType\":integer,\n  \"defaultValue\":10\n}\n\n",
      "language": "json"
    }
  ]
}
[/block]
## Example Request 
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request POST 'https://api.clevertap.com/1/keys/create' \\\n--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \\\n--header 'X-CleverTap-Passcode: PASSCODE' \\\n--header 'Content-Type: application/json' \\\n--header 'Content-Type: text/plain' \\\n--data-raw '{\n\t\"name\":\"ball_speed\",\n\t\"description\": \"Vary the speed of the ball\",\n\t\"defaultValue\": 10,\n\t\"dataType\": \"integer\"\n}'",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/keys/create\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-CleverTap-Passcode\"] = \"PASSCODE\"\nrequest[\"Content-Type\"] = [\"application/json\", \"text/plain\"]\nrequest.body = \"{\\n\\t\\\"name\\\":\\\"ball_speed\\\",\\n\\t\\\"description\\\": \\\"Vary the speed of the ball.\\\",\\n\\t\\\"defaultValue\\\": 10,\\n\\t\\\"dataType\\\": \\\"integer\\\"\\n}\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/keys/create\"\n\npayload = \"{\\n\\t\\\"name\\\":\\\"ball_speed\\\",\\n\\t\\\"description\\\": \\\"Vary the speed of the ball.\\\",\\n\\t\\\"defaultValue\\\": 10,\\n\\t\\\"dataType\\\": \\\"integer\\\"\\n}\"\nheaders = {\n  'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n  'X-CleverTap-Passcode': 'PASSCODE',\n  'Content-Type': 'application/json',\n  'Content-Type': 'text/plain'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/keys/create');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n  'X-CleverTap-Passcode' => 'PASSCODE',\n  'Content-Type' => 'application/json',\n  'Content-Type' => 'text/plain'\n));\n$request->setBody('{\\n\t\"name\":\"ball_speed\",\\n\t\"description\": \"Vary the speed of the ball.\",\\n\t\"defaultValue\": 10,\\n\t\"dataType\": \"integer\"\\n}');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/keys/create',\n  'headers': {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': ['application/json', 'text/plain']\n  },\n  body: \"{\\n\\t\\\"name\\\":\\\"ball_speed\\\",\\n\\t\\\"description\\\": \\\"Vary the speed of the ball.\\\",\\n\\t\\\"defaultValue\\\": 10,\\n\\t\\\"dataType\\\": \\\"integer\\\"\\n}\"\n\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/keys/create\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"{\\n\t\\\"name\\\":\\\"ball_speed\\\",\\n\t\\\"description\\\": \\\"Vary the speed of the ball.\\\",\\n\t\\\"defaultValue\\\": 10,\\n\t\\\"dataType\\\": \\\"integer\\\"\\n}\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"ACCOUNT_ID\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"PASSCODE\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n  req.Header.Add(\"Content-Type\", \"text/plain\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
      "language": "go"
    }
  ]
}
[/block]
## Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]

# Update Keys
Update key details for the key mentioned in the "keyName" URL parameter. Specify JSON payload with the key details in the request Body. 

## Base URL
https://api.clevertap.com/1/keys/update?keyName=<keyName>

## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
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
## Body Parameters
The keys and are uploaded as a JSON payload. 
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "description",
    "0-2": "string",
    "0-1": "Short description of the Key",
    "0-3": "Vary the speed of the ball.",
    "1-0": "dataType",
    "2-0": "defaultValue",
    "1-1": "data type of the key",
    "2-1": "default value of the key",
    "1-2": "string/integer/JSON/boolean/double/float",
    "2-2": "same as the dataType mentioned",
    "1-3": "integer",
    "2-3": "20"
  },
  "cols": 4,
  "rows": 3
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"description\": \"Vary the speed of the ball. Updated\",\n\t\"defaultValue\": 20,\n\t\"dataType\": \"integer\"\n}",
      "language": "json"
    }
  ]
}
[/block]
## Example Request 
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request POST 'https://api.clevertap.com/1/keys/update?keyName=<keyName>' \\\n--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \\\n--header 'X-CleverTap-Passcode: PASSCODE' \\\n--header 'Content-Type: application/json' \\\n--header 'Content-Type: text/plain' \\\n--data-raw '{\n\t\"description\": \"Vary the speed of the ball. Updated\",\n\t\"defaultValue\": 20,\n\t\"dataType\": \"integer\"\n}'",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/keys/update?keyName=<keyName>\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-CleverTap-Passcode\"] = \"PASSCODE\"\nrequest[\"Content-Type\"] = [\"application/json\", \"text/plain\"]\nrequest.body = \"{\\n\\t\\\"description\\\": \\\"Vary the speed of the ball. Updated\\\",\\n\\t\\\"defaultValue\\\": 20,\\n\\t\\\"dataType\\\": \\\"integer\\\"\\n}\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/keys/update?keyName=<keyName>\"\n\npayload = \"{\\n\\t\\\"description\\\": \\\"Vary the speed of the ball. Updated\\\",\\n\\t\\\"defaultValue\\\": 20,\\n\\t\\\"dataType\\\": \\\"integer\\\"\\n}\"\nheaders = {\n  'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n  'X-CleverTap-Passcode': 'PASSCODE',\n  'Content-Type': 'application/json',\n  'Content-Type': 'text/plain'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/keys/update?keyName=<keyName>');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n  'X-CleverTap-Passcode' => 'PASSCODE',\n  'Content-Type' => 'application/json',\n  'Content-Type' => 'text/plain'\n));\n$request->setBody('{\\n\t\"description\": \"Vary the speed of the ball. Updated\",\\n\t\"defaultValue\": 20,\\n\t\"dataType\": \"integer\"\\n}');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/keys/update?keyName=<keyName>',\n  'headers': {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': ['application/json', 'text/plain']\n  },\n  body: \"{\\n\\t\\\"description\\\": \\\"Vary the speed of the ball. Updated\\\",\\n\\t\\\"defaultValue\\\": 20,\\n\\t\\\"dataType\\\": \\\"integer\\\"\\n}\"\n\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/keys/update?keyName=%3CkeyName%3E\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"{\\n\t\\\"description\\\": \\\"Vary the speed of the ball. Updated\\\",\\n\t\\\"defaultValue\\\": 20,\\n\t\\\"dataType\\\": \\\"integer\\\"\\n}\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"ACCOUNT_ID\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"PASSCODE\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n  req.Header.Add(\"Content-Type\", \"text/plain\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
      "language": "go"
    }
  ]
}
[/block]
## Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]
# Get Keys
You can get details of all the keys using this API. Add the key name in the  URL parameter to get details of a specific key.

## Base URL
https://api.clevertap.com/1/keys/get?keyName=<optional keyName>

## HTTP Method
GET

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
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
## Example Request 
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request GET 'https://api.clevertap.com/1/keys/get' \\\n--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \\\n--header 'X-CleverTap-Passcode: PASSCODE' \\\n--header 'Content-Type: application/json' \\\n--data-raw ''",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/keys/get\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Get.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-CleverTap-Passcode\"] = \"PASSCODE\"\nrequest[\"Content-Type\"] = \"application/json\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/keys/get\"\n\npayload  = {}\nheaders = {\n  'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n  'X-CleverTap-Passcode': 'PASSCODE',\n  'Content-Type': 'application/json'\n}\n\nresponse = requests.request(\"GET\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/keys/get');\n$request->setMethod(HTTP_Request2::METHOD_GET);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n  'X-CleverTap-Passcode' => 'PASSCODE',\n  'Content-Type' => 'application/json'\n));\n$request->setBody('');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'GET',\n  'url': 'https://api.clevertap.com/1/keys/get',\n  'headers': {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n  }\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/keys/get\"\n  method := \"GET\"\n\n  payload := strings.NewReader(\"\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"ACCOUNT_ID\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"PASSCODE\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
      "language": "go"
    }
  ]
}
[/block]
## Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n  “output”: [\n    {\n      “name”: “ball_speed”,\n      “defaultValue”:20,\n      “dataType”: “integer”,\n      “description”: “Vary the speed of the ball. Updated”,\n      id”: 1589271701\n    },\n  ],\n  “status”: “success”\n}",
      "language": "json"
    }
  ]
}
[/block]

# Archive Keys
You can archive a key using this API by specifying the key name in the URL. 

## Base URL
https://api.clevertap.com/1/keys/archive?keyName=<keyName> 

## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
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
## Example Request 
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request POST 'https://api.clevertap.com/1/keys/archive?keyName=<keyName>' \\\n--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \\\n--header 'X-CleverTap-Passcode: PASSCODE' \\\n--header 'Content-Type: application/json' \\\n--data-raw ''",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/keys/archive?keyName=<keyName>\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-CleverTap-Passcode\"] = \"PASSCODE\"\nrequest[\"Content-Type\"] = \"application/json\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/keys/archive?keyName=<keyName>\"\n\npayload  = {}\nheaders = {\n  'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n  'X-CleverTap-Passcode': 'PASSCODE',\n  'Content-Type': 'application/json'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/keys/archive?keyName=<keyName>');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n  'X-CleverTap-Passcode' => 'PASSCODE',\n  'Content-Type' => 'application/json'\n));\n$request->setBody('');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/keys/archive?keyName=<keyName>',\n  'headers': {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n  }\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/keys/archive?keyName=<keyName>\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"ACCOUNT_ID\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"PASSCODE\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
      "language": "go"
    }
  ]
}
[/block]
## Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"success\": true,\n    \"status\": \"success\",\n    \"error\": \"Successfully changed key status\",\n    \"code\": 200\n}",
      "language": "json"
    }
  ]
}
[/block]
# Activate Keys
You can activate any key via this API by specifying the key name in the URL.

## Base URL
https://api.clevertap.com/1/keys/activate?keyName=<keyName>

## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
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
## Example Request 
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request POST 'https://api.clevertap.com/1/keys/activate?keyName=<keyName>' \\\n--header 'X-CleverTap-Account-Id: ACCOUNT_ID' \\\n--header 'X-CleverTap-Passcode: PASSCODE' \\\n--header 'Content-Type: application/json' \\\n--data-raw ''",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/keys/activate?keyName=<keyName>\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-CleverTap-Passcode\"] = \"PASSCODE\"\nrequest[\"Content-Type\"] = \"application/json\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/keys/activate?keyName=<keyName>\"\n\npayload  = {}\nheaders = {\n  'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n  'X-CleverTap-Passcode': 'PASSCODE',\n  'Content-Type': 'application/json'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/keys/activate?keyName=<keyName>');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n  'X-CleverTap-Passcode' => 'PASSCODE',\n  'Content-Type' => 'application/json'\n));\n$request->setBody('');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/keys/activate?keyName=<keyName>',\n  'headers': {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n  }\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/keys/activate?keyName=<keyName>\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"ACCOUNT_ID\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"PASSCODE\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
      "language": "go"
    }
  ]
}
[/block]
## Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"success\": true,\n    \"status\": \"success\",\n    \"error\": \"Successfully changed key status\",\n    \"code\": 200\n}",
      "language": "json"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000.",
  "title": "Note"
}
[/block]