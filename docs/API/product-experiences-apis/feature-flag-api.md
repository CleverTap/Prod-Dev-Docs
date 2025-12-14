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
https://api.clevertap.com/1/featureflag/create

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
The Feature Flag  and its keys with the relevant segment are uploaded as a JSON payload. 
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "name",
    "4-0": "segmentValue",
    "0-1": "Name of the feature flag",
    "4-1": "value for this a segment",
    "0-2": "string",
    "4-2": "same as keys data type",
    "0-3": "dark_mode",
    "4-3": "true",
    "1-0": "description",
    "2-0": "keys",
    "3-0": "segmentName",
    "3-2": "string",
    "2-2": "Object",
    "1-2": "string",
    "1-1": "Description of the feature flag",
    "2-1": "These are the keys that  you have defined and want to be a part of this Feature flag. These keys must be strictly boolean. For more information, see [Keys API](doc:keys-api) ]",
    "3-1": "name of the segment. You have created this segment via our dashboard.",
    "1-3": "enables/disables dark mode based on profile custom properties",
    "2-3": "\"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },",
    "3-3": "New York Users",
    "5-0": "segmentsPriorityList",
    "5-1": "This list specifies the priority of the segments. In case a profile gets qualified for multiple segment, this list determine which segment value should be send to the users device.",
    "5-2": "Object",
    "5-3": "\"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 80\n    }\n  ]",
    "6-0": "percentageRollout",
    "6-1": "Percentage rollout for a particular segment. Use this, if you want to deliver to only a percentage of users qualified for a segment. It helps you to measure the adoption of a feature.",
    "6-2": "Integer",
    "6-3": "20"
  },
  "cols": 4,
  "rows": 7
}
[/block]
So considering the parameters given above, let's say, you want to test your dark mode feature on 20% of people staying in New York you can use the payload below. 


