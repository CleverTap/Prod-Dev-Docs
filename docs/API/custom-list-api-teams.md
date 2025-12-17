---
title: Custom List API (Teams)
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

Custom list segments are segments you create from your own list of users. You create a custom list segment in three steps as follows:

1. **Request a pre-signed URL** with your first API call. A pre-signed URL gives you temporary access to upload your list of users.
2. **Upload your file** to the pre-signed URL.
3. **Indicate completion** of the upload with a second API call. CleverTap processes the file and displays the segment on the dashboard.

   * If your account has **Teams** enabled, you can allocate the segment to one or more teams in this step.

# Step 1: Create a Pre-signed URL

This request generates a pre-signed URL that you use to upload your custom list file. The URL remains valid for **24 hours**.

## Base URL

Here is an example base URL from the India region:  
[https://in1.api.clevertap.com/get_custom_list_segment_url](https://in1.api.clevertap.com/get_custom_list_segment_url)  
Refer to [Region](https://developer.clevertap.com/docs/common-api-components#region) for region-specific endpoints.

## HTTP Method

`POST`

## Headers

The following headers are required. For details, see [Headers](https://developer.clevertap.com/docs/common-api-components#headers).

## Example Request

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
    "Cache-Control": "no-cache",
    "Content-Type": "application/json",
    "X-CleverTap-Account-Id": "ACCOUNT_ID",
    "X-CleverTap-Passcode": "PASSCODE"
}

response = requests.post(url, headers=headers)
print(response.text)
```
```javascript

const url = "https://in1.api.clevertap.com/get_custom_list_segment_url";
const headers = {
  "Cache-Control": "no-cache",
  "Content-Type": "application/json",
  "X-CleverTap-Account-Id": "ACCOUNT_ID",
  "X-CleverTap-Passcode": "PASSCODE"
};

axios.post(url, {}, { headers })
  .then(res => console.log(res.data))
  .catch(err => console.error(err.response ? err.response.data : err.message));
```
```ruby
require 'net/http'
require 'uri'

uri = URI.parse("https://in1.api.clevertap.com/get_custom_list_segment_url")
request = Net::HTTP::Post.new(uri)
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request["Content-Type"] = "application/json"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```
```php
<?php
$headers = array(
  'X-CleverTap-Account-Id: ACCOUNT_ID',
  'X-CleverTap-Passcode: PASSCODE',
  'Content-Type: application/json'
);
$url = "https://in1.api.clevertap.com/get_custom_list_segment_url";

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$response = curl_exec($ch);
curl_close($ch);

echo $response;
```

# Step 2: Upload Custom List File

Once you have the pre-signed URL, upload the user list file.

## File Format Requirements

* File must be **CSV**.
* Include a **header row**.
* Use **UTF-8** encoding.
* Maximum file size: **5 GB**.
* Mandatory columns: `identity` (user identifier).

## Example Request

```curl
curl --request PUT \
  --upload-file "<path to file on your local machine>" \
  "<PRE-SIGNED URL>"
```
```python
import requests

file_path = "<path to file on your local machine>"
presigned_url = "<PRE-SIGNED URL>"

with open(file_path, "rb") as file:
    response = requests.put(presigned_url, data=file)

print(response.status_code)
```
```ruby
require "net/http"

uri = URI("<PRE-SIGNED URL>")
request = Net::HTTP::Put.new(uri)
request.body = File.read("<path to file on your local machine>")
request["Content-Type"] = "text/csv"

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end
```
```javascript
const fs = require("fs");
const axios = require("axios");

const filePath = "<path to file on your local machine>";
const presignedUrl = "<PRE-SIGNED URL>";

axios.put(presignedUrl, fs.createReadStream(filePath), {
  headers: { "Content-Type": "text/csv" }
})
.then(res => console.log("Upload complete:", res.status))
.catch(err => console.error(err.message));
```
```php
<?php
$filePath = "<path to file on your local machine>";
$presignedUrl = "<PRE-SIGNED URL>";

$ch = curl_init($presignedUrl);
curl_setopt($ch, CURLOPT_PUT, true);
curl_setopt($ch, CURLOPT_INFILE, fopen($filePath, "r"));
curl_setopt($ch, CURLOPT_INFILESIZE, filesize($filePath));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$response = curl_exec($ch);
curl_close($ch);

echo $response;
```

# Step 3: Indicate Completion of File Upload

After you upload the file, call this endpoint to confirm completion. CleverTap processes the file and creates the custom list segment.  
If your account has **Teams** enabled, include the optional `team_allocations` array to allocate the new segment to team(s).

## Base URL

Example base URL from the India region:  
[https://in1.api.clevertap.com/upload_custom_list_segment_completed](https://in1.api.clevertap.com/upload_custom_list_segment_completed)

## HTTP Method

POST

## Headers

All headers are required.

The following table lists the required headers for this request.

| Header                   | Description                       | Example            |
| ------------------------ | --------------------------------- | ------------------ |
| `X-CleverTap-Account-Id` | Your CleverTap Account ID.        | `ACCOUNT_ID`       |
| `X-CleverTap-Passcode`   | Your CleverTap Account Passcode.  | `PASSCODE`         |
| `Content-Type`           | Always set to `application/json`. | `application/json` |

## Body Parameters

Provide the following body parameters. Include the Teams parameter only if Teams are enabled for your account.

| Parameter          | Description                                                                                                        | Type             | Required | Example                  |
| ------------------ | ------------------------------------------------------------------------------------------------------------------ | ---------------- | -------- | ------------------------ |
| `name`             | Name of the segment.                                                                                               | String           | Yes      | `"sample_segment"`       |
| `creator`          | Name of the user making this API call.                                                                             | String           | Yes      | `"Jane"`                 |
| `filename`         | Name of the uploaded file.                                                                                         | String           | Yes      | `"my_file_name.csv"`     |
| `email`            | Admin email address of the owner.                                                                                  | String           | Yes      | `"jane@clevertap.com"`   |
| `url`              | Pre-signed URL used for upload, obtained in Step 1.                                                                | String           | Yes      | `"<PRE-SIGNED URL>"`     |
| `replace`          | If `true`, replaces an existing segment with the same name.                                                        | Boolean          | No       | `false`                  |
| `team_allocations` | **Teams only.** Array that maps the created segment to one or more team names. Omit this field if not using Teams. | Array of objects | No*      | See “Teams Object” below |

* Required only when allocating segments to teams due to account policy or configuration.

Each object in `team_allocations` uses the following keys.

| Key                   | Description                                                                                                                     | Type             | Required | Example           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ---------------- | -------- | ----------------- |
| `segment_name`        | Name of the segment to allocate (use the same value as `name` in this request).                                                 | String           | Yes      | `"Premium Users"` |
| `customer_segment_id` | Internal ID if already available; omit when creating a new segment without an existing ID.                                      | String           | No       | `"SEG12345"`      |
| `team_names`          | One or more team names to assign to the segment. Team names must match names configured in your account and are case-sensitive. | Array of strings | Yes      | `["team A"]`      |

### Example Teams Payload

```json
[
  {
    "segment_name": "Premium Users",
    "customer_segment_id": "SEG12345",
    "team_names": ["team A"]
  }
]
```

## Example Request

```curl
curl -X POST \
  https://in1.api.clevertap.com/upload_custom_list_segment_completed \
  -H 'Content-Type: application/json' \
  -H 'X-CleverTap-Account-Id: ACCOUNT_ID' \
  -H 'X-CleverTap-Passcode: PASSCODE' \
  -d '{
    "name":"sample_segment",
    "email":"jane@clevertap.com",
    "filename":"my_file_name.csv",
    "creator":"Jane",
    "url":"<PRE-SIGNED URL>",
    "replace":false,
    "team_names":["team A"]
  }'
```
```python
import requests

url = "https://in1.api.clevertap.com/upload_custom_list_segment_completed"
headers = {
    "Content-Type": "application/json",
    "X-CleverTap-Account-Id": "ACCOUNT_ID",
    "X-CleverTap-Passcode": "PASSCODE"
}
data = {
    "name": "sample_segment",
    "email": "jane@clevertap.com",
    "filename": "my_file_name.csv",
    "creator": "Jane",
    "url": "<PRE-SIGNED URL>",
    "replace": False,
    "team_allocations": [
        {
            "segment_name": "sample_segment",
            "customer_segment_id": "SEG12345",
            "team_names": ["team A"]
        }
    ]
}

response = requests.post(url, json=data, headers=headers)
print(response.json())
```
```javascript
const axios = require("axios");

const url = "https://in1.api.clevertap.com/upload_custom_list_segment_completed";
const headers = {
  "Content-Type": "application/json",
  "X-CleverTap-Account-Id": "ACCOUNT_ID",
  "X-CleverTap-Passcode": "PASSCODE"
};

const data = {
  name: "sample_segment",
  email: "jane@clevertap.com",
  filename: "my_file_name.csv",
  creator: "Jane",
  url: "<PRE-SIGNED URL>",
  replace: false,
  team_allocations: [
    {
      segment_name: "sample_segment",
      customer_segment_id: "SEG12345",
      team_names: ["team A"]
    }
  ]
};

axios.post(url, data, { headers })
  .then(res => console.log(res.data))
  .catch(err => console.error(err.response ? err.response.data : err.message));
```
```ruby
require 'net/http'
require 'json'

uri = URI("https://in1.api.clevertap.com/upload_custom_list_segment_completed")
request = Net::HTTP::Post.new(uri, 'Content-Type' => 'application/json')
request["X-CleverTap-Account-Id"] = "ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "PASSCODE"
request.body = {
  name: "sample_segment",
  email: "jane@clevertap.com",
  filename: "my_file_name.csv",
  creator: "Jane",
  url: "<PRE-SIGNED URL>",
  replace: false,
  team_allocations: [
    {
      segment_name: "sample_segment",
      customer_segment_id: "SEG12345",
      team_names: ["team A"]
    }
  ]
}.to_json

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts response.body
```
```php
<?php
$url = "https://in1.api.clevertap.com/upload_custom_list_segment_completed";
$headers = array(
  'Content-Type: application/json',
  'X-CleverTap-Account-Id: ACCOUNT_ID',
  'X-CleverTap-Passcode: PASSCODE'
);
$data = array(
  "name" => "sample_segment",
  "email" => "jane@clevertap.com",
  "filename" => "my_file_name.csv",
  "creator" => "Jane",
  "url" => "<PRE-SIGNED URL>",
  "replace" => false,
  "team_allocations" => array(
    array(
      "segment_name" => "sample_segment",
      "customer_segment_id" => "SEG12345",
      "team_names" => array("team A")
    )
  )
);

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$response = curl_exec($ch);
curl_close($ch);

echo $response;
```

# Error Codes

The Custom List API may return various error responses depending on the request.

Use the following table to troubleshoot common errors. (Teams-specific checks can also trigger `team_ids_required` if your account requires team allocation.)

| **HTTP Status** | **Error**                 | **Description**                                                                                         |
| --------------- | ------------------------- | ------------------------------------------------------------------------------------------------------- |
| 400             | `json_parse_failed`       | The request body contains invalid JSON.                                                                 |
| 400             | `invalid_payload`         | The `segmentUploadDetails` field is missing or empty.                                                   |
| 401             | `missing_fields`          | One or more required fields are missing — segment name, email ID, creator, filename, or pre-signed URL. |
| 401             | `invalid_email`           | The email format is invalid or the email is not associated with an admin.                               |
| 401             | `segment_not_found`       | No existing segment found to replace.                                                                   |
| 401             | `segment_in_progress`     | A previous segment upload is still being processed.                                                     |
| 401             | `duplicate_name`          | A segment with the same name already exists.                                                            |
| 401             | `url_already_used`        | The pre-signed URL has already been used.                                                               |
| 401             | `file_not_found`          | The file was not found in cloud storage.                                                                |
| 401             | `file_not_uploaded`       | The file has not been uploaded to cloud storage yet.                                                    |
| 401             | `cloud_connection_failed` | Failed to connect to cloud storage.                                                                     |
| 401             | `team_ids_required`       | Team IDs must be provided when team-based segmentation is enabled.                                      |
| 401             | `feature_not_enabled`     | The custom list segments feature is not enabled for this account.                                       |
| 401             | `account_meta_missing`    | Account metadata not found for the specified account ID.                                                |
| 401             | `url_generation_failed`   | Failed to generate the pre-signed URL.                                                                  |
| 401             | `unexpected_url_error`    | An unexpected error occurred during pre-signed URL generation.                                          |
| 413             | `file_too_large`          | The uploaded file exceeds the maximum allowed size (5 GB).                                              |
| 415             | `invalid_file_format`     | The file format is invalid (only `.csv` with UTF-8 encoding is supported).                              |
| 429             | `rate_limit_exceeded`     | Too many requests have been made in a short time.                                                       |
| 503             | `worker_unavailable`      | The account’s processing worker is currently unavailable.                                               |

# Notes

* `team_allocations` is optional and only applies to accounts with Teams enabled.
* Use the exact team names configured in your account for `team_names`. Team names are case-sensitive.
* When creating a brand-new segment, you can omit `customer_segment_id`.
* If your account enforces team allocation, omitting `team_allocations` may result in `team_ids_required`.
