---
title: Create Single Product API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Creates a single product in a catalog. Each product must include `identity`, `name`, and `imageUrl`.

Use this endpoint for real-time or incremental writes. The maximum request payload for this endpoint is 100 KB. For larger writes, use the [Create Products (Bulk) API](doc:create-products-bulk-api), which supports 500 KB per request, or upload a file via the [Pre-Signed URL API.](doc:pre-signed-url-api)

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

# Request Parameter

The following parameters define the product to be created in the catalog:

| Parameter          | Type   | Required | Description                                                                                                                   |
| ------------------ | ------ | -------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `identity`         | string | Yes      | Unique identifier of the product within the catalog, for example, a SKU.                                                      |
| `name`             | string | Yes      | Name of the product.                                                                                                          |
| `imageUrl`         | string | Yes      | Public URL of the product image.                                                                                              |
| `attributes`       | object | No       | Key-value pairs for the product's custom attributes. Omit it, or send an empty object, to create a product with no attributes |
| `attributes.field` | varies | No       | Value of a specific catalog attribute. The data type must match the type defined in the catalog schema                        |
| `createdBy`        | string | Yes      | Email address of the user or system creating the product.                                                                     |

<Callout icon="📘" theme="info">
  ### Attribute Conventions

  Attributes such as price, description, and category are conventions, not system fields. The API does not require them and does not reject a product that omits them. Only attributes defined in your catalog schema can be used in campaign catalog rules.
</Callout>

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
  "updatedAt": "2026-08-14T10:30:00",

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
| `updatedAt` | string | Date and time when the product was last updated.                    |
| `updatedBy` | string | Email address of the user who last updated the product.             |

# Error Codes

This API returns a standard HTTP status code along with a plain-text message describing the issue. Common status codes include:

| HTTP Status | Description                                                                                                                                     |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 400         | catalogId is missing or invalid, the body is not valid JSON, a required field is missing, or an attribute value does not match its schema type. |
| 401         | Authentication failed, or the catalogs feature is not enabled on the account's plan.                                                            |
| 403         | Catalog feature is not enabled for this account (CleverTap code 101).                                                                           |
| 404         | The specified catalog does not exist.                                                                                                           |
| 409         | A product with this identity already exists in the catalog.                                                                                     |
| 413         | Request payload exceeds 100 KB (CleverTap code 105).                                                                                            |
| 500         | Unexpected server-side error.                                                                                                                   |
| 503         | The account is not ready to serve requests yet. Retry shortly.                                                                                  |
