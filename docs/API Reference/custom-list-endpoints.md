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
Custom list segments are segments that are created from your list of users. 
[block:api-header]
{
  "title": "Overview"
}
[/block]
You can create a custom list segment in three steps:

 1. Request for a pre-signed URL with your first API call. A pre-signed URL gives you temporary access to upload your list of users. This request returns a URL that you can use in the second step to upload your file.
 2. Upload your file to the URL received from step 1.
 3. Make a second API call to indicate that you have completed the file upload. CleverTap selects the file to create the segment that it displays on the dashboard. 
[block:api-header]
{
  "title": "Step 1: Create a pre-signed URL"
}
[/block]
This request returns a pre-signed URL to upload your custom list.

## Base URL
https://location.api.clevertap.com/get_custom_list_segment_url

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
      "code": "curl -X POST \\\n  https://location.api.clevertap.com/get_custom_list_segment_url \\\n  -H 'Cache-Control: no-cache' \\\n  -H 'Content-Type: application/json' \\\n  -H 'X-CleverTap-Account-Id: ACCOUNT_ID' \\\n  -H 'X-CleverTap-Passcode: PASSCODE'\n",
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
      "code": "    \n{\n    \"presignedS3URL\": <PRE-SIGNED URL>,\n    \"expiry\": \"7 May, 01:29 PM\",\n    \"status\": \"success\"\n}\n",
      "language": "json"
    }
  ]
}
[/block]
A success response is displayed if the pre-signed URL is generated.
[block:code]
{
  "codes": [
    {
      "code": "{ \"status\" : \"success\"}\n\n",
      "language": "json"
    }
  ]
}
[/block]
The pre-signed URL is valid for a maximum of 24 hours from the time of generation.
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
If the pre-signed URL is not generated successfully, then you get the following response:
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
  "title": "Step 2: Upload custom list file"
}
[/block]
Upload the list of users file to the pre-signed URL received from step 1. This file must be a .CSV file. See [Creating a file for custom list segment](https://docs.clevertap.com/docs/custom-list-segments#section-file-for-custom-list-segment) for more information about the specifications of the custom list segment file.

Use the following command to upload the file:
[block:code]
{
  "codes": [
    {
      "code": "$(curl --request PUT --upload-file \"<path to file on your local machine>\" \"<presigned URL>\")\n\n",
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
      "code": "$(curl --request PUT \n--upload-file \"Users⁩/janedoe/Documents⁩/custom_list_file.csv”\n\"<PRE-SIGNED URL\")\n\n",
      "language": "curl"
    }
  ]
}
[/block]
## Example Response

A success response displays the progress of the file upload until it completes. 

[block:code]
{
  "codes": [
    {
      "code": "  %   Total    % Received %       Xferd  Average   Speed      Time      Time    Time    Current\n                                       Dload  Upload      Total     Spent     Left      Speed\n100  1909    0               0  100  1909           0          1438  0:00:01  0:00:01    --:--:--        1439\n",
      "language": "text"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Step 3: Indicate completion of file upload"
}
[/block]
Using the pre-signed URL received from step 1, make an API call to indicate that you have completed the upload of the custom list. When this step is completed successfully, CleverTap selects the uploaded custom list and displays it on the dashboard.

## Base URL
https://location.api.clevertap.com/upload_custom_list_segment_completed

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
    "0-1": "Name of the segment.",
    "0-2": "string",
    "0-3": "sample segment",
    "1-0": "creator",
    "1-1": "Name of the user making this API call.",
    "1-2": "string",
    "1-3": "Jane",
    "3-0": "email",
    "3-1": "Email address of the owner. This must be an Admin email.",
    "3-2": "string",
    "3-3": "jane@clevertap.com",
    "4-0": "url",
    "4-1": "The pre-signed URL that identifies this file's location received as a response to the first API call",
    "4-2": "string",
    "4-3": "<PRE-SIGNED URL>",
    "5-0": "replace",
    "5-1": "Optional parameter. This will be used when you want to replace a set of users in an existing segment",
    "5-2": "boolean",
    "5-3": "true",
    "2-0": "filename",
    "2-1": "Name of the file that you had uploaded previously.",
    "2-2": "string",
    "2-3": "my_file_name.csv"
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
      "code": "curl -X POST \\\n  https://location.api.clevertap.com/upload_custom_list_segment_completed \\\n  -H 'Cache-Control: no-cache' \\\n  -H 'Content-Type: application/json' \\\n  -H 'X-CleverTap-Account-Id: ACCOUNT_ID' \\\n  -H 'X-CleverTap-Passcode: PASSCODE' \\\n  -H 'cache-control: no-cache' \\\n  -d '{ \"name\":\"sample segment\", \"email\":\"jane@clevertap.com\",\"filename\": \"my_file_name\",\"creator\":\"Jane\", \"url\":\"<PRE-SIGNED URL>\", \"replace\":false}\n",
      "language": "json"
    }
  ]
}
[/block]
## Example Response

A success response appears if there are no immediate errors in the custom list file upload. The segment id for the segment created is returned in the response.
[block:code]
{
  "codes": [
    {
      "code": "{\n    \"Segment ID\": 1598345958,\n    \"status\": \"success\"\n}\n\n",
      "language": "json"
    }
  ]
}
[/block]
If the file is not found at the specified URL, the following response appears:
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
      "code": "{\n   \"status\": \"fail\",\n   \"Error\": “name, email ID,filename,creator and pre-signed url are mandatory”\n}\n\n",
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
If an old URL, which was successfully processed previously, is re-used with a new custom list name, you’ll get the following response:
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
## Example Response on email
The following responses are emailed to the owner’s email address mentioned in this API call as these need some processing.

