---
title: Get Campaigns API
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

The Get Campaigns API lets you get a list of campaigns created using the API.

# Base URL
https://api.clevertap.com/1/targets/list.json

# HTTP Method
POST

# Headers
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
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "2-1": "Request content-type is always set to application/json.",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
# Body Parameters
The body is uploaded as a JSON payload.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "from",
    "0-1": "Start of date range for events needed. Value specified in format YYYYMMDD.",
    "0-2": "int",
    "0-3": "20171201",
    "1-0": "to",
    "1-1": "End of date range for events needed. Value specified in format YYYYMMDD.",
    "1-2": "int",
    "1-3": "20171225"
  },
  "cols": 4,
  "rows": 2
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"from\": 20160101,\n    \"to\": 20160101\n}",
      "language": "json"
    }
  ]
}
[/block]
# Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST -d '{\"from\": 20160101,\"to\": 20160101}' \"https://api.clevertap.com/1/targets/list.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    }
  ]
}
[/block]
# Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"status\": \"success\",\n    \"targets\": [\n        {\n            \"id\": 1457429935,\n            \"name\": \"My API Campaign\",\n            \"scheduled_on\": 201703081508,\n            \"status\": \"pending\"\n        },\n        {\n            \"id\": 1457432766,\n            \"name\": \"My API Campaign 2\",\n            \"scheduled_on\": 201603081556,\n            \"status\": \"completed\"\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]