---
title: Mark a Signed URL API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Marks a signed URL upload as complete and triggers server-side processing of the uploaded catalog file. This API is typically called after you successfully upload the file to the `signedUrl` returned by the [Generate Signed URL API](doc:pre-signed-url-api).

# Method

POST

# Endpoint

`POST /v2/catalogs/signedurl/complete`

# Base URL

`https://in1.api.clevertap.com/v2/catalogs/signedurl/complete`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and correct request handling.

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

# Request Parameters

The following parameters specify the uploaded file and the catalog that should ingest it.

| Parameter     | Type   | Required | Description                                                         |
| ------------- | ------ | -------- | ------------------------------------------------------------------- |
| `uploadId`    | String | Yes      | Upload session identifier returned by the Generate Signed URL API.  |
| `catalogId`   | Number | Yes      | Unique identifier of the catalog that the uploaded file belongs to. |
| `fileName`    | String | Yes      | Name of the uploaded file (for example, `products.csv`).            |
| `fileSize`    | Number | Yes      | Size of the uploaded file in bytes.                                 |
| `completedBy` | String | Yes      | Identifier of the user or system marking the upload as complete.    |

# Example Request

This example marks the upload identified by `upload_abc123` as complete and starts processing the uploaded file into catalog `123`.

```json
{
  "uploadId": "upload_abc123",
  "catalogId": 123,
  "fileName": "products.csv",
  "fileSize": 12345,
  "completedBy": "user@example.com"
}
```

# Example Response

This example response indicates the catalog file has been accepted, and processing has started asynchronously.

```json
{
  "status": "PROCESSING",
  "jobId": "job_xyz789",
  "estimatedCompletionTime": "2025-11-06T10:45:00Z"
}
```

# Response Parameters

The following parameters are returned when the upload completion request is accepted successfully.

| Parameter                 | Type   | Description                                                          |
| ------------------------- | ------ | -------------------------------------------------------------------- |
| `status`                  | String | Current processing status of the upload. Common value: `PROCESSING`. |
| `jobId`                   | String | Identifier of the processing job created for this upload.            |
| `estimatedCompletionTime` | String | Estimated completion timestamp for processing (ISO 8601).            |

# Error Codes

The following error codes may be returned by this API:

| Error Code                | Description                                                            |
| ------------------------- | ---------------------------------------------------------------------- |
| `UPLOAD_NOT_FOUND`        | The specified `uploadId` does not exist or is not valid.               |
| `CATALOG_NOT_FOUND`       | The specified `catalogId` does not exist.                              |
| `CATALOG_INVALID_REQUEST` | The request payload is invalid (for example, missing required fields). |
| `AUTHENTICATION_FAILED`   | Authentication failed due to invalid or missing token.                 |
| `AUTHORIZATION_FAILED`    | Caller does not have permission to complete uploads for this catalog.  |
| `INTERNAL_ERROR`          | An unexpected server-side error occurred.                              |
