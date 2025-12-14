---
title: Common API Components
excerpt: Understand the common components used in CleverTap APIs.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
CleverTap APIs contain several common components in their design and usage.
This page details the common components found within CleverTap APIs.

# Region
The following table lists your API endpoint based on the region of your account:
[block:parameters]
{
  "data": {
    "h-0": "Region",
    "h-1": "API Endpoint",
    "0-0": "India",
    "0-1": "`in1.api.clevertap.com`",
    "2-0": "United States",
    "2-1": "`us1.api.clevertap.com`",
    "h-2": "CleverTap Dashboard URL",
    "0-2": "[https://in1.dashboard.clevertap.com/login.html](https://in1.dashboard.clevertap.com/login.html#/)",
    "2-2": "[https://us1.dashboard.clevertap.com/login.html](https://us1.dashboard.clevertap.com/login.html#/)",
    "3-0": "Indonesia",
    "4-0": "Middle East (UAE)",
    "3-1": "`aps3.api.clevertap.com`",
    "3-2": "[https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html#/)",
    "4-1": "`mec1.api.clevertap.com`",
    "4-2": "[https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html#/)",
    "h-3": "CleverTap Dashboard URL",
    "0-3": "[https://in1.dashboard.clevertap.com/login.html](https://in1.dashboard.clevertap.com/login.html#/)",
    "2-3": "[https://us1.dashboard.clevertap.com/login.html](https://us1.dashboard.clevertap.com/login.html#/)",
    "3-3": "[https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html#/)",
    "4-3": "[https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html#/)",
    "5-0": "Europe (default region)",
    "5-1": "`api.clevertap.com`",
    "5-2": "[https://eu1.dashboard.clevertap.com/login.html](https://eu1.dashboard.clevertap.com/login.html#/)",
    "1-0": "Singapore",
    "1-1": "`sg1.api.clevertap.com`",
    "1-2": "[https://sg1.dashboard.clevertap.com/login.html](https://sg1.dashboard.clevertap.com/login.html#/)"
  },
  "cols": 3,
  "rows": 6
}
[/block]
# API Headers
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