---
title: Update Products (Bulk) API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Updates multiple existing products in a catalog in a single request. This API allows you to modify product fields and custom attributes for existing products.

Only products with matching `identity` values are updated. Products that fail validation do not prevent other valid products from being processed.

# Method

PUT

# Endpoint

`PUT /v2/catalogs/{catalogId}/products/bulk`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{catalogId}/products/bulk`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and request validation.

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

# Path Parameter

The following path parameter is required to identify the catalog:

| Parameter   | Type   | Required | Description                                               |
| ----------- | ------ | -------- | --------------------------------------------------------- |
| `catalogId` | Number | Yes      | Unique identifier of the catalog containing the products. |

# Request Parameters

The following request body parameters define the products to be updated:

| Parameter                   | Type   | Required | Description                                                                         |
| --------------------------- | ------ | -------- | ----------------------------------------------------------------------------------- |
| `products`                  | array  | Yes      | List of products to be updated.                                                     |
| `products.identity`         | string | Yes      | Unique identifier of the product within the catalog.                                |
| `products.name`             | string | No       | Updated name of the product.                                                        |
| `products.imageUrl`         | string | No       | Public URL of the product image.                                                    |
| `products.attributes`       | object | No       | Key-value pairs representing the product attributes to update.                      |
| `products.attributes.field` | varies | No       | Value of a specific product attribute. The data type must match the catalog schema. |
| `updatedBy`                 | string | Yes      | Email address of the user performing the update.                                    |

<Callout icon="📘" theme="info">
  ### Note

  When the attributes field is included in an update, it replaces the entire attributes object rather than merging individual keys. To update a single attribute without affecting others, include all attributes you want to keep in the request.
</Callout>

# Example Request

This example updates product details for two existing products in the catalog.

```json
{
  "products": [
    {
      "identity": "SKU-12345",
      "name": "Wireless Bluetooth Headphones Pro",
      "attributes": {
        "price": 99.99,
        "rating": 4.8
      }
    },
    {
      "identity": "SKU-12346",
      "attributes": {
        "price": 11.99,
        "in_stock": false
      }
    }
  ],
  "updatedBy": "admin@example.com"
}
```

# Example Response

This example response indicates that all requested product updates were processed successfully.

```json
{
  "successCount": 2,
  "failedCount": 0,
  "products": [
    {
      "identity": "SKU-12345",
      "name": "Wireless Bluetooth Headphones Pro",
      "imageUrl": "https://example.com/images/headphones.jpg",
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
    },
    {
      "identity": "SKU-12346",
      "name": "USB-C Charging Cable",
      "imageUrl": "https://example.com/images/cable.jpg",
      "attributes": {
        "brand": "TechCable",
        "price": 11.99,
        "category": "Accessories",
        "in_stock": false
      },
      "status": "ACTIVE",
      "createdAt": "2023-11-10T10:30:00Z",
      "createdBy": "admin@example.com",
      "updatedBy": "admin@example.com"
    }
  ],
  "errors": []
}
```

# Response Parameters

The response includes bulk operation results along with system-generated fields for successfully updated products.

| Parameter            | Type    | Description                                                           |
| -------------------- | ------- | --------------------------------------------------------------------- |
| `successCount`       | integer | Number of products that were successfully updated.                    |
| `failedCount`        | integer | Number of products that failed to be updated.                         |
| `products`           | array   | List of products that were successfully updated.                      |
| `products.status`    | string  | Current status of the product, for example, `ACTIVE` or `INACTIVE`.   |
| `products.createdAt` | string  | Date and time when the product was originally created.                |
| `products.createdBy` | string  | Email address of the user who created the product.                    |
| `products.updatedBy` | string  | Email address of the user who last updated the product.               |
| `errors`             | array   | List of products that could not be updated, along with error details. |

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
