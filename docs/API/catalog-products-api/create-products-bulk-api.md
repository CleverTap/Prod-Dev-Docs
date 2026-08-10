---
title: Create Products (Bulk) API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Creates multiple products in a single request.

# Method

POST

# Endpoint

`POST /v2/catalogs/{catalogId}/products/bulk`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{catalogId}/products/bulk`

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

The following path parameter is required to identify the catalog:

| Parameter   | Type   | Required | Description                                                  |
| ----------- | ------ | -------- | ------------------------------------------------------------ |
| `catalogId` | Number | Yes      | Unique identifier of the catalog where products are created. |

# Request Parameters

The following parameters define the products to be created in the catalog:

| Parameter                   | Type   | Required | Description                                                                                             |
| --------------------------- | ------ | -------- | ------------------------------------------------------------------------------------------------------- |
| `products`                  | array  | Yes      | List of products to be added to the catalog.                                                            |
| `products.identity`         | string | Yes      | Unique identifier of the product within the catalog, for example, a SKU.                                |
| `products.name`             | string | Yes      | Name of the product.                                                                                    |
| `products.imageUrl`         | string | No       | Public URL of the product image.                                                                        |
| `products.attributes`       | object | Yes      | Key-value pairs representing the product attributes defined in the catalog schema.                      |
| `products.attributes.field` | varies | Yes      | Value of a specific catalog attribute. The data type must match the type defined in the catalog schema. |
| `createdBy`                 | string | Yes      | Email address of the user or system creating these products.                                            |

# Example Request

This example creates multiple products in the specified catalog:

```json
{
  "products": [
    {
      "identity": "SKU-12345",
      "name": "Wireless Bluetooth Headphones",
      "imageUrl": "https://example.com/images/headphones.jpg",
      "attributes": {
        "brand": "AudioTech",
        "price": 89.99,
        "category": "Electronics",
        "in_stock": true
      }
    },
    {
      "identity": "SKU-12346",
      "name": "USB-C Charging Cable",
      "imageUrl": "https://example.com/images/cable.jpg",
      "attributes": {
        "brand": "TechCable",
        "price": 12.99,
        "category": "Accessories",
        "in_stock": true
      }
    }
  ],
  "createdBy": "admin@example.com"
}
```

<Callout icon="📘" theme="info">
  **Note**
  If a product fails validation, the error is reported for that product while other valid products continue to be processed.
</Callout>

# Example Response

This example shows a successful response with all products created.

```json
{
  "successCount": 2,
  "failedCount": 0,
  "products": [
    {
      "identity": "SKU-12345",
      "name": "Wireless Bluetooth Headphones",
      "imageUrl": "https://example.com/images/headphones.jpg",
      "attributes": {
        "brand": "AudioTech",
        "price": 89.99,
        "category": "Electronics",
        "in_stock": true
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
        "price": 12.99,
        "category": "Accessories",
        "in_stock": true
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

# Partial Success Response

When some products fail, the response includes both successful products and error details.

```json
{
  "successCount": 1,
  "failedCount": 1,
  "products": [
    {
      "identity": "SKU-12345",
      "name": "Wireless Bluetooth Headphones",
      "status": "ACTIVE",
      "createdAt": "2023-11-10T10:30:00Z"
    }
  ],
  "errors": [
    {
      "identity": "SKU-12346",
      "errorMessage": "Product with identity SKU-12346 already exists",
      "errorType": "ALREADY_EXISTS"
    }
  ]
}
```

# Response Parameters

The response includes bulk operation results along with system-generated fields for successful products.

| Parameter             | Type    | Description                                                                      |
| --------------------- | ------- | -------------------------------------------------------------------------------- |
| `successCount`        | integer | Number of products that were successfully created.                               |
| `failedCount`         | integer | Number of products that failed to be created.                                    |
| `products`            | array   | List of products that were successfully created.                                 |
| `products.status`     | string  | Current status of the product, for example, `ACTIVE` or `INACTIVE`.              |
| `products.createdAt`  | string  | Date and time when the product was created.                                      |
| `products.updatedBy`  | string  | Email address of the user who last updated the product.                          |
| `errors`              | array   | List of products that could not be created, along with error details.            |
| `errors.identity`     | string  | Identity of the product that failed to be created.                               |
| `errors.errorMessage` | string  | Description of why the product creation failed.                                  |
| `errors.errorType`    | string  | Error category indicating the reason for failure, for example, `ALREADY_EXISTS`. |

# Limitations

- Maximum 100 products per request
- Maximum payload size: 500KB (512,000 bytes)

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
