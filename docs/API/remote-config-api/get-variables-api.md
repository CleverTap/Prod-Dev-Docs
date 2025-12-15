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
[https://in1.api.clevertap.com/1/getVars](https://in1.api.clevertap.com/1/getVars)

# Authentication

Authentication is required using accountId and passcode headers.

# Headers

Refer to [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

# Region

Refer to [Regions](https://developer.clevertap.com/docs/common-api-components#region) to view region-specific sample endpoints

# HTTP Method

**POST**

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
        identity
      </td>

      <td>
        This indicates an identity to recognize a unique user. It can be the user’s email ID, phone number, or any other identifier that you are using to tag your users.
      </td>

      <td>
        String
      </td>

      <td>
        `("user@clevertap.com" / user )`
      </td>

      <td>
        Required

        (Either `identity` or `guid` is required)
      </td>
    </tr>

    <tr>
      <td>
        clevertapId
      </td>

      <td>
        Identify a unique user by CleverTap Global Object ID.
      </td>

      <td>
        String
      </td>

      <td>
        “12345678"
      </td>

      <td>
        Optional  
        (Either `identity` or `clevertapId` is required)
      </td>
    </tr>

    <tr>
      <td>
        includeDefaults
      </td>

      <td>
        This parameter determines if the response must return the default values of Variables that do not have set override.
      </td>

      <td>
        Boolean
      </td>

      <td>
        false (default)
      </td>

      <td>
        Optional
      </td>
    </tr>
  </tbody>
</Table>

<Callout icon="📘" theme="info">
  **Note**

  In case the identity or clevertapId does not exist, the API returns all defined Variables default values. New user profiles are not created.
</Callout>

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
