---
title: Top Property Counts API
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
# Overview

This endpoint is used to retrieve counts for the most and least frequently occurring properties for a particular event in a specified duration. 

For example, let’s you have an ecommerce app and you are tracking purchase events and storing the product purchased as a property on the event. You can use this endpoint to find out what products are the most frequently purchased. 

## Base URL

[https://api.clevertap.com/1/counts/top.json](https://api.clevertap.com/1/counts/top.json)

## HTTP Method

POST

## Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Header
      </th>

      <th>
        Description
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
        X-CleverTap-Account-Id
      </td>

      <td>
        Your CleverTap Account ID.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Account-Id: ACCOUNT\_ID"
      </td>
    </tr>

    <tr>
      <td>
        X-CleverTap-Passcode
      </td>

      <td>
        Your CleverTap Account Passcode.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Passcode: PASSCODE"
      </td>
    </tr>

    <tr>
      <td>
        Content-Type
      </td>

      <td>
        Request content-type is always set to application/json.
      </td>

      <td>
        string
      </td>

      <td>
        "Content-Type: application/json"
      </td>
    </tr>
  </tbody>
</Table>

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
        event\_name
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
        Start of date range within which users should have performed the event you specified in event\_name. Input values have to be formatted as integers in format YYYYMMDD.
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
        End of date range within which users should have performed the event you specified in event\_name. Input values have to be formatted as integers in format YYYYMMDD.
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
        "groups": \{\
        &#x9;	"foo": \{\
        &#x9;		"property\_type": "event\_properties",\
        &#x9;		"name": "Amount"\
        &#x9;	},\
        &#x9;	"bar": \{\
        &#x9;		"property\_type": "profile\_fields",\
        &#x9;		"name": "Customer Type",\
        &#x9;		"top\_n": 2,\
        &#x9;		"order": "asc"\
        &#x9;	}\
        &#x9;}
      </td>
    </tr>

    <tr>
      <td>
        groups.property\_type
      </td>

      <td>
        Type of property for which top breakdown is required. Can be event\_properties, session\_properties, profile\_fields, app\_fields, demographics, technographics, reachability or geo\_fields. Must be present inside each individual group.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        "event\_properties"
      </td>
    </tr>

    <tr>
      <td>
        groups.name
      </td>

      <td>
        Name of the property for which top breakdown is required. Must be present along with property\_type.
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
        top\_n
      </td>

      <td>
        Number of top values required for a given property. Can be specified along with property\_type and name. Defaults to 10 if absent.
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

The property\_type parameter enables you to specify the type of property you need metrics for. Here are the descriptions for the potential parameter values. 

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        property_type
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        event\_properties
      </td>

      <td>
        All event properties for the specified event\_name.
      </td>
    </tr>

    <tr>
      <td>
        profile\_fields
      </td>

      <td>
        All custom profile fields for the account.
      </td>
    </tr>

    <tr>
      <td>
        session\_properties
      </td>

      <td>
        utm\_source, utm\_medium, utm\_campaign, session\_referrer, session\_source, time\_of\_day (granularity up to hour of day).
      </td>
    </tr>

    <tr>
      <td>
        app\_fields
      </td>

      <td>
        All the app fields.
      </td>
    </tr>

    <tr>
      <td>
        demographics
      </td>

      <td>
        All the demographics fields.
      </td>
    </tr>

    <tr>
      <td>
        technographics
      </td>

      <td>
        All the technographics fields.
      </td>
    </tr>

    <tr>
      <td>
        reachability
      </td>

      <td>
        All the reachability fields.
      </td>
    </tr>

    <tr>
      <td>
        geo\_fields
      </td>

      <td>
        Country, region, city.
      </td>
    </tr>
  </tbody>
</Table>

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

```curl
curl -X POST -d '{"event_name":"Charged","from":20161229,"to":20170129,"groups":{"foo":{"property_type":"event_properties","name":"Amount"},"bar":{"property_type":"profile_fields","name":"CustomerType","top_n":2,"order":"asc"}}}' "https://api.clevertap.com/1/counts/top.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
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

Each property breakdown is referenced by the unique key assigned to it while querying. Within each property breakdown, there are separate objects specifying the data type for the property’s values that have been provided.

Data types can be STR, NUMBER, ENUM, or DATE. NUMBER and DATE breakdowns are ‘-‘ separated ranges. DATE values are in UNIX epoch format. Counts are present within the data type object for each property breakdown.

## Notes

The response is a JSON object containing the key status, which might be success, partial, or fail.

If the status is success, there will be a count key with an int value of the count for the specified query. If the status is fail, there will be an error key with a string value and a HTTP status code.

If the status is partial, the query has not finished processing in our system. In the response, you will receive a req\_id key with a long int value that you will use to poll for the result. After the query is completed, you will either receive a success response with the count if the query was successful, or a fail response with an error string and a HTTP status code. Please wait 30 seconds between polling requests.

Here is an example response for a partial status.

```json
{
  "req_id": 384649162721759,
  "status": "partial"
}
```

After getting the req\_id, you will poll the endpoint below and provide the value of req\_id as a query parameter.

GET [https://api.clevertap.com/1/counts/top.json?req\_id=](https://api.clevertap.com/1/counts/top.json?req_id=)\<your\_request\_id\_here>
