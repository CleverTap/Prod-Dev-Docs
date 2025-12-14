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

Here is a sample endpoint from the account in the India region:\
[https://in1.api.clevertap.com/1/deleteVars](https://in1.api.clevertap.com/1/deleteVars)

# Authentication

Authentication is required using accountId and passcode headers.

# Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

# Region

Refer to [Regions](https://developer.clevertap.com/docs/common-api-components#region) to view region-specific sample endpoints

# HTTP Method

The HTTP method is **POST** for the Delete Variables API

# Body Parameters

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Example
      </th>

      <th>
        Required/Optional
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        deleteAllVars
      </td>

      <td>
        Determines whether you want to delete all variables in your Remote Config.
      </td>

      <td>
        Boolean
      </td>

      <td>
        \{\
          "deleteAllVars": false\
        }
      </td>

      <td>
        Optional
      </td>
    </tr>

    <tr>
      <td>
        variableNames
      </td>

      <td>
        Contains a list of variable names you want to delete from your Remote Config.
      </td>

      <td>
        String
      </td>

      <td>
        \{\
        "variableNames": ["var1", "folder1.var2", "non.existing"]\
        }
      </td>

      <td>
        Required
      </td>
    </tr>
  </tbody>
</Table>

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
