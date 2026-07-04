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

This endpoint retrieves daily, weekly, and monthly event trends for a specified date range.

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/counts/trends.json`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload. The maximum date range is between `from` and `to` is 1 year. A maximum of 5 groups is supported per request.

The following table lists the body parameters.

| Parameter                                     | Description                                                                                                                                                                                                                                                                       | Required | Type    | Example Value                                                  |
| :-------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------- | :------ | :------------------------------------------------------------- |
| event\_name                                   | The name of the event.                                                                                                                                                                                                                                                            | Required | string  | "choseNewFavoriteFood"                                         |
| from                                          | Start of the date range within which users should have performed the event specified in `event_name`. Input values must be formatted as integers in YYYYMMDD format. The maximum date range between `from` and `to` is 1 year.                                                    | Required | integer | 20150810                                                       |
| to                                            | End of the date range within which users should have performed the event specified in `event_name`. Input values must be formatted as integers in YYYYMMDD format. The maximum date range between `from` and `to` is 1 year.                                                      | Required | integer | 20151025                                                       |
| groups                                        | Object containing the trend breakdowns required. Up to 5 groups are supported per request. Each trend object is referenced by a unique key within the `groups` object.                                                                                                            | Required | object  | `{"foo":{"trend_type":"daily"},"bar":{"trend_type":"weekly"}}` |
| trend\_type (nested within each group object) | The trend granularity for that group. Valid values: `daily`, `weekly`, `monthly`. Must be present inside each individual group.                                                                                                                                                   | Required | string  | "daily"                                                        |
| unique                                        | When set to `true`, returns trends based on unique users (distinct profiles) rather than total event occurrences. When `false` or omitted, returns total event counts.                                                                                                            | Optional | boolean | true                                                           |
| sum\_event\_prop                              | The name of an event property whose values will be summed across each time bucket. When this parameter is set, the numeric values in the response represent the sum of the specified property rather than event counts. This property must hold integer values in the event data. | Optional | string  | "Amount"                                                       |

The following is a sample payload.

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

The following is a sample request to the _Trends_ API, including the headers needed to authenticate it.

```curl
curl -X POST -d '{"event_name":"Charged","from":20161224,"to":20170103,"unique":false,"sum_event_prop":"Amount","groups":{"foo":{"trend_type":"daily"},"bar":{"trend_type":"weekly"},"foobar":{"trend_type":"monthly"}}}' "https://<region>.api.clevertap.com/1/counts/trends.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/counts/trends.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "event_name" => "Charged",
  "from" => 20161224,
  "to" => 20170103,
  "unique" => false,
  "sum_event_prop" => "Amount",
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

response = requests.post('https://<region>.api.clevertap.com/1/counts/trends.json', headers=headers, data=data)
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
$response = Requests::post('https://<region>.api.clevertap.com/1/counts/trends.json', $headers, $data);
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
    url: 'https://<region>.api.clevertap.com/1/counts/trends.json',
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

The following is a sample response.

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

Each trend is referenced by the unique key assigned to it in the request. The date key formats are as follows:

- Daily (`foo`): `YYYYMMDD`
- Weekly (`bar`): `YYYYWW`, where WW is the ISO week of the year. For example, `201701` means week 1 of 2017.
- Monthly (`foobar`): `YYYYMM`. For example, `201701` means January 2017.

Note that weekly and monthly keys can look identical at year boundaries (for example, both `bar` and `foobar` show `201701` in the sample above). The formats are distinct: `bar`'s `201701` refers to week 1 of 2017, while `foobar`'s `201701` refers to January 2017.

When `sum_event_prop` is set, the numeric values in the response represent the sum of that property across each time bucket, not event counts. For example, if `sum_event_prop` is set to `"Amount"`, each value is the total of all `Amount` values recorded for that date or period.

## Response Handling

The response is a JSON object containing the key `status`, which can be `success`, `partial`, or `fail`.

