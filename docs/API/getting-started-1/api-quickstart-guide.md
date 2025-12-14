---
title: API Quick Start Guide
excerpt: Understand how to perform CleverTap API requests.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
We provide a REST API that lets you create, read, update, and delete data in CleverTap.

This guide shows you how to authenticate with the CleverTap API, create a CleverTap user profile, and quickly get a CleverTap user profile.

# Get CleverTap Account Credentials to Authenticate API Requests

CleverTap uses a header-based authentication model to authenticate requests to the API. Every CleverTap API call should include your *Project ID (Account ID)* and *Account Passcode* as the request headers.

Perform the following steps to get CleverTap Account credentials:

1. Log into your [CleverTap Account](https://eu1.dashboard.clevertap.com/login.html) and click *Settings* on the dashboard.

<Image alt="CleverTap Dashboard Settings" align="center" border={true} src="https://files.readme.io/6a02f58d6af68e0262192666230518a916a453075802233bd66b4c2635acef47-image.png">
  CleverTap Dashboard Settings
</Image>

2. Click *Project* . Save the values of *Project ID* and *Passcode*. We will need this information in the next two steps.

<Image alt="View Project Credentials" align="center" border={true} src="https://files.readme.io/d9769fd0761163ea70b573668fb4af1dce6bfdc5e9f8ae84bf4361efe6810bf4-image.png">
  View Project Credentials
</Image>

> 📘 Create Account Passcode
>
> You can also create an account passcode from the *Projects* page by clicking the **+Passcode** link. On clicking, you are navigated to the *Passcodes* page. For more information about the steps involved, refer to [Create Account Passcode](doc:authentication_multiple-passcode#create-account-passcode).

# Create a CleverTap User Profile with the API

After you integrate our SDK, we will create a user profile for each person who launches your app or visits your website. You can also create or update CleverTap user profiles with our API. 

A CleverTap user profile has a set of default fields, such as email, phone number, and language.

<Image alt="CleverTap User Profile" align="center" border={true} src="https://files.readme.io/1fe744537cd16ea4c3c517b9b3c13daf9181231abc2bc0c917ce8ac436acfce0-image.png">
  CleverTap User Profile
</Image>

You can also extend the default user profile by adding custom fields that are specific to your business. For example, if you offer a subscription service in your app, you can create a custom profile field to track what type of plan the user purchased.

<Image alt="User Profile Properties" align="center" width="50% " border={true} src="https://files.readme.io/4140a932b3e8a0164270677316d4e39b5273c8ba7a3400dfcbd5e24b6e259aa8-image.png">
  User Profile Properties
</Image>

To create a new user profile in CleverTap, make a request to the [Upload User Profile API](doc:upload-user-profiles-api).

For example, we can create a user profile with the email [*george.montana@clevertap.com*](mailto:_george.montana@clevertap.com_), the name *George Montana*, and the customer type *Silver* using the following steps:

1. Run the cURL request on your terminal. Here is an example cURL request for the accounts in the India region:

```curl
curl -X POST -d '{"d":[{"identity":"george.montana@clevertap.com","type":"profile","profileData":{"Name": "George Montana", "Customer Type": "Silver"}}]}' "https://in1.api.clevertap.com/1/upload" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://api.clevertap.com/1/upload")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json; charset=utf-8"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "d" => [
    {
      "identity" => "george.montana@clevertap.com",
      "type" => "profile",
      "profileData" => {
        "Name" => "George Montana",
        "Customer Type" => "Silver"
      }
    }
  ]
})

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```
```python
import requests

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json; charset=utf-8',
}

data = '{"d":[{"identity":"george.montana@clevertap.com","type":"profile","profileData":{"Name": "George Montana", "Customer Type": "Silver"}}]}'

response = requests.post('https://api.clevertap.com/1/upload', headers=headers, data=data)
```
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
    'X-CleverTap-Passcode' => 'PASSCODE',
    'Content-Type' => 'application/json; charset=utf-8'
);
$data = '{"d":[{"identity":"george.montana@clevertap.com","type":"profile","profileData":{"Name": "George Montana", "Customer Type": "Silver"}}]}';
$response = Requests::post('https://api.clevertap.com/1/upload', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json; charset=utf-8'
};

var dataString = '{"d":[{"identity":"george.montana@clevertap.com","type":"profile","profileData":{"Name": "George Montana", "Customer Type": "Silver"}}]}';

var options = {
    url: 'https://api.clevertap.com/1/upload',
    method: 'POST',
    headers: headers,
    body: dataString
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```
```go
type Payload struct {
	D []struct {
		Identity    string `json:"identity"`
		Type        string `json:"type"`
		ProfileData struct {
			Name         string `json:"Name"`
			CustomerType string `json:"Customer Type"`
		} `json:"profileData"`
	} `json:"d"`
}

data := Payload{
// fill struct
}
payloadBytes, err := json.Marshal(data)
if err != nil {
	// handle err
}
body := bytes.NewReader(payloadBytes)

req, err := http.NewRequest("POST", "https://api.clevertap.com/1/upload", body)
if err != nil {
	// handle err
}
req.Header.Set("X-Clevertap-Account-Id", "ACCOUNT_ID")
req.Header.Set("X-Clevertap-Passcode", "PASSCODE")
req.Header.Set("Content-Type", "application/json; charset=utf-8")

resp, err := http.DefaultClient.Do(req)
if err != nil {
	// handle err
}
defer resp.Body.Close()
```

To know the API endpoint based on the region of your account, refer to [Region](doc:common-api-components#region).

> 📘 Python Version
>
> The recommended version for Python is 2.7 as we do not support the latest version at this time.

2. Replace the values for the headers `X-CleverTap-Account-Id` and `X-CleverTap-Passcode` with your CleverTap account credentials obtained from the second step in [Get CleverTap Account Credentials to Authenticate API Requests](doc:api-quickstart-guide#section-get-clever-tap-account-credentials-to-authenticate-api-requests). 
3. If the API request is successful, you will see the below response: If the request is not successful, you will see the [errors](doc:api-errors) in the response.

```json
{
    "status": "success",
    "processed": 1,
    "unprocessed": []
}
```

# Get a CleverTap User Profile with the API

You can get a CleverTap user profile with the API. In this example, we will get the CleverTap user profile for the one we created in the previous section.

Make a request to the [Get User Profile API](doc:get-user-profiles-api) for the user identified by the email *george.montana\@clevertap* using the below steps:

1. Run the cURL request on your terminal. Here is an example cURL request for the accounts in the India region:

```curl
curl "https://in1.api.clevertap.com/1/profile.json?email=george.montana@clevertap.com" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://api.clevertap.com/1/profile.json?email=george.montana@clevertap.com")
request = Net::HTTP::Get.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"

req_options = {
  use_ssl: uri.scheme == "https",
}

response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
```
```python
import requests

headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json',
}

params = (
    ('email', 'george.montana@clevertap.com'),
)

response = requests.get('https://api.clevertap.com/1/profile.json', headers=headers, params=params)
```
```php
<?php
include('vendor/rmccue/requests/library/Requests.php');
Requests::register_autoloader();
$headers = array(
    'X-CleverTap-Account-Id' => 'ACCOUNT_ID',
    'X-CleverTap-Passcode' => 'PASSCODE',
    'Content-Type' => 'application/json'
);
$response = Requests::get('https://api.clevertap.com/1/profile.json?email=george.montana@clevertap.com', $headers);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var options = {
    url: 'https://api.clevertap.com/1/profile.json?email=george.montana@clevertap.com',
    headers: headers
};

function callback(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body);
    }
}

