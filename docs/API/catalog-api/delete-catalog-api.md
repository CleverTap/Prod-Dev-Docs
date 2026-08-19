---
title: Delete Catalog API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Deletes an existing catalog. This API permanently removes the catalog from the system.

<Callout icon="📘" theme="info">
  ### **This is a soft delete**

  The catalog's status is set to DELETED, so it no longer appears in the catalog list or in campaign catalog pickers. The underlying record is retained, and the catalog's name is released for reuse; you can create a new catalog with the same name afterward.

  There is no hard-delete option on this endpoint. To have catalog data purged permanently, contact CleverTap support.
</Callout>

# Method

PUT

# Endpoint

`PUT /v2/catalogs/delete/{id}`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/delete/{id}`

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

The following path parameter is required to identify the catalog to delete:

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| `id`      | Number | Yes      | Unique identifier of the catalog to delete. |

# Query Parameters

The following query parameter is required to perform the delete operation:

| Parameter   | Type   | Required | Description                                                       |
| ----------- | ------ | -------- | ----------------------------------------------------------------- |
| `deletedBy` | String | Yes      | Identifier of the user or system performing the delete operation. |

# Example Request

The following is the sample request:

```
PUT /v2/catalogs/delete/123?deletedBy=admin@example.com
```

# Example Response

The following is the sample response:

```json
{
  "success": true,
  "message": "Catalog - 123 deleted successfully"
}
```

# Response Parameters

The following table shows the response parameter:

| Parameter | Type    | Description                                                                |
| --------- | ------- | -------------------------------------------------------------------------- |
| success   | boolean | True when the catalog was deleted.                                         |
| message   | string  | Confirmation message indicating that the catalog was deleted successfully. |

## Blocked - Catalog is in Use by Live Engagements

If any live campaign or journey references the catalog, the delete is refused, and the blocking engagements are returned so you can show or resolve them. The HTTP status is 400.

```json
{
  "success": false,
  "message": "Cannot delete catalog. It is used by active engagements.",
  "engagementList": [
    {
      "engagementId": 1776934833,
      "engagementName": "Weekly product push",
      "engagementType": "CAMPAIGN",
      "createdBy": "growth@example.com",
      "newJourneyUI": false
    }
  ]
}
```

### Parameters

| Parameter                        | Type    | Description                                           |
| -------------------------------- | ------- | ----------------------------------------------------- |
| engagementList                   | array   | Live engagements that reference this catalog.         |
| engagementList\[].engagementId   | integer | Identifier of the blocking campaign or journey.       |
| engagementList\[].engagementName | string  | Name of the blocking engagement.                      |
| engagementList\[].engagementType | string  | CAMPAIGN or JOURNEY.                                  |
| engagementList\[].createdBy      | string  | Who created the blocking engagement.                  |
| engagementList\[].newJourneyUI   | boolean | Whether the journey uses the current journey builder. |

Stop or unpublish the listed engagements, or remove the catalog block from them, then retry the delete.

# Error Codes

The following error codes may be returned by this API.

This API returns a standard HTTP status code with a plain-text message describing the issue.

| HTTP Status | When                                                                                                                        |
| ----------- | --------------------------------------------------------------------------------------------------------------------------- |
| 400         | deletedBy is missing, the catalog ID is not a valid number, or the catalog is referenced by live engagements (see above).   |
| 401         | Authentication failed, or the catalogs feature is not enabled on the account's plan.                                        |
| 403         | Catalog feature is not enabled for this account (CleverTap code 101).                                                       |
| 404         | No catalog exists for the specified id.                                                                                     |
| 422         | The catalog still contains products and cannot be deleted in its current state.                                             |
| 500         | Unexpected server-side error. The message is deliberately generic, contact support with your account ID and the catalog ID. |
| 503         | The account is not ready to serve requests yet. Retry shortly.                                                              |
