---
title: Get Single Product API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Retrieves details of a single product from a catalog using the product’s unique identity. This API is useful when you need complete information for a specific product, including its custom attributes.

# Method

GET

# Endpoint

`GET /v2/catalogs/{catalogId}/products/{productId}`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{catalogId}/products/{productId}`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and correct request handling.

| Header              | Description                                              | Required |
| ------------------- | -------------------------------------------------------- | -------- |
| `Content-Type`      | Content type of the request. Must be `application/json`. | Yes      |
| `X-Account-Id`      | CleverTap account ID associated with the catalog.        | Yes      |
| `X-CleverTap-Token` | API token used to authenticate the request.              | Yes      |

# Path Parameters

The following path parameters are required to identify the product:

| Parameter   | Type   | Required | Description                                              |
| ----------- | ------ | -------- | -------------------------------------------------------- |
| `catalogId` | Number | Yes      | Unique identifier of the catalog containing the product. |
| `productId` | String | Yes      | Unique product identity within the catalog.              |

# Example Request

This example retrieves the product with the identity SKU-12345 from catalog 123.

```
GET /v2/catalogs/123/products/SKU-12345
```

# Example Response

This example response shows the details of a successfully retrieved product.

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

The response includes additional system-generated fields returned for the product.

| Parameter   | Type   | Description                                                         |
| ----------- | ------ | ------------------------------------------------------------------- |
| `status`    | string | Current status of the product, for example, `ACTIVE` or `INACTIVE`. |
| `createdAt` | string | Date and time when the product was created.                         |
| `createdBy` | string | Email address of the user who created the product.                  |
| `updatedBy` | string | Email address of the user who last updated the product.             |

# Error Codes

The following error codes may be returned by this API:

| Error Code              | Description                                                    |
| ----------------------- | -------------------------------------------------------------- |
| `PRODUCT_NOT_FOUND`     | No product exists with the specified identity in this catalog. |
| `CATALOG_NOT_FOUND`     | The specified catalog does not exist.                          |
| `AUTHENTICATION_FAILED` | Authentication failed due to an invalid or missing token.      |
| `AUTHORIZATION_FAILED`  | The caller does not have permission to access this product.    |
| `INTERNAL_ERROR`        | An unexpected server-side error occurred.                      |
