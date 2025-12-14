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
https://api.clevertap.com/1/counts/trends.json

## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

[block:parameters]
{
  "data": {
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "X-CleverTap-Account-Id",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "0-1": "Your CleverTap Account ID.",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type is always set to application/json.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
## Body Parameters
The body is uploaded as a JSON payload. 

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Required",
    "h-3": "Type",
    "h-4": "Example Value",
    "0-0": "event_name",
    "1-0": "from",
    "2-0": "to",
    "3-0": "groups",
    "4-0": "groups.trend_type",
    "5-0": "unique",
    "5-1": "If set to true, returns trends of profiles instead of events.",
    "4-1": "Can be daily, weekly or monthly. Must be present inside each individual group.",
    "3-1": "Object containing information about trends whose breakdown is required. The endpoint can be used to obtain analysis of multiple trends in one request. Each trend object is referenced by a unique key within the groups object.",
    "2-1": "End of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.",
    "1-1": "Start of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.",
    "0-1": "The name of the event.",
    "0-2": "required",
    "1-2": "required",
    "2-2": "required",
    "3-2": "required",
    "4-2": "required",
    "5-2": "optional",
    "5-3": "boolean",
    "5-4": "true",
    "4-3": "string",
    "4-4": "\"daily\"",
    "3-3": "object",
    "3-4": "\"groups\":{\"foo\":{\"trend_type\":\"daily\"},\"bar\":{\"trend_type\":\"weekly\"},\"foobar\":{\"trend_type\":\"monthly\"}}",
    "1-3": "int",
    "2-3": "int",
    "2-4": "20151025",
    "1-4": "20150810",
    "0-4": "\"choseNewFavoriteFood\"",
    "0-3": "string"
  },
  "cols": 5,
  "rows": 6
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n\t\"event_name\": \"UTM Visited\",\n\t\"from\": 20161224,\n\t\"to\": 20170103,\n\t\"unique\": false,\n\t\"groups\": {\n\t\t\"foo\": {\n\t\t\t\"trend_type\": \"daily\"\n\t\t},\n\t\t\"bar\": {\n\t\t\t\"trend_type\": \"weekly\"\n\t\t},\n\t\t\"foobar\": {\n\t\t\t\"trend_type\": \"monthly\"\n\t\t}\n\t}\n}",
      "language": "json"
    }
  ]
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"event_name\":\"UTMVisited\",\"from\":20161224,\"to\":20170103,\"unique\":false,\"groups\":{\"foo\":{\"trend_type\":\"daily\"},\"bar\":{\"trend_type\":\"weekly\"},\"foobar\":{\"trend_type\":\"monthly\"}}}' \"https://api.clevertap.com/1/counts/trends.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    }
  ]
}
[/block]
## Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\",\n  \"foo\": {\n    \"20161224\": 10,\n    \"20161225\": 10,\n    \"20161226\": 10,\n    \"20161227\": 10,\n    \"20161228\": 10,\n    \"20161229\": 10,\n    \"20161230\": 10,\n    \"20161231\": 10,\n    \"20170101\": 10,\n    \"20170102\": 10,\n    \"20170103\": 10\n  },\n  \"bar\": {\n    \"201652\": 80,\n    \"201701\": 30\n  },\n  \"foobar\": {\n    \"201612\": 80,\n    \"201701\": 30\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]
Each trend is referenced by the unique key assigned to it while querying.

daily trend keys are in YYYYMMDD format. weekly trend keys are in YYYYWW format, where WW refers to week of year. monthly trend keys are in YYYYMM format.

## Notes
The response is a JSON object containing the key status, which might be success, partial, or fail.

If the status is success, there will be a count key with an int value of the count for the specified query. If the status is fail, there will be an error key with a string value and a HTTP status code.

If the status is partial, the query has not finished processing in our system. In the response, you will receive a req_id key with a long int value that you will use to poll for the result. After the query is completed, you will either receive a success response with the count if the query was successful, or a fail response with an error string and a HTTP status code.

Here is an example response for a partial status.

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"req_id\": 384649162721759,\n  \"status\": \"partial\"\n}",
      "language": "json"
    }
  ]
}
[/block]
After getting the req_id, you will poll the endpoint below and provide the value of req_id as a query parameter.

GET https://api.clevertap.com/1/counts/trends.json?req_id=<your_request_id_here>

# Next Steps

Now that you’ve learned how to get a message report from the CleverTap API, learn how to get events using the [Get Events API](doc:get-events-api).