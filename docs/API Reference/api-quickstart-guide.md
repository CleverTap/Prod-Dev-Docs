---
title: API Quickstart Guide
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

This guide will show you how to authenticate with the CleverTap API, create a CleverTap user profile, and get a CleverTap user profile in less than ten minutes.

# Step 1: Get CleverTap Account Credentials to Authenticate API Requests

CleverTap uses a header based authentication model to authenticate requests to the API. Every CleverTap API call should include both your Account ID and Account Passcode as the request headers.

Your CleverTap Account ID and Account Passcode are available on the Settings page. To navigate to the Settings page, log into [your CleverTap account](https://eu1.dashboard.clevertap.com/login.html), click on the gear icon on the top right navigation, and select Settings dashboard.

<Image title="image2.png" alt={489} className="border" border={true} src="https://files.readme.io/661a84e-image2.png" />

<Image title="image1.png" alt={769} className="border" border={true} src="https://files.readme.io/4301b9d-image1.png" />

Save the values of Account ID and Passcode. We'll need this information in the next two steps.

# Step 2: Create a CleverTap User Profile with the API

After you integrate our SDK, we will create a user profile for each person who launches your app or visits your website. You can also create or update CleverTap user profiles with our API. 

A CleverTap user profile has a set of default fields, such as email, phone number, and language.

![1408](https://files.readme.io/926a16f-user1.png "user1.png")

You can also extend the default user profile by adding custom fields that are specific to your business. For example, if you offer a subscription service in your app, you can create a custom profile field to track what type of plan the user purchased.

<Image title="user2.png" alt={577} className="border" border={true} src="https://files.readme.io/b4cfc0a-user2.png" />

To create a new user profile in CleverTap, we will make a request to the [Upload User Profiles API endpoint](doc:upload-user-profiles-api).

Let's create a user profile with the email "[george.montana@clevertap.com](mailto:george.montana@clevertap.com)", the name "George Montana", and the customer type "Silver". 

Run the cURL request below in your terminal. Replace the values for the headers X-CleverTap-Account-Id and X-CleverTap-Passcode with your CleverTap account credentials from step 1. 

```curl
curl -X POST -d '{"d":[{"identity":"george.montana@clevertap.com","type":"profile","profileData":{"Name": "George Montana", "Customer Type": "Silver"}}]}' "https://api.clevertap.com/1/upload" \
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

puts response.body
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

If the request is successful, you will see the response below. If the request is not successful, you will see the errors in the response.

```json
{
    "status": "success",
    "processed": 1,
    "unprocessed": []
}
```

# Step 3: Get a CleverTap User Profile with the API

You can get a CleverTap user profile with the API. Let's get the CleverTap user profile for the one we created in step 2.

We will make a request to the [Get User Profiles API endpoint](doc:get-user-profiles-api) for the user identified by the email "george.montana\@clevertap".

Run the cURL request below in your terminal. Replace the values for the headers X-CleverTap-Account-Id and X-CleverTap-Passcode with your CleverTap account credentials from step 1. 

```curl
curl "https://api.clevertap.com/1/profile.json?email=george.montana@clevertap.com" \
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

puts response.body
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

If you the request is successful, you will see the response below. If the request is not successful, you will see the errors in the response.

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

# Next Steps

Congrats on making your first requests with the CleverTap API!

You learned how to authenticate with the CleverTap API, create a CleverTap user profile, and get a CleverTap user profile.

Check out the [API Overview page](doc:api-overview) to learn about all the different API endpoints we offer.
