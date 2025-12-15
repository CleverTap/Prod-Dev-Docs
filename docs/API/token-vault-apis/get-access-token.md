---
title: Get Access Token
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

Before you can call any Vault APIs, your client must first obtain an OAuth 2.0 access token from the authentication server (Keycloak). Vault uses client credentials flow, which means your backend service authenticates using a _client_id_ and _client_secret_ assigned to your application.

The access token returned from this endpoint must be passed in the Authorization: Bearer \<token> header when calling all Vault APIs, such as get-value.

# HTTP Method

POST

# Base URL

[https\://your-keycloak-server/auth/realms/ct-vault/protocol/openid-connect/token](https://your-keycloak-server/auth/realms/ct-vault/protocol/openid-connect/token)

# Example Request

Use the following request to fetch an access token:

```curl
curl -X POST https\://your-keycloak-server/auth/realms/ct-vault/protocol/openid-connect/token  
  -H 'Content-Type: application/x-www-form-urlencoded'  
  -d 'grant_type=client_credentials'  
  -d 'client_id=YOUR_CLIENT_ID'  
  -d 'client_secret=YOUR_CLIENT_SECRET'
```

# Response Parameters

The following is the list of response parameters for getting an access token

| Field              | Type    | Description                                                                                                                        |
| ------------------ | ------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| access_token       | String  | The OAuth 2.0 bearer token your service must include in the `Authorization: Bearer <access_token>` header when calling Vault APIs. |
| expires_in         | Integer | Lifetime of the access token in seconds. After this period, the token is no longer valid and a new token must be generated.        |
| refresh_expires_in | Integer | Duration in seconds for which a refresh token remains valid. (Typically unused in client credentials flow.)                        |
| token_type         | String  | Type of token returned. Always `bearer`.                                                                                           |
| scope              | String  | Permissions granted to this token. Must include `vault.access` to interact with Vault APIs.                                        |

# Example Response

The following is the sample response:  

```json
{  
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI...",  
  "expires_in": 300,  
  "refresh_expires_in": 1800,  
  "token_type": "bearer",  
  "scope": "vault.access"  
}
```
