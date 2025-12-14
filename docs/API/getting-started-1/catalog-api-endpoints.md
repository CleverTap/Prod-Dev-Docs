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
## Overview

The catalog API endpoint is the point of entry in a communication channel where the catalog API interacts with a server.

A product catalog represents a collection of items in your inventory, such as the list of products in an e-commerce application or the list of shows in a video streaming application. A product location catalog represents the list of locations for a business, such as store IDs. 

# Upload a Catalog

 To upload a catalog, perform the following steps:

1. Request a pre-signed URL with your first API call. A pre-signed URL gives you temporary access to upload your catalog. This request returns a URL that you can use in the [Upload a Catalog File](https://developer.clevertap.com/docs/catalog-api-endpoints_wip#section-upload-a-catalog-file) step.
2. Upload your catalog to the URL received from the [Create a Pre-signed URL](https://developer.clevertap.com/docs/catalog-api-endpoints_wip#section-create-a-pre-signed-url) step.
3. Make a second API call to signal that you have completed the catalog upload. CleverTap selects the catalog and displays it on the dashboard. 

# Create a Pre-signed URL

This request returns a pre-signed URL to upload your catalog.

## Base URL

The base URL is: [https://location.api.clevertap.com/get\_catalog\_url](https://location.api.clevertap.com/get_catalog_url)

> 📘 Location-based URLs
>
> Use the appropriate URL based on your location:
>
> * India: in1.api.clevertap.com
> * Singapore: sg1.api.clevertap.com
> * U.S.: us1.api.clevertap.com

## HTTP POST Method

This section provides information on the HTTP POST method.

### Headers

The X-CleverTap-Account-Id and X-CleverTap-Passcode authenticate the request. Refer to the [authentication guide](doc:authentication) to learn how to get these values.

The headers in the following table are all required: 


<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Header
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
        X-CleverTap-Account-Id
      </td>

      <td>
        Your CleverTap account ID.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Account-Id: ACCOUNT\_ID"
      </td>
    </tr>

    <tr>
      <td>
        X-CleverTap-Passcode
      </td>

      <td>
        Your CleverTap account passcode.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Passcode: PASSCODE"
      </td>
    </tr>

    <tr>
      <td>
        Content-Type
      </td>

      <td>
        Request content-type is always set to application/JSON.
      </td>

      <td>
        string
      </td>

      <td>
        "Content-Type: application/json"
      </td>
    </tr>
  </tbody>
</Table>

### cURL Example Request

Below is a cURL example request:















































































































































































































































### JSON Example Response

Below is a JSON example request:
























































































































































A success response is displayed if the pre-signed URL is generated: `200 OK`.

The pre-signed URL is valid for a maximum of 24 hours from generation.





































































If the pre-signed URL is not successfully generated, you get the following response:
























































# Upload a Catalog File

To upload a catalog file, perform the following steps:

1. Upload the catalog file to the pre-signed URL received from the [Create a Pre-signed URL](https://developer.clevertap.com/docs/catalog-api-end) step above. 

The catalog must be in a CSV format. Refer to [Catalogs](https://docs.clevertap.com/docs/catalog) for more information about the specifications of the catalog file.

2. Use the following command to upload the file:



















































































































### cURL Example Request


Below is a cURL example request:













































































































### Example Response

A success response displays the progress of the file upload until it is completed. 

  %   Total    % Received %       Xferd  Average   Speed      Time      Time    Time    Current\
                                       Dload  Upload      Total     Spent     Left      Speed\
100  1909    0               0  100  1909           0          1438  0:00:01  0:00:01    --:--:--        1439

# Indicate the Completion of a Catalog Upload

Using the pre-signed URL received from the [Create a Pre-signed URL](https://developer.clevertap.com/docs/catalog-api-endpoints_wip#section-create-a-pre-signed-url) step above, make an API call to indicate that you have completed the upload of the catalog. When this step is completed successfully, CleverTap selects the uploaded catalog and displays it on the dashboard.

## Base URL

The base URL is: [https://location.api.clevertap.com/upload\_catalog\_completed](https://location.api.clevertap.com/upload_catalog_completed)

## HTTP POST Method

This section provides information on the HTTP POST method.

### Headers

The X-CleverTap-Account-Id and X-CleverTap-Passcode authenticate the request. Refer to the [authentication guide](doc:authentication) to learn how to get these values.

The headers in the following table are all required:

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Header
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
        X-CleverTap-Account-Id
      </td>

      <td>
        Your CleverTap account ID.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Account-Id: ACCOUNT\_ID"
      </td>
    </tr>

    <tr>
      <td>
        X-CleverTap-Passcode
      </td>

      <td>
        Your CleverTap account passcode.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Passcode: PASSCODE"
      </td>
    </tr>

    <tr>
      <td>
        Content-Type
      </td>

      <td>
        Request content-type is always set to application/JSON.
      </td>

      <td>
        string
      </td>

      <td>
        "Content-Type: application/json"
      </td>
    </tr>
  </tbody>
</Table>

### Body Parameters


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
        Product\_Catalog
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
        &lt;PRE-SIGNED URL&gt;
      </td>
    </tr>

    <tr>
      <td>
        replace
      </td>

      <td>
        Optional parameter. Set this parameter to *true* to replace a catalog that exists with the same name as the one on the current API call.
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
        Optional parameter. Prerequisite: The *replace* flag must be set to *true*.\
        The default value is false. The replacement will fail if any columns are missing.\
        Set this flag to *true* to  force an update.
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

### Example Requests

Below is a JSON example request for a product catalog:
For a product location catalog, use the following flag in the request: `“isLocationCatalog”: true`.

Below is a JSON example request for a product location catalog:



















































































































































































































































































































































































































































### Example Response

The following success response appears if there are no immediate errors in the catalog file upload: `200 OK`.

If you upload a catalog with an existing name, then CleverTap replaces the old catalog if the *replace* flag is set to *true*. If the *replace* flag is set to *false*, then the new upload is rejected, and the following response displays:












































































If the catalog file is not found at the specified URL, the following response displays:

































































If you do not specify the mandatory parameters, the following response displays:






























































































































If the email ID is either in the wrong format or is invalid, the following response displays: 




































































































































If an old URL which was successfully processed previously is re-used with a new catalog name, you will get the following response:



















































































































If you upload more than the specified five-catalog limit, the following response displays: 
































































































### Example Response on Email

The following responses are emailed to the owner’s email address mentioned in this API call as these need some processing.

If the catalog is uploaded successfully, the following response displays:








































































If the catalog is re-uploaded successfully, the following response displays:












































































If you upload more than the specified five-catalog limit, the following response displays: 




















































If the number of rows exceeds 1 million, the following response displays:











































































If any of the three mandatory fields (*Name*, *ImageURL*, or *Category*) are missing from the catalog, the following response displays. 

For more information on mandatory catalog fields, refer to [Catalogs](https://docs.clevertap.com/docs/catalog#section-mandatory).









































































































































































































If the number of columns in the catalog exceeds 20, the following response displays:

































































































































































































































If some rows in the catalog file do not adhere to the correct format, the following response displays:

















































































































































































































# Dashboard Statuses

You can view the catalog on the dashboard after it has been uploaded successfully. 

Other possible upload statuses include:

* Success: The catalog is uploaded successfully.
* Pending: The catalog upload is in progress.
* Error: The catalog upload failed.
* Replacement pending: The new catalog is still not in effect. Until replaced, any campaigns will be sent with the previously uploaded catalog. 

The campaigns and [catalog data ingestion](https://docs.clevertap.com/docs/catalog-data-ingestion) use the new catalog only after the old catalog is updated. In the meantime, the campaigns and  [catalog data ingestion](https://docs.clevertap.com/docs/catalog-data-ingestion) use the old catalog.