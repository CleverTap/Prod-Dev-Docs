---
title: Get Variables API
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

The Get API endpoint retrieves Variables and their values for the current user or device. If the profile or clevertap ID does not exist, the API returns the default values or values associated with all users for the variables. It does not generate new profiles or devices.

# Endpoint

Here is a base sample endpoint from the account in the India region:  
<https://in1.api.clevertap.com/1/getVars>

# Authentication

Authentication is required using accountId and passcode headers.

# Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

# Region

Refer to [Regions](https://developer.clevertap.com/docs/common-api-components#region) to view region-specific sample endpoints

# HTTP Method

**POST**

# Body Parameters

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example",
    "h-4": "Required/Optional",
    "0-0": "identity",
    "0-1": "This indicates an identity to recognize a unique user. It can be the user’s email ID, phone number, or any other identifier that you are using to tag your users.",
    "0-2": "String",
    "0-3": "`(\"user@clevertap.com\" / user )`",
    "0-4": "Required  \n  \n(Either `identity` or `guid` is required)",
    "1-0": "clevertapId",
    "1-1": "Identify a unique user by CleverTap Global Object ID.",
    "1-2": "String",
    "1-3": "“12345678\"",
    "1-4": "Optional  \n(Either `identity` or `clevertapId` is required)",
    "2-0": "includeDefaults",
    "2-1": "This parameter determines if the response must return the default values of Variables that do not have set override.",
    "2-2": "Boolean",
    "2-3": "false (default)",
    "2-4": "Optional"
  },
  "cols": 5,
  "rows": 3,
  "align": [
    "left",
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


> 📘 Note
> 
> In case the identity or clevertapId does not exist, the API returns all defined Variables default values. New user profiles are not created.

## Sample Request

```curl curl
curl -X POST -d '{"identity":"user@clevertap.com","cleverTapId":"12345678","includeDefaults":false}' "https://in1.api.clevertap.com/1/getVars" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```

## Sample Response

```json
{
    "variables": {
      	"zFolderA.aVariable": 9.9,
        "zFolderA.AVariable": 10.0,
        "variableB": 9.9,
        "zFolderB.dummy": 10.1,
        "zFolderA.zFolder.dummy": 25.0,
        "variableA": 10.0},
    "status": "success"
}

```

## Errors

The Get Variables endpoint can return some of the following warnings or errors:

| Error Name                                         | Reason                                                                          |
| :------------------------------------------------- | :------------------------------------------------------------------------------ |
| Profile not found                                  | The user profile could not be located.                                          |
| Profile lookup is not available at the moment      | Access to user profile lookup is currently unavailable.                         |
| Blacklisted device                                 | This device is blacklisted.                                                     |
| Given identity is not in device profile identities | The provided identity does not match any identities associated with the device. |
| Something happened. Contact the support.           | An unexpected error. Contact support for assistance.                            |