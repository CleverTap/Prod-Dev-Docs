---
title: SCIM User Provisioning using API
excerpt: >-
  Learn how to use CleverTap’s SCIM endpoints to provision users
  programmatically.
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

CleverTap supports automated user provisioning through SCIM, allowing developers to create, update, and remove dashboard users. The SCIM API accepts structured requests that define user details, roles, teams, and account access.

CleverTap validates all SCIM requests to ensure that accounts, roles, and teams are valid and the provisioning rules for the account are followed. By integrating SCIM, developers can keep CleverTap user access aligned with their organization’s identity systems, thereby reducing the need for manual onboarding and offboarding.

With the SCIM API, you can do the following:

* Provision and manage roles and teams at scale using REST API endpoints.
* Maintain consistent, policy-aligned access without manual intervention.

> 📘 Regions
>
> Your base URL depends on your account’s region (e.g., `https://in1.api.clevertap.com`). To identify your region, refer to [Region Details](https://staging.docs.dev.clevertap.net/docs/common-api-components#region).

# SCIM API Endpoint

CleverTap provides the following SCIM API endpoint:

`/nx/v2/scim/v2/Users`

# Authenticate API Requests

SCIM endpoints require a token instead of standard dashboard authentication. You must generate the token in the CleverTap dashboard before making any SCIM requests.

## Token Behavior

The following guidelines outline the process for generating, maintaining, and utilizing SCIM tokens within the system.

* Only one SCIM token exists per account.
* Tokens remain valid for _one year_ if not regenerated. Regenerating a token at any time immediately invalidates the existing token.
* SCIM APIs and IdP APIs are not interchangeable and must be used only for their respective purposes.

## Generate SCIM Token for API-Based Provisioning

Before making SCIM API calls, you must generate a token in the CleverTap dashboard. The SCIM token authenticates all provisioning requests made by your API client.

To generate a SCIM token:

1. Go to _Organization_ > _SCIM Management_ in the CleverTap dashboard.
2. In _Configuration Type_, select **API**.
3. Click **Generate Endpoint & Token**.

<Image align="center" alt="Generate Endpoint & Token" border={true} caption="Generate Endpoint & Token" src="https://files.readme.io/dbb9ca11cbda86ec704c81000c5ff8c1bbdfd81efe240d3b0e798e17a287ce06-image.png" width="75% " />

4. Copy the displayed **SCIM Token** and store it securely.

<Image align="center" alt="Generated SCIM Token and API Endpoint" border={true} caption="Generated SCIM Token and API Endpoint" src="https://files.readme.io/cead045e1a410cb2a2f356428601e9cfec9e988a1257159dfd8db7fc51c73b5f-image.png" width="75% " />

> 🚧 Important
>
> The SCIM token is displayed only once. If you lose the token, you must regenerate it by clicking Reset Token, which will invalidate the existing token.

5. Use the generated _API Configuration_ endpoint (ending with `/nx/v2/scim/v2/Users`) in your SCIM API requests.

## Header

All SCIM api endpoints require Bearer token authentication. Include the token in the Authorization header with every request. The following is the request header format:

`Authorization: Bearer <SCIM_TOKEN>`
`Content-Type: application/json`

# Rate Limits

To maintain predictable performance, SCIM requests follow rate limits:

* Up to 3 requests per second
* Use exponential backoff for 429 responses

## Audit Logs and Notifications

Every API call that modifies Role-Based Access Control (RBAC) is recorded, and the following logs are monitored:

* Target user email
* Action performed and status

Admins and users receive email alerts when roles are added, updated, or deleted.

## Best Practices

Protect your credentials and ensure secure API communication at all times.

* Use `HTTPS` for all requests
* Restrict token visibility to trusted automation systems
* Configure monitoring alerts for failed requests or unusual activity

# FAQs

The following are the common questions and answers on SCIM:

### What happens when a user is deleted?

The user’s dashboard access is revoked immediately, admins are notified by email, and the event is logged for compliance.
