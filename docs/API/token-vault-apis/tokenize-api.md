---
title: Tokenize API
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
# Overview

The Tokenize API converts a PII value, such as an email address, phone number, or any custom property, into a token when the token does not exist.

The API is idempotent. If you send the same input value multiple times, the service returns the same token each time. The response indicates whether the token already exists or was created in the current request.

# HTTP Method

POST

# Base URL

Use the following base URL for all regions:

`https://vault-endpoint/ct-vault/api/v2/tokenize`

# Authentication and Headers

The Tokenize API uses Bearer token authentication. You must include a valid access token in the Authorization header for every request. This token is specific to your vault service.

Include the following headers in every Tokenize API request.

| Header        | Required | Description                                                                                          |
| :------------ | :------- | :--------------------------------------------------------------------------------------------------- |
| Content-Type  | Yes      | Must be `application/json`.                                                                          |
| Authorization | Yes      | Use `Authorization: Bearer YOUR_ACCESS_TOKEN`, where `YOUR_ACCESS_TOKEN` is your Vault access token. |

<Callout icon="📘" theme="info">
  **Note**

  To use this API, first authenticate and retrieve an access token. To get your access token, refer to [Get Access Token](doc:get-access-token).
</Callout>

# Request Parameters

Send a JSON object in the request body. The following table describes the request fields:

| Field    | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                 |
| :------- | :----- | :------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| value    | String | Yes      | The PII value to tokenize. Common examples include email addresses, phone numbers, customer IDs, and other sensitive identifiers. Use normalized formats when possible, such as lowercase email addresses and phone numbers in E.164 format. The value must be a UTF-8 string. As a safe baseline, keep values under a few kilobytes and avoid binary data. |
| metadata | Object | No       | Optional, free-form JSON object that contains non-sensitive metadata about the value, for example, `{"source": "mobile_app", "timestamp": "2025-09-24T12:34:56Z"}`. This API does not return metadata. It is stored for operational and audit use cases and may be available in Vault logs or related APIs, depending on your configuration.                |

# Example Request for Single Value

The following is the sample request for getting a token for a single value:

```curl
curl -X POST https://vault-endpoint/ct-vault/api/v2/tokenize \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "value": "user@example.com"
  }'
```
```python
import requests

url = "https://vault-endpoint/ct-vault/api/tokenization/getToken"
headers = {
    "Authorization": "Bearer YOUR_ACCESS_TOKEN",
    "Content-Type": "application/json"
}
payload = {
    "value": "user@example.com"
}

try:
    response = requests.post(url, json=payload, headers=headers, timeout=10)
    if response.status_code == 200:
        print("Success:")
        print(response.json())
    else:
        # The API returns JSON error responses
        try:
            error_body = response.json()
        except ValueError:
            error_body = response.text
        print("Error:", response.status_code, error_body)
except requests.exceptions.RequestException as e:
    print("Request failed:", e)
```
```javascript
// Run with: node --experimental-modules or a recent Node.js that supports ESM

const url = "https://vault-endpoint/ct-vault/api/tokenization/getToken";

const headers = {
  Authorization: "Bearer YOUR_ACCESS_TOKEN",
  "Content-Type": "application/json"
};

const body = JSON.stringify({
  value: "user@example.com"
});

async function getToken() {
  try {
    const response = await fetch(url, {
      method: "POST",
      headers,
      body
    });

    const text = await response.text();
    let data;

    try {
      data = JSON.parse(text);
    } catch {
      data = text;
    }

    if (!response.ok) {
      console.error("API error:", response.status, data);
      return;
    }

    console.log("Success:", data);
  } catch (err) {
    console.error("Network error:", err);
  }
}

getToken();
```
```php
<?php

$url = "https://vault-endpoint/ct-vault/api/tokenization/getToken";

$payload = json_encode([
    "value" => "user@example.com"
]);

$ch = curl_init($url);

curl_setopt($ch, CURLOPT_HTTPHEADER, [
    "Authorization: Bearer YOUR_ACCESS_TOKEN",
    "Content-Type: application/json"
]);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $payload);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$responseBody = curl_exec($ch);
$curlError = curl_error($ch);
$status = curl_getinfo($ch, CURLINFO_HTTP_CODE);

curl_close($ch);

if ($curlError) {
    echo "Request failed: $curlError\n";
} else {
    echo "Status: $status\n";
    echo "Response: $responseBody\n";
}
```

In this example:

* Replace `vault-endpoint` with the base URL for your vault deployment.
* Replace `YOUR_ACCESS_TOKEN` with your vault access token.
* Replace `user@example.com` with the PII value you want to tokenize.

# Example Request for Multiple Values

The following is the sample request for getting tokens for multiple values:

```curl
curl -X POST https://vault-endpoint/ct-vault/api/tokenization/tokens/batch \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "values": [
      "555-12-3456",
      9870344567,
      "999-88-7777",
      "john.doe@example.com",
      "4111-2222-3333-4444"
    ]
  }'

```
```python
```
```javascript
```
```php
```

# Response Parameters

