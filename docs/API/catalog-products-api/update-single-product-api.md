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

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

# Path Parameters

The following path parameters are required to identify the product to update.

| Parameter   | Type   | Required | Description                                               |
| ----------- | ------ | -------- | --------------------------------------------------------- |
| `catalogId` | Number | Yes      | Unique identifier of the catalog containing the products. |
| `productId` | String | Yes      | Unique identifier of the product (URL encoded).           |

# Request Parameters

The following request body parameters define the product fields to update:

| Parameter          | Type   | Required | Description                                                                                                                                                                       |
| ------------------ | ------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`             | string | No       | Updated name of the product.                                                                                                                                                      |
| `imageUrl`         | string | No       | Public URL of the product image.                                                                                                                                                  |
| `attributes`       | object | No       | Key-value pairs representing the product attributes to update.                                                                                                                    |
| `attributes.field` | varies | No       | Value of a specific product attribute. The data type must match the type defined in the catalog schema.                                                                           |
| `updatedBy`        | string | No       | Email address of the user who last updated the product. <br />**Note:** This field is not currently required by this endpoint, though it is required on the bulk update endpoint. |

<Callout icon="📘" theme="info">
  ### Note

  When the attributes field is included in an update, it replaces the entire attributes object rather than merging individual keys. To update a single attribute without affecting others, include all attributes you want to keep in the request.
</Callout>

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

This API returns a standard HTTP status code along with a plain-text message describing the issue. Common status codes include:

| HTTP Status | Description                                                                                               |
| ----------- | --------------------------------------------------------------------------------------------------------- |
| 400         | The request is invalid, missing required fields, or exceeds a bulk limit (product count or payload size). |
| 403         | The caller lacks permission, or the catalog feature is not enabled for this account.                      |
| 404         | The specified catalog or product does not exist.                                                          |
| 409         | A conflicting resource already exists.                                                                    |
| 413         | The request payload exceeds the maximum allowed size.                                                     |
| 500         | An unexpected server-side error occurred.                                                                 |
