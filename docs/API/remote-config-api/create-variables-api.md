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

The Create API allows you to define variables with valid names, types, and default values.

# Endpoint

Here is a sample endpoint from the account in the India region:  
[https://in1.api.clevertap.com/1/createVars](https://in1.api.clevertap.com/1/createVars)

# Authentication

Authentication is required using accountId and passcode headers.

# Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

# Region

Refer to [Regions](https://developer.clevertap.com/docs/common-api-components#region) to view region-specific sample endpoints

# HTTP Method

The HTTP method is **POST** for the Create Variables API.

## Body Parameters

| Object              | Parameter    | Description                       | Type                    | Required/Optional |
| :------------------ | :----------- | :-------------------------------- | :---------------------- | :---------------- |
| variableDefinitions | type         | The type of the Variable          | String, Boolean, Number | Required          |
|                     | description  | The description of the Variable   | String                  | Optional          |
|                     | defaultValue | The default value of the variable | String, Boolean, Number | Required          |

```json JSON
{
  "variableDefinitions": {
    "variableName": {
      "type": string ("string", "boolean", "number"),
      "description": string ("New Variable used for controlling XYZ"),
      "defaultValue": (type depends on the type)
    },
    
    }
  }

```

## Sample Request

```json json
curl -X POST -d '{"variableNAmes":["var1","folder1.var2"]}' "https://in1.api.clevertap.com/1/createVars" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```

## Sample Response

```json
{
    "status": "success",
    "warning": ""
}


```

<Callout icon="📘" theme="info">
  **Note**

  Newly defined Variables need to be Published from the Dashboard in order for it to persist.
</Callout>
