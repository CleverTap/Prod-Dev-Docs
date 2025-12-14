---
title: Catalog API Endpoints
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
A catalog represents a collection of items in your inventory. For example, the list of products in an e-commerce application or the list of shows in a video streaming application.
[block:api-header]
{
  "title": "Overview"
}
[/block]
You can upload a catalog in three steps:

 1. Request for a pre-signed URL with your first API call. A pre-signed URL gives you temporary access to upload your catalog. This request returns a URL that you can use in the second step to upload your catalog.
 2. Upload your catalog to the URL received from step 1.
 3. Make a second API call to signal that you have completed the catalog upload. CleverTap selects the catalog displays it on the dashboard. 
[block:api-header]
{
  "title": "Step 1: Create a pre-signed URL"
}
[/block]
This request returns a pre-signed URL to upload your catalog.

## Base URL
https://location.api.clevertap.com/get_catalog_url
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Use the URL based on your location:\n  * India - in1.api.clevertap.com\n  * Singapore - sg1.api.clevertap.com\n  * U.S - us1.api.clevertap.com"
}
[/block]
## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode authenticate the request. See the [authentication guide](doc:authentication) to learn about getting their values.


[block:parameters]
{
  "data": {
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "X-CleverTap-Account-Id",
    "0-1": "Your CleverTap Account ID.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-0": "X-CleverTap-Passcode",
    "1-1": "Your CleverTap Account Passcode.",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-0": "Content-Type",
    "2-1": "Request content-type is always set to application/JSON.",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n  https://location.api.clevertap.com/get_catalog_url \\\n  -H 'Cache-Control: no-cache' \\\n  -H 'Content-Type: application/json' \\\n  -H 'X-CleverTap-Account-Id: ACCOUNT_ID' \\\n  -H 'X-CleverTap-Passcode: PASSCODE'\n",
      "language": "curl"
    }
  ]
}
[/block]
## Example Response
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"presignedS3URL\": <PRE-SIGNED URL>,\n    \"expiryEpoch\": {\n        \"$date\": \"2019-04-24T13:04:37.088Z\"\n    },\n    \"status\": \"success\"\n    \n",
      "language": "json"
    }
  ]
}
[/block]
A success response is displayed if the pre-signed URL is generated:  200 OK.

