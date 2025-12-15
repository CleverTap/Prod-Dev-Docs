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

This endpoint is used to retrieve counts for the most and least frequently occurring properties for a particular event in a specified duration.

For example, let’s you have an ecommerce app and you are tracking purchase events and storing the product purchased as a property on the event. You can use this endpoint to find out what products are the most frequently purchased.

## Base URL

Here is an example base URL from the account in the India region:  
[https://in1.api.clevertap.com/1/counts/top.json](https://in1.api.clevertap.com/1/counts/top.json)

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The body is uploaded as a JSON payload.

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Required
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        event_name
      </td>

      <td>
        The name of the event.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        "choseNewFavoriteFood"
      </td>
    </tr>

    <tr>
      <td>
        from
      </td>

      <td>
        Start of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.
      </td>

      <td>
        required
      </td>

      <td>
        int
      </td>

      <td>
        20150810
      </td>
    </tr>

    <tr>
      <td>
        to
      </td>

      <td>
        End of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.
      </td>

      <td>
        required
      </td>

      <td>
        int
      </td>

      <td>
        20151025
      </td>
    </tr>

    <tr>
      <td>
        groups
      </td>

      <td>
        Object containing information about properties for which breakdown is required. The endpoint can be used to obtain analysis of multiple properties in one request. Each property object is referenced by a unique key within the groups object.
      </td>

      <td>
        required
      </td>

      <td>
        object
      </td>

      <td>
        "groups": \{  
        		"foo": \{  
        			"property_type": "event_properties",  
        			"name": "Amount"  
        		},  
        		"bar": \{  
        			"property_type": "profile_fields",  
        			"name": "Customer Type",  
        			"top_n": 2,  
        			"order": "asc"  
        		}  
        	}
      </td>
    </tr>

    <tr>
      <td>
        groups.property_type
      </td>

      <td>
        Type of property for which top breakdown is required. Can be event_properties, session_properties, profile_fields, app_fields, demographics, technographics, reachability or geo_fields. Must be present inside each individual group.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        "event_properties"
      </td>
    </tr>

    <tr>
      <td>
        groups.name
      </td>

      <td>
        Name of the property for which top breakdown is required. Must be present along with property_type.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        “Amount”
      </td>
    </tr>

    <tr>
      <td>
        top_n
      </td>

      <td>
        Number of top values required for a given property. Can be specified along with property_type and name. Defaults to 10 if absent.
      </td>

      <td>
        optional
      </td>

      <td>
        int
      </td>

      <td>
        2
      </td>
    </tr>

    <tr>
      <td>
        order
      </td>

      <td>
        Sort order. Either desc or asc. Defaults to desc if absent.
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>
        "asc"
      </td>
    </tr>
  </tbody>
</Table>

The property_type parameter enables you to specify the type of property you need metrics for. Here are the descriptions for the potential parameter values.

| property_type      | Description                                                                                                          |
| :----------------- | :------------------------------------------------------------------------------------------------------------------- |
| event_properties   | All event properties for the specified event_name.                                                                   |
| profile_fields     | All custom profile fields for the account.                                                                           |
| session_properties | utm_source, utm_medium, utm_campaign, session_referrer, session_source, time_of_day (granularity up to hour of day). |
| app_fields         | All the app fields.                                                                                                  |
| demographics       | All the demographics fields.                                                                                         |
| technographics     | All the technographics fields.                                                                                       |
| reachability       | All the reachability fields.                                                                                         |
| geo_fields         | Country, region, city.                                                                                               |

Below is an example payload.

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

Here is an example cURL request to the _Top Property Counts_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST -d '{"event_name":"Charged","from":20161229,"to":20170129,"groups":{"foo":{"property_type":"event_properties","name":"Amount"},"bar":{"property_type":"profile_fields","name":"CustomerType","top_n":2,"order":"asc"}}}' "https://in1.api.clevertap.com/1/counts/top.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
require 'json'

uri = URI.parse("https://in1.api.clevertap.com/1/counts/top.json")
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

response = requests.post('https://in1.api.clevertap.com/1/counts/top.json', headers=headers, data=data)
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
$response = Requests::post('https://in1.api.clevertap.com/1/counts/top.json', $headers, $data);
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
    url: 'https://in1.api.clevertap.com/1/counts/top.json',
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

This API checks the top item count in each memory bucket on the basis of the event property. if the _top_n_value is 20, it will check the top 20 values in all the 23 memory buckets. If the _top_n_ value is 1, it gives the highest value in each bucket based on availability.

Each property breakdown is referenced by the unique key assigned to it while querying. Within each property breakdown, there are separate objects specifying the data type for the property’s values that have been provided.

Data types can be STR, NUMBER, ENUM, or DATE. NUMBER and DATE breakdowns are ‘-‘ separated ranges. DATE values are in UNIX epoch format. Counts are present within the data type object for each property breakdown.

## Notes

The response is a JSON object containing the key status, which might be success, partial, or fail.

If the status is success, there will be a count key with an int value of the count for the specified query. If the status is fail, there will be an error key with a string value and a HTTP status code.

If the status is partial, the query has not finished processing in our system. In the response, you will receive a req_id key with a long int value that you will use to poll for the result. After the query is completed, you will either receive a success response with the count if the query was successful, or a fail response with an error string and a HTTP status code. Please wait 30 seconds between polling requests.

Here is an example response for a partial status.

```json
{
  "req_id": 384649162721759,
  "status": "partial"
}
```

After getting the req_id, you will poll the endpoint below and provide the value of req_id as a query parameter.

GET [https://in1.api.clevertap.com/1/counts/top.json?req_id=](https://in1.api.clevertap.com/1/counts/top.json?req_id=)\<your_request_id_here>

<Callout icon="📘" theme="info">
  **Region**

  This is an example base URL from the account in the India region. To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/top-property-counts-api#region).
</Callout>

For more information on request limit, refer to [API Request Limit](doc:api-request-limit). To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
