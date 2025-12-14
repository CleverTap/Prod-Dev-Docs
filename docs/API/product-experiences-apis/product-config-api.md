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
https://api.clevertap.com/1/productconfig/create

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
The Product Config and its keys with the relevant segment are uploaded as a JSON payload. 
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "Name",
    "5-0": "SegmentPriorityList",
    "0-1": "Name of the Product config",
    "5-1": "This list specifies the priority of the segments. If a profile is qualified for multiple segments, this list determines which segment value must be sent to the users device.",
    "0-2": "string",
    "5-2": "Object",
    "0-3": "gameConfig",
    "5-3": "\"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"Champion Users\"\n    }\n  ]",
    "1-0": "Description",
    "2-0": "keys",
    "3-0": "segmentName",
    "3-2": "String",
    "2-2": "Object",
    "1-2": "string",
    "1-1": "Description of the Product Config",
    "2-1": "keys which you have define and want to be a part of this Feature flag. These keys should be strictly boolean. For more information, see [Keys API](doc:keys-api) API].",
    "3-1": "Name of the segment created from CleverTap dashboard. .",
    "1-3": "Change the ball speed and ball velocity based on custom profile property.",
    "2-3": "\"keys\": {\n    \"ballSpeed\": [\n      {\n        \"segmentName\": \"New Users\",\n        \"segmentValue\": 10\n      }\n    ]\n  },",
    "3-3": "New Users",
    "4-0": "segmentValue",
    "4-1": "value for this a segment",
    "4-2": "same as the keys data type",
    "4-3": "10"
  },
  "cols": 4,
  "rows": 6
}
[/block]
 For example, you want to change the ball speed key to 10 km/hr for **New users ** segment and to 20 km/hr for all " Champion users" segment,  you can use the payload below. 


Here is an example JSON payload for iOS/Android.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": {\n    \"ballSpeed\": [\n      {\n        \"segmentName\": \"New Users\",\n        \"segmentValue\": 10\n      },\n      {\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 20\n      }  \n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New Users\"\n    },\n    {\n       \"segmentName\": \"Champion Users\"\n    }\n  ]\n}",
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
      "code": "curl --location --request POST 'https://api.clevertap.com/1/productconfig/create' \\\n--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \\\n--header 'X-CleverTap-Passcode: <PASSCODE>' \\\n--header 'Content-Type: application/json' \\\n--header 'Content-Type: text/plain' \\\n--data-raw '{\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": {\n    \"ballSpeed\": [\n      {\n        \"segmentName\": \"New Users\",\n        \"segmentValue\": 10\n      },\n      {\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 20\n      }  \n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New Users\"\n    },\n    {\n       \"segmentName\": \"Champion Users\"\n    }\n  ]\n}'",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/productconfig/create\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"<ACCOUNT_ID>\"\nrequest[\"X-CleverTap-Passcode\"] = \"<PASSCODE>\"\nrequest[\"Content-Type\"] = [\"application/json\", \"text/plain\"]\nrequest.body = \"{\\n  \\\"name\\\": \\\"gameConfig\\\",\\n  \\\"description\\\": \\\"Change the ball speed and ball velocity based on custom profile property.\\\",\\n  \\\"keys\\\": {\\n    \\\"ballSpeed\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"Champion Users\\\",\\n        \\\"segmentValue\\\": 10\\n      },\\n      {\\n        \\\"segmentName\\\": \\\"Champion Users\\\",\\n        \\\"segmentValue\\\": 20\\n      }  \\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"Champion Users\\\"\\n    },\\n    {\\n       \\\"segmentName\\\": \\\"Champion Users\\\"\\n    }\\n  ]\\n}\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/productconfig/create\"\n\npayload = \"{\\n  \\\"name\\\": \\\"gameConfig\\\",\\n  \\\"description\\\": \\\"Change the ball speed and ball velocity based on custom profile property.\\\",\\n  \\\"keys\\\": {\\n    \\\"ballSpeed\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"Champion Users\\\",\\n        \\\"segmentValue\\\": 10\\n      },\\n      {\\n        \\\"segmentName\\\": \\\"Champion Users\\\",\\n        \\\"segmentValue\\\": 20\\n      }  \\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"Champion Users\\\"\\n    },\\n    {\\n       \\\"segmentName\\\": \\\"Champion Users\\\"\\n    }\\n  ]\\n}\"\nheaders = {\n  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode': '<PASSCODE>',\n  'Content-Type': 'application/json',\n  'Content-Type': 'text/plain'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/productconfig/create');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode' => '<PASSCODE>',\n  'Content-Type' => 'application/json',\n  'Content-Type' => 'text/plain'\n));\n$request->setBody('{\\n  \"name\": \"gameConfig\",\\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\\n  \"keys\": {\\n    \"ballSpeed\": [\\n      {\\n        \"segmentName\": \"New Users\",\\n        \"segmentValue\": 10\\n      },\\n      {\\n        \"segmentName\": \"Champion Users\",\\n        \"segmentValue\": 20\\n      }  \\n    ]\\n  },\\n  \"segmentsPriorityList\": [\\n    {\\n      \"segmentName\": \"New Users\"\\n    },\\n    {\\n       \"segmentName\": \" Champion Users\"\\n    }\\n  ]\\n}');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/productconfig/create',\n  'headers': {\n    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n    'X-CleverTap-Passcode': '<PASSCODE>',\n    'Content-Type': ['application/json', 'text/plain']\n  },\n  body: \"{\\n  \\\"name\\\": \\\"gameConfig\\\",\\n  \\\"description\\\": \\\"Change the ball speed and ball velocity based on custom profile property.\\\",\\n  \\\"keys\\\": {\\n    \\\"ballSpeed\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New Users\\\",\\n        \\\"segmentValue\\\": 10\\n      },\\n      {\\n        \\\"segmentName\\\": \\\"Champion Users\\\",\\n        \\\"segmentValue\\\": 20\\n      }  \\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New Users\\\"\\n    },\\n    {\\n       \\\"segmentName\\\": \\\"Champion Users\\\"\\n    }\\n  ]\\n}\"\n\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/productconfig/create\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"{\\n  \\\"name\\\": \\\"gameConfig\\\",\\n  \\\"description\\\": \\\"Change the ball speed and ball velocity based on custom profile property.\\\",\\n  \\\"keys\\\": {\\n    \\\"ballSpeed\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New Users\\\",\\n        \\\"segmentValue\\\": 10\\n      },\\n      {\\n        \\\"segmentName\\\": \\\"Champion Users\\\",\\n        \\\"segmentValue\\\": 20\\n      }  \\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New Users\\\"\\n    },\\n    {\\n       \\\"segmentName\\\": \\\"Champion Users\\\"\\n    }\\n  ]\\n}\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"<ACCOUNT_ID>\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"<PASSCODE>\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n  req.Header.Add(\"Content-Type\", \"text/plain\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
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
      "code": "{\n    \"status\": \"success\",\n    \"code\": 200\n}",
      "language": "json"
    }
  ]
}
[/block]

