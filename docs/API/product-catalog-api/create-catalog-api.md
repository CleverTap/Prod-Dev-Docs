---
title: Create Catalog API
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

Creates a new catalog in CleverTap to store product data. You can optionally define a custom schema and configure automatic ingestion from external feeds such as Google Merchant XML.

# Method

POST

# Endpoint

`POST /v2/catalogs`

# &#x20;Base URL

`https://in1.api.clevertap.com/v2/catalogs`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

The following headers must be included with every API request to ensure proper authentication and content handling.

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

# Request Parameters

The following table lists and describes the fields accepted by this API:

| Parameter                           | Type    | Required | Description                                                                                                                                                                                                                  |
| ----------------------------------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| catalogName                         | string  | Yes      | Unique name of the catalog. This name is used to identify the catalog in future operations.                                                                                                                                  |
| catalogSchema                       | array   | Yes      | List of fields that define the structure of items in the catalog. Each entry represents one field in the catalog.                                                                                                            |
| catalogSchema.name                  | string  | Yes      | Name of the catalog field. This is how the attribute will be stored and referenced.                                                                                                                                          |
| catalogSchema.type                  | string  | Yes      | Data type of the field. One of string, number, boolean, date, datetime, array, object. Case-insensitive.                                                                                                                     |
| catalogSchema.filterable            | boolean | No       | Indicates whether this field can be used in filters when querying the catalog. Default is `false` if not specified.                                                                                                          |
| fieldMappings                       | object  | No       | Maps incoming source fields to catalog fields. Useful when the data source uses field names different from those in the catalog schema.                                                                                      |
| description                         | string  | No       | Short description of the catalog and its purpose.                                                                                                                                                                            |
| createdBy                           | string  | Yes      | Email address of the user creating the catalog. Used for ownership and audit purposes.                                                                                                                                       |
| recurringSyncConfig                 | object  | No       | Configuration for automatically syncing catalog data from an external source.                                                                                                                                                |
| recurringSyncConfig.syncFrequency   | string  | No       | Sets how often the catalog syncs with the source. Accepted values are HOURLY, DAILY, WEEKLY, MONTHLY, CUSTOM, and NONE. NONE runs a single one-time import: CleverTap queues one upload job and leaves the sync disabled. \| |
| recurringSyncConfig.integrationType | string  | No       | Sets how data is imported. Accepted values are GOOGLE_MERCHANT, FILE_BASED_UPLOAD, CLOUD_BUCKET, and EXTERNAL_SOURCE_PULL.                                                                                                   |
| recurringSyncConfig.sourceUrl       | string  | No       | URL of the external file or data source used for syncing.                                                                                                                                                                    |
| recurringSyncConfig.fileFormat      | string  | No       | Format of the source file. For example, `CSV` or `JSON`.                                                                                                                                                                     |

<Callout icon="📘" theme="info">
  ### Catalog Schema Constraints

  - A catalog can include 0 to 97 custom attributes in catalogSchema.
  - The total number of columns, 3 fixed fields plus all custom attributes, must not exceed 100.
  - The built-in fields identity, name, and imageUrl are always present and must not appear inside catalogSchema.
  - identity, name, image_url, and imageurl are reserved. Using any of them as a custom attribute name returns 400 with Attribute name 'X' is reserved and cannot be used.
  - Attribute names must be unique within the catalog. Comparison is case-insensitive, so Brand and brand collide and return 400 with Duplicate attribute name 'X' found in schema.
</Callout>

# Example Request

The following is the sample request body for creating a catalog with custom attributes and a recurring sync configuration:

