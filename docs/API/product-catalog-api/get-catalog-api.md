---
title: Get Catalog API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Retrieves detailed information about a specific catalog using its unique catalog ID. This API is useful when you need full metadata about a catalog, including schema, status, and product summary.

# Method

GET

# Endpoint

`GET /v2/catalogs/{id}`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{id}`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with this API request to ensure proper authentication and content handling.

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

# Path Parameter

The following path parameter is required to retrieve a specific catalog.

| Parameter | Type   | Required | Description                                   |
| --------- | ------ | -------- | --------------------------------------------- |
| `id`      | Number | Yes      | Unique identifier of the catalog to retrieve. |

# Example Request

This request fetches details for the catalog with ID 123.

If you do not have the catalog ID, you can retrieve it in either of the following ways:

- Use the List Catalogs API to fetch all catalogs associated with your account
- Copy the catalog ID from the CleverTap dashboard

```
GET /v2/catalogs/123
```

# Example Response

This example illustrates what a successful response for a catalog retrieval looks like.

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
  "fieldMappings": {
    "manufacturer": "brand",
    "cost": "price"
  },
  "description": "Product catalog for electronics store",
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

The response returns the catalog details along with system-generated metadata:

| Parameter      | Type    | Description                                                                                 |
| -------------- | ------- | ------------------------------------------------------------------------------------------- |
| id             | integer | Unique identifier of the catalog.                                                           |
| catalogName    | string  | Name of the catalog.                                                                        |
| catalogSchema  | array   | List of fields defined for the catalog. Each entry represents one attribute in the catalog. |
| fieldMappings  | object  | Mapping between source fields and catalog fields.                                           |
| description    | string  | Description of the catalog.                                                                 |
| totalProducts  | integer | Total number of products currently available in the catalog.                                |
| status         | string  | Current status of the catalog, for example, `ACTIVE` or `INACTIVE`.                         |
| createdAt      | integer | Time when the catalog was created, in Unix timestamp format.                                |
| createdBy      | string  | Email address of the user who created the catalog.                                          |
| updatedBy      | string  | Email address of the user who last updated the catalog.                                     |
| isActive       | boolean | Indicates whether the catalog is currently active.                                          |
| attributeCount | integer | Total number of attributes defined in the catalog schema.                                   |
| totalColumns   | integer | Total number of columns available in the catalog, including system-defined fields.          |

# Error Codes

The following error codes may be returned when attempting to retrieve a catalog.

| Error Code              | Description                                                 |
| ----------------------- | ----------------------------------------------------------- |
| `CATALOG_NOT_FOUND`     | No catalog exists for the specified `id`.                   |
| `AUTHENTICATION_FAILED` | The provided token is missing or invalid.                   |
| `AUTHORIZATION_FAILED`  | The caller does not have permission to access this catalog. |
| `INTERNAL_ERROR`        | An unexpected server-side error occurred.                   |
