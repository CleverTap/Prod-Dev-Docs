---
title: Trends
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
# Overview

This endpoint is used to retrieve daily, weekly, and monthly event trends in a specified duration.

## Base URL

Here is an example base URL from the account in the India region:\
[https://in1.api.clevertap.com/1/counts/trends.json](https://in1.api.clevertap.com/1/counts/trends.json)

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload. 

| Parameter          | Description                                                                                                                                                                                                                      | Required | Type    | Example Value                                                                                                  |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------- | :------ | :------------------------------------------------------------------------------------------------------------- |
| event\_name        | The name of the event.                                                                                                                                                                                                           | required | string  | "choseNewFavoriteFood"                                                                                         |
| from               | Start of date range within which users should have performed the event you specified in event\_name. Input values have to be formatted as integers in format YYYYMMDD.                                                           | required | int     | 20150810                                                                                                       |
| to                 | End of date range within which users should have performed the event you specified in event\_name. Input values have to be formatted as integers in format YYYYMMDD.                                                             | required | int     | 20151025                                                                                                       |
| groups             | Object containing information about trends whose breakdown is required. The endpoint can be used to obtain analysis of multiple trends in one request. Each trend object is referenced by a unique key within the groups object. | required | object  | "groups":\{"foo":\{"trend\_type":"daily"},"bar":\{"trend\_type":"weekly"},"foobar":\{"trend\_type":"monthly"}} |
| groups.trend\_type | Can be daily, weekly or monthly. Must be present inside each individual group.                                                                                                                                                   | required | string  | "daily"                                                                                                        |
| unique             | If set to true, returns trends of profiles instead of events.                                                                                                                                                                    | optional | boolean | true                                                                                                           |
| sum\_event\_prop   | This event property can display values based on daily, weekly, or monthly trends. This property can process only Integer data types.                                                                                             | Optional | Integer | "Amount"                                                                                                       |

Below is an example payload.

```json
{
	"event_name": "Charged",
	"from": 20161224,
	"to": 20170103,
	"unique": false,
  "sum_event_prop": "Amount",
	"groups": {
		"foo": {
			"trend_type": "daily"
		},
		"bar": {
			"trend_type": "weekly"
		},
		"foobar": {
			"trend_type": "monthly"
		}
	}
}
```

## Example Request

Here is an example cURL request to the *Trends* API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"event_name":"Charged","from":20161224,"to":20170103,"unique":false,"sum_event_prop":"Amount","groups":{"foo":{"trend_type":"daily"},"bar":{"trend_type":"weekly"},"foobar":{"trend_type":"monthly"}}}' "https://in1.api.clevertap.com/1/counts/trends.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/counts/trends.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "event_name" => "Charged",
  "from" => 20161224,
  "to" => 20170103,
  "unique" => false,
  "sum_event_prop":"Amount",
  "groups" => {
    "foo" => {
      "trend_type" => "daily"
    },
    "bar" => {
      "trend_type" => "weekly"
    },
    "foobar" => {
      "trend_type" => "monthly"
    }
  }
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

data = '{"event_name":"Charged","from":20161224,"to":20170103,"unique":false,"sum_event_prop":"Amount","groups":{"foo":{"trend_type":"daily"},"bar":{"trend_type":"weekly"},"foobar":{"trend_type":"monthly"}}}'

response = requests.post('https://in1.api.clevertap.com/1/counts/trends.json', headers=headers, data=data)
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
$data = '{"event_name":"Charged","from":20161224,"to":20170103,"unique":false,"sum_event_prop":"Amount","groups":{"foo":{"trend_type":"daily"},"bar":{"trend_type":"weekly"},"foobar":{"trend_type":"monthly"}}}';
$response = Requests::post('https://in1.api.clevertap.com/1/counts/trends.json', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var dataString = '{"event_name":"Charged","from":20161224,"to":20170103,"unique":false,"sum_event_prop":"Amount","groups":{"foo":{"trend_type":"daily"},"bar":{"trend_type":"weekly"},"foobar":{"trend_type":"monthly"}}}';

var options = {
    url: 'https://in1.api.clevertap.com/1/counts/trends.json',
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
  "foo": {
    "20161224": 10,
    "20161225": 10,
    "20161226": 10,
    "20161227": 10,
    "20161228": 10,
    "20161229": 10,
    "20161230": 10,
    "20161231": 10,
    "20170101": 10,
    "20170102": 10,
    "20170103": 10
  },
  "bar": {
    "201652": 80,
    "201701": 30
  },
  "foobar": {
    "201612": 80,
    "201701": 30
  }
}
```

Each trend is referenced by the unique key assigned to it while querying.

daily trend keys are in YYYYMMDD format. weekly trend keys are in YYYYWW format, where WW refers to week of year. monthly trend keys are in YYYYMM format.

## Notes

The response is a JSON object containing the key status, which might be success, partial, or fail.

If the status is success, there will be a count key with an int value of the count for the specified query. If the status is fail, there will be an error key with a string value and a HTTP status code.

If the status is partial, the query has not finished processing in our system. In the response, you will receive a req\_id key with a long int value that you will use to poll for the result. After the query is completed, you will either receive a success response with the count if the query was successful, or a fail response with an error string and a HTTP status code.

Here is an example response for a partial status.

```json
{
  "req_id": 384649162721759,
  "status": "partial"
}
```

After getting the req\_id, you will poll the endpoint below and provide the value of req\_id as a query parameter.

GET [https://in1.api.clevertap.com/1/counts/trends.json?req\_id=](https://in1.api.clevertap.com/1/counts/trends.json?req_id=)\<your\_request\_id\_here>

> 📘 Region
>
> This is an example base URL from the account in the India region. To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/trends-api#region).

# Next Steps

Now that you’ve learned how to get a message report from the CleverTap API, learn how to get events using the [Get Events API](doc:get-events-api).

## Notes

* Calls to this endpoint are synchronous. We recommend making individual calls, waiting for our response, and then make another request.
* For more information on request limit, refer to [API Request Limit](doc:api-request-limit).
* To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
