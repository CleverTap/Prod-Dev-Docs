---
title: Inbox API Errors
excerpt: >-
  Learn about Inbox Channel API errors you may encounter if the API requests
  fail.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

If your Inbox channel API request fails, you will receive an error response, including a response code and a message explaining the reason for the error.

# Error Codes

The following table lists the errors you may encounter if there is an error in the Inbox channel API request:

[block:parameters]
{
  "data": {
    "h-0": "Error Code",
    "h-1": "Error Description",
    "h-2": "Details",
    "h-3": "Status Code",
    "h-4": "Response Payload",
    "0-0": "401",
    "0-1": "Authentication Failed",
    "0-2": "The AccountId and passcode provided in the headers are not authorized.",
    "0-3": "401",
    "0-4": "{  \n    \"status\": \"fail\",  \n    \"error\": \"Invalid Credentials\",  \n    \"code\": 401  \n}",
    "1-0": "10001",
    "1-1": "User not present in CleverTap database",
    "1-2": "The user is not present in the CleverTap database.",
    "1-3": "200",
    "1-4": "{  \n    \"status\": \"fail\",  \n    \"error\": \"User not present\",  \n    \"code\": 10001  \n}",
    "2-0": "10002",
    "2-1": "Internal Error",
    "2-2": "This can be caused due to internal exceptions.",
    "2-3": "500",
    "2-4": "{  \n    \"status\": \"fail\",  \n    \"error\": \"Internal Error\",  \n    \"code\": 10002  \n}",
    "3-0": "10003",
    "3-1": "Exceeded max messages/response size",
    "3-2": "If the response size exceeds the specified limit, some messages are removed from the response until the total size is under the limit.",
    "3-3": "200",
    "3-4": "{  \n    \"status\": \"success\",  \n    \"error\": \"Exceeded max messages/response size\",  \n    \"code\": 10003  \n}",
    "4-0": "10004",
    "4-1": "Incorrect Payload format",
    "4-2": "The payload received is incorrect. ",
    "4-3": "400",
    "4-4": "{  \n    \"status\": \"fail\",  \n    \"error\": \"Incorrect Payload Format\",  \n    \"code\": 10004  \n}",
    "5-0": "10005",
    "5-1": "Internal Server Error",
    "5-2": "The CleverTap Inbox Server is down.",
    "5-3": "503",
    "5-4": "{  \n    \"status\": \"fail\",  \n    \"error\": \"Internal Server Error\",  \n    \"code\": 10005  \n}",
    "6-0": "10006",
    "6-1": "Inbox API endpoints not enabled for account",
    "6-2": "Endpoints are not enabled for the account.",
    "6-3": "403",
    "6-4": "{  \n  \"status\": \"fail\",  \n  \"error\": \"Inbox API endpoints not enabled for account\",  \n  \"code\": 10006  \n}",
    "7-0": "10007",
    "7-1": "Exceeded max messages in a single request.",
    "7-2": "The number of messages included in a single request exceeds the specified limit.",
    "7-3": "400",
    "7-4": "{  \n  \"status\": \"fail\",  \n  \"error\": \"Exceeded max messages in a single request.\",  \n  \"code\": 10007  \n}",
    "8-0": "10008",
    "8-1": "Endpoint invalid. Please check your endpoint and try again.",
    "8-2": "The endpoint specified does not exist or is invalid.",
    "8-3": "404",
    "8-4": "{  \n  \"status\": \"fail\",  \n  \"error\": \"Endpoint invalid. Please check your endpoint and try again.\",  \n  \"code\": 10008  \n}"
  },
  "cols": 5,
  "rows": 9,
  "align": [
    "left",
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]