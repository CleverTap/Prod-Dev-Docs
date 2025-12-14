---
title: Delete Variables API
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The Delete Variables API endpoint allows you to delete variables individually or all at once.

# Endpoint

Here is a sample endpoint from the account in the India region:  
<https://in1.api.clevertap.com/1/deleteVars>

# Authentication

Authentication is required using accountId and passcode headers.

# Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

# Region

Refer to [Regions](https://developer.clevertap.com/docs/common-api-components#region) to view region-specific sample endpoints

# HTTP Method

The HTTP method is **POST** for the Delete Variables API

# Body Parameters

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example",
    "h-4": "Required/Optional",
    "0-0": "deleteAllVars",
    "0-1": "Determines whether you want to delete all variables in your Remote Config.",
    "0-2": "Boolean",
    "0-3": "{  \n  \"deleteAllVars\": false  \n}",
    "0-4": "Optional",
    "1-0": "variableNames",
    "1-1": "Contains a list of variable names you want to delete from your Remote Config.",
    "1-2": "String",
    "1-3": "{  \n\"variableNames\": [\"var1\", \"folder1.var2\", \"non.existing\"]  \n}",
    "1-4": "Required"
  },
  "cols": 5,
  "rows": 2,
  "align": [
    "left",
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


# Sample Request

```json json
curl -X POST -d '{"variableNames":["var1","folder1.var2"]}' "https://in1.api.clevertap.com/1/deleteVars" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```

# Sample Response

```json
{
    "status": "success",
    "deletedVariables": [
        "var1",
        "folder1.var2"
    ],
    "problematicVariables": [
        "non.existing"
    ]
}

```