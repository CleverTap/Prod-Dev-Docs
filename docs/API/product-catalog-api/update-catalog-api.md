---
title: Update Catalog API
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

Updates the metadata, schema, and sync configuration of an existing catalog. This API performs a full update, so all fields that should remain unchanged must be explicitly provided in the request.

This API can also be used to update or remove recurring sync configurations for external feeds.

# Method

PUT

# Endpoint

`PUT /v2/catalogs/{id}`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{id}`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and validate the request.

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

# Path Parameter

The following parameters define how an existing catalog is updated:

| Parameter | Type | Required | Description                                 |
| --------- | ---- | -------- | ------------------------------------------- |
| `id`      | Long | Yes      | Unique identifier of the catalog to update. |

## Request Parameters

The following request body parameters define how an existing catalog is updated:

| Parameter                  | Type    | Required | Description                                                                               |
| -------------------------- | ------- | -------- | ----------------------------------------------------------------------------------------- |
| `catalogName`              | string  | No       | Name of the catalog. This value identifies which catalog you want to update.              |
| `description`              | string  | No       | Updated description of the catalog.                                                       |
| `status`                   | string  | No       | Current status of the catalog, for example, `ACTIVE` or `INACTIVE`.                       |
| `updatedBy`                | string  | Yes      | Email address of the user making the update.                                              |
| `catalogSchema`            | array   | No       | Updated list of fields that define the structure of the catalog.                          |
| `catalogSchema.name`       | string  | No       | Name of the catalog field.                                                                |
| `catalogSchema.type`       | string  | No       | Data type of the field, such as `string`, `number`, or `boolean`.                         |
| `catalogSchema.filterable` | boolean | No       | Indicates whether this field can be used in filters. Default is `false` if not specified. |

# Example Request

This example updates the catalog name, schema, and recurring sync configuration.

```json
{
  "catalogName": "electronics_catalog_v2",
  "description": "Updated product catalog for electronics store",
  "status": "ACTIVE",
  "updatedBy": "admin@example.com",
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
    },
    {
      "name": "category",
      "type": "string",
      "filterable": true
    },
    {
      "name": "rating",
      "type": "number",
      "filterable": false
    }
  ]
}
```

<Callout icon="📘" theme="info">
  **Note**
  This API performs a full update. Any fields omitted from the request may be reset to default values.
</Callout>

# Example Response

This response confirms that the catalog was updated successfully.

```json
{
  "id": 123,
  "catalogName": "electronics_catalog_v2",
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
    },
    {
      "name": "category",
      "type": "string",
      "filterable": true
    },
    {
      "name": "rating",
      "type": "number",
      "filterable": false
    }
  ],
  "fieldMappings": {
    "manufacturer": "brand",
    "cost": "price"
  },
  "description": "Updated product catalog for electronics store",
  "totalProducts": 1500,
  "status": "ACTIVE",
  "createdAt": 1699564800,
  "createdBy": "admin@example.com",
  "updatedBy": "admin@example.com",
  "isActive": true,
  "attributeCount": 4,
  "totalColumns": 7
}
```

# Response Parameters

The response includes additional system-generated fields returned after a successful update.

| Parameter        | Type    | Description                                                                        |
| ---------------- | ------- | ---------------------------------------------------------------------------------- |
| `id`             | Number  | Unique identifier of the updated catalog.                                          |
| `totalProducts`  | integer | Total number of products currently available in the catalog.                       |
| `createdAt`      | integer | Time when the catalog was created, in Unix timestamp format.                       |
| `createdBy`      | string  | Email address of the user who created the catalog.                                 |
| `isActive`       | boolean | Indicates whether the catalog is currently active.                                 |
| `attributeCount` | integer | Total number of attributes defined in the catalog schema.                          |
| `totalColumns`   | integer | Total number of columns available in the catalog, including system-defined fields. |

# Error Codes

The following error codes may be returned by this API:

| Error Code                | Description                                                 |
| ------------------------- | ----------------------------------------------------------- |
| `CATALOG_NOT_FOUND`       | The catalog specified by the ID does not exist.             |
| `CATALOG_NAME_CONFLICT`   | Another catalog with the same name already exists.          |
| `CATALOG_INVALID_REQUEST` | The request payload is invalid or incomplete.               |
| `AUTHENTICATION_FAILED`   | Authentication failed due to an invalid or missing token.   |
| `AUTHORIZATION_FAILED`    | The caller does not have permission to update this catalog. |
| `INTERNAL_ERROR`          | An unexpected server-side error occurred.                   |