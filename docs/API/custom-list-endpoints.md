---
title: Custom List API
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
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

Here is an example base URL from the account in the India region:  
[https://in1.api.clevertap.com/get_custom_list_segment_url](https://in1.api.clevertap.com/get_custom_list_segment_url)

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

## HTTP Method

POST

## Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

<Callout icon="📘" theme="info">
  **Note**

  Currently, we don't have any Body Parameters for this API.
</Callout>

## Example Request

Here is an example cURL request to the _Custom List_ Endpoint showing the headers needed to authenticate the request from the account in the India region:

```curl
curl -X POST \
  https://in1.api.clevertap.com/get_custom_list_segment_url \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/json' \
  -H 'X-CleverTap-Account-Id: ACCOUNT_ID' \
  -H 'X-CleverTap-Passcode: PASSCODE'
```
```python
import requests

url = "https://in1.api.clevertap.com/get_custom_list_segment_url"
headers = {
    'Cache-Control': 'no-cache',
    'Content-Type': 'application/json',
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE'
}

response = requests.post(url, headers=headers)

print(response.text)

```

## Example Response

```json
{
    "presignedS3URL": <PRE-SIGNED URL>,
    "expiry": "7 May, 01:29 PM",
    "status": "success"
}
```

A success response is displayed if the pre-signed URL is generated.

```json
{ "status" : "success"}
```

The pre-signed URL is valid for a maximum of 24 hours from the time of generation.

```json
{ "expiryEpoch": {"$date": "2019-04-24T13:04:37.088Z"}
```

If the pre-signed URL is not generated successfully, then you get the following response:

```
URL not generated, Please come back later
```

## Step 2: Upload custom list file

Upload the list of users file to the pre-signed URL received from step 1. This file must be a .CSV file. See [Creating a file for custom list segment](https://docs.clevertap.com/docs/custom-list-segments#section-file-for-custom-list-segment) for more information about the specifications of the custom list segment file.

Use the following command to upload the file:

```curl
$(curl --request PUT --upload-file "<path to file on your local machine>" "<presigned URL>")
```
```python
import requests

# Replace '<path to file on your local machine>' with the path to your local file
file_path = "<path to file on your local machine>"

# Replace '<presigned URL>' with the presigned URL provided
presigned_url = "<presigned URL>"

with open(file_path, 'rb') as file:
    response = requests.put(presigned_url, data=file)

print(response.status_code)

```

## Example Request

```curl
$(curl --request PUT 
--upload-file "Users⁩/janedoe/Documents⁩/custom_list_file.csv”
"<PRE-SIGNED URL")
```

## Example Response

A success response displays the progress of the file upload until it completes.

```
%   Total    % Received %       Xferd  Average   Speed      Time      Time    Time    Current

                                       Dload  Upload      Total     Spent     Left      Speed

100  1909    0               0  100  1909           0          1438  0:00:01  0:00:01    --:--:--        1439
```

## Step 3: Indicate completion of file upload

Using the pre-signed URL received from step 1, make an API call to indicate that you have completed the upload of the custom list. When this step is completed successfully, CleverTap selects the uploaded custom list and displays it on the dashboard.

## Base URL

Here is an example base URL from the account in the India region:  
[https://in1.api.clevertap.com/upload_custom_list_segment_completed](https://in1.api.clevertap.com/upload_custom_list_segment_completed)

To know the API endpoint for your account, refer to [Region](https://developer.clevertap.com/docs/custom-list-endpoints#region).

## HTTP Method

POST

## Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode authenticate the request. See the [authentication guide](doc:authentication) to learn about getting their values.

| Header                 | Description                                             | Type   | Example Value                        |
| :--------------------- | :------------------------------------------------------ | :----- | :----------------------------------- |
| X-CleverTap-Account-Id | Your CleverTap Account ID.                              | string | "X-CleverTap-Account-Id: ACCOUNT_ID" |
| X-CleverTap-Passcode   | Your CleverTap Account Passcode.                        | string | "X-CleverTap-Passcode: PASSCODE"     |
| Content-Type           | Request content-type is always set to application/json. | string | "Content-Type: application/json"     |

## Body Parameters

| Parameter | Description                                                                                          | Type    | Example Value                                   |
| :-------- | :--------------------------------------------------------------------------------------------------- | :------ | :---------------------------------------------- |
| name      | Name of the segment.                                                                                 | string  | sample segment                                  |
| creator   | Name of the user making this API call.                                                               | string  | Jane                                            |
| filename  | Name of the file that you had uploaded previously.                                                   | string  | my_file_name.csv                                |
| email     | Email address of the owner. This must be an Admin email.                                             | string  | [jane@clevertap.com](mailto:jane@clevertap.com) |
| url       | The pre-signed URL that identifies this file's location received as a response to the first API call | string  | \<PRE-SIGNED URL>                               |
| replace   | Optional parameter. This will be used when you want to replace a set of users in an existing segment | boolean | true                                            |

## Example Request

Here is an example cURL request to the _Upload Custom List Segment Completion_ API showing the headers needed to authenticate the request from the account in the India region:

```curl cURL
curl -X POST \
  https://in1.api.clevertap.com/upload_custom_list_segment_completed \
  -H 'Cache-Control: no-cache' \
  -H 'Content-Type: application/json' \
  -H 'X-CleverTap-Account-Id: ACCOUNT_ID' \
  -H 'X-CleverTap-Passcode: PASSCODE' \
  -H 'cache-control: no-cache' \
  -d '{ "name":"sample segment", "email":"jane@clevertap.com","filename": "my_file_name","creator":"Jane", "url":"<PRE-SIGNED URL>", "replace":false}
```
```python
import requests

url = "https://in1.api.clevertap.com/upload_custom_list_segment_completed"

headers = {
    'Cache-Control': 'no-cache',
    'Content-Type': 'application/json',
    'X-CleverTap-Account-Id': 'ACCOUNT_ID',
    'X-CleverTap-Passcode': 'PASSCODE'
}

data = {
    "name": "sample",
    "email": "jane@clevertap.com",
    "filename": "my_file_name",
    "creator": "Jane",
    "url": "<PRE-SIGNED URL>",
    "replace": False
}

response = requests.post(url, json=data, headers=headers)

print(response.text)

```

## Example Response

A success response appears if there are no immediate errors in the custom list file upload. The segment id for the segment created is returned in the response.

```json
{
    "Segment ID": 1598345958,
    "status": "success"
}
```

If the file is not found at the specified URL, the following response appears:

```json
{
   "status": "fail",
   "Error": “Invalid URL”
}
```

If you do not specify the mandatory parameters, the following response appears:

```json
{
   "status": "fail",
   "Error": “name, email ID,filename,creator and pre-signed url are mandatory”
}
```

If the email ID is either not in the right format or it is invalid, the following response appears:

```json
{
   "status": "fail",
   "Error": “email id is either not in the right format or does not belong to a valid admin”
}
```

If an old URL, which was successfully processed previously, is reused with a new custom list name, you’ll get the following response:

```json
{
   "status": "fail",
   "Error": “New file referencing a previously used URL. Please change URL”
}
```

## Example Response on email

The following responses are emailed to the owner’s email address mentioned in this API call as these need some processing.

If the file is uploaded successfully and a segment is created/replaced, the following response appears:

```
my_file_name.csv was sucessfully processed for Custom list segment sample segment.

Segment details
- Total users in uploaded list: 1
- Users in list identified on CleverTap: 1
```

If the segment could not be created/updated from the file uploaded, the following response appears:

```
File my_file_name.csv could not be processed for Custom list segment sample segment.
Please, check file uploaded and try again.
```

## Status on dashboard

You can see the segment on the dashboard after it is processed successfully. The possible statuses are:

* Segment successfully created/updated: the segment is processed successfully from the file.  
  *File upload failed: The segment could not be created from the file.
* File re-upload failed: The segment could not be replaced with the new file. The previous set of users exist in the file.  
  *Segment unavailable: The segment is being processed now. Wait till it is complete. An email is sent on the completion.

The campaigns can be created on the segment after the processing is complete. See  [Custom Lists](https://docs.clevertap.com/docs/custom-list-segments) to create a campaign on the custom list segment.

## Sample code for uploading large files

```
package myjava;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.net.HttpURLConnection;
import java.net.URL;

public class UploadFileToS3 {
    private static boolean uploadCsvFile(File file, String preSignURL) throws Exception {
        long startTime = System.currentTimeMillis();

        System.out.println("Starting with Clevertap file upload for file: " + file.getName() + ", now:" + startTime);

        OutputStream bos = null;
        HttpURLConnection connection = null;
        try {
            URL url = new URL(preSignURL);
            connection = (HttpURLConnection) url.openConnection();

            long fileLength = file.length();

            System.out.println("Length of  the file: " + (fileLength / (1024 * 1024)) + " MB");

            // to enable streaming of a HTTP request body without internal buffering,

            // when the content length is known in advance

            connection.setFixedLengthStreamingMode(fileLength);
            connection.setConnectTimeout(10000);
            connection.setReadTimeout(90000000);
            connection.setDoOutput(true);
            connection.setRequestMethod("PUT");

            bos = connection.getOutputStream();
            FileInputStream inFileReader = new FileInputStream(file);
            int batchSize = 10240; // buffer size to stream, can be decreased/increased according to the need

            byte[] localbuffer = new byte[batchSize];
            long bytesTransferred = 0;
            while (bytesTransferred + batchSize <= fileLength) {
                inFileReader.read(localbuffer);
                bos.write(localbuffer);

                // To have an idea about how much memory the system is using at an instant

                if ((bytesTransferred % (1024 * 1024)) == 0) {

                    System.out.println("Data transferred: " + (bytesTransferred / (1024 * 1024)) + " MB, current system mem: "

                            + (Runtime.getRuntime().totalMemory() - Runtime.getRuntime().freeMemory()) / (1024 * 1024) + " MB");
                }

                bytesTransferred += batchSize;

            }

            int remainingBytes = (int) (fileLength - bytesTransferred);
            System.out.println("writing the remaining bytes: " + remainingBytes);
            localbuffer = new byte[remainingBytes]; // the remaining number of bytes can be less than the buffer size, so new byte array needed

            inFileReader.read(localbuffer);
            bos.write(localbuffer);
            bos.flush(); // finally flush

            inFileReader.close();
            int responseCode = connection.getResponseCode();
            System.out.println("ResponseCode for Clevertap file upload for file: " + file.getName() + ", code:" + responseCode);

            System.out.println("final " + (Runtime.getRuntime().totalMemory() - Runtime.getRuntime().freeMemory()));

            System.out.println("uploaded successfully");

            System.out.println("timetaken = " + ((int) ((System.currentTimeMillis() - startTime) / 1000)) + " sec");

            return true;
        }  catch (Exception e) {

            System.out.println("Exception with Clevertap file upload for file: " + file.getName());

        } finally {

            System.out.println("/: " + file.getName() + ", url:" + preSignURL);

            if (bos != null) {
                bos.close();
            }

            if (connection != null) {
                connection.disconnect();
            }
        }

        System.out.println("uploading failed");

        System.out.println("timetaken = " + ((int) ((System.currentTimeMillis() - startTime) / 1000)) + " sec");

        return false;
    }

    public static void main(String[] args) throws Exception {

        String url = "<S3_PresignedUrl>";
        File f = new File("<File_Path>");
        uploadCsvFile(f, url);
    }

}
```

# Error Codes

The following table lists the error codes for the Custom list API:

| **Status Code** | **Error**                 | **Description**                                                                                         |
| --------------- | ------------------------- | ------------------------------------------------------------------------------------------------------- |
| 400             | `json_parse_failed`       | The request body contains invalid JSON.                                                                 |
| 400             | `invalid_payload`         | The `segmentUploadDetails` field is missing or empty.                                                   |
| 401             | `missing_fields`          | One or more required fields are missing — segment name, email ID, creator, filename, or pre-signed URL. |
| 401             | `invalid_email`           | The email format is invalid or the email is not associated with an admin.                               |
| 401             | `segment_not_found`       | No existing segment found to replace.                                                                   |
| 401             | `segment_in_progress`     | An older segment upload is still being processed.                                                       |
| 401             | `duplicate_name`          | A segment with the same name already exists.                                                            |
| 401             | `url_already_used`        | The pre-signed URL has already been used.                                                               |
| 401             | `file_not_found`          | The file is not found in cloud storage.                                                                 |
| 401             | `file_not_uploaded`       | The file has not been uploaded to cloud storage yet.                                                    |
| 401             | `cloud_connection_failed` | Failed to connect to cloud storage.                                                                     |
| 401             | `team_ids_required`       | Team IDs must be provided when team-based segmentation is enabled.                                      |
| 401             | `feature_not_enabled`     | The custom list segments feature is not enabled for this account.                                       |
| 401             | `account_meta_missing`    | Account metadata not found for the specified account ID.                                                |
| 401             | `url_generation_failed`   | Failed to generate the pre-signed URL.                                                                  |
| 401             | `unexpected_url_error`    | An unexpected error occurred during pre-signed URL generation.                                          |
| 503             | `worker_unavailable`      | The account’s processing worker is currently unavailable.                                               |

<br />
