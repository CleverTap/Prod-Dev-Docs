---
title: List Catalogs API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Retrieves a list of catalogs associated with the CleverTap account. This API supports filtering, searching, sorting, and pagination, enabling clients to efficiently fetch catalogs based on their specific criteria.

# Method

GET

# Endpoint

`GET /v2/catalogs`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure successful authentication and correct payload handling.

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

# Query Parameters

The following parameters can be used to refine the catalog listing results.

| Parameter   | Type    | Required | Description                                                                                               |
| ----------- | ------- | -------- | --------------------------------------------------------------------------------------------------------- |
| `status`    | String  | No       | Filter catalogs by status: `ACTIVE`, `INACTIVE`, or `DELETED`.                                            |
| `search`    | String  | No       | Performs a partial match on catalog names.                                                                |
| `page`      | Integer | No       | Page number for paginated results. Defaults to `0`.                                                       |
| `size`      | Integer | No       | Number of records per page. Defaults to `20`. The maximum allowed is`100`.                                |
| `sortBy`    | String  | No       | Sort field. Options include `createdTs`, `name`, `updatedTs`, `totalProducts`, `status`, and `createdBy`. |
| `sortOrder` | String  | No       | Sort direction. Allowed values: `ASC` or `DESC`. Defaults to `DESC`.                                      |

# Example Request

This example demonstrates how to retrieve catalogs with filters and pagination applied.

```curl
GET /v2/catalogs?status=ACTIVE&search=electronics&page=0&size=20&sortBy=createdTs&sortOrder=DESC
```

# Example Response

This sample response illustrates the structure returned when catalog records are successfully retrieved.

```json
{
  "content": [
    {
      "id": 123,
      "catalogName": "electronics_catalog",
      "catalogSchema": [
        {
          "name": "brand",
          "type": "string",
          "filterable": true
        }
      ],
      "fieldMappings": {
        "manufacturer": "brand"
      },
      "description": "Product catalog for electronics store",
      "totalProducts": 1500,
      "status": "ACTIVE",
      "createdAt": 1699564800,
      "createdBy": "admin@example.com",
      "updatedBy": "admin@example.com",
      "isActive": true,
      "attributeCount": 4,
      "totalColumns": 7
    }
  ],
  "page": 0,
  "size": 20,
  "totalElements": 1,
  "totalPages": 1
}
```

# Response Parameters

The response includes a list of catalogs along with pagination details. Each catalog object contains the standard catalog fields along with additional system-generated fields.

| Parameter              | Type    | Description                                                                        |
| ---------------------- | ------- | ---------------------------------------------------------------------------------- |
| content                | array   | List of catalogs returned for the current page.                                    |
| content.id             | integer | Unique identifier of the catalog.                                                  |
| content.totalProducts  | integer | Total number of products available in the catalog.                                 |
| content.status         | string  | Current status of the catalog, for example, `ACTIVE` or `INACTIVE`.                |
| content.createdAt      | integer | Time when the catalog was created, in Unix timestamp format.                       |
| content.updatedBy      | string  | Email address of the user who last updated the catalog.                            |
| content.isActive       | boolean | Indicates whether the catalog is currently active.                                 |
| content.attributeCount | integer | Total number of attributes defined in the catalog schema.                          |
| content.totalColumns   | integer | Total number of columns available in the catalog, including system-defined fields. |
| page                   | integer | Current page number returned in the response.                                      |
| size                   | integer | Number of records returned per page.                                               |
| totalElements          | integer | Total number of catalogs available.                                                |
| totalPages             | integer | Total number of pages available based on the page size.                            |

# Errors

The following errors may be returned by the API:

| Errors                    | Description                                               |
| ------------------------- | --------------------------------------------------------- |
| `CATALOG_INVALID_REQUEST` | One or more query parameters are invalid.                 |
| `AUTHENTICATION_FAILED`   | Authentication failed due to an invalid or missing token. |
| `AUTHORIZATION_FAILED`    | User does not have permission to access catalog data.     |
| `INTERNAL_ERROR`          | An unexpected server error occurred.                      |
