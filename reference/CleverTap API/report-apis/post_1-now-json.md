---
title: Real-Time Counts
excerpt: https://developer.clevertap.com/docs/real-time-counts-api#section-overview
api:
  file: clevertap-api-24.json
  operationId: post_1-now-json
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
Use this API to get a real-time count of active users in the past five minutes in your app. The results can be spliced by `user_type`, `session_source`, browser, os, and device for active and new users.


# Body Parameters
The body is uploaded as a JSON payload. An empty payload {} will return a real-time count of active users.
[block:parameters]
{
  "data": {
    "0-0": "user_type",
    "h-0": "Parameter",
    "h-1": "Description",
    "0-1": "If set to true, will return the split of users by type.",
    "0-2": "optional",
    "h-2": "Required",
    "h-3": "Type",
    "h-4": "Example Value",
    "0-3": "boolean",
    "0-4": "true"
  },
  "cols": 5,
  "rows": 1
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"user_type\": true\n}\n",
      "language": "json"
    }
  ]
}
[/block]
# Sample Request
Here is an example cURL request to the *Real-Time Counts* API showing the headers needed to authenticate the request from the account in the India region:
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"user_type\":true\"}' \"https://in1.api.clevertap.com/1/now.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nrequire 'json'\n\nuri = URI.parse(\"https://in1.api.clevertap.com/1/now.json\")\nrequest = Net::HTTP::Post.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\nrequest.body = JSON.dump({\n  \"user_type\" => true\n  \n})\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby",
      "name": null
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json',\n}\n\ndata = '{\"user_type\":true}'\n\nresponse = requests.post('https://in1.api.clevertap.com/1/now.json', headers=headers, data=data)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json'\n);\n$data = '{\"user_type\":true}';\n$response = Requests::post('https://in1.api.clevertap.com/1/now.json', $headers, $data);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n};\n\nvar dataString = '{\"user_type\":true}';\n\nvar options = {\n    url: 'https://in1.api.clevertap.com/1/now.json',\n    method: 'POST',\n    headers: headers,\n    body: dataString\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    }
  ]
}
[/block]
# Sample Response
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\",\n  \"count\": 214\n}",
      "language": "json"
    }
  ]
}
[/block]
Here is an example response if you set user_type parameter to true.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\",\n  \"count\": 214,\n  \"user_type\": {\n    \"new\": 58\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]
# Additional Information
* The response will be a JSON object containing either the success or failure status. If no payload was provided and the request was successful, the response will include a count of active users in real time.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\",\n  \"count\": 214\n}",
      "language": "json"
    }
  ]
}
[/block]
* For more information on rate limit, refer to [API Rate Limit](doc:api-rate-limit).
* For more information on Region, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).
* For more information on API Headers, refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers)