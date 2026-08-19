---
title: Delete Single Product API
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

Deletes a single product by identity. This operation marks the product as inactive rather than permanently removing it from the catalog.

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

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

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

This API returns a standard HTTP status code along with a plain-text message describing the issue. Common status codes include:

| HTTP Status | Description                                                                                               |
| ----------- | --------------------------------------------------------------------------------------------------------- |
| 400         | The request is invalid, missing required fields, or exceeds a bulk limit (product count or payload size). |
| 403         | The caller lacks permission, or the catalog feature is not enabled for this account.                      |
| 404         | The specified catalog or product does not exist.                                                          |
| 409         | A conflicting resource already exists.                                                                    |
| 413         | The request payload exceeds the maximum allowed size.                                                     |
| 500         | An unexpected server-side error occurred.                                                                 |