```json
{
  "catalogName": "electronics_catalog",
  "catalogSchema": [
    {
      "name": "brand",
      "type": "string",
      "filterable": true
    },
    {
      "name": "price",
      "type": "number",
      "filterable": true
    },
    {
      "name": "category",
      "type": "string",
      "filterable": true
    },
    {
      "name": "in_stock",
      "type": "boolean"
    }
  ],
  "fieldMappings": {
    "manufacturer": "brand",
    "cost": "price"
  },
  "description": "Product catalog for electronics store",
  "createdBy": "admin@example.com",
  "recurringSyncConfig": {
    "syncFrequency": "DAILY",
    "integrationType": "FILE_BASED_UPLOAD",
    "sourceUrl": "https://example.com/products/feed.csv",
    "fileFormat": "CSV"
  }
}
```

<Callout icon="📘" theme="info">
  ### **Note**

  The built-in fields `identity`, `name`, and `imageUrl` are always included and do not appear inside `catalogSchema`.
</Callout>

# Example Response

The following is an example of a successful response when a new catalog is created.

```json
{
  "id": 123,
  "catalogName": "electronics_catalog",
  "catalogSchema": [
    {
      "name": "brand",
      "type": "string",
      "filterable": true
    },
    {
      "name": "price",
      "type": "number",
      "filterable": true
    },
    {
      "name": "category",
      "type": "string",
      "filterable": true
    },
    {
      "name": "in_stock",
      "type": "boolean",
      "filterable": false
    }
  ],
  "fieldMappings": {
    "manufacturer": "brand",
    "cost": "price"
  },
  "description": "Product catalog for electronics store",
  "totalProducts": 0,
  "status": "ACTIVE",
  "createdAt": 1699564800,
  "updatedAt": 1699564800,
  "createdBy": "admin@example.com",
  "updatedBy": "admin@example.com",
  "isActive": true, 
  "attributeCount": 4,
  "totalColumns": 7
}
```

# Response Parameters

The response includes all request parameters along with the following additional response parameters:

| Parameter           | Type    | Description                                                                        |
| ------------------- | ------- | ---------------------------------------------------------------------------------- |
| id                  | integer | Unique identifier of the catalog.                                                  |
| totalProducts       | integer | Total number of products currently available in the catalog.                       |
| status              | string  | Current status of the catalog, for example, `ACTIVE` or `INACTIVE`.                |
| createdAt           | integer | Time when the catalog was created, in Unix timestamp format.                       |
| updatedAt           | integer | When the catalog was last updated, in Unix epoch seconds.                          |
| recurringSyncConfig | object  |                                                                                    |
| updatedBy           | string  | Email address of the user who last updated the catalog.                            |
| isActive            | boolean | Indicates whether the catalog is currently active.                                 |
| attributeCount      | integer | Total number of attributes defined in the catalog schema.                          |
| totalColumns        | integer | Total number of columns available in the catalog, including system-defined fields. |

# Catalog Schema Constraints

A catalog can include 0 to 197 custom attributes defined in the catalogSchema.

Attribute names must:

- Start with a letter or underscore (\_)
- Contain only alphanumeric characters, underscores (\_), dashes (-), spaces, or parentheses ()
- The total number of columns, including 3 fixed attributes and all custom attributes, must not exceed 200.

# Errors

The following table lists the possible errors that this API may return:

| HTTP Status | Description                                                                                                                                                                                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 400         | Validation failure, invalid JSON, or an empty body. The message names the failing field, for example, Validation failed: catalogName: Catalog name must be 1-255 characters. CleverTap codes 106 (invalid JSON) and 108 (invalid payload) may accompany this. |
| 401         | Authentication failed, or the catalogs feature is not enabled on the account's plan.                                                                                                                                                                          |
| 403         | Catalog feature is not enabled for this account (CleverTap code 101).                                                                                                                                                                                         |
| 409         | A non-deleted catalog with this catalogName already exists: 'X'. A name belonging to a deleted catalog is free to reuse.                                                                                                                                      |
| 413         | Request payload exceeds 100 KB (CleverTap code 105).                                                                                                                                                                                                          |
| 500         | Unexpected server-side error.                                                                                                                                                                                                                                 |
| 503         | The account is not ready to serve requests yet. Retry shortly.                                                                                                                                                                                                |