request(options, callback);
```
```go
req, err := http.NewRequest("GET", "https://api.clevertap.com/1/profile.json?email=george.montana@clevertap.com", nil)
if err != nil {
	// handle err
}
req.Header.Set("X-Clevertap-Account-Id", "ACCOUNT_ID")
req.Header.Set("X-Clevertap-Passcode", "PASSCODE")
req.Header.Set("Content-Type", "application/json")

resp, err := http.DefaultClient.Do(req)
if err != nil {
	// handle err
}
defer resp.Body.Close()
```

To know the API endpoint based on the region of your account, refer to [Region](doc:common-api-components#region).

2. Replace the values for the headers `X-CleverTap-Account-Id` and `X-CleverTap-Passcode` with your CleverTap account credentials obtained from the second step in [Get CleverTap Account Credentials to Authenticate API Requests](doc:api-quickstart-guide#section-get-clever-tap-account-credentials-to-authenticate-api-requests).
3. If the API request is successful, you will see the below response: If the request is not successful, you will see the errors in the response.

```json
{
  "record": {
    "platformInfo": [
      {
        "df": {
         
        },
        "objectId": "Aa1bb2-b0jaa-z8QXgAM9w",
        "platform": "Web"
      }
    ],
    "identity": "george.montana@clevertap.com",
    "name": "George Montana",
    "email": "george.montana@clevertap.com",
    "all_identities": [
      "george.montana@clevertap.com"
    ],
    "profileData": {
      "customertype": "Silver"
    }
  },
  "status": "success"
}
```

# Next Step

Congratulations! on making your first requests with the CleverTap API.

Refer to [API Reference](doc:api-reference) to learn more about all the CleverTap APIs.
