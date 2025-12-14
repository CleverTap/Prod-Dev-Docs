---
title: Get Message Reports API
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
The Get Message Reports API lets you download a list of messages sent by CleverTap. 

For example, you can this API to get a report of how many in-app messages and push notifications were sent to users in the past week. 

# Overview
To get a message report, you submit a request with the date range needed. You can also filter these results by setting optional parameters that will return only specific channels or message statuses.

## Base URL
https://api.clevertap.com/1/message/report.json

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
The body is uploaded as a JSON payload. from & to are required parameters. All the optional parameters, except daily, let input multiple values by separating them with commas.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "from",
    "1-0": "to",
    "2-0": "channel",
    "3-0": "delivery",
    "4-0": "daily",
    "5-0": "status",
    "6-0": "message_type",
    "7-0": "label",
    "6-1": "Determines the type of messages for which you want a report. Supported values include single, ab, message_on_user_property. Optional.",
    "7-1": "Result will include a report only for messages that have the labels specified in your query. Optional.",
    "5-1": "Determines the status of messages for which you want a report. Supported values include scheduled, running, stopped, and completed. Optional.",
    "4-1": "Determines if the report should be a day-wise split, or an aggregate report. Supported values are true or false. Unless specified otherwise, the value of daily is set to false. Optional.",
    "3-1": "Delivery type of communications for which you want a report. Supported input values are one_time, inaction, action, recurring, property_time, and api. Optional.",
    "2-1": "The channels of communication needed. Supported values include push (mobile push messages), email, sms, browser (browser push messages), audiences (Facebook messages), inapp (in-app messages), webhooks (webhooks), web_pop_up (Web pop-ups), web_exit_intent (web exit intent pop-ups). Optional.",
    "1-1": "End of date range for events needed. Value specified in format YYYYMMDD. Required.",
    "0-1": "Start of date range for events needed. Value specified in format YYYYMMDD. Required.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "array of strings",
    "3-2": "array of strings",
    "4-2": "boolean",
    "5-2": "array of strings",
    "6-2": "array of strings",
    "7-2": "array of strings",
    "6-3": "[“single”, “ab”]",
    "7-3": "[“Onboarding”]",
    "5-3": "[“completed”,”running”]",
    "4-3": "false",
    "3-3": "[“inaction”,”action”]",
    "2-3": "[“push”,”email”]",
    "1-3": "“20171225”",
    "0-3": "“20171201”"
  },
  "cols": 4,
  "rows": 8
}
[/block]
Below is an example payload.
[block:code]
{
  "codes": [
    {
      "code": "{\n   \"from\":\"20171011\",\n   \"to\":\"20171130\",\n   \"daily\":false,\n   \"channel\":[\n    \"InApp\"\n   ],\n   \"delivery\":[\n    \"inaction\", \"action\"\n   ],\n   \"status\":[\n    \"completed\"\n   ],\n   \"message_type\":[\n    \"single\"\n   ],\n   \"label\":[\n   ]\n}",
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
      "code": "curl -X POST -d '{\"from\":\"20171101\",\"to\":\"20171225\"}' \"https://api.clevertap.com/1/message/report.json\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
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
      "code": "{\n    \"status\": \"success\",\n    \"total_results\": 1,\n    \"messages\": [\n        {\n            \"message id\": 1508323121,\n            \"data\": [\n                [\n                    {\n                        \"sent\": 0,\n                        \"viewed\": 0,\n                        \"clicked\": 0\n                    }\n                ]\n            ],\n            \"start_date\": \"Oct 18, 4:08 PM\",\n            \"device\": [\n                \"Android\",\n                \"iOS\",\n                \"WindowsMobile\"\n            ],\n            \"conversion_event\": null,\n            \"labels\": [],\n            \"channel\": \"InApp\",\n            \"message_name\": \"in_app_outputs_test\",\n            \"delivery\": \"Action\"\n        }\n    ]\n}",
      "language": "json"
    }
  ]
}
[/block]
# Next Steps

Now that you’ve learned how to get a message report from the CleverTap API, learn how to get events using the [Get Events API](doc:get-events-api).