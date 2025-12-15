---
title: Token Vault APIs
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

The Token Vault provides a secure mapping between raw PII values and tokens.

* [_Get Token API_](https://developer.clevertap.com/docs/get-token-api): Converts PII into a secure token
* [_Get Value API_](https://developer.clevertap.com/docs/get-value-api): Resolves a token back to the original PII

<Callout icon="📘" theme="info">
  **Security Tip**

  Never send PII to CleverTap directly. You can use your customer-hosted Vault and the Vault SDK.
</Callout>

# Authentication

All endpoints require the following authentication:

* `Authorization: Bearer <vault_api_key>`
* `Content-Type: application/json`

OAuth is also supported if your vault implements it.
