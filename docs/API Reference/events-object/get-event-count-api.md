---
title: Get Event Count API
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
## Overview

This endpoint is used to retrieve counts for an event in a specified duration. For example, you can get the total number of charge events in the past day. 

## Base URL
https://api.clevertap.com/1/counts/events.json

## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "0-0": "X-CleverTap-Account-Id",
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
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
    "1-0": "event_properties",
    "2-0": "from",
    "3-0": "to",
    "0-2": "required",
    "1-2": "optional",
    "2-2": "required",
    "3-2": "required",
    "0-1": "The name of the event.",
    "1-1": "The event properties that you want to filter the results on.",
    "2-1": "Start of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.",
    "3-1": "End of date range within which users should have performed the event you specified in event_name. Input values have to be formatted as integers in format YYYYMMDD.",
    "0-4": "\"choseNewFavoriteFood\"",
    "0-3": "string",
    "1-3": "string key/value pairs",
    "2-3": "int",
    "3-3": "int",
    "1-4": "\"event_properties\": [\n        {\n            \"name\": \"value\",\n            \"operator\": \"contains\",\n            \"value\": \"piz\"\n        }\n    ]",
    "2-4": "20150810",
    "3-4": "20151025"
  },
  "cols": 5,
  "rows": 4
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"event_name\": \"choseNewFavoriteFood\",\n    \"event_properties\": [\n        {\n            \"name\": \"value\",\n            \"operator\": \"contains\",\n            \"value\": \"piz\"\n        }\n    ],\n    \"from\": 20150810,\n    \"to\": 20151025\n}",
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
      "code": "curl -X POST -d '{\"event_name\":\"choseNewFavoriteFood\",\"event_properties\":[{\"name\":\"value\",\"operator\":\"contains\",\"value\":\"piz\"}],\"from\":20150810,\"to\":20151025}' \"https://api.clevertap.com/1/counts/events.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
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
      "code": "{\n  \"status\": \"success\",\n  \"count\": 7138\n}",
      "language": "json"
    }
  ]
}
[/block]
## Notes
The response is a JSON object containing the key status, which might be success, partial, or fail.

If the status is success, there will be a count key with an int value of the count for the specified query. If the status is fail, there will be an error key with a string value and a HTTP status code.

If the status is partial, the query has not finished processing in our system. In the response, you will receive a req_id key with a long int value that you will use to poll for the result. After the query is completed, you will either receive a success response with the count if the query was successful, or a fail response with an error string and a HTTP status code. Please wait 30 seconds between polling requests.

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

GET https://api.clevertap.com/1/counts/events.json?req_id=<your_request_id_here>