---
title: Real-Time Counts API
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

This endpoint enables you to get a real-time count of active users in the past five minutes in your app. The results can be spliced by user_type, session_source, browser, os, and device for active and new users.

## Base URL
https://api.clevertap.com/1/now.json

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
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type is always set to application/json.",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
## Body Parameters
The body is uploaded as a JSON payload. An empty payload {} will return a real-time count of active users.
[block:parameters]
{
  "data": {
    "0-0": "user_type",
    "1-0": "session_source",
    "2-0": "browser",
    "3-0": "os",
    "4-0": "device",
    "5-0": "new",
    "h-0": "Parameter",
    "h-1": "Description",
    "0-1": "If set to true, will return the split of users by type. Can be new, repeat, addressable or customer.",
    "1-1": "If set to true, will return the split of users by session source. Can be direct, search, social, others or unavailable.",
    "2-1": "If set to true, will return the split of users by browser.",
    "3-1": "If set to true, will return the split of users by OS.",
    "4-1": "If set to true, will return the split of users by device type. Can be desktop, mobile, or tablet.",
    "5-1": "Set to true for only new users.",
    "0-2": "optional",
    "h-2": "Required",
    "h-3": "Type",
    "h-4": "Example Value",
    "1-2": "optional",
    "2-2": "optional",
    "3-2": "optional",
    "4-2": "optional",
    "5-2": "optional",
    "0-3": "boolean",
    "1-3": "boolean",
    "2-3": "boolean",
    "3-3": "boolean",
    "4-3": "boolean",
    "5-3": "boolean",
    "0-4": "true",
    "1-4": "true",
    "2-4": "true",
    "3-4": "true",
    "4-4": "true",
    "5-4": "true"
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
      "code": "{\n\t\"user_type\": true,\n\t\"session_source\": true,\n\t\"browser\": true,\n\t\"os\": true,\n\t\"device\": true,\n\t\"new\": true\n}\n",
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
      "code": "curl -X POST -d '{\"user_type\":true,\"session_source\":true,\"browser\":true,\"os\":true,\"device\":true,\"new\":true}' \"https://api.clevertap.com/1/now.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
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
      "code": "{\n  \"status\": \"success\",\n  \"count\": 214,\n  \"last_processed_ts\": 1486536600\n}",
      "language": "json"
    }
  ]
}
[/block]
Here is an example response if you set all optional parameters to true.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\",\n  \"count\": 214,\n  \"user_type\": {\n    \"new\": 58,\n    \"repeat\": 25,\n    \"addressable\": 38,\n    \"customer\": 93\n  },\n  \"session_source\": {\n    \"direct\": 122,\n    \"others\": 115,\n    \"search\": 18,\n    \"social\": 2,\n    \"email\": 1,\n    \"unavailable\": 4\n  },\n  \"browser\": {\n    \"others\": 4,\n    \"firefox\": 2,\n    \"chrome\": 52,\n    \"ie\": 3,\n    \"mobileapp\": 201\n  },\n  \"os\": {\n    \"others\": 2,\n    \"android\": 196,\n    \"ios\": 35,\n    \"windows\": 25,\n    \"mac\": 3,\n    \"linux\": 2\n  },\n  \"device\": {\n    \"desktop\": 32,\n    \"mobile\": 230,\n    \"tablet\": 1\n  },\n  \"new\": {\n    \"session_source\": {\n      \"direct\": 18,\n      \"others\": 50,\n      \"search\": 12,\n      \"social\": 2,\n      \"email\": 1,\n      \"unavailable\": 3\n    },\n    \"browser\": {\n      \"others\": 1,\n      \"firefox\": 0,\n      \"chrome\": 36,\n      \"ie\": 3,\n      \"mobileapp\": 47\n    },\n    \"os\": {\n      \"others\": 0,\n      \"android\": 67,\n      \"ios\": 2,\n      \"windows\": 15,\n      \"mac\": 2,\n      \"linux\": 1\n    },\n    \"device\": {\n      \"desktop\": 18,\n      \"mobile\": 68,\n      \"tablet\": 1\n    }\n  },\n  \"last_processed_ts\": 1486536600\n}",
      "language": "json"
    }
  ]
}
[/block]
## Notes
The response will be a JSON object containing either the success or fail status.

If no payload was provided and the request was successful, the response will include a count of active users in real-time. The response will also include last_processed_ts, which is a UNIX epoch timestamp of the last processed event in your account.
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"status\": \"success\",\n  \"count\": 214,\n  \"last_processed_ts\": 1486536600\n}",
      "language": "json"
    }
  ]
}
[/block]