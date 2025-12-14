---
title: Settings API Endpoints
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
These endpoints allow you to both fetch data about dashboard settings and also allow you to setup/update existing dashboard settings

# Subscription Groups
You can use this API to get the list of all subscription groups created on the dashboard

### Base Method
https://api.clevertap.com/category-groups

### HTTP Method
GET

### Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.
[block:parameters]
{
  "data": {
    "h-0": "Header",
    "0-0": "X-CleverTap-Account-Id",
    "1-0": "X-CleverTap-Passcode",
    "2-0": "Content-Type",
    "h-1": "Description",
    "0-1": "Your CleverTap Account ID.",
    "1-1": "Your CleverTap Account Passcode.",
    "2-1": "Request content-type is always set to application/json.",
    "h-2": "Type",
    "0-2": "String",
    "1-2": "String",
    "2-2": "String",
    "h-3": "Example Value",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
### Example Response
[block:code]
{
  "codes": [
    {
      "code": "//response will be in the below format\n{\n   \"list\": [\n       {\n           \"name\": \"Newsletter\",\n           \"key\": 1,\n           \"description\": \"Newslettter\",\n           \"channels\": [\n               \"Email\"\n           ],\n           \"updated_on\": 1597812749,\n           \"created_on\": 1597812749\n       },\n       {\n           \"name\": \"Product Updates\",\n           \"key\": 2,\n           \"description\": \"Product Updates\",\n           \"channels\": [\n               \"Email\"\n           ],\n           \"updated_on\": 1597826038,\n           \"created_on\": 1597826038\n       }\n   ],\n   \"status\": \"success\"\n}\n",
      "language": "json"
    }
  ]
}
[/block]