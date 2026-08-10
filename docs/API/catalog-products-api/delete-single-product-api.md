---
title: Delete Single Product API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Deletes a single product by identity.

# Method

DELETE

# Endpoint

`DELETE /v2/catalogs/{catalogId}/products/{productId}`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/{catalogId}/products/{productId}`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and correct request handling.

| Header              | Description                                                      | Required |
| ------------------- | ---------------------------------------------------------------- | -------- |
| `Content-Type`      | Content type of the request payload. Must be `application/json`. | Yes      |
| `X-Account-Id`      | CleverTap account ID associated with the catalog.                | Yes      |
| `X-CleverTap-Token` | API token used to authenticate the request.                      | Yes      |

# Path Parameters

The following path parameters are required to identify the product to delete.

| Parameter   | Type   | Required | Description                                                       |
| ----------- | ------ | -------- | ----------------------------------------------------------------- |
| `catalogId` | Number | Yes      | Unique identifier of the catalog from which products are deleted. |
| `productId` | String | Yes      | Unique identifier of the product (URL encoded)                    |

# Example Request

This example deletes a product from the catalog:

```json
DELETE /v2/catalogs/123/products/SKU-12345
```

# Example Response

This example response indicates that the delete request was processed successfully.

```json
{
  "message": "Operation completed successfully"
}
```

# Response Parameters

The response returns a confirmation message indicating that the product was deleted successfully.

| Parameter | Type   | Description                                                                    |
| --------- | ------ | ------------------------------------------------------------------------------ |
| message   | string | Confirmation message indicating that the operation was completed successfully. |

# Error Codes

The following error codes may be returned by this API:

| Error Code                | Description                                                             |
| ------------------------- | ----------------------------------------------------------------------- |
| `CATALOG_NOT_FOUND`       | The specified catalog does not exist.                                   |
| `PRODUCT_NOT_FOUND`       | Product specified by `identity` does not exist.                         |
| `PRODUCT_INVALID_REQUEST` | The request is invalid (for example, a missing or malformed productId). |
| `CATALOG_INVALID_REQUEST` | The request payload is invalid or exceeds size limits.                  |
| `AUTHENTICATION_FAILED`   | Authentication failed due to an invalid or missing token.               |
| `AUTHORIZATION_FAILED`    | The caller does not have permission to delete products in this catalog. |
| `INTERNAL_ERROR`          | An unexpected server-side error occurred.                               |
