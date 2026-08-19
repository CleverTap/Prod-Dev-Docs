---
title: List Products API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Retrieves a paginated list of products for a specific catalog. This API supports pagination and sorting. It does not support filtering by attribute value.

<Callout icon="📘" theme="info">
  ### **Filtering by Attribute Value is not Available on this Endpoint**

  This API has no attribute-filter parameter. Use search for free-text matching on product name, identity, and image URL, then filter client-side if you need attribute-level conditions.

  The only customer-facing way to filter or target products by attribute value is through campaign catalog include/exclude rules at send time, on attributes marked filterable. For more information, refer to Product Catalogs in Campaigns.
</Callout>

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

| Parameter   | Type    | Required | Default   | Description                                                                                                                                                                 |
| ----------- | ------- | -------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `page`      | integer | No       | 0         | Page number to return. Page numbering starts at 0.                                                                                                                          |
| `size`      | integer | No       | 20        | Number of products to return per page. Allowed range is 1 to 100.                                                                                                           |
| `sortBy`    | string  | No       | createdAt | Field used to sort the results. Field to sort by. Allowed values: `identity`, `name`, `imageUrl`, `createdAt`, `updatedAt`.                                                 |
| `sortOrder` | string  | No       | DESC      | Sort direction. Allowed values are `ASC` and `DESC`.                                                                                                                        |
| `search`    | string  | No       |           | Case-insensitive partial match against product name, identity, and imageUrl. A product matches if any of the three contain the term. If omitted, all products are returned. |

# Example Request

This example retrieves the first page of products, sorted by creation time, and filters the results using a search term.

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
      "updatedAt": "2023-11-10T10:30:00Z",
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

| Parameter             | Type    | Description                                                         |
| --------------------- | ------- | ------------------------------------------------------------------- |
| `content`             | array   | List of products returned for the current page.                     |
| `content.status`      | string  | Current status of the product, for example, `ACTIVE` or `INACTIVE`. |
| `content.createdAt`   | string  | Date and time when the product was created.                         |
| `content[].updatedAt` | string  | Date and time when the product was last updated                     |
| `content.createdBy`   | string  | Email address of the user who created the product.                  |
| `content.updatedBy`   | string  | Email address of the user who last updated the product.             |
| `page`                | integer | Current page number returned in the response.                       |
| `size`                | integer | Number of products returned per page.                               |
| `totalElements`       | integer | Total number of products available for the catalog.                 |
| `totalPages`          | integer | Total number of pages available based on the page size.             |

# Error Codes

This API returns a standard HTTP status code along with a plain-text message describing the issue. Common status codes include:

| HTTP Status | Description                                                                          |
| ----------- | ------------------------------------------------------------------------------------ |
| 400         | catalogId is missing or not a valid number, or page/size is not a valid integer.     |
| 401         | Authentication failed, or the catalogs feature is not enabled on the account's plan. |
| 403         | Catalog feature is not enabled for this account (CleverTap code 101).                |
| 404         | The specified catalog does not exist.                                                |
| 500         | Unexpected server-side error.                                                        |
| 503         | The account is not ready to serve requests yet. Retry shortly.                       |