If the file is uploaded successfully and a segment is created/replaced, the following response appears:
[block:code]
{
  "codes": [
    {
      "code": "my_file_name.csv was sucessfully processed for Custom list segment sample segment.\n\nSegment details\n- Total users in uploaded list: 1\n- Users in list identified on CleverTap: 1",
      "language": "text"
    }
  ]
}
[/block]
If the segment could not be created/updated from the file uploaded, the following response appears:
[block:code]
{
  "codes": [
    {
      "code": "File my_file_name.csv could not be processed for Custom list segment sample segment.\nPlease, check file uploaded and try again.\n\n",
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
You can see the segment on the dashboard after it is processed successfully. The possible statuses are:
* Segment successfully created/updated: the segment is processed successfully from the file.
*File upload failed: The segment could not be created from the file.
* File re-upload failed: The segment could not be replaced with the new file. The previous set of users exist in the file.
*Segment unavailable: The segment is being processed now. Wait till it is complete. An email is sent on the completion.

The campaigns can be created on the segment after the processing is complete. See  [Custom Lists](https://docs.clevertap.com/docs/custom-list-segments) to create a campaign on the custom list segment.


[block:api-header]
{
  "title": "Sample code for uploading large files"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "package myjava;\n\nimport java.io.File;\nimport java.io.FileInputStream;\nimport java.io.IOException;\nimport java.io.OutputStream;\nimport java.net.HttpURLConnection;\nimport java.net.URL;\n\npublic class UploadFileToS3 {\n\n    private static boolean uploadCsvFile(File file, String preSignURL) throws Exception {\n        long startTime = System.currentTimeMillis();\n        System.out.println(\"Starting with Clevertap file upload for file: \" + file.getName() + \", now:\" + startTime);\n        OutputStream bos = null;\n        HttpURLConnection connection = null;\n        try {\n            URL url = new URL(preSignURL);\n            connection = (HttpURLConnection) url.openConnection();\n\n            long fileLength = file.length();\n            System.out.println(\"Length of  the file: \" + (fileLength / (1024 * 1024)) + \" MB\");\n            // to enable streaming of a HTTP request body without internal buffering,\n            // when the content length is known in advance\n            connection.setFixedLengthStreamingMode(fileLength);\n\n            connection.setConnectTimeout(10000);\n            connection.setReadTimeout(90000000);\n            connection.setDoOutput(true);\n            connection.setRequestMethod(\"PUT\");\n\n            bos = connection.getOutputStream();\n            FileInputStream inFileReader = new FileInputStream(file);\n            int batchSize = 10240; // buffer size to stream, can be decreased/increased according to the need\n            byte[] localbuffer = new byte[batchSize];\n            long bytesTransferred = 0;\n            while (bytesTransferred + batchSize <= fileLength) {\n                inFileReader.read(localbuffer);\n                bos.write(localbuffer);\n\n                // To have an idea about how much memory the system is using at an instant\n                if ((bytesTransferred % (1024 * 1024)) == 0) {\n                    System.out.println(\"Data transferred: \" + (bytesTransferred / (1024 * 1024)) + \" MB, current system mem: \"\n                            + (Runtime.getRuntime().totalMemory() - Runtime.getRuntime().freeMemory()) / (1024 * 1024) + \" MB\");\n                }\n                bytesTransferred += batchSize;\n            }\n\n            int remainingBytes = (int) (fileLength - bytesTransferred);\n            System.out.println(\"writing the remaining bytes: \" + remainingBytes);\n            localbuffer = new byte[remainingBytes]; // the remaining number of bytes can be less than the buffer size, so new byte array needed\n            inFileReader.read(localbuffer);\n            bos.write(localbuffer);\n            bos.flush(); // finally flush\n\n            inFileReader.close();\n            int responseCode = connection.getResponseCode();\n            System.out.println(\"ResponseCode for Clevertap file upload for file: \" + file.getName() + \", code:\" + responseCode);\n            System.out.println(\"final \" + (Runtime.getRuntime().totalMemory() - Runtime.getRuntime().freeMemory()));\n            System.out.println(\"uploaded successfully\");\n            System.out.println(\"timetaken = \" + ((int) ((System.currentTimeMillis() - startTime) / 1000)) + \" sec\");\n            return true;\n        }  catch (Exception e) {\n            System.out.println(\"Exception with Clevertap file upload for file: \" + file.getName());\n        } finally {\n            System.out.println(\"/: \" + file.getName() + \", url:\" + preSignURL);\n            if (bos != null) {\n                bos.close();\n            }\n            if (connection != null) {\n                connection.disconnect();\n            }\n        }\n\n        System.out.println(\"uploading failed\");\n        System.out.println(\"timetaken = \" + ((int) ((System.currentTimeMillis() - startTime) / 1000)) + \" sec\");\n        return false;\n    }\n\n    public static void main(String[] args) throws Exception {\n\n        String url = \"<S3_PresignedUrl>\";\n        File f = new File(\"<File_Path>\");\n        uploadCsvFile(f, url);\n\n    }\n}\n",
      "language": "java"
    }
  ]
}
[/block]