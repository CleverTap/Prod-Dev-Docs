---
title: Update Single Product API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Updates a single product by identity.

# Method

PUT

# Endpoint

`PUT /v2/catalogs/{catalogId}/products/{productId}`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{catalogId}/products/{productId}`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and request validation.

| Header              | Description                                                      | Required |
| ------------------- | ---------------------------------------------------------------- | -------- |
| `Content-Type`      | Content type of the request payload. Must be `application/json`. | Yes      |
| `X-Account-Id`      | CleverTap account ID associated with the catalog.                | Yes      |
| `X-CleverTap-Token` | API token used to authenticate the request.                      | Yes      |

# Path Parameters

The following path parameters are required to identify the product to update.

| Parameter   | Type   | Required | Description                                               |
| ----------- | ------ | -------- | --------------------------------------------------------- |
| `catalogId` | Number | Yes      | Unique identifier of the catalog containing the products. |
| `productId` | String | Yes      | Unique identifier of the product (URL encoded).           |

# Request Parameters

The following request body parameters define the product fields to update:

| Parameter          | Type   | Required | Description                                                                                             |
| ------------------ | ------ | -------- | ------------------------------------------------------------------------------------------------------- |
| `name`             | string | No       | Updated name of the product.                                                                            |
| `imageUrl`         | string | No       | Public URL of the product image.                                                                        |
| `attributes`       | object | No       | Key-value pairs representing the product attributes to update.                                          |
| `attributes.field` | varies | No       | Value of a specific product attribute. The data type must match the type defined in the catalog schema. |
| `updatedBy`        | string | Yes      | Email address of the user who last updated the product.                                                 |

# Example Request

This example updates product details in the catalog.

```json
{
  "name": "Wireless Bluetooth Headphones Pro",
  "imageUrl": "https://example.com/images/headphones-pro.jpg",
  "attributes": {
    "price": 99.99,
    "rating": 4.8
  },
  "updatedBy": "admin@example.com"
}
```

# Example Response

This example response indicates that the product update was processed successfully.

```json
{
  "identity": "SKU-12345",
  "name": "Wireless Bluetooth Headphones Pro",
  "imageUrl": "https://example.com/images/headphones-pro.jpg",
  "attributes": {
    "brand": "AudioTech",
    "price": 99.99,
    "category": "Electronics",
    "in_stock": true,
    "rating": 4.8
  },
  "status": "ACTIVE",
  "createdAt": "2023-11-10T10:30:00Z",
  "createdBy": "admin@example.com",
  "updatedBy": "admin@example.com"
}
```

# Response Parameters

The response includes additional system-generated fields returned after the product is updated.

| Parameter   | Type   | Description                                                         |
| ----------- | ------ | ------------------------------------------------------------------- |
| `status`    | string | Current status of the product, for example, `ACTIVE` or `INACTIVE`. |
| `createdAt` | string | Date and time when the product was originally created.              |
| `createdBy` | string | Email address of the user who created the product.                  |

# Error Codes

The following error codes may be returned by this API:

| Error Code                | Description                                                             |
| ------------------------- | ----------------------------------------------------------------------- |
| `CATALOG_NOT_FOUND`       | The specified catalog does not exist.                                   |
| `PRODUCT_NOT_FOUND`       | One or more products specified by `identity` do not exist.              |
| `PRODUCT_INVALID_REQUEST` | One or more products failed schema validation.                          |
| `CATALOG_INVALID_REQUEST` | The request payload is invalid or exceeds size limits.                  |
| `AUTHENTICATION_FAILED`   | Authentication failed due to an invalid or missing token.               |
| `AUTHORIZATION_FAILED`    | The caller does not have permission to update products in this catalog. |
| `INTERNAL_ERROR`          | An unexpected server-side error occurred.                               |
