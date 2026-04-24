---
title: Delete Catalog API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Deletes an existing catalog. This API permanently removes the catalog from the system.

# Method

PUT

# Endpoint

`PUT /v2/catalogs/delete/{id}`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/delete/{id}`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and correct request handling.

| Header              | Description                                                      | Required |
| ------------------- | ---------------------------------------------------------------- | -------- |
| `Content-Type`      | Content type of the request payload. Must be `application/json`. | Yes      |
| `X-Account-Id`      | CleverTap account ID associated with the catalog.                | Yes      |
| `X-CleverTap-Token` | API token used to authenticate the request.                      | Yes      |

# Path Parameters

The following path parameter is required to identify the catalog to delete:

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| `id`      | Number | Yes      | Unique identifier of the catalog to delete. |

# Query Parameters

The following query parameter is required to perform the delete operation:

| Parameter   | Type   | Required | Description                                                       |
| ----------- | ------ | -------- | ----------------------------------------------------------------- |
| `deletedBy` | String | Yes      | Identifier of the user or system performing the delete operation. |

# Example Request

This example performs a hard delete on the catalog with ID `123`.

```
PUT /v2/catalogs/delete/123?deletedBy=admin@example.com
```

# Example Response

The following is the sample response:

```json
{
  "message": "Catalog - 123 deleted successfully"
}
```

# Response Parameters

The following table shows the response parameter:

| Parameter | Type   | Description                                                                |
| --------- | ------ | -------------------------------------------------------------------------- |
| message   | string | Confirmation message indicating that the catalog was deleted successfully. |

# Error Codes

The following error codes may be returned by this API.

| Error Code                   | Description                                                                                |
| ---------------------------- | ------------------------------------------------------------------------------------------ |
| `CATALOG_NOT_FOUND`          | The catalog specified by the ID does not exist.                                            |
| `CATALOG_DELETE_NOT_ALLOWED` | The catalog cannot be deleted due to constraints (for example, hard delete not permitted). |
| `CATALOG_INVALID_REQUEST`    | One or more request parameters are invalid or missing (for example, missing `deletedBy`).  |
| `AUTHENTICATION_FAILED`      | Authentication failed due to invalid or missing token.                                     |
| `AUTHORIZATION_FAILED`       | Caller does not have permission to delete this catalog.                                    |
| `INTERNAL_ERROR`             | An unexpected server-side error occurred.                                                  |
