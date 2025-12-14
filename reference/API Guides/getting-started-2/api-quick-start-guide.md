---
title: API Quick Start Guide
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
We provide a REST API that lets you create, read, update, and delete data in CleverTap.

This guide shows you how to authenticate with the CleverTap API, create a CleverTap user profile, and get a CleverTap user profile in less than ten minutes.

# Get CleverTap Account Credentials to Authenticate API Requests 

CleverTap uses a header based authentication model to authenticate requests to the API. Every CleverTap API call should include both your *Project ID* and *Account Passcode* as the request headers.

Your CleverTap *Project ID* and *Account Passcode* are available on the *Settings* page. 

1. To navigate to the *Settings* page, log into [your CleverTap account](https://eu1.dashboard.clevertap.com/login.html), click on the gear icon on the bottom left navigation, and select *Settings* dashboard.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/caac571-12029.png",
        "1,20,29.png",
        1088,
        840,
        "#efeff0"
      ],
      "border": true
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8b12f15-22730.png",
        "2,27,30.png",
        1449,
        1419,
        "#f9f9fb"
      ],
      "border": true
    }
  ]
}
[/block]
2. Save the values of *Project ID* and *Passcode*. We will need this information in the next two steps.

# Create a CleverTap User Profile with the API

After you integrate our SDK, we will create a user profile for each person who launches your app or visits your website. You can also create or update CleverTap user profiles with our API. 

A CleverTap user profile has a set of default fields, such as email, phone number, and language.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b6c6f5d-CleverTap_User_Profile.png",
        "CleverTap User Profile.png",
        2721,
        1644,
        "#f9f9fa"
      ]
    }
  ]
}
[/block]
You can also extend the default user profile by adding custom fields that are specific to your business. For example, if you offer a subscription service in your app, you can create a custom profile field to track what type of plan the user purchased.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/50e5071-832.png",
        "8,32.png",
        1298,
        770,
        "#fafafa"
      ]
    }
  ]
}
[/block]
To create a new user profile in CleverTap, make a request to the [user profile API endpoint](doc:upload-user-profiles-api).

For example, we can create a user profile with the email *george.montana@clevertap.com*, the name *George Montana*, and the customer type *Silver*. 

1. Run the cURL request below in your terminal. 
2. Replace the values for the headers X-CleverTap-Account-Id and X-CleverTap-Passcode with your CleverTap account credentials from the top section above. Refer to [Get CleverTap Account Credentials to Authenticate API Requests](https://developer.clevertap.com/docs/api-quickstart-guide#section-get-clever-tap-account-credentials-to-authenticate-api-requests).

*Note*: The recommended version for Python is 2.7 as we do not support the latest version at this time.

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Use the URL based on your region:\n  * India - `in1.api.clevertap.com`\n  * Singapore - `sg1.api.clevertap.com`\n  * U.S - `us1.api.clevertap.com`\n  * Indonesia - `aps3.api.clevertap.com`\n  * Middle East (UAE) - `mec1.api.clevertap.com`\n  * Europe (default region) - `api.clevertap.com`"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"d\":[{\"identity\":\"george.montana@clevertap.com\",\"type\":\"profile\",\"profileData\":{\"Name\": \"George Montana\", \"Customer Type\": \"Silver\"}}]}' \"https://location.api.clevertap.com/1/upload\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json; charset=utf-8\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nrequire 'json'\n\nuri = URI.parse(\"https://api.clevertap.com/1/upload\")\nrequest = Net::HTTP::Post.new(uri)\nrequest.content_type = \"application/json; charset=utf-8\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\nrequest.body = JSON.dump({\n  \"d\" => [\n    {\n      \"identity\" => \"george.montana@clevertap.com\",\n      \"type\" => \"profile\",\n      \"profileData\" => {\n        \"Name\" => \"George Montana\",\n        \"Customer Type\" => \"Silver\"\n      }\n    }\n  ]\n})\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json; charset=utf-8',\n}\n\ndata = '{\"d\":[{\"identity\":\"george.montana@clevertap.com\",\"type\":\"profile\",\"profileData\":{\"Name\": \"George Montana\", \"Customer Type\": \"Silver\"}}]}'\n\nresponse = requests.post('https://api.clevertap.com/1/upload', headers=headers, data=data)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json; charset=utf-8'\n);\n$data = '{\"d\":[{\"identity\":\"george.montana@clevertap.com\",\"type\":\"profile\",\"profileData\":{\"Name\": \"George Montana\", \"Customer Type\": \"Silver\"}}]}';\n$response = Requests::post('https://api.clevertap.com/1/upload', $headers, $data);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json; charset=utf-8'\n};\n\nvar dataString = '{\"d\":[{\"identity\":\"george.montana@clevertap.com\",\"type\":\"profile\",\"profileData\":{\"Name\": \"George Montana\", \"Customer Type\": \"Silver\"}}]}';\n\nvar options = {\n    url: 'https://api.clevertap.com/1/upload',\n    method: 'POST',\n    headers: headers,\n    body: dataString\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "type Payload struct {\n\tD []struct {\n\t\tIdentity    string `json:\"identity\"`\n\t\tType        string `json:\"type\"`\n\t\tProfileData struct {\n\t\t\tName         string `json:\"Name\"`\n\t\t\tCustomerType string `json:\"Customer Type\"`\n\t\t} `json:\"profileData\"`\n\t} `json:\"d\"`\n}\n\ndata := Payload{\n// fill struct\n}\npayloadBytes, err := json.Marshal(data)\nif err != nil {\n\t// handle err\n}\nbody := bytes.NewReader(payloadBytes)\n\nreq, err := http.NewRequest(\"POST\", \"https://api.clevertap.com/1/upload\", body)\nif err != nil {\n\t// handle err\n}\nreq.Header.Set(\"X-Clevertap-Account-Id\", \"ACCOUNT_ID\")\nreq.Header.Set(\"X-Clevertap-Passcode\", \"PASSCODE\")\nreq.Header.Set(\"Content-Type\", \"application/json; charset=utf-8\")\n\nresp, err := http.DefaultClient.Do(req)\nif err != nil {\n\t// handle err\n}\ndefer resp.Body.Close()",
      "language": "go"
    }
  ]
}
[/block]
If the request is successful, you will see the response below. If the request is not successful, you will see the errors in the response.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"success\",\n    \"processed\": 1,\n    \"unprocessed\": []\n}",
      "language": "json"
    }
  ]
}
[/block]
# Get a CleverTap User Profile with the API

