---
title: Add User API
excerpt: Learn how to create and manage CleverTap users using the SCIM Users API.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

Adds a new user to one or more CleverTap accounts. If the user already exists in any of the accounts specified in the `POST` request, the request fails with an error message indicating that the user already exists. Only add users to accounts where they are not already present.

# HTTP Method

`POST`

# Endpoint

`/nx/v2/scim/v2/Users`

# Base URL

The following is an example base URL for a CleverTap account in the United States region:

`https://<clevertap_domain>/nx/v2/scim/v2/Users`

Example: `https://us1.dashboard.clevertap.com/nx/v2/scim/v2/Users`

<Callout icon="🚧" theme="warn">
  #### Region-Specific

  The CleverTap domain is environment- and region-specific. Do not copy the example URL directly. Always replace `<clevertap_domain>` with the domain for your region. Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.
</Callout>

# Headers

The following headers are required for every request.

| Header          | Description                                                          | Required |
| --------------- | -------------------------------------------------------------------- | -------- |
| `Content-Type`  | Content type of the request payload. Must be `application/scim+json` | Yes      |
| `Authorization` | `Bearer <SCIM_TOKEN>`                                                | Yes      |

Replace `<SCIM_TOKEN>` with a valid SCIM Bearer token generated from the CleverTap dashboard. Refer to [Generate SCIM Token for API-Based Provisioning](doc:scim-user-provisioning-using-api#generate-scim-token-for-api-based-provisioning).

# Example Request

The following is the sample request:

```curl
curl -X POST https://<clevertap_domain>/nx/v2/scim/v2/Users
-H "Authorization: Bearer <SCIM_TOKEN>"
-H "Content-Type: application/scim+json"
{
"schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
"userName": "jane.smith@example.com",
  "name": {
    "givenName": "Jane",
    "familyName": "Smith"
  },
  "active": true,
  "invitedBy": "admin@example.com",
  "accounts": [
    {
      "accountId": "CLEVERTAP_ACCOUNT_ID",
      "roles": "Editor",
      "teams": "Marketing",
      "status": "Active"
    }
  ]
}
```

# Request Parameters

The following table lists the request body parameters.

| Parameter            | Type            | Required    | Description                                                                                                                            |
| :------------------- | :-------------- | :---------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| `schemas`            | Array of String | No          | SCIM schema identifiers. Must include urn:ietf:params:scim:schemas:core:2.0:User.                                                      |
| `userName`           | String          | No          | User’s email address.                                                                                                                  |
| `name`               | Object          | Yes         | Contains the user’s name attributes.                                                                                                   |
| `name.givenName`     | String          | Yes         | Specifies the user’s first name.                                                                                                       |
| `name.familyName`    | String          | Yes         | Specifies the user’s last name.                                                                                                        |
| `active`             | Boolean         | Yes         | Indicates whether the user account is active. Must be set to `true` in `POST` requests.                                                |
| `invitedBy`          | String          | Yes         | Email address of the user who invited this user. The inviter must be an **admin**.                                                     |
| `accounts`           | List (Object)   | Yes         | Contains account-level associations for the user. This field is required; the request will fail if omitted.                            |
| `accounts.accountId` | String          | Yes         | Specifies the CleverTap account identifier associated with the user.                                                                   |
| `accounts.roles`     | String          | Yes         | Comma-separated list of role names assigned to the user for the account.                                                               |
| `accounts.teams`     | String          | Conditional | Comma-separated list of team names the user belongs to for the account. Required only if your organization uses the **Teams** feature. |
| `accounts.status`    | String          | Yes         | Specifies the user’s access status for the account. Supported values: `Active`, `Delete`, and `Revoke`.                                |

## Notes

* You must use valid team names and roles.
* At least one account entry must include a status set to `Active`.
* The user specified in `invitedBy` must have admin permissions.
* Multiple accounts may be included only when they belong to the same parent-child structure.
* Validation is strictly applied for email, account information, roles, and teams.

# Example Response

The following is the sample response:

```
{
  "id": "user-unique-id",
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
  "userName": "jane.smith@example.com",
  "name": {
    "givenName": "Jane",
    "familyName": "Smith"
  },
  "active": true,
  "accounts": [
    {
      "accountId": "CLEVERTAP_ACCOUNT_ID",
      "roles": "Editor",
      "teams": "Marketing",
      "status": "Active"
    }
  ],
  "invitedBy": "admin@example.com"
}
```

# Response Parameters

The following table lists the response parameters.

| Parameter            | Type            | Description                                                                         |
| -------------------- | --------------- | ----------------------------------------------------------------------------------- |
| `id`                 | String          | Specifies the unique identifier of the user generated by the system.                |
| `schemas`            | Array of String | SCIM schema identifiers. Must include `urn:ietf:params:scim:schemas:core:2.0:User`. |
| `userName`           | String          | User’s email address.                                                               |
| `name`               | Object          | Contains the user’s name attributes.                                                |
| `name.givenName`     | String          | Specifies the user’s first name.                                                    |
| `name.familyName`    | String          | Specifies the user’s last name.                                                     |
| `active`             | Boolean         | Indicates whether the user account is active.                                       |
| `accounts`           | List (Object)   | Contains account-level roles, teams, and status assignments for the user.           |
| `accounts.accountId` | String          | Specifies the CleverTap account identifier associated with the user.                |
| `accounts.roles`     | String          | Specifies the role assigned to the user for the account.                            |
| `accounts.teams`     | String          | Specifies the team to which the user belongs for the account.                       |
| `accounts.status`    | String          | Specifies the user’s access status for the account.                                 |
| `invitedBy`          | String          | Specifies the email address of the user who invited this user.                      |

# Error Codes

The following table lists possible error responses.

| Error Code | Description                                       | Sample Payload                                                                                                                                                                                     |
| ---------- | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `400`      | `active` is set to false                          | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "active: User must be active for provisioning", "status": 400 } `                                                    |
| `400`      | `name` object is missing                          | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "name: Cannot be null", "status": 400 } `                                                                            |
| `400`      | `givenName` or `familyName` is missing or empty   | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "givenName: Cannot be empty, familyName: Cannot be empty", "status": 400 } `                                         |
| `400`      | `accounts` is missing                             | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account: Cannot be null", "status": 400 } `                                                                         |
| `400`      | `accounts` array is empty                         | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account: Cannot be empty", "status": 400 } `                                                                        |
| `400`      | Invalid `accountId` provided                      | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account[ACC123].accountId: Invalid account", "status": 400 } `                                                      |
| `400`      | `roles` not provided or empty                     | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account[ACC123].roles: Cannot be empty", "status": 400 } `                                                          |
| `400`      | One or more role names are invalid.               | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account[ACC123].roles: Invalid role names present: Creator1", "status": 400 } `                                     |
| `400`      | `teams` passed but Teams feature is not enabled.  | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account[ACC123].teams: Teams are not enabled for this account. Please remove teams from payload", "status": 400 } ` |
| `400`      | `status` not passed or empty.                     | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account[ACC123].status: Cannot be empty", "status": 400 } `                                                         |
| `400`      | Invalid `status` value                            | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "account[ACC123].status: Invalid status", "status": 400 } `                                                          |
| `400`      | No account has status `Active`                    | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "General: At least one account must have Active status", "status": 400 } `                                           |
| `400`      | `invitedBy` is missing or empty                   | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "General: Invited by user cannot be empty", "status": 400 } `                                                        |
| `400`      | One or more teams do not exist.                   | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "Invalid teams: team4, team5 do not exist", "status": 400 } `                                                        |
| `401`      | Missing or invalid authentication token           | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "Authentication failed: Invalid or missing bearer token", "status": 401 } `                                          |
| `403`      | `invitedBy` user does not have Admin permissions. | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "General: User does not have admin permissions to invite user", "status": 403 } `                                    |
| `409`      | User already exists in one or more accounts.      | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "User already exists in accounts: ACC123", "status": 409 } `                                                         |
| `429`      | Rate limit exceeded                               | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "Rate limit exceeded. Please retry after 60 seconds", "status": 429 } `                                              |
| `500`      | Internal server error                             | `json { "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"], "detail": "An internal error occurred. Please contact support", "status": 500 } `                                              |
