---
title: Delete Products (Bulk) API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Deletes multiple products from a catalog in a single request. This API removes products by their `identity` values and reports per-product failures without failing the entire request.

# Method

DELETE

# Endpoint

`DELETE /v2/catalogs/{catalogId}/products/bulk`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{catalogId}/products/bulk`

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

| Parameter   | Type   | Required | Description                                                       |
| ----------- | ------ | -------- | ----------------------------------------------------------------- |
| `catalogId` | Number | Yes      | Unique identifier of the catalog from which products are deleted. |

# Request Parameters

The following request body parameters define the products to be deleted:

| Parameter      | Type   | Required | Description                                           |
| -------------- | ------ | -------- | ----------------------------------------------------- |
| `identities`   | array  | Yes      | List of product identities to be deleted.             |
| `identities[]` | string | Yes      | Unique identifier of each product within the catalog. |

# Example Request

This example deletes multiple products from the catalog:

```json
{
  "identities": [
    "SKU-12345",
    "SKU-12346",
    "SKU-12347"
  ]
}
```

<Callout icon="📘" theme="info">
  **Note**
  If an identity does not exist, it is returned as an error entry while other valid deletions continue to be processed.
</Callout>

# Example Response

This example response indicates that the delete request was processed successfully.

```json
{
  "successCount": 3,
  "failedCount": 0,
  "products": [],
  "errors": []
}
```

# Partial Response

When some products fail to be deleted, the response includes the number of successful deletions and details for the products that could not be deleted.

```json
{
  "successCount": 2,
  "failedCount": 1,
  "products": [],
  "errors": [
    {
      "identity": "SKU-12347",
      "errorMessage": "Product with identity SKU-12347 not found",
      "errorType": "NOT_FOUND"
    }
  ]
}
```

# Response Parameters

The response includes the results of the bulk operation for the delete request.

| Parameter             | Type    | Description                                                                       |
| --------------------- | ------- | --------------------------------------------------------------------------------- |
| `successCount`        | integer | Number of products successfully deleted.                                          |
| `failedCount`         | integer | Number of products that failed to be deleted.                                     |
| `products`            | array   | List of successfully processed products. This may be empty for delete operations. |
| `errors`              | array   | List of products that failed to be deleted, along with error details.             |
| `errors.identity`     | string  | Identity of the product that failed to be deleted.                                |
| `errors.errorMessage` | string  | Description of why the deletion failed.                                           |
| `errors.errorType`    | string  | Error category indicating the reason for failure, for example, `NOT_FOUND`.       |

# Limitations

- Maximum 100 identities per request
- Maximum payload size: 500KB (512,000 bytes)

# Error Codes

The following error codes may be returned by this API:

| Error Code                | Description                                                             |
| ------------------------- | ----------------------------------------------------------------------- |
| `CATALOG_NOT_FOUND`       | The specified catalog does not exist.                                   |
| `PRODUCT_NOT_FOUND`       | Product specified by `identity` does not exist.                         |
| `PRODUCT_INVALID_REQUEST` | The request payload is invalid (for example, empty identities list).    |
| `CATALOG_INVALID_REQUEST` | The request payload is invalid or exceeds size limits.                  |
| `AUTHENTICATION_FAILED`   | Authentication failed due to an invalid or missing token.               |
| `AUTHORIZATION_FAILED`    | The caller does not have permission to delete products in this catalog. |
| `INTERNAL_ERROR`          | An unexpected server-side error occurred.                               |