If the status is `success`, the response contains the group keys matching those defined in the `groups` parameter of the request. Each key maps to an object of date-bucketed trend values. When `sum_event_prop` is not set, each value is an event count. When `sum_event_prop` is set, each value is the sum of that property for the period.

If the status is `fail`, the response contains a `error` key with a string value and an HTTP status code.

If the status is `partial`, the query has not finished processing. The response contains a `req_id` key with a long integer value to use for polling. After the query completes, polling returns either a success response with the trend data or a failure response with an error string and HTTP status code. Wait 30 seconds between polling requests.

The following is a sample response for a partial status.

```json
{
  "req_id": 384649162721759,
  "status": "partial"
}
```

After getting the `req_id`, poll, the following endpoint provides the value  `req_id` as a query parameter.

```
GET https://<region>.api.clevertap.com/1/counts/trends.json?req_id=<your_request_id_here>
```

## Error Codes

The following is the list of errors returned by this API:

| HTTP Status | Error                                                 | Description                                                                    | Example Error Response                                                                     |
| ----------- | ----------------------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ |
| 400         | `"Invalid event"`                                     | `event_name` missing or the event does not exist                               | `{"status":"fail","error":"Invalid event","code":400}`                                     |
| 400         | `"Invalid date format. Use YYYYMMDD (e.g. 20240115)"` | `from` or `to` not in YYYYMMDD format                                          | `{"status":"fail","error":"Invalid date format. Use YYYYMMDD (e.g. 20240115)","code":400}` |
| 400         | `"At least one trend group must be provided"`         | `groups` missing or empty                                                      | `{"status":"fail","error":"At least one trend group must be provided","code":400}`         |
| 400         | `"trend_type missing or invalid in group: {key}"`     | A group entry is missing `trend_type`                                          | `{"status":"fail","error":"trend_type missing or invalid in group: foo","code":400}`       |
| 400         | `"Invalid trend type: {value}"`                       | `trend_type` is not `daily`, `weekly`, or `monthly`                            | `{"status":"fail","error":"Invalid trend type: hourly","code":400}`                        |
| 400         | `"Date range cannot exceed 1 year"`                   | Date range exceeds 1 year                                                      | `{"status":"fail","error":"Date range cannot exceed 1 year","code":400}`                   |
| 400         | `"Max 5 groups allowed per request"`                  | More than 5 groups in a single request                                         | `{"status":"fail","error":"Max 5 groups allowed per request","code":400}`                  |
| 401         | `"Authentication failure"`                            | Missing or invalid Account ID or Passcode headers                              | `{"status":"fail","error":"Authentication failure","code":401}`                            |
| 408 / 504   | `"Request timed out"`                                 | Request timeout                                                                | `{"status":"fail","error":"Request timed out","code":408}`                                 |
| 500         | `"Internal server error"`                             | Internal server error. Retry; contact support if issue persists.               | `{"status":"fail","error":"Internal server error","code":500}`                             |
| 502 / 503   | `"Service unavailable"`                               | Service unavailable. Retry with backoff.                                       | `{"status":"fail","error":"Service unavailable","code":503}`                               |
| 200         | —                                                     | Query still processing. Poll using `req_id`. Wait 30 seconds between attempts. | `{"status":"partial","req_id":384649162721759}`                                            |
| 200         | `"Query failed after processing"`                     | Async query failed after polling completed                                     | `{"status":"fail","error":"Query failed after processing","code":200}`                     |
| 200         | `"Polling timed out"`                                 | Polling exhausted after 10 attempts at 30-second intervals                     | `{"status":"fail","error":"Polling timed out"}`                                            |
| 200         | —                                                     | No event activity in the specified range. Empty trend buckets returned.        | `{"status":"success","foo":{},"bar":{}}`                                                   |

## Notes

- CleverTap recommends making individual sequential calls and waiting for a response from each before making the next request.
- For more information on request limits, refer to [API Request Limit](doc:api-request-limit).
- To understand common queries and concerns about CleverTap APIs, refer to the [API FAQs](doc:api-faqs).

<br />
