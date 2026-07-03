---
title: Get Message Reports
excerpt: Lets you download a list of messages sent by CleverTap.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

To get a message report, you submit a request specifying the required date range. You can also filter results by setting optional parameters to return only specific channels or message statuses.

# Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/message/report.json`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

# HTTP Method

POST

# Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

# Body Parameters

The body is uploaded as a JSON payload. `from` and `to` are required. All optional parameters except `daily` accept multiple values as a comma-separated array.

The following table lists the body parameters.

| Parameter    | Description                                                                                                                                                                                                                                                                                                            | Type             | Example Value           |
| :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------- | :---------------------- |
| from         | Start of the report date range. Value specified in YYYYMMDD format. Required.                                                                                                                                                                                                                                          | string           | "20171201"              |
| to           | End of the date range for the report. Value specified in YYYYMMDD format. Required.                                                                                                                                                                                                                                    | string           | "20171225"              |
| channel      | The channels to include in the report. Supported values: `push` (mobile push), `email`, `sms`, `browser` (browser push), `audiences` (Facebook), `inapp` (in-app), `webhooks`, `web_pop_up`, `web_exit_intent`, `web_native_display`, `web_inbox`, `tiktok`, `nativedisplay` (alias for web_native_display). Optional. | array of strings | ["push","email"]        |
| delivery     | Delivery type of communications for the report. Supported values: `one_time`, `inaction`, `action`, `recurring`, `property_time`, `api`, `multiple_dates`. Optional.                                                                                                                                                   | array of strings | ["inaction","action"]   |
| daily        | Determines if the report is a day-wise split or an aggregate report. When `true`, the `data` array in the response contains one entry per day in the date range. When `false` (default), `data` contains a single aggregated entry. Supported values: `true` or `false`. Optional.                                     | boolean          | false                   |
| status       | The campaign statuses to include in the report. Supported values: `scheduled`, `running`, `stopped`, `completed`. Optional.                                                                                                                                                                                            | array of strings | ["completed","running"] |
| message_type | The message types to include in the report. Supported values: `single`, `ab`, `message_on_user_property`. Optional.                                                                                                                                                                                                    | array of strings | ["single","ab"]         |
| label        | Filters results to include only messages with the specified labels. Optional.                                                                                                                                                                                                                                          | array of strings | ["Onboarding"]          |

The following is a sample payload:

```json
{
   "from": "20171011",
   "to": "20171130",
   "daily": false,
   "channel": [
      "inapp"
   ],
   "delivery": [
      "inaction", "action"
   ],
   "status": [
      "completed"
   ],
   "message_type": [
      "single"
   ],
   "label": []
}
```

# Example Request

The following is a sample request to the _Get Message Reports_ API, showing the headers needed to authenticate the request.

```curl
curl -X POST -d '{"from":"20171101","to":"20171225"}' "https://<region>.api.clevertap.com/1/message/report.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/message/report.json")
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

response = requests.post('https://<region>.api.clevertap.com/1/message/report.json', headers=headers, data=data)
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
$response = Requests::post('https://<region>.api.clevertap.com/1/message/report.json', $headers, $data);
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
    url: 'https://<region>.api.clevertap.com/1/message/report.json',
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

# Example Response

The following is a sample response:

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

# Response Schema

The following table describes the top-level response fields.

| Field         | Type    | Description                                                              |
| :------------ | :------ | :----------------------------------------------------------------------- |
| status        | string  | Always "success" for successful requests.                                |
| total_results | integer | Total number of campaigns matching the query.                            |
| messages      | array   | An array of message report objects. See the message object fields below. |

The following table describes the fields inside each message object in the `messages` array.

| Field            | Type             | Description                                                                                                                                                                                                                                                                                |
| :--------------- | :--------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| message id       | integer          | Unique identifier for the message. Note: this field name contains a space, not an underscore. Attempting to access it as `message_id` will not work.                                                                                                                                       |
| data             | array            | Double-nested array containing delivery metrics (`sent`, `viewed`, `clicked`). When `daily` is `false`, contains a single aggregated entry: `[[{"sent": N, "viewed": N, "clicked": N}]]`. When `daily` is `true`, contains one entry per day in the date range, in the same nested format. |
| start_date       | string           | The campaign start date and time, formatted as "MMM D, h:mm AM/PM" (for example, "Oct 18, 4:08 PM"). The year is not included. For queries that span multiple years, use the `from` and `to` date range parameters to determine the year context.                                          |
| device           | array of strings | Platforms targeted by this campaign (for example, "Android", "iOS"). `WindowsMobile` may appear for legacy campaigns but is no longer actively supported.                                                                                                                                  |
| conversion_event | string or null   | The event is configured as the conversion goal for this campaign. `null` if no conversion event was set.                                                                                                                                                                                   |
| labels           | array of strings | Labels assigned to this campaign. Empty array if no labels are set.                                                                                                                                                                                                                        |
| status           | string           | Current status of the campaign. Possible values: `scheduled`, `running`, `stopped`, `completed`.                                                                                                                                                                                           |
| channel          | string           | The channel used by this campaign. Response values use display names that differ from input values (for example, input `inapp` returns as `"InApp"`, input `one_time` delivery returns as `"One Time"`). See the input-to-response mapping note below.                                     |
| message_name     | string           | The name of the campaign.                                                                                                                                                                                                                                                                  |
| delivery         | string           | The delivery type of the campaign. Response values use display names that differ from input values. See the input-to-response mapping note below.                                                                                                                                          |

<Callout icon="📘" theme="info">
  **API Behavior**

  The Get Message Reports API fetches campaigns based on their start dates. Campaigns that begin before the specified date range but are still active are not included in the results.

  For example, you have two email campaigns:

  * **Campaign A:** Started on June 1, 2023, and is still active, sending emails daily.
  * **Campaign B:** Started on June 7, 2023, and ended on June 10, 2023.

  When you query for June 5, 2023 to June 8, 2023:

  * **Campaign A** is excluded because it started before the range (June 1), even though it is active during the range.
  * **Campaign B** is included because its start date falls within the range (June 7).
</Callout>

For more information on request limits, refer to [API Request Limit](doc:api-request-limit). To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
