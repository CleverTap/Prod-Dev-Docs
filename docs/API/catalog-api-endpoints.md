---
title: Catalog API
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Overview

The catalog API endpoint is the point of entry in a communication channel where the catalog API interacts with a server.

A product catalog represents a collection of items in your inventory, such as the list of products in an e-commerce application or the list of shows in a video streaming application. A product location catalog represents the list of locations for a business, such as store IDs.

# Upload a Catalog

The maximum file size in a single upload is 5 GB.

1. Request a pre-signed URL with your first API call. A pre-signed URL gives you temporary access to upload your catalog. This request returns a URL that you can use in the [Upload a Catalog File](https://developer.clevertap.com/docs/catalog-api-endpoints#upload-a-catalog) step.
2. Upload your catalog to the URL received from the [Create a Pre-signed URL](https://developer.clevertap.com/docs/catalog-api-endpoints#create-a-pre-signed-url) step.
3. Make a second API call to signal that you have completed the catalog upload. CleverTap selects the catalog and displays it on the dashboard.

# Upload Single Catalog

## Create a Pre-signed URL

This request returns a pre-signed URL to upload your catalog.

## Base URL

Here is an example base URL from the account in the India region:  
[https://in1.api.clevertap.com/get_catalog_url](https://in1.api.clevertap.com/get_catalog_url)

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP POST Method

This section provides information on the HTTP POST method.

### Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

### Example Request

Here is an example cURL request to the _Catalog_ API showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST \
  https://in1.api.clevertap.com/get_catalog_url \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/json' \
  -H 'X-CleverTap-Account-Id: ACCOUNT_ID' \
  -H 'X-CleverTap-Passcode: PASSCODE'
```

### JSON Example Response

Below is a JSON example request:

```json
{
    "presignedS3URL": <PRE-SIGNED URL>,
    "expiryEpoch": {
        "$date": "2019-04-24T13:04:37.088Z"
    },
    "status": "success"
```

A success response is displayed if the pre-signed URL is generated: `200 OK`.

The pre-signed URL is valid for a maximum of 24 hours from generation.

If the pre-signed URL is not successfully generated, you get the following response:

```json
{ "expiryEpoch": {"$date": "2019-04-24T13:04:37.088Z"}
```

## Upload a Catalog File

To upload a catalog file, perform the following steps:

1. Upload the catalog file to the pre-signed URL received from the [Create a Pre-signed URL](https://developer.clevertap.com/docs/catalog-api-end) step above.

The catalog must be in a CSV format. Refer to [Catalogs](https://docs.clevertap.com/docs/catalog) for more information about the specifications of the catalog file.

2. Use the following command to upload the file:

### cURL Example Request

Below is a cURL example request:

### Example Response

A successful response displays the file upload's progress until it is completed.

## Complete Single Catalog Upload

Using the pre-signed URL received from the [Create a Pre-signed URL](https://developer.clevertap.com/docs/catalog-api-endpoints#create-a-pre-signed-url) step above, make an API call to indicate that you have completed the upload of the catalog. When this step is completed successfully, CleverTap selects the uploaded catalog and displays it on the dashboard.

### Base URL

Here is an example base URL from the account in the India region:  
[https://in1.api.clevertap.com/upload_catalog_completed](https://in1.api.clevertap.com/upload_catalog_completed)

To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/catalog-api-endpoints#region).

## HTTP POST Method

This section provides information on the HTTP POST method.

#### Headers

The X-CleverTap-Account-Id and X-CleverTap-Passcode authenticate the request. Refer to the [authentication guide](doc:authentication) to learn how to get these values.

The headers in the following table are all required:

| Header                 | Description                                             | Type   | Example Value                        |
| :--------------------- | :------------------------------------------------------ | :----- | :----------------------------------- |
| X-CleverTap-Account-Id | Your CleverTap account ID.                              | string | "X-CleverTap-Account-Id: ACCOUNT_ID" |
| X-CleverTap-Passcode   | Your CleverTap account passcode.                        | string | "X-CleverTap-Passcode: PASSCODE"     |
| Content-Type           | Request content-type is always set to application/JSON. | string | "Content-Type: application/json"     |

#### Body Parameters

The following table explains the body parameters:

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        name
      </td>

      <td>
        Catalog name.
      </td>

      <td>
        string
      </td>

      <td>
        Product_Catalog
      </td>
    </tr>

    <tr>
      <td>
        creator
      </td>

      <td>
        Name of the user making this API call.
      </td>

      <td>
        string
      </td>

      <td>
        Jane
      </td>
    </tr>

    <tr>
      <td>
        email
      </td>

      <td>
        The owner's email address which should be an admin email.
      </td>

      <td>
        string
      </td>

      <td>
        [jane@clevertap.com](mailto:jane@clevertap.com)
      </td>
    </tr>

    <tr>
      <td>
        url
      </td>

      <td>
        The pre-signed URL that identifies this catalog’s location received as a response to the first API call.
      </td>

      <td>
        string
      </td>

      <td>
        \<PRE-SIGNED URL>
      </td>
    </tr>

    <tr>
      <td>
        replace
      </td>

      <td>
        Optional parameter. Set this parameter to _true_ to replace a catalog that exists with the same name as the one on the current API call.
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        override
      </td>

      <td>
        Optional parameter. Prerequisite: The _replace_ flag must be set to _true_.  
        The default value is false. The replacement will fail if any columns are missing.  
        Set this flag to _true_ to  force an update.
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>
  </tbody>
</Table>

#### Example Requests

Below is a JSON example request for a product catalog:
For a product location catalog, use the following flag in the request: `"isLocationCatalog": true`.

Below is a JSON example request for a product location catalog:

#### Example Response

The following success response appears if there are no immediate errors in the catalog file upload: `200 OK`.

If you upload a catalog with an existing name, then CleverTap replaces the old catalog if the _replace_ flag is set to _true_. If the _replace_ flag is set to _false_, then the new upload is rejected, and the following response displays:

If the catalog file is not found at the specified URL, the following response displays:

If you do not specify the mandatory parameters, the following response displays:

If the email ID is either in the wrong format or is invalid, the following response displays:

If an old URL which was successfully processed previously is re-used with a new catalog name, you will get the following response:

If you upload more than the specified five-catalog limit, the following response displays:

#### Example Response on Email

The following responses are emailed to the owner’s email address mentioned in this API call as these need some processing.

If the catalog is uploaded successfully, the following response displays:

If the catalog is re-uploaded successfully, the following response displays:

If you upload more than the specified five-catalog limit, the following response displays:

If the number of rows exceeds 5 million, the following response displays:

If any of the three mandatory fields (_Name_, _ImageURL_, or _Category_) are missing from the catalog, the following response displays.

For more information on mandatory catalog fields, refer to [Catalogs](https://docs.clevertap.com/docs/catalog#section-mandatory).

If the number of columns in the catalog exceeds 20, the following response displays:

If some rows in the catalog file do not adhere to the correct format, the following response displays:

# Upload Multipart Catalog

If the file size of the catalog is greater than 5GB, you can split the file into smaller parts (minimum 2 or maximum 4) and upload them. Ensure that the last row of these files must end with \n. Only the first file in the multipart catalog upload must contain the header row.

# Create Pre-signed URLs

This request returns _n_ number of pre-signed URLs to upload your catalog.

## Base URL

Here is an example base URL from the account in the India region for uploading multiple parts:  
_[https://in1.api.clevertap.com/get_multipart_upload_catalog_url?totalParts=n](https://in1.api.clevertap.com/get_multipart_upload_catalog_url?totalParts=n)_  
where _n_ is the number of parts that you have split the catalog.

To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/catalog-api-endpoints#region).

### Headers

The X-CleverTap-Account-Id and X-CleverTap-Passcode authenticate the request. Refer to the [authentication guide](doc:authentication) to learn how to get these values.

The headers in the following table are all required:

| Header                 | Description                                             | Type   | Example Value                        |
| :--------------------- | :------------------------------------------------------ | :----- | :----------------------------------- |
| X-CleverTap-Account-Id | Your CleverTap account ID.                              | string | "X-CleverTap-Account-Id: ACCOUNT_ID" |
| X-CleverTap-Passcode   | Your CleverTap account passcode.                        | string | "X-CleverTap-Passcode: PASSCODE"     |
| Content-Type           | Request content-type is always set to application/JSON. | string | "Content-Type: application/json"     |

### Example Request

Here is an example cURL request to the _Catalog_ API showing the headers needed to authenticate the request from the account in the India region:

Check that you provide _totalParts=n_in the request, where _n_ is the number of splits to the file.

The response will provide _n_ pre-signed URLs in an array along with the `uploadId`.  Save the `uploadId` because it is necessary for the last step.

### Example Response

A success response is displayed if the pre-signed URL is generated: `200 OK`. The pre-signed URLs are valid for a maximum of 24 hours from generation.

## Upload catalog part

Upload each file part. Following is an example for the first part. Repeat this command for all the successive parts.

### JSON Example Response

You will receive a corresponding `eTag` after each successful upload. Save this `eTag`. as shown in the exacmple below.

## Complete Multipart Upload

You must indicate that the upload is complete. Following is an example request.

The `partNumber`, `eTag`, and `uploadId` are required.

> 📘 Process Duration
>
> All the parts are merged back into a single file. Large files take some time to process.

# Dashboard Statuses

You can view the catalog on the dashboard after it has been uploaded successfully.

Other possible upload statuses include:

* Success: The catalog is uploaded successfully.
* Pending: The catalog upload is in progress.
* Error: The catalog upload failed.
* Replacement pending: The new catalog is still not in effect. Until replaced, any campaigns will be sent with the previously uploaded catalog.

The campaigns and [catalog data ingestion](https://docs.clevertap.com/docs/catalog-data-ingestion) use the new catalog only after the old catalog is updated. In the meantime, the campaigns and  [catalog data ingestion](https://docs.clevertap.com/docs/catalog-data-ingestion) use the old catalog.

# FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).
