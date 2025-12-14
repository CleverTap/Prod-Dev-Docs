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

## Overview

You can upload a catalog in three steps:

1. Request for a pre-signed URL with your first API call. A pre-signed URL gives you temporary access to upload your catalog. This request returns a URL that you can use in the second step to upload your catalog.
2. Upload your catalog to the URL received from step 1.
3. Make a second API call to signal that you have completed the catalog upload. CleverTap selects the catalog displays it on the dashboard. 

## Step 1: Create a pre-signed URL

This request returns a pre-signed URL to upload your catalog.

## Base URL

[https://location.api.clevertap.com/get\_catalog\_url](https://location.api.clevertap.com/get_catalog_url)

> 📘 Note
>
> Use the URL based on your location:
>
> * India - in1.api.clevertap.com
> * Singapore - sg1.api.clevertap.com
> * U.S - us1.api.clevertap.com

## HTTP Method

POST

## Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode authenticate the request. See the [authentication guide](doc:authentication) to learn about getting their values.

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
        Your CleverTap Account ID.
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
        Your CleverTap Account Passcode.
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

## Example Request
















































































































































































































































## Example Response
























































































































































A success response is displayed if the pre-signed URL is generated:  200 OK.

The pre-signed URL is valid for a maximum of 24 hours from generation.





































































If the pre-signed URL is not successfully generated, you get the following response:
























































## Step 2: Upload catalog file

Upload the catalog file to the pre-signed URL received from step 1. The catalog must be a .CSV file. See [Creating a Catalog](https://docs.clevertap.com/docs/catalog) for more information about the specifications of the catalog file.

Use the following command to upload the file:



















































































































## Example Request













































































































## Example Response

A success response displays the progress of the file upload until it completes. 

  %   Total    % Received %       Xferd  Average   Speed      Time      Time    Time    Current\
                                       Dload  Upload      Total     Spent     Left      Speed\
100  1909    0               0  100  1909           0          1438  0:00:01  0:00:01    --:--:--        1439

## Step 3: Indicate completion of catalog upload

Using the pre-signed URL received from step 1, make an API call to indicate that you have completed the upload of the catalog. When this step is completed successfully, CleverTap selects the uploaded catalog and displays it on the dashboard.

## Base URL

[https://location.api.clevertap.com/upload\_catalog\_completed](https://location.api.clevertap.com/upload_catalog_completed)

## HTTP Method

POST

## Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode authenticate the request. See the [authentication guide](doc:authentication) to learn about getting their values.


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
        Your CleverTap Account ID.
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
        Your CleverTap Account Passcode.
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
        Request content-type is always set to application/json.
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

## Body Parameters


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
        Name of the catalog
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
        Name of the user making this API call
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
        Email address of the owner. This should be an Admin email.
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
        The pre-signed URL that identifies this catalog’s location received as a response to the first API call
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
        Optional parameter. Set this to true to replace a catalog that exists with the same name as the one on the current API call
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
        Optional parameter. Prerequisite: The replace flag must be set to true.\
        The default value is false. The replacement will fail if any columns are missing.\
        Set this flag to true to  force an update.
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

## Example Request
























































































































































































































































































































































































































## Example Response

The following success response appears if there are no immediate errors in the catalog file upload:`200 OK`
If you upload a catalog with a name that already exists then CleverTap replaces the old catalog if the replace flag is set to 'true'. If the replace flag is set to 'false' then the new upload is rejected and the following response appears:












































































If the catalog file is not found at the specified URL, the following response appears:

































































If you do not specify the mandatory parameters, the following response appears:






























































































































If the email ID is either not in the right format or it is invalid, the following response appears: 




































































































































If an old URL, which was successfully processed previously, is re-used with a new catalog name, you’ll get the following response:



















































































































If you upload more than the specified 5 catalog limit, the following response appears: 
































































































## Example Response on email

The following responses are emailed to the owner’s email address mentioned in this API call as these need some processing.

If the catalog is uploaded successfully, the following response appears:








































































If the catalog is re-uploaded successfully, the following response appears:











































































If you upload more than the specified 5 catalog limit, the following response appears: 




















































If the number of rows exceeds 1 million, the following response appears:











































































If any of the three mandatory fields (Name, ImageURL,  or Category) are missing from the catalog, the following response appears:









































































































































































































If the number of columns in the catalog exceeds 20, the following response appears:


































































































































































































































If some rows in the catalog file do not adhere to the correct format, the following response appears: :

















































































































































































































## Status on dashboard

You can see the catalog on the dashboard after it is uploading it successfully. You can also see the status of the upload. The possible statuses are:

* Success: The Catalog is uploaded successfully
* Pending: The catalog upload is in progress
* Error: The catalog upload failed 
* Replacement pending: The new catalog is still not in effect. Until replaced, any campaigns will be sent with the previously uploaded catalog. 

The campaigns and [catalog data ingestion](https://docs.clevertap.com/docs/catalog-data-ingestion) use the new catalog only after the old catalog is updated.  In the meanwhile, the campaigns and  [catalog data ingestion](https://docs.clevertap.com/docs/catalog-data-ingestion) use the old catalog.