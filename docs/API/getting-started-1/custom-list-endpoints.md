---
title: Custom List Endpoints
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
  pages:
    - type: link
      title: Create custom lists from dashboard
      url: https://docs.clevertap.com/docs/custom-list-segments
---
## Overview

Custom list segments are segments that are created from your list of users. You can create a custom list segment in three steps:

1. Request for a pre-signed URL with your first API call. A pre-signed URL gives you temporary access to upload your list of users. This request returns a URL that you can use in the second step to upload your file.
2. Upload your file to the URL received from step 1.
3. Make a second API call to indicate that you have completed the file upload. CleverTap selects the file to create the segment that it displays on the dashboard. 

## Step 1: Create a pre-signed URL

This request returns a pre-signed URL to upload your custom list.

## Base URL

[https://location.api.clevertap.com/get\_custom\_list\_segment\_url](https://location.api.clevertap.com/get_custom_list_segment_url)

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



















































































































A success response is displayed if the pre-signed URL is generated.






































The pre-signed URL is valid for a maximum of 24 hours from the time of generation.





































































If the pre-signed URL is not generated successfully, then you get the following response:
























































## Step 2: Upload custom list file

Upload the list of users file to the pre-signed URL received from step 1. This file must be a .CSV file. See [Creating a file for custom list segment](https://docs.clevertap.com/docs/custom-list-segments#section-file-for-custom-list-segment) for more information about the specifications of the custom list segment file.

Use the following command to upload the file:











































































































## Example Request






















































































































## Example Response

A success response displays the progress of the file upload until it completes. 
























































































































































































































































































































## Step 3: Indicate completion of file upload

Using the pre-signed URL received from step 1, make an API call to indicate that you have completed the upload of the custom list. When this step is completed successfully, CleverTap selects the uploaded custom list and displays it on the dashboard.

## Base URL

[https://location.api.clevertap.com/upload\_custom\_list\_segment\_completed](https://location.api.clevertap.com/upload_custom_list_segment_completed)

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
        Name of the segment.
      </td>

      <td>
        string
      </td>

      <td>
        sample segment
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
        filename
      </td>

      <td>
        Name of the file that you had uploaded previously.
      </td>

      <td>
        string
      </td>

      <td>
        my\_file\_name.csv
      </td>
    </tr>

    <tr>
      <td>
        email
      </td>

      <td>
        Email address of the owner. This must be an Admin email.
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
        The pre-signed URL that identifies this file's location received as a response to the first API call
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
        Optional parameter. This will be used when you want to replace a set of users in an existing segment
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

A success response appears if there are no immediate errors in the custom list file upload. The segment id for the segment created is returned in the response.
If the file is not found at the specified URL, the following response appears:

































































If you do not specify the mandatory parameters, the following response appears:






















































































































If the email ID is either not in the right format or it is invalid, the following response appears: 




































































































































If an old URL, which was successfully processed previously, is re-used with a new custom list name, you’ll get the following response:



















































































































## Example Response on email

The following responses are emailed to the owner’s email address mentioned in this API call as these need some processing.

If the file is uploaded successfully and a segment is created/replaced, the following response appears:































































































































































































If the segment could not be created/updated from the file uploaded, the following response appears:














































































































































## Status on dashboard

You can see the segment on the dashboard after it is processed successfully. The possible statuses are:

* Segment successfully created/updated: the segment is processed successfully from the file.\
  \*File upload failed: The segment could not be created from the file.
* File re-upload failed: The segment could not be replaced with the new file. The previous set of users exist in the file.\
  \*Segment unavailable: The segment is being processed now. Wait till it is complete. An email is sent on the completion.

The campaigns can be created on the segment after the processing is complete. See  [Custom Lists](https://docs.clevertap.com/docs/custom-list-segments) to create a campaign on the custom list segment.

## Sample code for uploading large files