The pre-signed URL is valid for a maximum of 24 hours from generation.
[block:code]
{
  "codes": [
    {
      "code": "{ \"expiryEpoch\": {\"$date\": \"2019-04-24T13:04:37.088Z\"}\n \n ",
      "language": "json"
    }
  ]
}
[/block]
If the pre-signed URL is not successfully generated, you get the following response:
[block:code]
{
  "codes": [
    {
      "code": "URL not generated, Please come back later\n\n",
      "language": "text"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Step 2: Upload catalog file"
}
[/block]
Upload the catalog file to the pre-signed URL received from step 1. The catalog must be a .CSV file. See [Creating a Catalog](https://docs.clevertap.com/docs/catalog) for more information about the specifications of the catalog file.

Use the following command to upload the file:
[block:code]
{
  "codes": [
    {
      "code": "$(curl --request PUT --upload-file \"<path to catalog file on your local machine>\" \"<presigned URL>\")\n\n",
      "language": "curl"
    }
  ]
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "$(curl --request PUT \n--upload-file \"Users⁩/janedoe/Documents⁩/catalog.csv”\n\"<PRE-SIGNED URL\")\n\n",
      "language": "curl"
    }
  ]
}
[/block]
## Example Response

A success response displays the progress of the file upload until it completes. 

  %   Total    % Received %       Xferd  Average   Speed      Time      Time    Time    Current
                                       Dload  Upload      Total     Spent     Left      Speed
100  1909    0               0  100  1909           0          1438  0:00:01  0:00:01    --:--:--        1439

[block:api-header]
{
  "title": "Step 3: Indicate completion of catalog upload"
}
[/block]
Using the pre-signed URL received from step 1, make an API call to indicate that you have completed the upload of the catalog. When this step is completed successfully, CleverTap selects the uploaded catalog and displays it on the dashboard.

## Base URL
https://location.api.clevertap.com/upload_catalog_completed

## HTTP Method
POST

## Headers
These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode authenticate the request. See the [authentication guide](doc:authentication) to learn about getting their values.
[block:parameters]
{
  "data": {
    "h-0": "Header",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "X-CleverTap-Account-Id",
    "0-1": "Your CleverTap Account ID.",
    "0-2": "string",
    "1-2": "string",
    "2-2": "string",
    "0-3": "\"X-CleverTap-Account-Id: ACCOUNT_ID\"",
    "1-0": "X-CleverTap-Passcode",
    "1-1": "Your CleverTap Account Passcode.",
    "1-3": "\"X-CleverTap-Passcode: PASSCODE\"",
    "2-0": "Content-Type",
    "2-1": "Request content-type is always set to application/json.",
    "2-3": "\"Content-Type: application/json\""
  },
  "cols": 4,
  "rows": 3
}
[/block]
## Body Parameters
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "h-2": "Type",
    "h-3": "Example Value",
    "0-0": "name",
    "0-1": "Name of the catalog",
    "0-2": "string",
    "0-3": "Product_Catalog",
    "1-0": "creator",
    "1-1": "Name of the user making this API call",
    "1-2": "string",
    "1-3": "Jane",
    "2-0": "email",
    "2-1": "Email address of the owner. This should be an Admin email.",
    "2-2": "string",
    "2-3": "jane@clevertap.com",
    "3-0": "url",
    "3-1": "The pre-signed URL that identifies this catalog’s location received as a response to the first API call",
    "3-2": "string",
    "3-3": "<PRE-SIGNED URL>",
    "4-0": "replace",
    "4-1": "Optional parameter. Set this to true to replace a catalog that exists with the same name as the one on the current API call",
    "4-2": "boolean",
    "4-3": "true",
    "5-0": "override",
    "5-1": "Optional parameter. Prerequisite: The replace flag must be set to true. \nThe default value is false. The replacement will fail if any columns are missing. \nSet this flag to true to  force an update.",
    "5-2": "boolean",
    "5-3": "true"
  },
  "cols": 4,
  "rows": 6
}
[/block]
## Example Request
[block:code]
{
  "codes": [
    {
      "code": "curl -X POST \\\n  https://location.api.clevertap.com/upload_catalog_completed \\\n  -H 'Cache-Control: no-cache' \\\n  -H 'Content-Type: application/json' \\\n  -H 'X-CleverTap-Account-Id: ACCOUNT_ID' \\\n  -H 'X-CleverTap-Passcode: PASSCODE' \\\n  -H 'cache-control: no-cache' \\\n  -d '{ \"name\":\"Product_Catalog\", \"email\":\"jane@clevertap.com\", \"creator\":\"Jane\", \"url\":\"<PRE-SIGNED URL>\", \"replace\":false}\n",
      "language": "json"
    }
  ]
}
[/block]
## Example Response

The following success response appears if there are no immediate errors in the catalog file upload:`200 OK`

If you upload a catalog with a name that already exists then CleverTap replaces the old catalog if the replace flag is set to 'true'. If the replace flag is set to 'false' then the new upload is rejected and the following response appears:
[block:code]
{
  "codes": [
    {
      "code": "{\n   \"status\": \"fail\",\n   \"error\": \"Duplicate Catalog name\"\n}\n\n",
      "language": "json"
    }
  ]
}
[/block]
If the catalog file is not found at the specified URL, the following response appears:
[block:code]
{
  "codes": [
    {
      "code": "{\n   \"status\": \"fail\",\n   \"Error\": “Invalid URL”\n}\n\n",
      "language": "json"
    }
  ]
}
[/block]
If you do not specify the mandatory parameters, the following response appears:
[block:code]
{
  "codes": [
    {
      "code": "{\n   \"status\": \"fail\",\n   \"Error\": “Catalog name, email ID, catalog creator and pre-signed url are mandatory”\n}\n\n",
      "language": "json"
    }
  ]
}
[/block]
If the email ID is either not in the right format or it is invalid, the following response appears: 
[block:code]
{
  "codes": [
    {
      "code": "{\n   \"status\": \"fail\",\n   \"Error\": “email id is either not in the right format or does not belong to a valid admin”\n}\n\n",
      "language": "json"
    }
  ]
}
[/block]
If an old URL, which was successfully processed previously, is re-used with a new catalog name, you’ll get the following response:
[block:code]
{
  "codes": [
    {
      "code": "{\n   \"status\": \"fail\",\n   \"Error\": “New file referencing a previously used URL. Please change URL”\n}\n",
      "language": "json"
    }
  ]
}
[/block]
If you upload more than the specified 5 catalog limit, the following response appears: 

