---
title: Get User Details API
excerpt: Learn how to retrieve CleverTap users via SCIM.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

This API retrieves CleverTap user details. You can:

- Fetch a specific user by email address, or
- Retrieve a paginated list of users with optional SCIM-compliant filtering.

# HTTP Method

`GET`

# Endpoint

- List users: `/nx/v2/scim/v2/Users`
- Get user by email: `/nx/v2/scim/v2/Users/{email}`

# Base URL

The following is an example base URL for a CleverTap account in the United States region:

`https://<clevertap_domain>/nx/v2/scim/v2/Users`

Example: `https://us1.dashboard.clevertap.com/nx/v2/scim/v2/Users`

<Callout icon="🚧" theme="warn">
  #### Region-Specific

  The CleverTap domain is environment- and region-specific. Do not copy the example URL directly. Always replace `<clevertap_domain>` with the domain for your region. Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.
</Callout>

# Headers

The following headers must be included with every API request:

| Header          | Description                                                               | Required |
| :-------------- | :------------------------------------------------------------------------ | :------- |
| `Content-Type`  | Content type of the request payload.<br />Must be `application/scim+json` | Yes      |
| `Authorization` | `Bearer <SCIM_TOKEN>`                                                     | Yes      |

Replace `<SCIM_TOKEN>` with a valid SCIM Bearer token generated from the CleverTap dashboard. Refer to [Generate SCIM Token for API-Based Provisioning](doc:scim-user-provisioning-using-api#generate-scim-token-for-api-based-provisioning).

# Example Request (Get User by Email)

The following is the sample request:

```curl
curl -X GET /nx/v2/scim/v2/Users/john.doe@example.com \
-H "Authorization: Bearer <SCIM_TOKEN>" \
-H "Content-Type: application/scim+json"
```

# Path Parameters

The following is the list of path parameters:

| Parameter | Type   | Required | Description               |
| --------- | ------ | -------- | ------------------------- |
| `email`   | String | Yes      | Email address of the user |

# Example Response 1

The following is the sample response:

```json
{
  "id": "user-unique-id",
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
  "userName": "john.doe@example.com",
  "name": {
    "givenName": "John",
    "familyName": "Doe"
  },
  "active": true,
  "accounts": [
    {
      "accountId": "CLEVERTAP_ACCOUNT_ID",
      "roles": "Admin,Editor",
      "teams": "Engineering,Product",
      "status": "Active",
      "lastAccessed": 1735689600000
    }
  ],
  "invitedBy": "admin@example.com"
}

```

# Example Request 2 (List Users)

The following is the sample request with a query parameter:

```curl
curl -X GET "https://<clevertap_domain>/nx/v2/scim/v2/Users?startIndex=0&count=20" \
-H "Authorization: Bearer <SCIM_TOKEN>" \
-H "Content-Type: application/scim+json"
```

# Query Parameters

The following is the list of query parameters:

| Parameter    | Type   | Required | Description                                                            |
| ------------ | ------ | -------- | ---------------------------------------------------------------------- |
| `startIndex` | int    | No       | Zero-based index to start returning results (pagination).              |
| `count`      | int    | No       | Maximum number of users to return in the response.                     |
| `filter`     | string | No       | SCIM-compliant filter expression (for example, `userName eq "email"`). |

# Example Response 2

The following is the sample response:

```json
{
  "schemas": ["urn:ietf:params:scim:api:messages:2.0:ListResponse"],
  "totalResults": 50,
  "startIndex": 0,
  "itemsPerPage": 20,
  "resources": [
    {
      "id": "user-unique-id",
      "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
      "name": {
        "givenName": "John",
        "familyName": "Doe"
      },
      "active": true,
      "accounts": [
        {
          "accountId": "CLEVERTAP_ACCOUNT_ID",
          "roles": "Admin,Editor",
          "teams": "Engineering,Product",
          "status": "Active",
          "lastAccessed": 1735689600000
        }
      ],
      "invitedBy": "admin@example.com"
    }
  ]
}
```

# Response Parameters

The following is the list of response parameters:

| Parameter Path                     | Type            | Description                                                                       |
| ---------------------------------- | --------------- | --------------------------------------------------------------------------------- |
| `schemas`                          | Array of String | SCIM schema identifiers. Must include urn:ietf:params:scim:schemas:core:2.0:User. |
| `totalResults`                     | int             | Total number of matching users.                                                   |
| `startIndex`                       | int             | Index of the first returned result.                                               |
| `itemsPerPage`                     | int             | Number of users returned in the response.                                         |
| `resources`                        | Array           | List of user resources.                                                           |
| `resources[].id`                   | String          | Unique identifier of the user.                                                    |
| `resources[].active`               | Boolean         | Indicates whether the user is active.                                             |
| `resources[].invitedBy`            | String          | Email address of the user who invited this user. (Mandatory)                      |
| `resources[].name`                 | Object          | User name details.                                                                |
| `resources[].name.givenName`       | String          | First name.                                                                       |
| `resources[].name.familyName`      | String          | Last name.                                                                        |
| `resources[].accounts`             | Array           | Accounts associated with the user.                                                |
| `resources[].accounts[].accountId` | String          | CleverTap account identifier.                                                     |
| `resources[].accounts[].roles`     | String          | Comma-separated list of roles.                                                    |
| `resources[].accounts[].teams`     | String          | Comma-separated list of teams.                                                    |
| `resources[].accounts[].status`    | String          | User status in the account (`Active`, `Revoke`, `Delete`).                        |

# Error Codes

The following is the list of error codes and responses:

| Error Code | Description                                                                  | Sample Payload                                                                                                                                            |
| ---------- | ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `400`      | **Bad Request** – Invalid or malformed input (e.g., incorrect email format). | `json\n{\n  "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"],\n  "detail": "Invalid email format",\n  "status": 400\n}`                         |
| `401`      | **Unauthorized** – SCIM token is missing or invalid.                         | `json\n{\n  "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"],\n  "detail": "Unauthorized: Invalid token",\n  "status": 401\n}`                  |
| `403`      | **Forbidden** – You do not have permission to access this resource.          | `json\n{\n  "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"],\n  "detail": "Forbidden: Access denied",\n  "status": 403\n}`                     |
| `404`      | **Not Found** – User not found with the provided identifier.                 | `json\n{\n  "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"],\n  "detail": "User not found: john.doe@example.com",\n  "status": 404\n}`         |
| `429`      | **Too Many Requests** – Rate limit exceeded.                                 | `json\n{\n  "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"],\n  "detail": "Rate limit exceeded. Please try again later.",\n  "status": 429\n}` |
| `500`      | **Internal Server Error** – Something went wrong on the server.              | `json\n{\n  "schemas": ["urn:ietf:params:scim:api:messages:2.0:Error"],\n  "detail": "Unexpected server error",\n  "status": 500\n}`                      |
