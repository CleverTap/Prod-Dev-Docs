---
title: Pre-Signed URL API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

Upload catalog files to CleverTap managed storage using a secure, scalable mechanism. The process involves generating _pre-signed URLs_, _uploading the file (single or multipart)_, and _completing the upload when required_. Both single-file and multipart uploads require this completion step. Skipping it for a single-file upload leaves the catalog inactive on the dashboard, and its products are never processed.

This flow is designed for efficient bulk ingestion of catalog data, such as CSV or XML files. You must reference an existing catalog by its ID or name.

Follow these steps to upload a catalog file:

1. [Generate upload URLs using the Get Signed URL API](doc:pre-signed-url-api#generate-upload-urls-using-the-get-signed-url-api)
2. [Upload the file using the returned pre-signed URLs](doc:pre-signed-url-api#upload-file-using-the-returned-pre-signed-urls)
3. [Complete the upload using the Complete Multipart API](doc:pre-signed-url-api#complete-multipart-upload-using-the-complete-multipart-api)

<Callout icon="📘" theme="info">
  ### **Availability**

  The Catalog v2 endpoints require the Catalogs feature to be enabled on your account and plan. If it is not enabled, requests fail with 401 or 403. To enable the feature, contact your Customer Success Manager.
</Callout>

# Method

GET

# Base URL

`https://in1.api.clevertap.com`

# Region

Refer [Region](doc:https://developer.clevertap.com/docs/common-api-components#region) for more details.

# Headers

These headers must be included with every API request to ensure authentication and authorization.

| Header                 | Description                                            | Required |
| ---------------------- | ------------------------------------------------------ | -------- |
| Content-Type           | Content type of the request. Must be application/json. | Yes      |
| X-CleverTap-Account-Id | CleverTap account ID associated with the catalog.      | Yes      |
| X-CleverTap-Passcode   | API passcode used to authenticate the request.         | Yes      |

# Generate Upload URLs using the Get Signed URL API

Generate pre-signed upload URLs for your catalog file.

## Endpoint

`GET /v2/catalogs/signedurl`

## Upload Type

Choose from the upload methods based on the size of the file you want to upload:

<Tabs>
  <Tab title="Single Upload">
    Use this API for files smaller than 100 MB.

    ### Query Parameters

    | Parameter   | Type    | Required    | Description                                                                                                                                                                                                                 |
    | ----------- | ------- | ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | catalogId   | long    | Yes         | Unique identifier of the catalog where the product file will be uploaded.                                                                                                                                                   |
    | fileName    | string  | Yes         | Name of the file to upload. Maximum length is 500 characters.                                                                                                                                                               |
    | contentType | string  | Yes         | MIME type of the file being uploaded. Use text/csv for FILE_BASED_UPLOAD and CLOUD_BUCKET integrations, and text/xml or application/xml for GOOGLE_MERCHANT integrations.                                                   |
    | multiPart   | boolean | Yes         | Indicates whether the upload will use a multipart upload. Set to `true` to generate multiple upload URLs, or `false` for a single file upload.                                                                              |
    | totalParts  | integer | Conditional | Total number of parts for multipart upload. Required when multiPart is set to true. The minimum value is 1. There is currently no enforced maximum, but very large values are discouraged for practical upload performance. |
    | createdBy   | string  | Yes         | Identifier of the user or system requesting the signed URL. Maximum length is 100 characters. The identifier must be an email ID.                                                                                           |

    ### Example Request

    ```json
    GET /v2/catalogs/signedurl?catalogId=123&fileName=products.csv&contentType=text/csv&multiPart=false&createdBy=admin@example.com
    ```

    ### Example Response

    ```json
    {
      "requestId": "550e8400-e29b-41d4-a716-446655440000",
      "catalogId": 123,
      "fileName": "products.csv",
      "multipartUpload": false,
      "uploadId": null,
      "presignedUrls": [
        "https://s3.amazonaws.com/..."
      ],
      "expiresAt": "2023-11-10T18:30:00Z",
      "instructions": "Upload the file using PUT request to the provided URL",
      "createdAt": "2023-11-10T17:30:00Z"
    }
    ```

    ### Response Parameters

    | Parameter       | Type    | Description                                                                                                       |
    | --------------- | ------- | ----------------------------------------------------------------------------------------------------------------- |
    | requestId       | string  | Unique identifier for this signed URL request. Can be used for tracking or support purposes.                      |
    | catalogId       | integer | Unique identifier of the catalog where the file will be uploaded.                                                 |
    | fileName        | string  | Name of the file to be uploaded.                                                                                  |
    | multipartUpload | boolean | Indicates whether the upload is configured as multipart.                                                          |
    | uploadId        | string  | Upload identifier for multipart uploads. This value is `null` for single-file uploads.                            |
    | presignedUrls   | array   | List of pre-signed URLs that can be used to upload the file. For multipart uploads, one URL is returned per part. |
    | expiresAt       | string  | Date and time when the signed URL expires. After this time, the URL can no longer be used.                        |
    | instructions    | string  | Short message explaining how to use the provided URL to upload the file.                                          |
    | createdAt       | string  | Date and time when the signed URL request was generated.                                                          |
  </Tab>

  <Tab title="MultiPart Upload">
    Use this for files larger than 100 MB.

    ### Query Parameters

    | Parameter   | Type    | Required | Description                                                                      |
    | ----------- | ------- | -------- | -------------------------------------------------------------------------------- |
    | catalogId   | long    | Yes      | Unique identifier of the catalog where the product file will be uploaded.        |
    | fileName    | string  | Yes      | Name of the file to upload. Maximum length is 500 characters.                    |
    | contentType | string  | Yes      | MIME type of the file being uploaded.                                            |
    | multiPart   | boolean | Yes      | Set to true to generate multiple upload URLs.                                    |
    | totalParts  | integer | Yes      | Total number of parts for the multipart upload. Minimum value is 1.              |
    | createdBy   | string  | Yes      | Identifier of the user or system requesting the signed URL. Must be an email ID. |

    ### Example Request

    ```json
    GET /v2/catalogs/signedurl?catalogId=123&fileName=large_products.csv&contentType=text/csv&multiPart=true&totalParts=5&createdBy=admin@example.com
    ```

    ### Example Response

    ```json
    {
      "requestId": "550e8400-e29b-41d4-a716-446655440001",
      "catalogId": 123,
      "fileName": "large_products.csv",
      "multipartUpload": true,
      "uploadId": "exampleUploadId123",
      "presignedUrls": [
        "https://s3.amazonaws.com/...partNumber=1...",
        "https://s3.amazonaws.com/...partNumber=2..."
      ],
      "expiresAt": "2023-11-10T18:30:00Z",
      "instructions": "Upload each part using PUT request and then complete the upload",
      "createdAt": "2023-11-10T17:30:00Z"
    }
    ```

    ### Response Parameters

    | Parameter       | Type    | Description                                                                                 |
    | --------------- | ------- | ------------------------------------------------------------------------------------------- |
    | requestId       | string  | Unique identifier for the upload request.                                                   |
    | catalogId       | integer | Unique identifier of the catalog where the file was uploaded.                               |
    | fileName        | string  | Name of the uploaded file.                                                                  |
    | multipartUpload | boolean | Indicates whether the upload was completed as a multipart upload.                           |
    | uploadId        | string  | Identifier of the multipart upload process.                                                 |
    | presignedUrls   | array   | List of presigned URLs. This will typically be empty once the upload is completed.          |
    | expiresAt       | string  | Date and time when the upload session was set to expire.                                    |
    | instructions    | string  | Message confirming that the upload is complete and that file processing will begin shortly. |
    | createdAt       | string  | Date and time when the upload request was originally created.                               |
  </Tab>
</Tabs>

# Upload File using the Returned Pre-signed URLs

Use the returned pre-signed URLs to upload the file directly to storage.

- Use HTTP PUT requests on each pre-signed URL
- Do not add authentication headers to these requests
- For multipart uploads:

  - Upload each part separately
  - Capture the ETag from each upload response
  - Store partNumber and corresponding ETag

<Callout icon="📘" theme="info">
  ### **File Upload Guidelines**

  - Single file uploads are recommended for files under 100 MB, and multipart uploads for files larger than 100 MB. This is a recommendation, not an enforced limit, the API accepts either upload type regardless of file size.
  - Multipart uploads should be used for files larger than 100 MB.
  - Each presigned URL expires after 60 minutes by default.
  - The upload must be completed within that window. If the URLs expire, start again from step 1.
  - If a longer expiry is requested, it is capped at a maximum of 24 hours.
  - Pre-signed URLs expire 60 minutes after they are generated.&#x20;
  - The generated URLs must be used to upload the file using PUT requests
</Callout>

# Complete Multipart Upload using the Complete Multipart API

Complete a multipart upload by submitting the ETags for all uploaded parts. This endpoint finalizes the upload and initiates processing of the file.

## Steps for Multipart Upload

Follow these steps to upload a file using the multipart upload process:

1. Call `GET /v2/catalogs/signedurl` with `multiPart=true` and `totalParts=N` to get `N` presigned URLs
2. Upload each file part using a PUT request to the corresponding presigned URL
3. Collect the ETag from the response header of each part upload
4. Call `POST /v2/catalogs/complete-multipart` with the requestId and all part ETags
5. The system will finalize the upload and begin processing the file

## Endpoint

`POST /v2/catalogs/complete-multipart`

## Query Parameters

The following is the list of query parameters:

| Parameter        | Type    | Required | Description                                                                                                   |
| ---------------- | ------- | -------- | ------------------------------------------------------------------------------------------------------------- |
| requestId        | string  | Yes      | Unique identifier received when generating the signed URLs. Used to complete the multipart upload.            |
| updatedBy        | string  | Yes      | Email address of the user or system completing the upload.                                                    |
| parts            | array   | Yes      | List of uploaded parts that need to be combined to complete the file upload.                                  |
| parts.partNumber | integer | Yes      | Part number of the uploaded file segment. Must match the part number used during upload.                      |
| parts.etag       | string  | Yes      | ETag value returned by the storage service after uploading each part. Used to verify and finalize the upload. |

## Example Request

The following is the sample request:

```json
{
  "requestId": "550e8400-e29b-41d4-a716-446655440001",
  "updatedBy": "admin@example.com",
  "parts": [
    {
      "partNumber": 1,
      "etag": "\"33a64df551425fcc55e4d42a148795d9f25f89d4\""
    },
    {
      "partNumber": 2,
      "etag": "\"6821e4a7e3e3d5b4f3e9c8d7b6a5f4e3d2c1b0a9\""
    },
    {
      "partNumber": 3,
      "etag": "\"a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8\""
    },
    {
      "partNumber": 4,
      "etag": "\"b8a7f6e5d4c3b2a1f0e9d8c7b6a5f4e3d2c1b0a9\""
    },
    {
      "partNumber": 5,
      "etag": "\"c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0\""
    }
  ]
}
```

## Example Response

The following is the sample response:

```json
{
  "accountId": 12345,
  "requestId": "550e8400-e29b-41d4-a716-446655440001",
  "catalogId": 123,
  "fileName": "large_products.csv",
  "multipartUpload": true,
  "uploadId": "exampleUploadId123",
  "presignedUrls": [],
  "expiresAt": "2023-11-10T18:30:00Z",
  "instructions": "Upload completed successfully. File will be processed shortly.",
  "createdAt": "2023-11-10T17:30:00Z"
}
```

## Response Parameters

The following parameters are returned when the upload completion request is accepted successfully:

| Parameter       | Type    | Description                                                                                 |
| --------------- | ------- | ------------------------------------------------------------------------------------------- |
| accountId       | integer | CleverTap account ID for which the request was made.                                        |
| requestId       | string  | Unique identifier for the upload request.                                                   |
| catalogId       | integer | Unique identifier of the catalog where the file was uploaded.                               |
| fileName        | string  | Name of the uploaded file.                                                                  |
| multipartUpload | boolean | Indicates whether the upload was completed as a multipart upload.                           |
| uploadId        | string  | Identifier of the multipart upload process.                                                 |
| presignedUrls   | array   | List of presigned URLs. This will typically be empty once the upload is completed.          |
| expiresAt       | string  | Date and time when the upload session was set to expire.                                    |
| instructions    | string  | Message confirming that the upload is complete and that file processing will begin shortly. |
| createdAt       | string  | Date and time when the upload request was created.                                          |

<Callout icon="📘" theme="info">
  ### **Guidelines for Complete Multipart Upload**

  - Parts must be provided in order by partNumber
  - ETags are returned by S3 in the response headers after uploading each part
  - The upload must be completed within the expiry time of the presigned URLs
</Callout>

# Error Handling

This section lists the errors returned by both the _Get Signed URL_ and _Complete Multipart Upload_ APIs.

## Get Signed URL Errors

The following error codes may be returned by this API:

| Error Code                | Description                                                                |
| ------------------------- | -------------------------------------------------------------------------- |
| `CATALOG_NOT_FOUND`       | The specified catalog does not exist.                                      |
| `CATALOG_INVALID_REQUEST` | Both `catalogId` and `catalogName` were provided, or neither was provided. |
| `AUTHENTICATION_FAILED`   | Authentication failed due to an invalid or missing token.                  |
| `AUTHORIZATION_FAILED`    | The caller does not have permission to access this catalog.                |
| `INTERNAL_ERROR`          | An unexpected server-side error occurred.                                  |

## Complete Multipart Upload Errors

The following error codes may be returned by this API:

| Error Code              | Description                                                                                                      |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------- |
| REQUEST_NOT_FOUND       | The specified requestId does not exist or has expired.                                                           |
| CATALOG_NOT_FOUND       | The specified catalog does not exist.                                                                            |
| CATALOG_INVALID_REQUEST | The request payload is invalid or missing required fields (for example, missing requestId, updatedBy, or parts). |
| AUTHENTICATION_FAILED   | Authentication failed due to an invalid or missing token.                                                        |
| AUTHORIZATION_FAILED    | The caller does not have permission to complete this upload.                                                     |
| INTERNAL_ERROR          | An unexpected server-side error occurred.                                                                        |