[block:code]
{
  "codes": [
    {
      "code": "{\n   \"status\": \"fail\",\n   \"Error\": “Already 5 catalogs present for the account“\n}\n ",
      "language": "json"
    }
  ]
}
[/block]
## Example Response on email
The following responses are emailed to the owner’s email address mentioned in this API call as these need some processing.

If the catalog is uploaded successfully, the following response appears:
[block:code]
{
  "codes": [
    {
      "code": "Catalog with name ‘Product_Catalog’ uploaded successfully\n\n",
      "language": "text"
    }
  ]
}
[/block]
If the catalog is re-uploaded successfully, the following response appears:
[block:code]
{
  "codes": [
    {
      "code": "Catalog with name ‘Product_Catalog’ re-uploaded successfully\n\n",
      "language": "text"
    }
  ]
}
[/block]
If you upload more than the specified 5 catalog limit, the following response appears: 
[block:code]
{
  "codes": [
    {
      "code": "Only 5 number of catalogs are allowed\n\n",
      "language": "text"
    }
  ]
}
[/block]
If the number of rows exceeds 1 million, the following response appears:
[block:code]
{
  "codes": [
    {
      "code": "Your catalog exceeds maximum allowed limit of 1,000,000 rows\n\n",
      "language": "text"
    }
  ]
}
[/block]
If any of the three mandatory fields (Name, ImageURL,  or Category) are missing from the catalog, the following response appears:
[block:code]
{
  "codes": [
    {
      "code": "Identity, ImageURL, Category, SubCategory, Brand, Price, MaxDiscount, Season, UserRating, PartnerPromotion, Margin, \"Missing Name field.Please add the field and upload the catalog again\"\n\n",
      "language": "text"
    }
  ]
}
[/block]
If the number of columns in the catalog exceeds 20, the following response appears:
[block:code]
{
  "codes": [
    {
      "code": "Identity, Name, ImageURL, Col3, Col4, Col5, Col6, Col7, Col8, Col9, Col10, Col11 Col12, Col13, Col14, Col15, Col16, Col17, Col18, Col19, Col20, Col21, \"Your catalog exceeds maximum allowed limit of  20 columns\"\n\n",
      "language": "text"
    }
  ]
}
[/block]
If some rows in the catalog file do not adhere to the correct format, the following response appears: :
[block:code]
{
  "codes": [
    {
      "code": "CT000003, CleverTap Navy Collar T-Shirt, https://www.images.com/CTNavyCollarT-Shirt.jpg, , Identities with extra fields found in the catalog. Correct the information and upload the catalog again\n\n",
      "language": "text"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Status on dashboard"
}
[/block]
You can see the catalog on the dashboard after it is uploading it successfully. You can also see the status of the upload. The possible statuses are:
* Success: The Catalog is uploaded successfully
* Pending: The catalog upload is in progress
* Error: The catalog upload failed 
* Replacement pending: The new catalog is still not in effect. Until replaced, any campaigns will be sent with the previously uploaded catalog. 

The campaigns and [catalog data ingestion](https://docs.clevertap.com/docs/catalog-data-ingestion) use the new catalog only after the old catalog is updated.  In the meanwhile, the campaigns and  [catalog data ingestion](https://docs.clevertap.com/docs/catalog-data-ingestion) use the old catalog.