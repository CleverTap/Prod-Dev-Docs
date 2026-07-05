---
title: Get Variables API
excerpt: Learn how get all the variables for your Remote Config or AB Tests.
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

The Get Variables API retrieves variables and their values for the current user or device. The API returns default values or values associated with all users in the following scenarios:

- If the profile or CleverTap ID does not exist. No new profiles or devices are created.
- If the user segment contains any of the following user properties:
  - Geography
  - App Fields: OS, App Version, SDK Version, Make
  - Technographics: Browser, Device, OS

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/getVars`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The following table lists the body parameters. Provide either `identity` or `clevertapId` to identify the user.

| Parameter       | Description                                                                                                                                                                                                     | Type    | Example                | Required/Optional                                                                          |
| :-------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------ | :--------------------- | :----------------------------------------------------------------------------------------- |
| identity        | An identity to recognize a unique user. Can be the user's email address, phone number, or any other identifier used to tag your users.                                                                          | string  | `"user@clevertap.com"` | Required if `clevertapId` is not provided. Either `identity` or `clevertapId` is required. |
| clevertapId     | The CleverTap Global Object ID for the user. When both `identity` and `clevertapId` are provided, `clevertapId` takes precedence for the profile lookup and `identity` is used only for post-lookup validation. | string  | `"12345678"`           | Required if `identity` is not provided. Either `identity` or `clevertapId` is required.    |
| includeDefaults | When `true`, the response includes variables that have only default values (no user-specific overrides). When `false` (default), variables with only default values are omitted.                                | boolean | `false`                | Optional. Defaults to `false`.                                                             |

<Callout icon="📘" theme="info">
  ### **Note**

  If the `identity` or `clevertapId` does not exist, the API returns all defined variable default values. New user profiles are not created.
</Callout>

## Example Request

The following are sample requests to the _Get Variables_ API. Provide `identity` or `clevertapId`, but not both unless you intend `clevertapId` to take precedence.

```curl Using identity
curl -X POST "https://<region>.api.clevertap.com/1/getVars" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8" \
-d '{"identity":"user@clevertap.com","includeDefaults":false}'
```
```curl Using clevertapId
curl -X POST "https://<region>.api.clevertap.com/1/getVars" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8" \
-d '{"clevertapId":"12345678","includeDefaults":false}'
```

## Example Response

The following is a sample success response.

```json
{
    "status": "success",
    "variables": {
        "zFolderA.aVariable": 9.9,
        "zFolderA.AVariable": 10.0,
        "variableB": 9.9,
        "zFolderB.dummy": 10.1,
        "zFolderA.zFolder.dummy": 25.0,
        "variableA": 10.0
    }
}
```

The following is a sample warning response. The request succeeds (HTTP 200) and variables are returned, but a `warning` field is included when the provided identity does not match the identities associated with the located profile.

```json
{
    "status": "success",
    "variables": {
        "variableA": 10.0
    },
    "warning": "Given identity is not in device profile identities."
}
```

The following is a sample error response. Variables are not returned when a hard error occurs.

```json
{
    "status": "error",
    "error": "No variables were found. Please ensure that you have a live version and retry."
}
```

## Response Fields

The following table describes the response fields.

| Field     | Type   | Description                                                                                                                                  |
| :-------- | :----- | :------------------------------------------------------------------------------------------------------------------------------------------- |
| status    | string | `success` for successful requests and warning responses. `error` when a hard error occurs.                                                   |
| variables | object | Map of variable names to their current values for the user. Present on success and warning responses. Absent on error responses.             |
| warning   | string | Warning message. Present when `identity` does not match the located profile's identities. Variables are still returned alongside this field. |
| error     | string | Error message. Present when a hard error occurs. Variables are not returned alongside this field.                                            |

## Errors and Warnings

The following table describes warnings returned in HTTP 200 responses. Variables are still returned in the response.

| Warning Message                                     | Reason                                                                                                                   |
| :-------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------- |
| Given identity is not in device profile identities. | The provided `identity` does not match the identities associated with the located profile. Variables are still returned. |

The following table describes errors that prevent variables from being returned.

| HTTP Status | Error                                        | Description                                                                     | Example Error Response                                                            |
| ----------- | -------------------------------------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| 400         | `"Failed to parse request."`                 | Request body is not valid JSON                                                  | `{"status":"fail","error":"Failed to parse request.","code":400}`                 |
| 400         | `"Failed to process request."`               | Unexpected error during request setup                                           | `{"status":"fail","error":"Failed to process request.","code":400}`               |
| 401         | `"Invalid credentials"`                      | Account ID not found or passcode is incorrect                                   | `{"status":"fail","error":"Invalid credentials","code":401}`                      |
| 403         | `"API access has been temporally suspended"` | Account's API access has been suspended                                         | `{"status":"fail","error":"API access has been temporally suspended","code":403}` |
| 503         | `"12 digit account ID mandatory."`           | `X-CleverTap-Account-Id` header is missing from the request                     | `{"status":"fail","error":"12 digit account ID mandatory.","code":503}`           |
| 503         | `"Please come back later"`                   | Server at global capacity limit or warming up. Retry after at least 30 seconds. | `{"status":"fail","error":"Please come back later","code":503}`                   |

