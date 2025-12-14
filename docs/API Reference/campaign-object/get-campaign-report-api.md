---
title: Get Campaign Report API
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

The Get Campaign Report API lets you get campaign metrics.

To get a campaign report, you specify the id of the report needed. If the campaign has not completed yet, you will receive an error message indicating that status.

## Base URL
https://api.clevertap.com/1/targets/result.json

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
    "0-1": "CleverTap campaign id of the campaign whose metrics you want to view. You can find this id in your CleverTap dashboard under the Push section",
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
      "code": "{\n    \"id\": 1457432766\n}",
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
      "code": "curl -X POST -d '{ \"id\": 1457432766}' \"https://api.clevertap.com/1/targets/result.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
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
      "code": "{\n    \"result\": {\n        \"clicked\": 28,\n        \"sent\": 82\n    },\n    \"status\": \"success\"\n}",
      "language": "json"
    }
  ]
}
[/block]
## Notes
If the campaign has not been completed yet, you will receive the error response below.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"error\": \"This target hasn't been completed\",\n    \"status\": \"fail\"\n}",
      "language": "json"
    }
  ]
}
[/block]
# Next Steps
Now that you’ve learned how to get a campaign report from the CleverTap API, learn how to get events using the [Get Events API](doc:get-events-api).