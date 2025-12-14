---
title: Get Message Reports
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The Get Message Reports API lets you download a list of messages sent by CleverTap. 

For example, you can this API to get a report of how many in-app messages and push notifications were sent to users in the past week. 

# Overview

To get a message report, you submit a request with the date range needed. You can also filter these results by setting optional parameters that will return only specific channels or message statuses.

## Base URL

Here is an example base URL from the account in the India region:  
<https://in1.api.clevertap.com/1/message/report.json>

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload. from & to are required parameters. All the optional parameters, except daily, let input multiple values by separating them with commas.

| Parameter    | Description                                                                                                                                                                                                                                                                                                                                                         | Type             | Example Value           |
| :----------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :--------------- | :---------------------- |
| from         | Start of date range for events needed. Value specified in format YYYYMMDD. Required.                                                                                                                                                                                                                                                                                | string           | “20171201”              |
| to           | End of date range for events needed. Value specified in format YYYYMMDD. Required.                                                                                                                                                                                                                                                                                  | string           | “20171225”              |
| channel      | The channels of communication needed. Supported values include push (mobile push messages), email, sms, browser (browser push messages), audiences (Facebook messages), inapp (in-app messages), webhooks (webhooks), web_pop_up (Web pop-ups), web_exit_intent (web exit intent pop-ups), web_native_display (Web native display), web_inbox (Web inbox).Optional. | array of strings | [“push”,”email”]        |
| delivery     | Delivery type of communications for which you want a report. Supported input values are one_time, inaction, action, recurring, property_time, and api. Optional.                                                                                                                                                                                                    | array of strings | [“inaction”,”action”]   |
| daily        | Determines if the report should be a day-wise split, or an aggregate report. Supported values are true or false. Unless specified otherwise, the value of daily is set to false. Optional.                                                                                                                                                                          | boolean          | false                   |
| status       | Determines the status of messages for which you want a report. Supported values include scheduled, running, stopped, and completed. Optional.                                                                                                                                                                                                                       | array of strings | [“completed”,”running”] |
| message_type | Determines the type of messages for which you want a report. Supported values include single, ab, message_on_user_property. Optional.                                                                                                                                                                                                                               | array of strings | [“single”, “ab”]        |
| label        | Result will include a report only for messages that have the labels specified in your query. Optional.                                                                                                                                                                                                                                                              | array of strings | [“Onboarding”]          |

Below is an example payload.

```json
{
   "from":"20171011",
   "to":"20171130",
   "daily":false,
   "channel":[
    "InApp"
   ],
   "delivery":[
    "inaction", "action"
   ],
   "status":[
    "completed"
   ],
   "message_type":[
    "single"
   ],
   "label":[
   ]
}
```

## Example Request

Here is an example cURL request to the _Get Message Reports_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"from":"20171101","to":"20171225"}' "https://in1.api.clevertap.com/1/message/report.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/message/report.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "from" => "20171101",
  "to" => "20171225"
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
    'Content-Type': 'application/json',
}

data = '{"from":"20171101","to":"20171225"}'

response = requests.post('https://in1.api.clevertap.com/1/message/report.json', headers=headers, data=data)
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
$data = '{"from":"20171101","to":"20171225"}';
$response = Requests::post('https://in1.api.clevertap.com/1/message/report.json', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var dataString = '{"from":"20171101","to":"20171225"}';

var options = {
    url: 'https://in1.api.clevertap.com/1/message/report.json',
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

## Example Response

```json
{
    "status": "success",
    "total_results": 1,
    "messages": [
        {
            "message id": 1508323121,
            "data": [
                [
                    {
                        "sent": 0,
                        "viewed": 0,
                        "clicked": 0
                    }
                ]
            ],
            "start_date": "Oct 18, 4:08 PM",
            "device": [
                "Android",
                "iOS",
                "WindowsMobile"
            ],
            "conversion_event": null,
            "labels": [],
            "status": "completed",
            "channel": "InApp",
            "message_name": "in_app_outputs_test",
            "delivery": "Action"
        }
    ]
}
```

For more information on request limit, refer to [API Request Limit](doc:api-request-limit). To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).

> 📘 API Behavior
> 
> The Get Message Reports API fetches campaigns based on their start dates. Therefore, the campaigns that begin before the specified date range but are still active are not included in the API results.
> 
> For example, you have two email campaigns as follows:
> 
> - **Campaign A:** Started on June 1, 2023, and is still active, sending emails daily.
> - **Campaign B:** Started on June 7, 2023, and ended on June 10, 2023.
> 
> Now, you want to retrieve campaign data for the period from June 5, 2023, to June 8, 2023, using the Get Message Reports API.
> 
> In that case, this API retrieves the campaign data as follows:
> 
> **Campaign A:** Although it is active during the specified date range (June 5 to June 8), but it started before this range (on June 1). Therefore, it is not included in the API results.
> 
> **Campaign B:** It falls entirely within the specified date range (June 7 to June 8). Therefore, it is included in the API results.
> 
> So, when you query the API for this date range, it retrieves data only for Campaign B, and Campaign A is excluded due to its earlier start date.