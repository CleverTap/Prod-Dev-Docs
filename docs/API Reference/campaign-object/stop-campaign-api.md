---
title: Stop Scheduled Campaign API
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

The Stop Scheduled Campaign API lets you stop a campaign scheduled through the API.

## Base URL
https://api.clevertap.com/1/targets/stop.json

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
## Body Parameter
The body is uploaded as a JSON payload.

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "id",
    "0-1": "CleverTap ID of the scheduled campaign you want to stop. You can find this id in your CleverTap dashboard under the push section.",
    "0-2": "int",
    "0-3": "1457432766"
  },
  "cols": 4,
  "rows": 1
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"id\": 1457432766\n}\n",
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
      "code": "curl -X POST -d '{ \"id\": 1457432766}' \"https://api.clevertap.com/1/targets/stop.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
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
      "code": "{\n    \"status\": \"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]
# Next Steps

Now that you’ve learned how to stop a scheduled campaign with the CleverTap API, learn how to get a campaign report using the [Get Campaign Report API](doc:get-campaign-report-api).