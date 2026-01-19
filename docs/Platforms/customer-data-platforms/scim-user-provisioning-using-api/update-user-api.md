---
title: Update User API
excerpt: Learn how to update CleverTap users using the SCIM Users API.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

Updates an existing user’s details in CleverTap, including roles, teams, and account access. Use this endpoint to modify a user’s configuration across one or more accounts. This is a complete update operation, existing values are replaced with those in the request body.

# HTTP Method

`PUT`

# Endpoint

`/nx/v2/scim/v2/Users/{email}`

# Base URL

The following is an example base URL for a CleverTap account in the United States region:

`https://<clevertap_domain>/nx/v2/scim/v2/Users/{email}`

Example: `https://us1.dashboard.clevertap.com/nx/v2/scim/v2/Users/{email}`

<Callout icon="🚧" theme="warn">
  #### Region-Specific

  The CleverTap domain is environment- and region-specific. Do not copy the example URL directly. Always replace `<clevertap_domain>` with the domain for your region. Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.
</Callout>

# Authentication

This endpoint requires Bearer token authentication. Include the token in the Authorization header for all requests.

## Required Headers

The following headers must be included with every API request:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Header
      </th>

      <th>
        Description
      </th>

      <th>
        Required
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `Content-Type`
      </td>

      <td>
        Content type of the request payload.  
        Must be `application/scim+json`
      </td>

      <td>
        Yes
      </td>
    </tr>

    <tr>
      <td>
        `Authorization`
      </td>

      <td>
        `Bearer <SCIM_TOKEN>`
      </td>

      <td>
        Yes
      </td>
    </tr>
  </tbody>
</Table>

Replace `<SCIM_TOKEN>` with a valid SCIM Bearer token generated from the CleverTap dashboard. Refer to [Generate SCIM Token for API-Based Provisioning](doc:scim-user-provisioning-using-api#generate-scim-token-for-api-based-provisioning).

# Path Parameters

The following is the path parameter and its description:

| Parameter | Type   | Required | Description                                                                                                                |
| --------- | ------ | -------- | -------------------------------------------------------------------------------------------------------------------------- |
| `email`   | String | Yes      | Email address of the user to update. This value uniquely identifies the user and overrides `userName` in the request body. |

# Example Request

The following is the sample request:

```curl
curl -X PUT https://<clevertap_domain>/nx/v2/scim/v2/Users/{email}
-H "Authorization: Bearer <SCIM_TOKEN>"
-H "Content-Type: application/scim+json"
{
  "name": {
    "givenName": "John",
    "familyName": "Doe-Smith"
  },
  "active": true,
  "invitedBy": "admin@example.com",
  "accounts": [
    {
      "accountId": "CLEVERTAP_ACCOUNT_ID",
      "roles": "Editor",
      "teams": "Marketing,Engineering,Product",
      "status": "Active"
    },
    {
      "accountId": "CLEVERTAP_ACCOUNT_ID",
      "roles": "Editor",
      "teams": "Marketing,Engineering,Product",
      "status": "Revoke"
    }
  ]
}
```

# Request Parameters

The following table lists the request parameters:

| Parameter            | Type            | Required | Description                                                            |
| -------------------- | --------------- | -------- | ---------------------------------------------------------------------- |
| `name`               | Object          | No       | User’s name details.                                                   |
| `name.givenName`     | String          | No       | User’s first name.                                                     |
| `name.familyName`    | String          | No       | User’s last name.                                                      |
| `active`             | Boolean         | No       | Indicates whether the user is active.                                  |
| `invitedBy`          | String          | Yes      | Email address of the user who invited this user.                       |
| `accounts`           | Array of Object | No       | List of account-level role, team, and status assignments for the user. |
| `accounts.accountId` | String          | Yes      | Unique identifier of the account.                                      |
| `accounts.roles`     | String          | No       | Comma-separated list of roles assigned to the user for the account.    |
| `accounts.teams`     | String          | No       | Comma-separated list of teams the user belongs to for the account.     |
| `accounts.status`    | String          | No       | Account access status. Supported values include `Active` and `Revoke`. |

<Callout icon="🚧" theme="warn">
  **Important**

  * All fields in the request body replace existing values (full update, not partial)
</Callout>

# Example Response

The following is the sample response:

```json
{
  "id": "user-unique-id",
  "userName": "john.doe@example.com",
  "name": {
    "givenName": "John",
    "familyName": "Doe-Smith"
  },
  "active": true,
  "accounts": [
    {
      "accountId": "CLEVERTAP_ACCOUNT_ID",
      "roles": "Editor",
      "teams": "Marketing,Engineering,Product",
      "status": "Active"
    },
    {
      "accountId": "CLEVERTAP_ACCOUNT_ID",
      "roles": "Editor",
      "teams": "Marketing,Engineering,Product",
      "status": "Revoke"
    }
  ],
  "invitedBy": "admin@example.com"
}

```

# Response Parameters

The following table lists the response parameters returned by the API.

| Parameter            | Type            | Description                                                                       |
| -------------------- | --------------- | --------------------------------------------------------------------------------- |
| `id`                 | String          | Unique identifier of the user.                                                    |
| `userName`           | String          | User’s email address.                                                             |
| `name`               | Object          | User’s name details.                                                              |
| `name.givenName`     | String          | User’s first name.                                                                |
| `name.familyName`    | String          | User’s last name.                                                                 |
| `active`             | Boolean         | Indicates whether the user is active.                                             |
| `accounts`           | Array of Object | List of account-level role, team, and status assignments for the user.            |
| `accounts.accountId` | String          | Unique identifier of the account.                                                 |
| `accounts.roles`     | String          | Comma-separated list of roles assigned to the user for the account.               |
| `accounts.teams`     | String          | Comma-separated list of teams the user belongs to for the account.                |
| `accounts.status`    | String          | Account access status. Supported values include `Active`, `Revoke`, and `Delete`. |
| `invitedBy`          | String          | Email address of the user who invited this user.                                  |

# Error Codes

All errors follow the SCIM v2.0 error schema and return standard HTTP status codes.

| Error Code | Description                                                               | Sample Payload                                                                                                                                                                                     |
| ---------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **400**    | One or more fields contain invalid values (e.g., roles, teams, or status) | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account[ACC123].roles: Invalid role names present: Creator1", "status": 400 } `                                     |
| **400**    | Invalid `status` value (must be `Active`, `Revoke`, or `Delete`)          | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account[ACC456].status: Invalid status", "status": 400 } `                                                          |
| **400**    | `teams` specified but Teams feature is not enabled                        | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account[ACC123].teams: Teams are not enabled for this account. Please remove teams from payload", "status": 400 } ` |
| **400**    | One or more team names are invalid                                        | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "Invalid teams: team4, team5 do not exist", "status": 400 } `                                                        |
| **403**    | `invitedBy` user does not have admin permissions                          | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "User does not have admin permissions to invite user", "status": 403 } `                                             |
| **404**    | User not found using the provided `{email}` path parameter                | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "User not found: john.doe@example.com", "status": 404 } `                                                            |
| **500**    | Internal server error while updating the user                             | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "Something went wrong while updating user", "status": 500 } `                                                        |

## Supported Status Values

| Value    | Description                   |
| -------- | ----------------------------- |
| `Active` | Grants access to the account. |
| `Revoke` | Temporarily suspends access.  |
| `Delete` | Mark's access for deletion.   |
