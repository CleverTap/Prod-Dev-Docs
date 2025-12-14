---
title: Common API Components
excerpt: Understand the common components used in CleverTap APIs.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
CleverTap APIs contain several common components in their design and usage.  
This page details the common components available in CleverTap APIs.

# Region

The following table lists your API endpoint based on the region of your account:

| Region                  | API Endpoint             | CleverTap Dashboard URL                                                                              |
| :---------------------- | :----------------------- | :--------------------------------------------------------------------------------------------------- |
| India                   | `in1.api.clevertap.com`  | [https://in1.dashboard.clevertap.com/login.html](https://in1.dashboard.clevertap.com/login.html#/)   |
| Singapore               | `sg1.api.clevertap.com`  | [https://sg1.dashboard.clevertap.com/login.html](https://sg1.dashboard.clevertap.com/login.html#/)   |
| United States           | `us1.api.clevertap.com`  | [https://us1.dashboard.clevertap.com/login.html](https://us1.dashboard.clevertap.com/login.html#/)   |
| Indonesia               | `aps3.api.clevertap.com` | [https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html#/) |
| Middle East (UAE)       | `mec1.api.clevertap.com` | [https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html#/) |
| Europe (default region) | `api.clevertap.com`      | [https://eu1.dashboard.clevertap.com/login.html](https://eu1.dashboard.clevertap.com/login.html#/)   |

# API Headers

The API headers are used while processing the API request. The below headers are required when you use CleverTap APIs:

The `X-CleverTap-Account-Id` and `X-CleverTap-Passcode` authenticate the request.

| Header                 | Description                                             | Type   | Example Value                        |
| :--------------------- | :------------------------------------------------------ | :----- | :----------------------------------- |
| X-CleverTap-Account-Id | Your CleverTap Account ID.                              | string | "X-CleverTap-Account-Id: ACCOUNT_ID" |
| X-CleverTap-Passcode   | Your CleverTap Account Passcode.                        | string | "X-CleverTap-Passcode: PASSCODE"     |
| Content-Type           | Request content-type is always set to application/json. | string | "Content-Type: application/json"     |

Refer to the [API Authentication](doc:authentication) to obtain the header values. To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).