You can get a CleverTap user profile with the API. In this example, we will get the CleverTap user profile for the one we created in the previous section.

We will make a request to the [user profile API endpoint](doc:get-user-profiles-api) for the user identified by the email *george.montana@clevertap*.

1. Run the cURL request below in your terminal.
2. Replace the values for the headers X-CleverTap-Account-Id and X-CleverTap-Passcode with your CleverTap account credentials from the top section above. Refer to [Get CleverTap Account Credentials to Authenticate API Requests](https://developer.clevertap.com/docs/api-quickstart-guide#section-get-clever-tap-account-credentials-to-authenticate-api-requests).

*Note*: The recommended version for Python is 2.7 as we do not support the latest version at this time.
[block:code]
{
  "codes": [
    {
      "code": "curl \"https://location.api.clevertap.com/1/profile.json?email=george.montana@clevertap.com\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\n\nuri = URI.parse(\"https://api.clevertap.com/1/profile.json?email=george.montana@clevertap.com\")\nrequest = Net::HTTP::Get.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"PASSCODE\"\n\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\n\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend",
      "language": "ruby"
    },
    {
      "code": "import requests\n\nheaders = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json',\n}\n\nparams = (\n    ('email', 'george.montana@clevertap.com'),\n)\n\nresponse = requests.get('https://api.clevertap.com/1/profile.json', headers=headers, params=params)",
      "language": "python"
    },
    {
      "code": "<?php\ninclude('vendor/rmccue/requests/library/Requests.php');\nRequests::register_autoloader();\n$headers = array(\n    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',\n    'X-CleverTap-Passcode' => 'PASSCODE',\n    'Content-Type' => 'application/json'\n);\n$response = Requests::get('https://api.clevertap.com/1/profile.json?email=george.montana@clevertap.com', $headers);",
      "language": "php"
    },
    {
      "code": "var request = require('request');\n\nvar headers = {\n    'X-CleverTap-Account-Id': 'ACCOUNT_ID',\n    'X-CleverTap-Passcode': 'PASSCODE',\n    'Content-Type': 'application/json'\n};\n\nvar options = {\n    url: 'https://api.clevertap.com/1/profile.json?email=george.montana@clevertap.com',\n    headers: headers\n};\n\nfunction callback(error, response, body) {\n    if (!error && response.statusCode == 200) {\n        console.log(body);\n    }\n}\n\nrequest(options, callback);",
      "language": "javascript",
      "name": "Node.js"
    },
    {
      "code": "req, err := http.NewRequest(\"GET\", \"https://api.clevertap.com/1/profile.json?email=george.montana@clevertap.com\", nil)\nif err != nil {\n\t// handle err\n}\nreq.Header.Set(\"X-Clevertap-Account-Id\", \"ACCOUNT_ID\")\nreq.Header.Set(\"X-Clevertap-Passcode\", \"PASSCODE\")\nreq.Header.Set(\"Content-Type\", \"application/json\")\n\nresp, err := http.DefaultClient.Do(req)\nif err != nil {\n\t// handle err\n}\ndefer resp.Body.Close()",
      "language": "go"
    }
  ]
}
[/block]
If the request is successful, you will see the response below. If the request is not successful, you will see the errors in the response.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"record\": {\n    \"platformInfo\": [\n      {\n        \"df\": {\n         \n        },\n        \"objectId\": \"Aa1bb2-b0jaa-z8QXgAM9w\",\n        \"platform\": \"Web\"\n      }\n    ],\n    \"identity\": \"george.montana@clevertap.com\",\n    \"name\": \"George Montana\",\n    \"email\": \"george.montana@clevertap.com\",\n    \"all_identities\": [\n      \"george.montana@clevertap.com\"\n    ],\n    \"profileData\": {\n      \"customertype\": \"Silver\"\n    }\n  },\n  \"status\": \"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]
# Next Steps

Congrats on making your first requests with the CleverTap API.

You learned how to authenticate with the CleverTap API, create a CleverTap user profile, and get a CleverTap user profile.

Check out the [API Overview](doc:api-overview) page to learn about all the different API endpoints we offer.