Here is an example JSON payload for iOS/Android. 
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 20\n    }\n  ]\n}",
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
      "code": "curl --location --request POST 'https://api.clevertap.com/1/featureflag/create' \\\n--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \\\n--header 'X-CleverTap-Passcode: <PassCode>' \\\n--header 'Content-Type: application/json' \\\n--header 'Content-Type: text/plain' \\\n--data-raw '{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 20\n    }\n  ]\n}'",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/featureflag/create\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"<ACCOUNT_ID>\"\nrequest[\"X-CleverTap-Passcode\"] = \"<PassCode>\"\nrequest[\"Content-Type\"] = [\"application/json\", \"text/plain\"]\nrequest.body = \"{\\n  \\\"name\\\": \\\"darkMode\\\",\\n  \\\"description\\\": \\\"enables/disables dark mode based on profile custom properties\\\",\\n  \\\"keys\\\": {\\n    \\\"dark\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New York User\\\",\\n        \\\"segmentValue\\\": true\\n      }\\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New York User\\\",\\n      \\\"percentageRollout\\\": 20\\n    }\\n  ]\\n}\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/featureflag/create\"\n\npayload = \"{\\n  \\\"name\\\": \\\"darkMode\\\",\\n  \\\"description\\\": \\\"enables/disables dark mode based on profile custom properties\\\",\\n  \\\"keys\\\": {\\n    \\\"dark\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New York User\\\",\\n        \\\"segmentValue\\\": true\\n      }\\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New York User\\\",\\n      \\\"percentageRollout\\\": 20\\n    }\\n  ]\\n}\"\nheaders = {\n  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode': '<PassCode>',\n  'Content-Type': 'application/json',\n  'Content-Type': 'text/plain'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/featureflag/create');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode' => '<PassCode>',\n  'Content-Type' => 'application/json',\n  'Content-Type' => 'text/plain'\n));\n$request->setBody('{\\n  \"name\": \"darkMode\",\\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\\n  \"keys\": {\\n    \"dark\": [\\n      {\\n        \"segmentName\": \"New York User\",\\n        \"segmentValue\": true\\n      }\\n    ]\\n  },\\n  \"segmentsPriorityList\": [\\n    {\\n      \"segmentName\": \"New York User\",\\n      \"percentageRollout\": 20\\n    }\\n  ]\\n}');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/featureflag/create',\n  'headers': {\n    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n    'X-CleverTap-Passcode': '<PassCode>',\n    'Content-Type': ['application/json', 'text/plain']\n  },\n  body: \"{\\n  \\\"name\\\": \\\"darkMode\\\",\\n  \\\"description\\\": \\\"enables/disables dark mode based on profile custom properties\\\",\\n  \\\"keys\\\": {\\n    \\\"dark\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New York User\\\",\\n        \\\"segmentValue\\\": true\\n      }\\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New York User\\\",\\n      \\\"percentageRollout\\\": 20\\n    }\\n  ]\\n}\"\n\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/featureflag/create\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"{\\n  \\\"name\\\": \\\"darkMode\\\",\\n  \\\"description\\\": \\\"enables/disables dark mode based on profile custom properties\\\",\\n  \\\"keys\\\": {\\n    \\\"dark\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New York User\\\",\\n        \\\"segmentValue\\\": true\\n      }\\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New York User\\\",\\n      \\\"percentageRollout\\\": 20\\n    }\\n  ]\\n}\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"<ACCOUNT_ID>\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"<PassCode>\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n  req.Header.Add(\"Content-Type\", \"text/plain\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
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

# Update Feature Flag 
Update Feature Flag details for the key mentioned in the mandatory "experiencename" URL parameter. Any update to Feature flag will be **version controlled**. 

## Base URL
https://api.clevertap.com/1/featureflag/update?experiencename=<featureFlagName>

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
Continuing from our example above, if you are confident of the dark mode adoption for New York users and want to roll out to a wider set of audience then you can increase the "percentageRollout" key to a higher number.

## Body Parameters
The Feature Flag and its keys with the relevant segment are uploaded as a JSON payload. 

Here is an example JSON payload for iOS/Android. We have increased the percentage of rollout to 80 percent.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 80 //changed this from 20 to 80\n    }\n  ]\n}",
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
      "code": "curl --location --request POST 'https://api.clevertap.com/1/featureflag/update?experiencename=darkMode' \\\n--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \\\n--header 'X-CleverTap-Passcode: <PassCode>' \\\n--header 'Content-Type: application/json' \\\n--header 'Content-Type: text/plain' \\\n--data-raw '{\n  \"name\": \"darkMode\",\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\n  \"keys\": {\n    \"dark\": [\n      {\n        \"segmentName\": \"New York User\",\n        \"segmentValue\": true\n      }\n    ]\n  },\n  \"segmentsPriorityList\": [\n    {\n      \"segmentName\": \"New York User\",\n      \"percentageRollout\": 80\n    }\n  ]\n}'",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/featureflag/update?experiencename=darkMode\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"<ACCOUNT_ID>\"\nrequest[\"X-CleverTap-Passcode\"] = \"<PassCode>\"\nrequest[\"Content-Type\"] = [\"application/json\", \"text/plain\"]\nrequest.body = \"{\\n  \\\"name\\\": \\\"darkMode\\\",\\n  \\\"description\\\": \\\"enables/disables dark mode based on profile custom properties\\\",\\n  \\\"keys\\\": {\\n    \\\"dark\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New York User\\\",\\n        \\\"segmentValue\\\": true\\n      }\\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New York User\\\",\\n      \\\"percentageRollout\\\": 80\\n    }\\n  ]\\n}\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/featureflag/update?experiencename=darkMode\"\n\npayload = \"{\\n  \\\"name\\\": \\\"darkMode\\\",\\n  \\\"description\\\": \\\"enables/disables dark mode based on profile custom properties\\\",\\n  \\\"keys\\\": {\\n    \\\"dark\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New York User\\\",\\n        \\\"segmentValue\\\": true\\n      }\\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New York User\\\",\\n      \\\"percentageRollout\\\": 80\\n    }\\n  ]\\n}\"\nheaders = {\n  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode': '<PassCode>',\n  'Content-Type': 'application/json',\n  'Content-Type': 'text/plain'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/featureflag/update?experiencename=darkMode');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode' => '<PassCode>',\n  'Content-Type' => 'application/json',\n  'Content-Type' => 'text/plain'\n));\n$request->setBody('{\\n  \"name\": \"darkMode\",\\n  \"description\": \"enables/disables dark mode based on profile custom properties\",\\n  \"keys\": {\\n    \"dark\": [\\n      {\\n        \"segmentName\": \"New York User\",\\n        \"segmentValue\": true\\n      }\\n    ]\\n  },\\n  \"segmentsPriorityList\": [\\n    {\\n      \"segmentName\": \"New York User\",\\n      \"percentageRollout\": 80\\n    }\\n  ]\\n}');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/featureflag/update?experiencename=darkMode',\n  'headers': {\n    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n    'X-CleverTap-Passcode': '<PassCode>',\n    'Content-Type': ['application/json', 'text/plain']\n  },\n  body: \"{\\n  \\\"name\\\": \\\"darkMode\\\",\\n  \\\"description\\\": \\\"enables/disables dark mode based on profile custom properties\\\",\\n  \\\"keys\\\": {\\n    \\\"dark\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New York User\\\",\\n        \\\"segmentValue\\\": true\\n      }\\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New York User\\\",\\n      \\\"percentageRollout\\\": 80\\n    }\\n  ]\\n}\"\n\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/featureflag/update?experiencename=darkMode\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"{\\n  \\\"name\\\": \\\"darkMode\\\",\\n  \\\"description\\\": \\\"enables/disables dark mode based on profile custom properties\\\",\\n  \\\"keys\\\": {\\n    \\\"dark\\\": [\\n      {\\n        \\\"segmentName\\\": \\\"New York User\\\",\\n        \\\"segmentValue\\\": true\\n      }\\n    ]\\n  },\\n  \\\"segmentsPriorityList\\\": [\\n    {\\n      \\\"segmentName\\\": \\\"New York User\\\",\\n      \\\"percentageRollout\\\": 80\\n    }\\n  ]\\n}\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"<ACCOUNT_ID>\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"<PassCode>\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n  req.Header.Add(\"Content-Type\", \"text/plain\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
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
# Get Feature Flag
Getting Feature Flag requires a GET request with a JSON payload specifying the name and mapping.  There is no limit on the number of requests to the API, but the batch size for each request must be up to 1000. You can specify the **optional** "experiencename" parameter to get information about a particular Feature Flag object and also specify the **optional** "version" parameter to get information about a particular version of a Feature Flag .

## Base URL
https://api.clevertap.com/1/featureflag/get?experiencename=<featureFlagName>&version=<versionNumber>

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
      "code": "curl --location --request POST 'https://api.clevertap.com/1/featureflag/get' \\\n--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \\\n--header 'X-CleverTap-Passcode: <PASSCODE>' \\\n--header 'Content-Type: application/json' \\\n--data-raw ''",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/featureflag/get\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"<ACCOUNT_ID>\"\nrequest[\"X-CleverTap-Passcode\"] = \"<PASSCODE>\"\nrequest[\"Content-Type\"] = \"application/json\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/featureflag/get\"\n\npayload  = {}\nheaders = {\n  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode': '<PASSCODE>',\n  'Content-Type': 'application/json'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/featureflag/get');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode' => '<PASSCODE>',\n  'Content-Type' => 'application/json'\n));\n$request->setBody('');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/featureflag/get',\n  'headers': {\n    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n    'X-CleverTap-Passcode': '<PASSCODE>',\n    'Content-Type': 'application/json'\n  }\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/featureflag/get\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"<ACCOUNT_ID>\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"<PASSCODE>\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
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
      "code": "{\n    \"output\": [\n        {\n            \"name\": \"wishlist button\",\n            \"description\": \"to serve wishlist button to x users\",\n            \"segmentsPriorityList\": [\n                {\n                    \"segmentName\": \"p1did\",\n                    \"percentageRollout\": 60\n                }\n            ],\n            \"keys\": {\n                \"wishlist button\": [\n                    {\n                        \"segmentName\": \"p1did\",\n                        \"segmentValue\": \"true\"\n                    }\n                ]\n            },\n            \"keyIds\": [\n                1591242116\n            ],\n            \"start_time\": 1591242252,\n            \"end_time\": 2147483647,\n            \"status\": \"active\",\n            \"version\": {\n                \"update_mode\": \"create\",\n                \"created_by\": \"foo@bar.com\",\n                \"created_on\": 1591242252,\n                \"updated_by\": \"foo@bar.com\",\n                \"last_updated\": 1591242252,\n                \"versionNumber\": 1\n            },\n            \"id\": 1591242252\n        },\n        {\n            \"name\": \"Testing Flags\",\n            \"description\": \"Testing\",\n            \"segmentsPriorityList\": [\n                {\n                    \"segmentName\": \"p1did\",\n                    \"percentageRollout\": 5\n                },\n                {\n                    \"segmentName\": \"Test segment\",\n                    \"percentageRollout\": 20\n                },\n                {\n                    \"segmentName\": \"p1didnot\",\n                    \"percentageRollout\": 100\n                }\n            ],\n            \"keys\": {\n                \"Feature Flag\": [\n                    {\n                        \"segmentName\": \"p1did\",\n                        \"segmentValue\": \"true\"\n                    },\n                    {\n                        \"segmentName\": \"Test segment\",\n                        \"segmentValue\": \"true\"\n                    },\n                    {\n                        \"segmentName\": \"p1didnot\",\n                        \"segmentValue\": \"false\"\n                    }\n                ]\n            },\n            \"keyIds\": [\n                1588912793\n            ],\n            \"start_time\": 1588912841,\n            \"end_time\": 2147483647,\n            \"status\": \"active\",\n            \"version\": {\n                \"update_mode\": \"create\",\n                \"created_by\": \"foo@bar.com\",\n                \"created_on\": 1588912841,\n                \"updated_by\": \"foo@bar.com\",\n                \"last_updated\": 1588912841,\n                \"versionNumber\": 1,\n                \"status_change_by\": \"foo@bar.com\",\n                \"status_change_ts\": 1590093859\n            },\n            \"id\": 1588912841\n        }\n    ],\n    \"status\": \"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]
#Archive a feature flag
You can archive a particular feature flag if you wish by using this API and mentioning the **mandatory** "experiencename" in the URL parameter. 

## Base URL
https://api.clevertap.com/1/featureflag/archive?experiencename=<featureFlagName>

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
For example, you want to archive the featureFlag you created above you can use the following examples

## Example Request 
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request POST 'https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode' \\\n--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \\\n--header 'X-CleverTap-Passcode: <PASSCODE>' \\\n--header 'Content-Type: application/json' \\\n--data-raw ''",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"<ACCOUNT_ID>\"\nrequest[\"X-CleverTap-Passcode\"] = \"<PASSCODE>\"\nrequest[\"Content-Type\"] = \"application/json\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode\"\n\npayload  = {}\nheaders = {\n  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode': '<PASSCODE>',\n  'Content-Type': 'application/json'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode' => '<PASSCODE>',\n  'Content-Type' => 'application/json'\n));\n$request->setBody('');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode',\n  'headers': {\n    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n    'X-CleverTap-Passcode': '<PASSCODE>',\n    'Content-Type': 'application/json'\n  }\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/featureflag/archive?experiencename=darkMode\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"<ACCOUNT_ID>\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"<PASSCODE>\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
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
#Activate
You can use this API to either activate an archived Feature Flag or activate a particular version of an already active Feature Flag. If you want to just activate an archived Feature Flag you do not need to pass the **version** in the URL. 

## Base URL
https://api.clevertap.com/1/featureflag/activate?experienceName=<featureFlag>&version=<versionNumber>

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
For example, if you want to activate a Feature Flag that you archived above you can use the following examples

## Example Request 
[block:code]
{
  "codes": [
    {
      "code": "curl --location --request POST 'https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode' \\\n--header 'X-CleverTap-Account-Id: <ACCOUNT_ID>' \\\n--header 'X-CleverTap-Passcode: <PASSCODE>' \\\n--header 'Content-Type: application/json' \\\n--data-raw ''",
      "language": "curl"
    },
    {
      "code": "require \"uri\"\nrequire \"net/http\"\n\nurl = URI(\"https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode\")\n\nhttps = Net::HTTP.new(url.host, url.port);\nhttps.use_ssl = true\n\nrequest = Net::HTTP::Post.new(url)\nrequest[\"X-CleverTap-Account-Id\"] = \"<ACCOUNT_ID>\"\nrequest[\"X-CleverTap-Passcode\"] = \"<PASSCODE>\"\nrequest[\"Content-Type\"] = \"application/json\"\n\nresponse = https.request(request)\nputs response.read_body\n",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nurl = \"https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode\"\n\npayload  = {}\nheaders = {\n  'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode': '<PASSCODE>',\n  'Content-Type': 'application/json'\n}\n\nresponse = requests.request(\"POST\", url, headers=headers, data = payload)\n\nprint(response.text.encode('utf8'))\n",
      "language": "python"
    },
    {
      "code": "<?php\nrequire_once 'HTTP/Request2.php';\n$request = new HTTP_Request2();\n$request->setUrl('https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode');\n$request->setMethod(HTTP_Request2::METHOD_POST);\n$request->setConfig(array(\n  'follow_redirects' => TRUE\n));\n$request->setHeader(array(\n  'X-CleverTap-Account-Id' => '<ACCOUNT_ID>',\n  'X-CleverTap-Passcode' => '<PASSCODE>',\n  'Content-Type' => 'application/json'\n));\n$request->setBody('');\ntry {\n  $response = $request->send();\n  if ($response->getStatus() == 200) {\n    echo $response->getBody();\n  }\n  else {\n    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .\n    $response->getReasonPhrase();\n  }\n}\ncatch(HTTP_Request2_Exception $e) {\n  echo 'Error: ' . $e->getMessage();\n}",
      "language": "php"
    },
    {
      "code": "var request = require('request');\nvar options = {\n  'method': 'POST',\n  'url': 'https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode',\n  'headers': {\n    'X-CleverTap-Account-Id': '<ACCOUNT_ID>',\n    'X-CleverTap-Passcode': '<PASSCODE>',\n    'Content-Type': 'application/json'\n  }\n};\nrequest(options, function (error, response) { \n  if (error) throw new Error(error);\n  console.log(response.body);\n});\n",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "package main\n\nimport (\n  \"fmt\"\n  \"strings\"\n  \"net/http\"\n  \"io/ioutil\"\n)\n\nfunc main() {\n\n  url := \"https://api.clevertap.com/1/featureflag/activate?experiencename=darkMode\"\n  method := \"POST\"\n\n  payload := strings.NewReader(\"\")\n\n  client := &http.Client {\n  }\n  req, err := http.NewRequest(method, url, payload)\n\n  if err != nil {\n    fmt.Println(err)\n  }\n  req.Header.Add(\"X-CleverTap-Account-Id\", \"<ACCOUNT_ID>\")\n  req.Header.Add(\"X-CleverTap-Passcode\", \"<PASSCODE>\")\n  req.Header.Add(\"Content-Type\", \"application/json\")\n\n  res, err := client.Do(req)\n  defer res.Body.Close()\n  body, err := ioutil.ReadAll(res.Body)\n\n  fmt.Println(string(body))\n}",
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