# Update Product Config
Update Feature Flag details for the key mentioned in the mandatory "experiencename" URL parameter. Any update to Feature flag will be version controlled.


## Base URL
https://api.clevertap.com/1/productConfig/update?experiencename=<featureFlagName>

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

Now for example, if you want to decrease the ball speed for Champion Users from 20 to 15 km/hr then you can use the following payload:


Here is an example JSON payload for iOS/Android.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": {\n    \"ballSpeed\": [\n      {\n        \"segmentName\": \"New Users\",\n        \"segmentValue\": 10\n      },\n      {\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 15 //changed this to 15 from 20\n      }  \n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New Users\"\n    },\n    {\n       \"segmentName\": \"Champion Users\"\n    }\n  ]\n}",
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
      "code": "curl --location --request POST 'https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig' \\\n--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \\\n--header 'X-CleverTap-Passcode: <PASSCODE>' \\\n--header 'Content-Type: application/json' \\\n--header 'Content-Type: text/plain' \\\n--data-raw '{\n  \"name\": \"gameConfig\",\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n  \"keys\": {\n    \"ballSpeed\": [\n      {\n        \"segmentName\": \"New Users\",\n        \"segmentValue\": 10\n      },\n      {\n        \"segmentName\": \"Champion Users\",\n        \"segmentValue\": 15\n      }  \n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New Users\"\n    },\n    {\n       \"segmentName\": \"Champion Users\"\n    }\n  ]\n}'",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"<ACCOUNT_ID>\"\nrequest[\"X-CleverTap-Passcode\"] = \"<PASSCODE>\"\nrequest[\"Content-Type\"] = [\"application/json\", \"text/plain\"]\nrequest.body = \"{\\n  \\\"name\\\": \\\"gameConfig\\\",\\n  \\\"description\\\": \\\"Change the ball speed and ball velocity based on custom profile property.\\\",\\n  \\\"keys\\\": {\\n    \\\"ballSpeed\\\": [\\n      {\\n        \\\"segmentName\\\": \\”New Users\\\",\\n        \\\"segmentValue\\\": 10\\n      },\\n      {\\n        \\\"segmentName\\\": \\”Champion Users\\\",\\n        \\\"segmentValue\\\": 15\\n      }  \\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\”New Users\\\"\\n    },\\n    {\\n       \\\"segmentName\\\": \\”Champion Users\\\"\\n    }\\n  ]\\n}\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig\"\n\npayload = \"{\\n  \\\"name\\\": \\\"gameConfig\\\",\\n  \\\"description\\\": \\\"Change the ball speed and ball velocity based on custom profile property.\\\",\\n  \\\"keys\\\": {\\n    \\\"ballSpeed\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New Users\\\",\\n        \\\"segmentValue\\\": 10\\n      },\\n      {\\n        \\\"segmentName\\\": \\\"Champion Users\\\",\\n        \\\"segmentValue\\\": 15\\n      }  \\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New Users\\\"\\n    },\\n    {\\n       \\\"segmentName\\\": \\\"Champion Users\\\"\\n    }\\n  ]\\n}\"\nheaders = {\n  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode': '<PASSCODE>',\n  'Content-Type': 'application/json',\n  'Content-Type': 'text/plain'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode' => '<PASSCODE>',\n  'Content-Type' => 'application/json',\n  'Content-Type' => 'text/plain'\n));\n$request->setBody('{\\n  \"name\": \"gameConfig\",\\n  \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\\n  \"keys\": {\\n    \"ballSpeed\": [\\n      {\\n        \"segmentName\": \"New Users\",\\n        \"segmentValue\": 10\\n      },\\n      {\\n        \"segmentName\": \"Champion Users\",\\n        \"segmentValue\": 15\\n      }  \\n    ]\\n  },\\n  \"segmentsPriorityList\": [\\n    {\\n      \"segmentName\": \"New Users\"\\n    },\\n    {\\n       \"segmentName\": \"Champion Users\"\\n    }\\n  ]\\n}');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig',\n  'headers': {\n    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n    'X-CleverTap-Passcode': '<PASSCODE>',\n    'Content-Type': ['application/json', 'text/plain']\n  },\n  body: \"{\\n  \\\"name\\\": \\\"gameConfig\\\",\\n  \\\"description\\\": \\\"Change the ball speed and ball velocity based on custom profile property.\\\",\\n  \\\"keys\\\": {\\n    \\\"ballSpeed\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New Users\\\",\\n        \\\"segmentValue\\\": 10\\n      },\\n      {\\n        \\\"segmentName\\\": \\\"Champion Users\\\",\\n        \\\"segmentValue\\\": 15\\n      }  \\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New Users\\\"\\n    },\\n    {\\n       \\\"segmentName\\\": \\\"Champion Users\\\"\\n    }\\n  ]\\n}\"\n\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/productconfig/update?experiencename=gameConfig\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"{\\n  \\\"name\\\": \\\"gameConfig\\\",\\n  \\\"description\\\": \\\"Change the ball speed and ball velocity based on custom profile property.\\\",\\n  \\\"keys\\\": {\\n    \\\"ballSpeed\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New Users\\\",\\n        \\\"segmentValue\\\": 10\\n      },\\n      {\\n        \\\"segmentName\\\": \\\"Champion Users\\\",\\n        \\\"segmentValue\\\": 15\\n      }  \\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New Users\\\"\\n    },\\n    {\\n       \\\"segmentName\\\": \\\"Champion Users\\\"\\n    }\\n  ]\\n}\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"<ACCOUNT_ID>\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"<PASSCODE>\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n  req.Header.Add(\"Content-Type\", \"text/plain\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
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
      "code": "{\n    \"status\": \"success\",\n    \"code\": 200\n}",
      "language": "json"
    }
  ]
}
[/block]
# Get Product Config
Getting Product Config requires a GET request with a JSON payload specifying the name and mapping. There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000. You can specify the optional "experiencename" parameter to get information about a particular Product Config object and also specify the optional "version" parameter to get information about a particular version of a Product Config .

