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
<https://in1.api.clevertap.com/get_catalog_url>

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

```json
{ "expiryEpoch": {"$date": "2019-04-24T13:04:37.088Z"}
```

If the pre-signed URL is not successfully generated, you get the following response:

```text
URL not generated, Please come back later
```

## Upload a Catalog File

To upload a catalog file, perform the following steps:

1. Upload the catalog file to the pre-signed URL received from the [Create a Pre-signed URL](https://developer.clevertap.com/docs/catalog-api-end) step above. 

The catalog must be in a CSV format. Refer to [Catalogs](https://docs.clevertap.com/docs/catalog) for more information about the specifications of the catalog file.

2. Use the following command to upload the file:

```curl
$(curl --request PUT --upload-file "<path to catalog file on your local machine>" "<presigned URL>")
```

### cURL Example Request

Below is a cURL example request:

```curl
$(curl --request PUT 
--upload-file "Users⁩/janedoe/Documents⁩/catalog.csv”
"<PRE-SIGNED URL")
```

### Example Response

A successful response displays the file upload's progress until it is completed. 

```text
  %	Total %	Received %	 Xferd  Average Speed   Time     Time     Time    Current
                                Dload   Upload  Total    Spent    Left    Speed                               
100    92 0       0  100    92       0     105 --:--:-- --:--:-- --:--:--  105
```

## Complete Single Catalog Upload

Using the pre-signed URL received from the [Create a Pre-signed URL](https://developer.clevertap.com/docs/catalog-api-endpoints#create-a-pre-signed-url) step above, make an API call to indicate that you have completed the upload of the catalog. When this step is completed successfully, CleverTap selects the uploaded catalog and displays it on the dashboard.

### Base URL

Here is an example base URL from the account in the India region:  
<https://in1.api.clevertap.com/upload_catalog_completed>

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

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "name",
    "0-1": "Catalog name.",
    "0-2": "string",
    "0-3": "Product_Catalog",
    "1-0": "creator",
    "1-1": "Name of the user making this API call.",
    "1-2": "string",
    "1-3": "Jane",
    "2-0": "email",
    "2-1": "The owner's email address which should be an admin email.",
    "2-2": "string",
    "2-3": "[jane@clevertap.com](mailto:jane@clevertap.com)",
    "3-0": "url",
    "3-1": "The pre-signed URL that identifies this catalog’s location received as a response to the first API call.",
    "3-2": "string",
    "3-3": "<PRE-SIGNED URL>",
    "4-0": "replace",
    "4-1": "Optional parameter. Set this parameter to _true_ to replace a catalog that exists with the same name as the one on the current API call.",
    "4-2": "boolean",
    "4-3": "true",
    "5-0": "override",
    "5-1": "Optional parameter. Prerequisite: The _replace_ flag must be set to _true_.  \nThe default value is false. The replacement will fail if any columns are missing.  \nSet this flag to _true_ to  force an update.",
    "5-2": "boolean",
    "5-3": "true"
  },
  "cols": 4,
  "rows": 6,
  "align": [
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


#### Example Requests

Below is a JSON example request for a product catalog:

```json
curl -X POST \
  https://in1.api.clevertap.com/upload_catalog_completed \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/json' \
  -H 'X-CleverTap-Account-Id: ACCOUNT_ID' \
  -H 'X-CleverTap-Passcode: PASSCODE' \
  -H 'cache-control: no-cache' \
  -d '{ "name":"Product_Catalog", "email":"jane@clevertap.com", "creator":"Jane", "url":"<PRE-SIGNED URL>", "replace":false}'
```

For a product location catalog, use the following flag in the request: `"isLocationCatalog": true`.

Below is a JSON example request for a product location catalog:

```json
curl -X POST \
  https://in1.api.clevertap.com/upload_catalog_completed \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/json' \
  -H 'X-CleverTap-Account-Id: ACCOUNT_ID' \
  -H 'X-CleverTap-Passcode: PASSCODE' \
  -H 'cache-control: no-cache' \
  -d '{ "name":"Product_Catalog", "email":"jane@clevertap.com", "creator":"Jane", "url":"<PRE-SIGNED URL>", "replace":false, "isLocationCatalog": true}'
```

#### Example Response

The following success response appears if there are no immediate errors in the catalog file upload: `200 OK`.

If you upload a catalog with an existing name, then CleverTap replaces the old catalog if the _replace_ flag is set to _true_. If the _replace_ flag is set to _false_, then the new upload is rejected, and the following response displays:

```json
{
   "status": "fail",
   "error": "Duplicate Catalog name"
}
```

If the catalog file is not found at the specified URL, the following response displays:

```json
{
   "status": "fail",
   "Error": “Invalid URL”
}
```

If you do not specify the mandatory parameters, the following response displays:

```json
{
   "status": "fail",
   "Error": “Catalog name, email ID, catalog creator and pre-signed url are mandatory”
}
```

If the email ID is either in the wrong format or is invalid, the following response displays: 

```json
{
   "status": "fail",
   "Error": “email id is either not in the right format or does not belong to a valid admin”
}
```

If an old URL which was successfully processed previously is re-used with a new catalog name, you will get the following response:

```json
{
   "status": "fail",
   "Error": “New file referencing a previously used URL. Please change URL”
}
```

If you upload more than the specified five-catalog limit, the following response displays: 

```json
{
   "status": "fail",
   "Error": “Already 5 catalogs present for the account“
}
```

#### Example Response on Email

The following responses are emailed to the owner’s email address mentioned in this API call as these need some processing.

If the catalog is uploaded successfully, the following response displays:

```text
Catalog with name ‘Product_Catalog’ uploaded successfully
```

If the catalog is re-uploaded successfully, the following response displays:

```text
Catalog with name ‘Product_Catalog’ re-uploaded successfully
```

If you upload more than the specified five-catalog limit, the following response displays: 

```text
Only 5 number of catalogs are allowed
```

If the number of rows exceeds 5 million, the following response displays:

```text
Your catalog exceeds the maximum allowed limit of 5,000,000 rows
```

If any of the three mandatory fields (_Name_, _ImageURL_, or _Category_) are missing from the catalog, the following response displays. 

For more information on mandatory catalog fields, refer to [Catalogs](https://docs.clevertap.com/docs/catalog#section-mandatory).

```text
Identity, ImageURL, Category, SubCategory, Brand, Price, MaxDiscount, Season, UserRating, PartnerPromotion, Margin, "Missing Name field.Please add the field and upload the catalog again"
```

If the number of columns in the catalog exceeds 20, the following response displays:

```text
Identity, Name, ImageURL, Col3, Col4, Col5, Col6, Col7, Col8, Col9, Col10, Col11 Col12, Col13, Col14, Col15, Col16, Col17, Col18, Col19, Col20, Col21, "Your catalog exceeds maximum allowed limit of  20 columns"
```

If some rows in the catalog file do not adhere to the correct format, the following response displays:

```text
CT000003, CleverTap Navy Collar T-Shirt, https://www.images.com/CTNavyCollarT-Shirt.jpg, , Identities with extra fields found in the catalog. Correct the information and upload the catalog again
```

# Upload Multipart Catalog

If the file size of the catalog is greater than 5GB, you can split the file into smaller parts (minimum 2 or maximum 4) and upload them. Ensure that the last row of these files must end with \\n. Only the first file in the multipart catalog upload must contain the header row.

# Create Pre-signed URLs

This request returns _n_ number of pre-signed URLs to upload your catalog.

## Base URL

Here is an example base URL from the account in the India region for uploading multiple parts:  
_<https://in1.api.clevertap.com/get_multipart_upload_catalog_url?totalParts=n>_  
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

```curl Multipart Upload
curl -v --location --request GET
'https://in1.api.clevertap.com/get_multipart_upload_catalog_url?totalParts=2' \
--header 'X-CleverTap-Account-Id: XXXXXXXX' \
--header 'X-CleverTap-Passcode: XXXXXXX' \
--header 'Content-Type: application/json'
```

Check that you provide _totalParts=n \_in the request, where \_n_ is the number of splits to the file.

The response will provide _n_ pre-signed URLs in an array along with the `uploadId`.  Save the `uploadId` because it is necessary for the last step.

### Example Response

```json
{
    "presignedS3URL": [
        "<URL_1>",
        "<URL_2>"
    ],
    "expiryEpoch": "16 Aug, 06:21 PM",
    "isMultiPartUpload": true,
    "uploadId": "<Upload_ID>",
    "status": "success"
}
```

A success response is displayed if the pre-signed URL is generated: `200 OK`. The pre-signed URLs are valid for a maximum of 24 hours from generation. 

## Upload catalog part

Upload each file part. Following is an example for the first part. Repeat this command for all the successive parts. 

```curl
curl -v --request PUT 'pre-signed url 1' \
--upload-file '/Users/abc/Desktop/xaa.csv' \ #First Part of the file
--header 'Content-Type: application/octet-stream'
```

### JSON Example Response

You will receive a corresponding `eTag` after each successful upload. Save this `eTag`. as shown in the exacmple below.

```curl Text
> Host: eu1-sharedresources-platformstorages3bucket-np5wb2kjrbki.s3.eu-west-1.amazonaws.com
> User-Agent: curl/7.64.1
> Accept: */*
> Content-Type: application/octet-stream
> Content-Length: 415466
> Expect: 100-continue
>
< HTTP/1.1 100 Continue
* We are completely uploaded and fine
< HTTP/1.1 200 OK
< x-amz-id-2: E60IXfTU2a+yi9y9fcx0bfOw7Isa20CR5aBjx9eT2sYjPVp8D5nmd/6fhfVyGrr89FvB+PYKUY0=
< x-amz-request-id: H6V6YND252FK21YS
< Date: Sun, 15 Aug 2021 12:59:36 GMT
< ETag: "2ef9ab2f615e999f0fa29660f15d4366"
< Server: AmazonS3
< Content-Length: 0
<
* Connection #0 to host eu1-sharedresources-platformstorages3bucket-np5wb2kjrbki.s3.eu-west-1.amazonaws.com left intact
* Closing connection 0
```

## Complete Multipart Upload

You must indicate that the upload is complete. Following is an example request. 

The `partNumber`, `eTag`, and `uploadId` are required.

```curl
curl --location --request POST'https://in1.api.clevertap.com/upload_catalog_completed' \
--header 'X-CleverTap-Account-Id: XXXXX' \
--header 'X-CleverTap-Passcode: XXXXX' \
--header 'Content-Type: application/json' \
--data-raw '{
 "name": "catalogName",
 "email": "abc@abc.com",
 "creator": "creator name",
 "uploadId":
"vzxvLNNVhMDj6Cg573eMD7RWhiIw8oDH6sHTWHwB5b9FhjB31Vh6IZktktvXw38cE6B8Yls0o5ZlMiVksGH.PwulNwegDKDK8otnY38xnt3EfJZ4pSxyw5XpuCYWWKn7p4s1Inl3p3tNWaS9c2X5UCeIqFuHrNMpxRnSOMS403g-",
 "replace": false,
 "eTags": [
     {
         "partNumber": 1,
         "eTag": "527905dfaef5d252248b0e18d7117d10"
     },
     {
         "partNumber": 2,
         "eTag": "106f12611174ca77de94fbfb759104cf"
     }
 ]
}'
```

> 📘 Process Duration
> 
> All the parts are merged back into a single file. Large files take some time to process.

# Dashboard Statuses

You can view the catalog on the dashboard after it has been uploaded successfully. 

Other possible upload statuses include:

- Success: The catalog is uploaded successfully.
- Pending: The catalog upload is in progress.
- Error: The catalog upload failed.
- Replacement pending: The new catalog is still not in effect. Until replaced, any campaigns will be sent with the previously uploaded catalog. 

The campaigns and [catalog data ingestion](https://docs.clevertap.com/docs/catalog-data-ingestion) use the new catalog only after the old catalog is updated. In the meantime, the campaigns and  [catalog data ingestion](https://docs.clevertap.com/docs/catalog-data-ingestion) use the old catalog.

# FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).