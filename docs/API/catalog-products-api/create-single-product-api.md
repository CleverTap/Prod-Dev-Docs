---
title: Create Single Product API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Creates a single product in a catalog. Each product must include the mandatory fields `identity`, `name`, and `imageUrl`.

# Method

POST

# Endpoint

`POST /v2/catalogs/{catalogId}/products`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{catalogId}/products`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and validate the request.

| Header              | Description                                                      | Required |
| ------------------- | ---------------------------------------------------------------- | -------- |
| `Content-Type`      | Content type of the request payload. Must be `application/json`. | Yes      |
| `X-Account-Id`      | CleverTap account ID associated with the catalog.                | Yes      |
| `X-CleverTap-Token` | API token used to authenticate the request.                      | Yes      |

# Path Parameter

The following path parameter is required to identify the catalog:

| Parameter   | Type   | Required | Description                                                  |
| ----------- | ------ | -------- | ------------------------------------------------------------ |
| `catalogId` | Number | Yes      | Unique identifier of the catalog where products are created. |

# Request Parameter

The following parameters define the product to be created in the catalog:

| Parameter          | Type   | Required | Description                                                                                                                                       |
| ------------------ | ------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `identity`         | string | Yes      | Unique identifier of the product within the catalog, for example, a SKU.                                                                          |
| `name`             | string | Yes      | Name of the product.                                                                                                                              |
| `imageUrl`         | string | Yes      | Public URL of the product image.                                                                                                                  |
| `attributes`       | object | Yes      | Key-value pairs representing the product attributes defined in the catalog schema.                                                                |
| `attributes.field` | varies | Yes      | Value of a specific catalog attribute. The data type must match the type defined in the catalog schema, such as `string`, `number`, or `boolean`. |
| `createdBy`        | string | Yes      | Email address of the user or system creating the product.                                                                                         |

# Example Request

This example creates a product in the specified catalog.

```json
{
  "identity": "SKU-12345",
  "name": "Wireless Bluetooth Headphones",
  "imageUrl": "https://example.com/images/headphones.jpg",
  "attributes": {
    "brand": "AudioTech",
    "price": 89.99,
    "category": "Electronics",
    "in_stock": true,
    "rating": 4.5
  },
  "createdBy": "admin@example.com"
}
```

# Example Response

This example shows a successful response:

```json
{
  "identity": "SKU-12345",
  "name": "Wireless Bluetooth Headphones",
  "imageUrl": "https://example.com/images/headphones.jpg",
  "attributes": {
    "brand": "AudioTech",
    "price": 89.99,
    "category": "Electronics",
    "in_stock": true,
    "rating": 4.5
  },
  "status": "ACTIVE",
  "createdAt": "2023-11-10T10:30:00Z",
  "createdBy": "admin@example.com",
  "updatedBy": "admin@example.com"
}
```

# Response Parameters

The response includes additional system-generated fields returned after the product is created.

| Parameter   | Type   | Description                                                         |
| ----------- | ------ | ------------------------------------------------------------------- |
| `status`    | string | Current status of the product, for example, `ACTIVE` or `INACTIVE`. |
| `createdAt` | string | Date and time when the product was created.                         |
| `updatedBy` | string | Email address of the user who last updated the product.             |

# Error Codes

The following error codes may be returned by this API:

| Error Code                | Description                                                             |
| ------------------------- | ----------------------------------------------------------------------- |
| `CATALOG_NOT_FOUND`       | The specified catalog does not exist.                                   |
| `PRODUCT_INVALID_REQUEST` | One or more products failed schema validation.                          |
| `CATALOG_INVALID_REQUEST` | The request payload is invalid or exceeds size limits.                  |
| `AUTHENTICATION_FAILED`   | Authentication failed due to an invalid or missing token.               |
| `AUTHORIZATION_FAILED`    | The caller does not have permission to create products in this catalog. |
| `INTERNAL_ERROR`          | An unexpected server-side error occurred.                               |