## Base URL
https://api.clevertap.com/1/productconfig/get?experiencename=<productconfig>&version=<versionNumber>

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

Here is an example JSON payload for iOS/Android. 

## Example Request 
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request POST 'https://api.clevertap.com/1/productconfig/get' \\\n--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \\\n--header 'X-CleverTap-Passcode: <PASSCODE>' \\\n--header 'Content-Type: application/json' \\\n--data-raw ''",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/productconfig/get\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"<ACCOUNT_ID>\"\nrequest[\"X-CleverTap-Passcode\"] = \"<PASSCODE>\"\nrequest[\"Content-Type\"] = \"application/json\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/productconfig/get\"\n\npayload  = {}\nheaders = {\n  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode': '<PASSCODE>',\n  'Content-Type': 'application/json'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/productconfig/get');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode' => '<PASSCODE>',\n  'Content-Type' => 'application/json'\n));\n$request->setBody('');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/productconfig/get',\n  'headers': {\n    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n    'X-CleverTap-Passcode': '<PASSCODE>',\n    'Content-Type': 'application/json'\n  }\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/productconfig/get\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"<ACCOUNT_ID>\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"<PASSCODE>\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
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
      "code": "{\n  \"output\": [\n    {\n      \"name\": \"gameConfig\",\n      \"description\": \"Change the ball speed and ball velocity based on custom profile property.\",\n      \"segmentsPriorityList\": [\n        {\n          \"segmentName\": \"New Users\",\n          \"percentageRollout\": 100\n        },\n        {\n          \"segmentName\": \"Champion Users\",\n          \"percentageRollout\": 100\n        }\n      ],\n      \"keys\": {\n        \"ballSpeed\": [\n          {\n            \"segmentName\": \"New Users\",\n            \"segmentValue\": 10\n          },\n          {\n            \"segmentName\": \"Champion Users\",\n            \"segmentValue\": 15\n          }\n        ]\n      },\n      \"keyIds\": [\n        1589897424\n      ],\n      \"start_time\": 1589897616,\n      \"end_time\": 2147483647,\n      \"status\": \"active\",\n      \"version\": {\n        \"update_mode\": \"create\",\n        \"created_by\": \"foo@bar.com\",\n        \"created_on\": 1589897616,\n        \"updated_by\": \"foo@bar.com\",\n        \"last_updated\": 1589897616,\n        \"versionNumber\": 1\n      },\n      \"id\": 1589897616\n    }\n  ],\n  \"status\": \"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]

