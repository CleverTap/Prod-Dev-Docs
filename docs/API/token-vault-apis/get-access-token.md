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

Before you can call any Vault APIs, your client must first obtain an OAuth 2.0 access token from the authentication server (Keycloak). Vault uses client credentials flow, which means your backend service authenticates using a *client\_id* and *client\_secret* assigned to your application.

The access token returned from this endpoint must be passed in the Authorization: Bearer &lt;token&gt; header when calling all Vault APIs, such as get-value.

# HTTP Method

POST

# Base URL

[https\\://your-keycloak-server/auth/realms/ct-vault/protocol/openid-connect/token](https://your-keycloak-server/auth/realms/ct-vault/protocol/openid-connect/token)

# Example Request

Use the following request to fetch an access token.
























































































































































































































































































# Response Parameters

The following is the list of response parameters for getting an access token

| Field                | Type    | Description                                                                                                                        |
| -------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| access\_token        | String  | The OAuth 2.0 bearer token your service must include in the `Authorization: Bearer <access_token>` header when calling Vault APIs. |
| expires\_in          | Integer | Lifetime of the access token in seconds. After this period, the token is no longer valid and a new token must be generated.        |
| refresh\_expires\_in | Integer | Duration in seconds for which a refresh token remains valid. (Typically unused in client credentials flow.)                        |
| token\_type          | String  | Type of token returned. Always `bearer`.                                                                                           |
| scope                | String  | Permissions granted to this token. Must include `vault.access` to interact with Vault APIs.                                        |

# Example Response

The following is the sample response: