---
title: Top Property Counts
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

This endpoint retrieves counts for the most and least frequently occurring properties for a particular event in a specified date range.

For example, say you have an e-commerce app and you are tracking purchase events, storing the purchased product as a property on the event. You can use this endpoint to find the most frequently purchased products.

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/counts/top.json`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload.

The following table lists the body parameters.

| Parameter             | Description                                                                                                                                                                                                                  | Required | Type    | Example Value              |
| :-------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------- | :------ | :------------------------- |
| event\_name           | The name of the event.                                                                                                                                                                                                       | Required | string  | "choseNewFavoriteFood"     |
| from                  | Start of the date range within which users should have performed the event specified in `event_name`. Input values must be formatted as integers in YYYYMMDD format.                                                         | Required | integer | 20150810                   |
| to                    | End of the date range within which users should have performed the event specified in `event_name`. Input values must be formatted as integers in YYYYMMDD format.                                                           | Required | integer | 20151025                   |
| groups                | Object containing information about the properties for which breakdown is required. The endpoint supports multiple properties in one request. Each property object is referenced by a unique key within the `groups` object. | Required | object  | See example payload below. |
| groups.property\_type | Type of property for which the top breakdown is required. Must be present inside each individual group. See the `property_type` values table below.                                                                          | Required | string  | "event\_properties"        |
| groups.name           | Name of the property for which the top breakdown is required. Must be present along with `property_type`.                                                                                                                    | Required | string  | "Amount"                   |
| groups.top\_n         | Number of top values to return for the given property. Valid range: 1–100. Defaults to 10 if absent.                                                                                                                         | Optional | integer | 2                          |
| groups.order          | Sort order for the results. Accepted values: `desc` or `asc`. Defaults to `desc` if absent.                                                                                                                                  | Optional | string  | "asc"                      |

The `property_type` parameter specifies the type of property you need metrics for. The following table describes the accepted values.

| property\_type      | Description                                                                                                                      |
| :------------------ | :------------------------------------------------------------------------------------------------------------------------------- |
| event\_properties   | All event properties for the specified `event_name`.                                                                             |
| profile\_fields     | All custom profile fields for the account.                                                                                       |
| session\_properties | `utm_source`, `utm_medium`, `utm_campaign`, `session_referrer`, `session_source`, `time_of_day` (granularity up to hour of day). |
| app\_fields         | All app fields.                                                                                                                  |
| demographics        | All demographic fields.                                                                                                          |
| technographics      | All technographics fields.                                                                                                       |
| reachability        | All reachability fields.                                                                                                         |
| geo\_fields         | Country, region, city.                                                                                                           |

The following is a sample payload.

```json
{
    "event_name": "Charged",
    "from": 20161229,
    "to": 20170129,
    "groups": {
        "foo": {
            "property_type": "event_properties",
            "name": "Amount"
        },
        "bar": {
            "property_type": "profile_fields",
            "name": "Customer Type",
            "top_n": 2,
            "order": "asc"
        }
    }
}
```

## Example Request

The following is a sample request to the _Top Property Counts_ API, including the headers needed to authenticate it.

```curl
curl -X POST -d '{"event_name":"Charged","from":20161229,"to":20170129,"groups":{"foo":{"property_type":"event_properties","name":"Amount"},"bar":{"property_type":"profile_fields","name":"CustomerType","top_n":2,"order":"asc"}}}' "https://<region>.api.clevertap.com/1/counts/top.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://<region>.api.clevertap.com/1/counts/top.json")
request = Net::HTTP::Post.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "PASSCODE"
request.body = JSON.dump({
  "event_name" => "Charged",
  "from" => 20161229,
  "to" => 20170129,
  "groups" => {
    "foo" => {
      "property_type" => "event_properties",
      "name" => "Amount"
    },
    "bar" => {
      "property_type" => "profile_fields",
      "name" => "CustomerType",
      "top_n" => 2,
      "order" => "asc"
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

data = '{"event_name":"Charged","from":20161229,"to":20170129,"groups":{"foo":{"property_type":"event_properties","name":"Amount"},"bar":{"property_type":"profile_fields","name":"CustomerType","top_n":2,"order":"asc"}}}'

response = requests.post('https://<region>.api.clevertap.com/1/counts/top.json', headers=headers, data=data)
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
$data = '{"event_name":"Charged","from":20161229,"to":20170129,"groups":{"foo":{"property_type":"event_properties","name":"Amount"},"bar":{"property_type":"profile_fields","name":"CustomerType","top_n":2,"order":"asc"}}}';
$response = Requests::post('https://<region>.api.clevertap.com/1/counts/top.json', $headers, $data);
```
```javascript Node.js
var request = require('request');

var headers = {
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE',
    'Content-Type': 'application/json'
};

var dataString = '{"event_name":"Charged","from":20161229,"to":20170129,"groups":{"foo":{"property_type":"event_properties","name":"Amount"},"bar":{"property_type":"profile_fields","name":"CustomerType","top_n":2,"order":"asc"}}}';

var options = {
    url: 'https://<region>.api.clevertap.com/1/counts/top.json',
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
    "NUMBER": {
      "0-100": 10,
      "100-200": 9,
      "200-300": 8,
      "300-400": 7,
      "400-500": 6,
      "500-600": 5,
      "600-700": 4,
      "700-800": 3,
      "800-900": 2,
      "900-1000": 1
    }
  },
  "bar": {
    "STR": {
      "Gold": 5,
      "Silver": 10
    }
  }
}
```

Each property breakdown is referenced by the unique key assigned to it in the request. Within each property breakdown, results are grouped by data type. Data types can be `STR`, `NUMBER`, `ENUM`, or `DATE`. `NUMBER` and `DATE` breakdowns use hyphen-separated ranges. `DATE` values are in UNIX epoch format. Counts appear within each data type object.

This API checks the top item count in each memory bucket based on the event property. If the `top_n` value is 20, it checks the top 20 values in all 23 memory buckets. If the `top_n` value is 1, it returns the highest value in each bucket based on availability.

## Response Handling

The response is a JSON object containing the key `status`, which can be `success`, `partial`, or `fail`.

If the status is `success`, the response contains the group keys matching those defined in the `groups` parameter of the request. Each key maps to an object keyed by data type (`STR`, `NUMBER`, `ENUM`, `DATE`), which in turn maps property values to their counts.

If the status is `fail`, the response contains a `error` key with a string value and an HTTP status code.

If the status is `partial`, the query has not finished processing. The response contains a `req_id` key with a long integer value to use for polling. After the query completes, polling returns either a success response with the group breakdown data or a failure response with an error string and HTTP status code. Wait 30 seconds between polling requests.

The following is a sample response for a partial status.

```json
{
  "req_id": 384649162721759,
  "status": "partial"
}
```

After getting the `req_id`, poll the following endpoint and provide the value of `req_id` as a query parameter.

```
GET https://<region>.api.clevertap.com/1/counts/top.json?req_id=<your_request_id_here>
```

## Error Codes

The following is the list of error codes that may be returned by the API:

| HTTP Status | Error                                                       | Description                                                                                                                               | Example Error Response                                                                           |
| ----------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| 400         | `"Account credentials missing"`                             | Account ID header present, but could not be parsed                                                                                        | `{"status":"fail","error":"Account credentials missing","code":400}`                             |
| 400         | `"Payload is mandatory"`                                    | Request body missing or empty                                                                                                             | `{"status":"fail","error":"Payload is mandatory","code":400}`                                    |
| 400         | `"Invalid query"`                                           | Request body is malformed JSON, or query parameters failed validation. Also returned during polling if the event store rejects the query. | `{"status":"fail","error":"Invalid query","code":400}`                                           |
| 400         | `"Invalid request ID"`                                      | The `req_id` passed during polling is invalid or expired. Only occurs on `GET /1/counts/top.json?req_id=...`.                             | `{"status":"fail","error":"Invalid request ID","code":400}`                                      |
| 400         | `"Failed to process request."`                              | Unexpected error during request setup                                                                                                     | `{"status":"fail","error":"Failed to process request.","code":400}`                              |
| 401         | `"Invalid credentials"`                                     | Account ID not found or credentials are invalid                                                                                           | `{"status":"fail","error":"Invalid credentials","code":401}`                                     |
| 403         | `"API access has been temporally suspended"`                | Account's API access has been suspended                                                                                                   | `{"status":"fail","error":"API access has been temporally suspended","code":403}`                |
| 429         | `"Too many concurrent requests"`                            | Per-account concurrent request limit exceeded. Retry with exponential backoff.                                                            | `{"status":"fail","error":"Too many concurrent requests","code":429}`                            |
| 429         | `"Invalid query blocking this because of too many retries"` | Query rate-limited due to excessive retries for the same query. Retry with exponential backoff.                                           | `{"status":"fail","error":"Invalid query blocking this because of too many retries","code":429}` |
| 500         | `"Server error"`                                            | Internal error from the analytics engine                                                                                                  | `{"status":"fail","error":"Server error","code":500}`                                            |
| 500         | `"Failed to process request"`                               | Unexpected critical server-side failure                                                                                                   | `{"status":"fail","error":"Failed to process request","code":500}`                               |
| 503         | `"12 digit account ID mandatory."`                          | `X-CleverTap-Account-Id` header is missing from the request                                                                               | `{"status":"fail","error":"12 digit account ID mandatory.","code":503}`                          |
| 503         | `"System under maintenance. Please retry later"`            | Analytics engine temporarily unavailable. Retry after at least 30 seconds.                                                                | `{"status":"fail","error":"System under maintenance. Please retry later","code":503}`            |
| 503         | `"Please come back later"`                                  | Server at global capacity limit or warming up. Retry after at least 30 seconds.                                                           | `{"status":"fail","error":"Please come back later","code":503}`                                  |

## Notes

- For more information on request limits, refer to [API Request Limit](doc:api-request-limit).
- To understand common queries and concerns about CleverTap APIs, refer to the [API FAQs](doc:api-faqs).

<br />
