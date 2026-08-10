---
title: List Products API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Retrieves a paginated list of products for a specific catalog. This API supports pagination, sorting, and filtering using catalog attributes that are marked as filterable.

# Method

GET

# Endpoint

`GET /v2/catalogs/{catalogId}/products`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{catalogId}/products`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and correct request handling.

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

# Path Parameter

The following path parameter is required to identify the catalog:

| Parameter   | Type | Required | Description                                              |
| ----------- | ---- | -------- | -------------------------------------------------------- |
| `catalogId` | long | Yes      | Unique identifier of the catalog to fetch products from. |

# Query Parameters

The following query parameters control pagination, sorting, and filtering for product listing:

| Parameter   | Type    | Required | Default   | Description                                                                                                                                           |
| ----------- | ------- | -------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `page`      | integer | No       | 0         | Page number to return. Page numbering starts at 0.                                                                                                    |
| `size`      | integer | No       | 20        | Number of products to return per page. Allowed range is 1 to 100.                                                                                     |
| `sortBy`    | string  | No       | createdAt | Field used to sort the results. Allowed values are `createdAt`, `name`, and `identity`.                                                               |
| `sortOrder` | string  | No       | DESC      | Sort direction. Allowed values are `ASC` and `DESC`.                                                                                                  |
| `search`    | string  | No       |           | Search text used to match products by `name` and `identity`. If not provided, all products are returned.                                              |
| filters     | string  | No       |           | JSON string used to filter results by catalog attributes. Only attributes marked as filterable can be used. Example: `{"brand":"Nike","price_gt":50}` |

# Example Request

This example retrieves the first page of products sorted by creation time and filters the results by a search term.

```json
GET /v2/catalogs/123/products?page=0&size=20&sortBy=createdAt&sortOrder=DESC&search=headphones
```

# Example Response

This example response shows a paginated set of products.

```json
{
  "content": [
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
    }
  ],
  "page": 0,
  "size": 20,
  "totalElements": 1,
  "totalPages": 1
}
```

# Response Parameters

The response includes a list of products along with pagination details.

| Parameter           | Type    | Description                                                         |
| ------------------- | ------- | ------------------------------------------------------------------- |
| `content`           | array   | List of products returned for the current page.                     |
| `content.status`    | string  | Current status of the product, for example, `ACTIVE` or `INACTIVE`. |
| `content.createdAt` | string  | Date and time when the product was created.                         |
| `content.createdBy` | string  | Email address of the user who created the product.                  |
| `content.updatedBy` | string  | Email address of the user who last updated the product.             |
| `page`              | integer | Current page number returned in the response.                       |
| `size`              | integer | Number of products returned per page.                               |
| `totalElements`     | integer | Total number of products available for the catalog.                 |
| `totalPages`        | integer | Total number of pages available based on the page size.             |

# Error Codes

The following error codes may be returned by this API:

| Error Code                | Description                                                                                                                             |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `CATALOG_NOT_FOUND`       | The specified catalog does not exist.                                                                                                   |
| `CATALOG_INVALID_REQUEST` | The request is invalid. For example, `page` or `size` is out of the allowed range, or `sortBy` or `sortOrder` has an unsupported value. |
| `AUTHENTICATION_FAILED`   | Authentication failed due to an invalid or missing token.                                                                               |
| `AUTHORIZATION_FAILED`    | The caller does not have permission to access products in this catalog.                                                                 |
| `INTERNAL_ERROR`          | An unexpected server-side error occurred.                                                                                               |
