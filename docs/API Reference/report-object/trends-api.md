---
title: Trends API
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

This endpoint is used to retrieve daily, weekly, and monthly event trends in a specified duration.

## Base URL

[https://api.clevertap.com/1/counts/trends.json](https://api.clevertap.com/1/counts/trends.json)

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
        Object containing information about trends whose breakdown is required. The endpoint can be used to obtain analysis of multiple trends in one request. Each trend object is referenced by a unique key within the groups object.
      </td>

      <td>
        required
      </td>

      <td>
        object
      </td>

      <td>
        "groups":\{"foo":\{"trend\_type":"daily"},"bar":\{"trend\_type":"weekly"},"foobar":\{"trend\_type":"monthly"}}
      </td>
    </tr>

    <tr>
      <td>
        groups.trend\_type
      </td>

      <td>
        Can be daily, weekly or monthly. Must be present inside each individual group.
      </td>

      <td>
        required
      </td>

      <td>
        string
      </td>

      <td>
        "daily"
      </td>
    </tr>

    <tr>
      <td>
        unique
      </td>

      <td>
        If set to true, returns trends of profiles instead of events.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>
  </tbody>
</Table>

Below is an example payload.

```json
{
	"event_name": "UTM Visited",
	"from": 20161224,
	"to": 20170103,
	"unique": false,
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

```curl
curl -X POST -d '{"event_name":"UTMVisited","from":20161224,"to":20170103,"unique":false,"groups":{"foo":{"trend_type":"daily"},"bar":{"trend_type":"weekly"},"foobar":{"trend_type":"monthly"}}}' "https://api.clevertap.com/1/counts/trends.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
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

GET [https://api.clevertap.com/1/counts/trends.json?req\_id=](https://api.clevertap.com/1/counts/trends.json?req_id=)\<your\_request\_id\_here>

# Next Steps

Now that you’ve learned how to get a message report from the CleverTap API, learn how to get events using the [Get Events API](doc:get-events-api).
