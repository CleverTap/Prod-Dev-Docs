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

The Delete Variables API allows you to delete Remote Config variables individually by name, or all at once.

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/deleteVars`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

Provide either `deleteAllVars: true` to delete all variables, or a `variableNames` array to delete specific variables. You do not need to provide both.

If both are provided, `deleteAllVars: true` takes effect and all variables are deleted regardless of the `variableNames` list.

The following table lists the body parameters.

| Parameter     | Description                                                                                                                                                                                                                                 | Type             | Example                                       | Required/Optional                           |
| :------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :--------------- | :-------------------------------------------- | :------------------------------------------ |
| deleteAllVars | When `true`, deletes all variables in your Remote Config. When `deleteAllVars: true` is set, `variableNames` is not needed.                                                                                                                 | boolean          | `{"deleteAllVars": true}`                     | Optional                                    |
| variableNames | List of variable names to delete. Supports dot-notation names (for example, `"folder1.var2"`). Non-existent names are returned in `problematicVariables` rather than causing an error. Required when `deleteAllVars` is `false` or omitted. | array of strings | `{"variableNames": ["var1", "folder1.var2"]}` | Required when `deleteAllVars` is not `true` |

<Callout icon="⚠️" theme="warning">
  ### **All Names Non-Existent**

  If `deleteAllVars` is `false` or omitted and every name in `variableNames` does not exist, the API returns HTTP 400 with a `LIVE_VERSION_NOT_CHANGED` validation error. A caller sending a list of already-deleted variable names will receive a 400, not a 200 with all names in `problematicVariables`.
</Callout>

## Example Request

The following is a sample request to the _Delete Variables_ API, showing the headers needed to authenticate the request.

```curl
curl -X POST "https://<region>.api.clevertap.com/1/deleteVars" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8" \
-d '{"variableNames":["var1","folder1.var2","non.existing"]}'
```

## Example Response

The following is a sample response.

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

The following table describes the response fields.

| Field                | Type             | Description                                                                                   |
| :------------------- | :--------------- | :-------------------------------------------------------------------------------------------- |
| status               | string           | `"success"` on a successful request.                                                          |
| deletedVariables     | array of strings | Names of variables that were successfully deleted.                                            |
| problematicVariables | array of strings | Names from `variableNames` that could not be deleted, for example, because they do not exist. |

<Callout icon="📘" theme="info">
  ### **Draft Only**

  This API only modifies the current draft version of your Remote Config. Deleted variables remain live until the draft is published from the CleverTap dashboard.
</Callout>

<Callout icon="⚠️" theme="warning">
  ### **Draft Conflict**

  If another user already has an unpublished draft, the API returns the error "Draft of another user already exists." Only one draft can exist at a time. Publish or discard the existing draft before making further changes.
</Callout>

## Error Codes

The following is the list of error codes that may be returned by the API:

| HTTP Status | Error                                        | Description                                                                     | Example Error Response                                                            |
| ----------- | -------------------------------------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| 400         | `"Incomplete request body"`                  | Request body could not be read                                                  | `{"status":"fail","error":"Incomplete request body","code":400}`                  |
| 400         | `"Failed to process request."`               | Unexpected error during request setup                                           | `{"status":"fail","error":"Failed to process request.","code":400}`               |
| 401         | `"Invalid credentials"`                      | Account ID not found or passcode is incorrect                                   | `{"status":"fail","error":"Invalid credentials","code":401}`                      |
| 403         | `"API access has been temporally suspended"` | Account's API access has been suspended                                         | `{"status":"fail","error":"API access has been temporally suspended","code":403}` |
| 429         | `"Too many concurrent requests"`             | Per-account concurrent request limit exceeded. Retry after 120 seconds.         | `{"status":"fail","error":"Too many concurrent requests","code":429}`             |
| 500         | `"Please try again"`                         | Dashboard service timed out after 3 retry attempts                              | `{"status":"fail","error":"Please try again","code":500}`                         |
| 500         | `"Failed to process request"`                | Unexpected critical server-side failure                                         | `{"status":"fail","error":"Failed to process request","code":500}`                |
| 503         | `"12 digit account ID mandatory."`           | `X-CleverTap-Account-Id` header is missing from the request                     | `{"status":"fail","error":"12 digit account ID mandatory.","code":503}`           |
| 503         | `"Please come back later"`                   | Server at global capacity limit or warming up. Retry after at least 30 seconds. | `{"status":"fail","error":"Please come back later","code":503}`                   |

<br />

<br />

<br />