This section describes the fields in a successful response.

| Field        | Type    | Always present               | Description                                                                                                                                                    |
| :----------- | :------ | :--------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| token        | String  | Yes                          | The generated Vault token that represents the input value. Treat this as an opaque, case sensitive string. Do not parse or infer structure from it.            |
| exists       | Boolean | Yes                          | True if the token already exists and the service returned an existing mapping. False if the token was created in this request.                                 |
| newlyCreated | Boolean | Yes                          | True if the token was created in this request. False if the token already existed.                                                                             |
| dataType     | String  | Yes                          | Describes the type of the input value, for example `string`, `email`, or `phone`. The actual values depend on your Vault configuration.                        |
| expiry       | String  | Yes, if expiry is configured | The timestamp when the token expires, in ISO 8601 format, such as `2027-09-24T12:34:56Z`. If your Vault does not use token expiry, this field might be absent. |

* Store tokens as UTF-8 strings with enough length to accommodate the longest token your configuration can produce. As a safe default, use a column of at least 255 characters.
* Do not rely on token prefixes, length, or character patterns for security or business logic.

# Example Response for a Single Value

The following is the sample for getting a token for a single value:

Sample response when the token already exists (HTTP 200 OK):

```json
{
  "token": "TKN_456XYZ",
  "exists": true,
  "newlyCreated": false,
  "dataType": "string",
  "expiry": "2027-09-24T12:34:56Z"
}
```

Sample response when the token is newly created (HTTP 200 OK):

```json
{
  "token": "TKN_456XYZ",
  "exists": false,
  "newlyCreated": true,
  "dataType": "string",
  "expiry": "2027-09-24T12:34:56Z"
}
```

In both cases, the token value is the same. The `exists` and `newlyCreated` fields indicate whether the token already existed before this request.

# Example Response for Multiple Values

The following is the response for getting tokens for multiple values:

```json
{
  "results": [
    {
      "originalValue": "555-12-3456",
      "token": "555-67-8901",
      "exists": true,
      "newlyCreated": false,
      "dataType": "string"
    },
    {
      "originalValue": 9870344567,
      "token": 8918663499,
      "exists": false,
      "newlyCreated": true,
      "dataType": "number"
    },
    {
      "originalValue": "999-88-7777",
      "token": "999-11-2222",
      "exists": false,
      "newlyCreated": true,
      "dataType": "string"
    },
    {
      "originalValue": "john.doe@example.com",
      "token": "fxyz.abc@example.com",
      "exists": true,
      "newlyCreated": false,
      "dataType": "string"
    },
    {
      "originalValue": "4111-2222-3333-4444",
      "token": "4333-5555-7777-8888",
      "exists": false,
      "newlyCreated": true,
      "dataType": "string"
    }
  ],
  "summary": {
    "processedCount": 5,
    "existingCount": 2,
    "newlyCreatedCount": 3
  }
}

```

# Idempotency Behavior

This API is idempotent for the same input value. If you send the same `value` multiple times, the following happens:

* The API returns the same `token`.
* On the first request, `newlyCreated` is true and `exists` is false.
* On subsequent requests, `newlyCreated` is false and `exists` is true.

For example,

**First Call**:

```json
{
  "token": "TKN_456XYZ",
  "exists": false,
  "newlyCreated": true,
  "dataType": "string",
  "expiry": "2027-09-24T12:34:56Z"
}
```

**Second Call with the Same Value**:

```json
{
  "token": "TKN_456XYZ",
  "exists": true,
  "newlyCreated": false,
  "dataType": "string",
  "expiry": "2027-09-24T12:34:56Z"
}
```

# Error Responses and Codes

All error responses use `Content-Type: application/json`.

## Error Response

This section describes the fields in an error response.

| Field      | Type   | Description                                                |
| ---------- | ------ | ---------------------------------------------------------- |
| error      | String | Machine-readable error code.                               |
| message    | String | Human-readable description of the error and how to fix it. |
| request_id | String | Optional correlation ID for support and debugging.         |

The following is the sample error response:

```json
{
  "error": "invalid_input",
  "message": "The value field is required",
  "request_id": "req_abc123"
}
```

## Error Codes

This table lists the error codes for the Tokenize API.

| HTTP status | error               | Description                                                                                                                                     |
| :---------- | :------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------- |
| 400         | invalid_input       | The request payload is missing required fields or contains malformed data. For example, `value` is absent or does not meet format requirements. |
| 401         | unauthorized        | The request does not include valid credentials. The access token is missing, invalid, or expired.                                               |
| 403         | forbidden           | The caller is authenticated but lacks permission to use the Vault or perform tokenization.                                                      |
| 500         | server_error        | The service encountered an unexpected error. You can retry after a short delay or contact support with the `request_id`.                        |
| 503         | service_unavailable | The service is temporarily unavailable. Implement retries with exponential backoff.                                                             |

# Related API

For a complete token lifecycle, use the Tokenzie API with the following related endpoints:

* [Detokenize API](doc:detokenize-api) to convert a token back to the original PII value, subject to permissions.