# Archive Product Config
You can archive any Product Config using this API and mentioning the mandatory "experiencename" in the URL parameter.

## Base URL
https://api.clevertap.com/1/productconfig/archive?experiencename=<productConfig>


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


See the following examples:

## Example Request 
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request GET 'https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig' \\\n--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \\\n--header 'X-CleverTap-Passcode: <PASSCODE>' \\\n--header 'Content-Type: application/json' \\\n--data-raw ''",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Get.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"<ACCOUNT_ID>\"\nrequest[\"X-CleverTap-Passcode\"] = \"<PASSCODE>\"\nrequest[\"Content-Type\"] = \"application/json\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig\"\n\npayload  = {}\nheaders = {\n  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode': '<PASSCODE>',\n  'Content-Type': 'application/json'\n}\n\nresponse = requests.request(\"GET\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig');\n$request->setMethod(HTTP_Request2::METHOD_GET);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode' => '<PASSCODE>',\n  'Content-Type' => 'application/json'\n));\n$request->setBody('');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'GET',\n  'url': 'https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig',\n  'headers': {\n    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n    'X-CleverTap-Passcode': '<PASSCODE>',\n    'Content-Type': 'application/json'\n  }\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/productconfig/archive?experiencename=gameConfig\"\n  method := \"GET\"\n\n  payload := strings.NewReader(\"\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"<ACCOUNT_ID>\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"<PASSCODE>\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
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
      "code": "{\n    \"success\": true,\n    \"status\": \"success\",\n    \"error\": \"Experiences successfully deactivated\",\n    \"code\": 200\n}",
      "language": "json"
    }
  ]
}
[/block]
# Activate Product Config
You can use this API to either activate an archived Product Config or activate a particular version of an already active Product Config. If you want to just activate an archived Product Config you don't need to pass the version in the URL.

## Base URL
https://api.clevertap.com/1/productconfig/activate?experienceName=<productconfig>&version=<versionNumber>

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
For example, you want to activate the product config you archived above you can use the following examples

Here is an example JSON payload for iOS/Android. 

## Example Request 
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request GET 'https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig' \\\n--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \\\n--header 'X-CleverTap-Passcode: <PASSCODE>' \\\n--header 'Content-Type: application/json' \\\n--data-raw ''",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Get.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"<ACCOUNT_ID>\"\nrequest[\"X-CleverTap-Passcode\"] = \"<PASSCODE>\"\nrequest[\"Content-Type\"] = \"application/json\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig\"\n\npayload  = {}\nheaders = {\n  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode': '<PASSCODE>',\n  'Content-Type': 'application/json'\n}\n\nresponse = requests.request(\"GET\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig');\n$request->setMethod(HTTP_Request2::METHOD_GET);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode' => '<PASSCODE>',\n  'Content-Type' => 'application/json'\n));\n$request->setBody('');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'GET',\n  'url': 'https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig',\n  'headers': {\n    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n    'X-CleverTap-Passcode': '<PASSCODE>',\n    'Content-Type': 'application/json'\n  }\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/productconfig/activate?experiencename=gameConfig\"\n  method := \"GET\"\n\n  payload := strings.NewReader(\"\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"<ACCOUNT_ID>\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"<PASSCODE>\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
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