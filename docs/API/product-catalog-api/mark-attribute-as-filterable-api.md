---
title: Mark Attribute as Filterable API
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

Marks a specific catalog attribute as filterable or non-filterable.

# Method

PATCH

# Endpoint

`PATCH /v2/catalogs/{catalogId}/schema/attributes/{attributeName}/filterable`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{catalogId}/schema/attributes/{attributeName}/filterable`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and correct request handling.

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

# Path Parameters

The following path parameters are required to identify the catalog attribute to update:

| Parameter       | Type   | Required | Description                                     |
| --------------- | ------ | -------- | ----------------------------------------------- |
| `catalogId`     | long   | Yes      | Unique identifier of the catalog.               |
| `attributeName` | string | Yes      | Name of the attribute that needs to be updated. |

# Query Parameters

The following query parameters are required to update the attribute's filterable status:

| Parameter    | Type    | Required | Description                                                                               |
| ------------ | ------- | -------- | ----------------------------------------------------------------------------------------- |
| `filterable` | boolean | Yes      | Sets whether the attribute can be used in filters. Accepted values are `true` or `false`. |
| `updatedBy`  | string  | Yes      | Email address of the user performing the update.                                          |

# Example Request

The following is the sample request:

```json
PATCH /v2/catalogs/123/schema/attributes/brand/filterable?filterable=true&updatedBy=admin@example.com
```

# Example Response

This example response indicates the catalog file has been accepted, and processing has started asynchronously.

```json
{
  "id": 123,
  "catalogName": "electronics_catalog",
  "catalogSchema": [
    {
      "name": "brand",
      "type": "string",
      "filterable": true
    },
    {
      "name": "price",
      "type": "number",
      "filterable": true
    }
  ],
  "totalProducts": 1500,
  "status": "ACTIVE",
  "createdAt": 1699564800,
  "createdBy": "admin@example.com",
  "updatedBy": "admin@example.com",
  "isActive": true,
  "attributeCount": 2,
  "totalColumns": 5
}
```

# Response Parameters

The response includes additional system-generated fields returned after the attribute is updated.

| Parameter        | Type    | Description                                                                        |
| ---------------- | ------- | ---------------------------------------------------------------------------------- |
| `id`             | integer | Unique identifier of the catalog.                                                  |
| `totalProducts`  | integer | Total number of products currently available in the catalog.                       |
| `createdAt`      | integer | Time when the catalog was created. The value is returned as a Unix timestamp.      |
| `createdBy`      | string  | Email address of the user who created the catalog.                                 |
| `isActive`       | boolean | Indicates whether the catalog is currently active.                                 |
| `attributeCount` | integer | Total number of attributes currently defined in the catalog.                       |
| `totalColumns`   | integer | Total number of columns available in the catalog, including system-defined fields. |

# Error Codes

The following error codes may be returned by this API:

| Error Code         | Description                                                                                                                                                        |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| INVALID_REQUEST    | The request is invalid or missing required parameters. For example, `catalogId`, `attributeName`, `filterable`, or `updatedBy` is missing or has an invalid value. |
| NOT_FOUND          | The specified catalog or attribute does not exist.                                                                                                                 |
| UPDATE_NOT_ALLOWED | The attribute cannot be updated due to a business rule. For example, the catalog is inactive or locked.                                                            |
| UNAUTHORIZED       | The user is not authorized to perform this update.                                                                                                                 |
| FORBIDDEN          | The user does not have permission to modify this catalog.                                                                                                          |
| INTERNAL_ERROR     | An unexpected error occurred on the server. Try again later.                                                                                                       |