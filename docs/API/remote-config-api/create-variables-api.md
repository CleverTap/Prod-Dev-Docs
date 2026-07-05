---
title: Create Variables API
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

The Create Variables API allows you to define variables with valid names, types, and default values. Variable names support dot-separated folder grouping (for example, `"ui.colors.primary"` creates the variable `primary` inside a `ui > colors` folder hierarchy). Variables defined via this API must be published from the CleverTap dashboard before they take effect.

## Base URL

The following is a sample base URL:

`https://<region>.api.clevertap.com/1/createVars`

For region-specific endpoints, refer to [Region](https://developer.clevertap.com/docs/common-api-components#region).

## HTTP Method

POST

## Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

## Body Parameters

The request body contains a `variableDefinitions` object. Each key in this object is a variable name, and its value is the variable definition.

The following table lists the parameters for each variable definition.

| Object              | Parameter           | Description                                                                                                                                                                                                                                      | Type                                        | Required/Optional                                                            |
| :------------------ | :------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------ | :--------------------------------------------------------------------------- |
| variableDefinitions | Variable name (key) | The name of the variable. Supports dot-separated folder notation (for example, `"folder1.myVar"`). Maximum 256 characters. Cannot be empty, start with a dot, end with a dot, or contain consecutive dots.                                       | string                                      | Required                                                                     |
|                     | type                | The type of the variable. Accepted values: `"string"`, `"boolean"`, `"number"`.  If omitted, type is inferred from `defaultValue`: a boolean JSON value infers `"boolean"`, a numeric value infers `"number"`, a string value infers `"string"`. | string                                      | Optional when `defaultValue` is provided (type inferred); Required otherwise |
|                     | defaultValue        | The default value of the variable. Maximum 400,000 characters for string variables. Required for `"string"`, `"boolean"`, and `"number"` types. Not applicable for `"file"` type.                                                                | Depends on type: string, boolean, or number | Required for string/boolean/number                                           |
|                     | description         | A description of the variable. Maximum 512 characters.                                                                                                                                                                                           | string                                      | Optional                                                                     |

The following is a sample payload showing multiple variable definitions, including folder-structured names.

```json
{
  "variableDefinitions": {
    "welcome_message": {
      "type": "string",
      "defaultValue": "Hello!",
      "description": "App welcome message"
    },
    "show_banner": {
      "type": "boolean",
      "defaultValue": true
    },
    "ui.colors.primary": {
      "type": "string",
      "defaultValue": "#FF5733",
      "description": "Primary brand color"
    },
    "max_retries": {
      "defaultValue": 3
    }
  }
}
```

Note that `max_retries` above omits `type` — the API infers `"number"` from the numeric `defaultValue`.

## Example Request

The following is a sample request to the _Create Variables_ API, showing the headers needed to authenticate the request.

```curl
curl -X POST "https://<region>.api.clevertap.com/1/createVars" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8" \
-d '{
  "variableDefinitions": {
    "welcome_message": {
      "type": "string",
      "defaultValue": "Hello!",
      "description": "App welcome message"
    },
    "show_banner": {
      "type": "boolean",
      "defaultValue": true
    }
  }
}'
```

## Example Response

The following is a sample response.

```json
{
    "status": "success",
    "warning": ""
}
```

<Callout icon="📘" theme="info">
  ### **Note**

  Newly defined variables must be published from the CleverTap dashboard for the changes to persist.
</Callout>

<Callout icon="⚠️" theme="warning">
  ### **Draft Conflict**

  If another user already has an unpublished draft, the API returns an error: "Draft of another user already exists." Publish or discard the existing draft before creating new variables.
</Callout>

## Error Codes

The following is the list of error codes returned by the API:

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
