---
title: API Encryption
excerpt: >-
  Learn how to encrypt and decrypt CleverTap API requests and responses using
  Hybrid Public Key Encryption (HPKE). This guide covers the required headers,
  key usage, and end-to-end integration flow.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

This guide explains how to integrate Hybrid Public Key Encryption (HPKE) with CleverTap APIs to encrypt request and response payloads. It outlines the prerequisites, describes the encryption flow, and includes [example implementations](doc:api-encryption#example-implementation-1-upload-user-profiles-api).

<Callout icon="📘" theme="info">
  This encryption approach applies uniformly to all CleverTap API endpoints.
</Callout>

This document demonstrates example implementations for the following APIs:

* [Upload User Profile API](doc:api-encryption#example-implementation-1-upload-user-profiles-api)
* [Get Events API](doc:api-encryption#example-implementation-2-get-events-api)
* [Get User Profile API](doc:api-encryption#example-implementation-3-get-user-profile-api)

For more information, refer to [API Encryption](https://staging.docs.user.clevertap.net/docs/api-encryption-in-transit).

# Prerequisites

Before you begin, check for the following requirements:

* CleverTap shares its public key with the customer. The customer uses this key to encrypt all API requests sent to CleverTap.
* The customer shares their public key with CleverTap. CleverTap uses this key to encrypt all API responses sent back to the customer.

For more information, refer to [Key Management](https://staging.docs.user.clevertap.net/docs/api-encryption-in-transit#key-management).

# Encryption Standard

CleverTap implements Hybrid Public Key Encryption (HPKE), as defined in RFC 9180.

### Cryptographic Profile

The following are the supported HPKE profiles for encryption:

* _Key Encapsulation Mechanism_: X25519
* _Key Derivation Function_: HKDF-SHA256
* _Authenticated Encryption_: AES-256-GCM

# End-to-End Encryption Flow

CleverTap uses end-to-end encryption to ensure that request and response payloads remain encrypted while in transit between the customer system and CleverTap. Only the sender and the intended recipient can decrypt and access the payload contents.

For more details,  refer to [End-to-End Encryption Flow](https://staging.docs.user.clevertap.net/docs/api-encryption-in-transit#end-to-end-api-encryption-workflow).

<Callout icon="📘" theme="info">
  **Encryption Guidelines**

  When using encryption, transmit request and response bodies as raw binary bytes. Do not encode the encrypted payloads using Base64.
</Callout>

# Example Implementation 1: Upload User Profiles API

The following example uses the `/1/upload` endpoint.

<Callout icon="📘" theme="info">
  **Same Encryption Approach for All API Endpoints**

  The same approach applies to all public CleverTap API endpoints.
</Callout>

## Method

POST

## Base URL

`https://api.clevertap.com`

## Region

Refer to [Region](doc:common-api-components#region) for more details.

## Endpoint

`https://api.clevertap.com/1/upload`

## Headers

The following are the critical headers that are required to be included in the payload for it to be identified as an encrypted payload:

| Header                 | Value                                 | Required |
| ---------------------- | ------------------------------------- | :------- |
| Content-Type           | `application/octet-stream+hpke`       | Yes      |
| Accept                 | `application/octet-stream+hpke`       | Yes      |
| X-CleverTap-Account-Id | Your CleverTap Account ID             | Yes      |
| X-CleverTap-Passcode   | CleverTap Passcode for authentication | Yes      |

Customer must include the API’s required authentication headers with every request. These headers are not encrypted and are sent in plaintext to allow CleverTap to authenticate and authorize the request. The same authentication headers are required for encrypted requests and are explicitly included in the encrypted request examples.

## Body Parameters

The request body structure and validation rules are identical to those of the standard [Upload User Profiles API](doc:upload-user-profiles-api). The payload is encrypted during transit.

For supported fields, limits, and identification requirements, refer to the [Upload User Profiles API](doc:upload-user-profiles-api).

## Example Payload

The following example shows a sample user profile payload in JSON format, structured according to the _Upload User Profiles API_ schema:

```json
{
  "d": [
    {
      "identity": "18121994201671",
      "type": "profile",
      "profileData": {
        "Name": "Test Name",
        "Email": "testname@gmail.com",
        "Phone": "+14155551235",
        "Gender": "M",
        "Customer Type": "Gold",
        "KYC Document": "PAN",
        "Game Level": "Intermediate"
      }
    }
  ]
}
```

## Example Request Encryption

The following example demonstrates how to encrypt a profile upload request using HPKE in Java and send the encrypted payload to CleverTap as a binary request:

```java
// Initialize HPKE
HybridConfig.register();
 
// Prepare payload
String payload = loadPayload(payload.json);
byte[] plaintext = payload.getBytes(StandardCharsets.UTF_8);
 
// Context info must match on both sides
byte[] contextInfo = String.valueOf(ENCODED_ACCOUNT_ID).getBytes(StandardCharsets.UTF_8);
 
// Encrypt using CleverTap's public key
HybridEncrypt encrypter = ctPublicKey.getPrimitive(HybridEncrypt.class);
byte[] encryptedPayload = encrypter.encrypt(plaintext, contextInfo);
 
// Build and send the request
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.clevertap.com/1/upload"))
    .header("X-CleverTap-Account-Id", ENCODED_ACCOUNT_ID)
    .header("X-CleverTap-Passcode", passcode)
    .header("Content-Type", "application/octet-stream+hpke")
    .header("Accept", "application/octet-stream+hpke")
    .POST(HttpRequest.BodyPublishers.ofByteArray(encryptedPayload))
    .build();
 
// Execute request

HttpResponse<byte[]> response = client.send(
    request,
    HttpResponse.BodyHandlers.ofByteArray()
);

```

<Callout icon="📘" theme="info">
  **Note**

  The `contextInfo` (Account ID) must match in both directions; when the customer encrypts and CleverTap decrypts, and when CleverTap encrypts and the customer decrypts.
</Callout>

## Example Response Decryption

The following example shows how to decrypt an HPKE-encrypted response returned by CleverTap using the customer’s private key. It uses the same context information used during request encryption.

```java
// Get encrypted response bytes
byte[] encryptedResponse = response.body();
 
// Decrypt using customer's private key
HybridDecrypt decrypter = custPrivateKey.getPrimitive(HybridDecrypt.class);
byte[] decryptedBytes = decrypter.decrypt(encryptedResponse, contextInfo);
 
// Convert to JSON string
String jsonResponse = new String(decryptedBytes, StandardCharsets.UTF_8);
System.out.println(jsonResponse);

```

## Example Response after Decryption

The following example shows a successful API response after decrypting the encrypted response payload:

```json
{
  "status": "success",
  "processed": 1,
  "unprocessed": []
}
```

# Example Implementation 2: Get Events API

The following example demonstrates how to retrieve events using the `/1/events.json` endpoint. Before fetching events, customer must first obtain a cursor link, which is then used to request the event data.

## Method

* POST (to get cursor)
* GET (to fetch events)

## Base URL

`https://api.clevertap.com`

## Region

Refer to [Region](doc:common-api-components#region) for more details.

# Endpoint

* POST: `https://api.clevertap.com/1/events.json` (to get cursor)
* GET: `https://api.clevertap.com/1/events.json?cursor=<cursor>` (to fetch events)

# Headers

The following are the headers listed for both POST and GET methods:

## Mandatory Headers (POST with encrypted body)

| Header                 | Value                                 | Required |
| :--------------------- | :------------------------------------ | :------- |
| Content-Type           | `application/octet-stream+hpke`       | Yes      |
| Accept                 | `application/octet-stream+hpke`       | Yes      |
| X-CleverTap-Account-Id | Your CleverTap Account ID             | Yes      |
| X-CleverTap-Passcode   | CleverTap Passcode for authentication | Yes      |

## Mandatory Headers (GET, no body, encrypted response)

| Header                 | Value                                 | Required |
| :--------------------- | :------------------------------------ | :------- |
| Content-Type           | `application/json`                    | Yes      |
| Accept                 | `application/octet-stream+hpke`       | Yes      |
| X-CleverTap-Account-Id | Your CleverTap Account ID             | Yes      |
| X-CleverTap-Passcode   | CleverTap Passcode for authentication | Yes      |

## Body Parameters

The POST request body contains the event query payload.

## Example Payload

The following example shows a sample event query payload in JSON format:

```json
{
  "event_name": "App Launched",
  "from": 20240101,
  "to": 20240131
}
```

## Example Request Encryption

The following example demonstrates how to encrypt the event query request using HPKE in Java and send the encrypted payload as a binary request to get a cursor:

```java
// Initialize HPKE
HybridConfig.register();


// Prepare payload
String payload = """
   {
     "event_name": "App Launched",
     "from": 20240101,
     "to": 20240131
   }
   """;
byte[] plaintext = payload.getBytes(StandardCharsets.UTF_8);


// Context info must match on both sides (use encoded Account ID)
byte[] contextInfo = ENCODED_ACCOUNT_ID.getBytes(StandardCharsets.UTF_8);


// Encrypt using CleverTap's public key
HybridEncrypt encrypter = ctPublicKey.getPrimitive(HybridEncrypt.class);
byte[] encryptedPayload = encrypter.encrypt(plaintext, contextInfo);


// Build and send the request
HttpRequest request = HttpRequest.newBuilder()
       .uri(URI.create("https://api.clevertap.com/1/events.json?batch_size=50"))
       .header("X-CleverTap-Account-Id", ENCODED_ACCOUNT_ID)
       .header("X-CleverTap-Passcode", PASSCODE)
       .header("Content-Type", "application/octet-stream+hpke")
       .header("Accept", "application/octet-stream+hpke")
       .POST(HttpRequest.BodyPublishers.ofByteArray(encryptedPayload))
       .build();


// Execute request
HttpResponse<byte[]> response = client.send(
       request,
       HttpResponse.BodyHandlers.ofByteArray()
);
```

## Example Response Decryption

The following example shows how to decrypt the cursor response returned by CleverTap using the customer’s private key and the same context information used during request encryption:

```java
// Get encrypted response bytes
byte[] encryptedResponse = response.body();


// Decrypt using customer's private key
HybridDecrypt decrypter = custPrivateKey.getPrimitive(HybridDecrypt.class);
byte[] decryptedBytes = decrypter.decrypt(encryptedResponse, contextInfo);


// Convert to JSON string
String jsonResponse = new String(decryptedBytes, StandardCharsets.UTF_8);
System.out.println(jsonResponse);
```

## Example Response after Decryption

The following example shows a successful cursor response after decrypting the encrypted response payload:

```json
{
  "cursor": "AfljfgIJBgBnamF5Kz8NegcBAwxhbCe%2Fbmhhe04BBAVlYjT4YG5reQEATQQrai57K2oue04FAUhnd38%3D",
  "status": "success"
}
```

## Example Request to Get Events using Cursor

For GET requests, there is no request body to encrypt. Use the _Accept_ header to request an encrypted response.

```java
// Cursor from Step 2
String cursor = "AfljfgIJBgBnamF5Kz8NegcBAwxhbCe%2Fbmhhe04BBAVlYjT4YG5reQEATQQrai57K2oue04FAUhnd38%3D";


// Build and send the GET request
HttpRequest request = HttpRequest.newBuilder()
       .uri(URI.create("https://api.clevertap.com/1/events.json?cursor=" + cursor))
       .header("X-CleverTap-Account-Id", ENCODED_ACCOUNT_ID)
       .header("X-CleverTap-Passcode", PASSCODE)
       .header("Content-Type", "application/json")
       .header("Accept", "application/octet-stream+hpke")
       .GET()
       .build();


// Execute request
HttpResponse<byte[]> response = client.send(
       request,
       HttpResponse.BodyPublishers.ofByteArray()
);
```

## Example Response Decryption for Events

```java
// Get encrypted response bytes
byte[] encryptedResponse = response.body();


// Decrypt using customer's private key
byte[] contextInfo = ENCODED_ACCOUNT_ID.getBytes(StandardCharsets.UTF_8);
HybridDecrypt decrypter = custPrivateKey.getPrimitive(HybridDecrypt.class);
byte[] decryptedBytes = decrypter.decrypt(encryptedResponse, contextInfo);


// Convert to JSON string
String jsonResponse = new String(decryptedBytes, StandardCharsets.UTF_8);
System.out.println(jsonResponse);
```

## Example Response after Decryption for Events

```json
{
  "status": "success",
  "next_cursor": "ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6",
  "records": [
    {
      "profile": {
        "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097",
        "platform": "Web",
        "email": "peter@foo.com",
        "identity": "5555555555"
      },
      "ts": 20240115140416,
      "event_props": {
        "value": "pizza"
      }
    }
  ]
}
```

# Example Implementation 3: Get User Profile API

The following example uses the `/1/profile.json` endpoint.

## Method

GET

## Base URL

`https://api.clevertap.com`

## Region

Refer to [Region](doc:common-api-components#region) for more details.

## Endpoint

`https://api.clevertap.com/1/profile.json`

## Headers

For GET requests, there is no request body to encrypt. Use the _Accept_ header to request an encrypted response.

| Header                 | Value                                 | Required |
| :--------------------- | :------------------------------------ | :------- |
| Content-Type           | `application/json`                    | Yes      |
| Accept                 | `application/octet-stream+hpke`       | Yes      |
| X-CleverTap-Account-Id | Your CleverTap Account ID             | Yes      |
| X-CleverTap-Passcode   | CleverTap Passcode for authentication | Yes      |

## Body Parameters

For GET requests, there is no request body.

## Query Parameters

Pass any one of the following parameters:

| Parameter | Description          | Example                                 |
| --------- | -------------------- | --------------------------------------- |
| email     | User's email address | [jack@gmail.com](mailto:jack@gmail.com) |
| identity  | Custom user identity | 5555555555                              |
| objectId  | CleverTap ID         | 1a063854f83a4c6484285039ecff87cb        |

## Example Request

The following example shows how to fetch a user profile using an email address as the identifier.

```java
// User identifier
String email = "jack@gmail.com";


// Build and send the GET request
HttpRequest request = HttpRequest.newBuilder()
       .uri(URI.create("https://api.clevertap.com/1/profile.json?email=" +
               URLEncoder.encode(email, StandardCharsets.UTF_8)))
       .header("X-CleverTap-Account-Id", ENCODED_ACCOUNT_ID)
       .header("X-CleverTap-Passcode", PASSCODE)
       .header("Content-Type", "application/json")
       .header("Accept", "application/octet-stream+hpke")
       .GET()
       .build();


// Execute request
HttpResponse<byte[]> response = client.send(
       request,
       HttpResponse.BodyHandlers.ofByteArray()
);
```

## Example Response Decryption

The following example shows how to decrypt the profile response returned by CleverTap using the customer’s private key and the same context information used in the request flow:

```java
// Get encrypted response bytes
byte[] encryptedResponse = response.body();


// Decrypt using customer's private key
byte[] contextInfo = ENCODED_ACCOUNT_ID.getBytes(StandardCharsets.UTF_8);
HybridDecrypt decrypter = custPrivateKey.getPrimitive(HybridDecrypt.class);
byte[] decryptedBytes = decrypter.decrypt(encryptedResponse, contextInfo);


// Convert to JSON string
String jsonResponse = new String(decryptedBytes, StandardCharsets.UTF_8);
System.out.println(jsonResponse);
```

## Example Response after Decryption

The following example shows a successful API response after decrypting the encrypted response payload:

```json
{
  "status": "success",
  "record": {
    "email": "jack@gmail.com",
    "profileData": {
      "Last Score": 308,
      "High Score": 308,
      "Replayed": true
    },
    "events": {
      "App Launched": {
        "count": 10,
        "first_seen": 1457271567,
        "last_seen": 1458041215
      },
      "Charged": {
        "count": 6,
        "first_seen": 1457962417,
        "last_seen": 1458041276
      }
    },
    "platformInfo": [
      {
        "platform": "iOS",
        "os_version": "10.2",
        "app_version": "6.1.3",
        "make": "Apple",
        "model": "iPhone7,2",
        "objectId": "-1a063854f83a4c6484285039ecff87cb"
      }
    ]
  }
}
```

# Error Handling

The following table describes how the system handles requests based on encryption headers, billing configuration, and payload state.

| Mandatory Encryption Headers Sent | Encryption Feature Enabled on Your Account | Request Encrypted | Result  |
| --------------------------------- | ------------------------------------------ | ----------------- | ------- |
| Yes                               | Yes                                        | Yes               | Success |
| Yes                               | Yes                                        | No                | Fail    |
| Yes                               | No                                         | Yes               | Fail    |
| Yes                               | No                                         | No                | Fail    |
| No                                | Yes                                        | Yes               | Fail    |
| No                                | No                                         | Yes               | Fail    |

# Frequently Asked Questions

### Do I need to change my API payload structure when using HPKE?

No. Payload structure and validation remain the same. Only the payload is encrypted in transit.

### Are authentication headers encrypted?

No. Authentication headers are always sent in plaintext.

### Does HPKE encryption apply to all CleverTap APIs?

Yes. The same encryption approach applies to all public CleverTap API endpoints.

### What happens if encryption headers are sent but the payload is not encrypted?

The request fails. Encryption headers and payload state must match the account configuration.
