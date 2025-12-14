---
title: Real-Time Counts API
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
# Overview

This endpoint enables you to get a real-time count of active users in the past five minutes in your app. The results can be spliced by user\_type, session\_source, browser, os, and device for active and new users.

## Base URL

[https://api.clevertap.com/1/now.json](https://api.clevertap.com/1/now.json)

## HTTP Method

POST

## Headers

These headers are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Header
      </th>

      <th>
        Description
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        X-CleverTap-Account-Id
      </td>

      <td>
        Your CleverTap Account ID.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Account-Id: ACCOUNT\_ID"
      </td>
    </tr>

    <tr>
      <td>
        X-CleverTap-Passcode
      </td>

      <td>
        Your CleverTap Account Passcode.
      </td>

      <td>
        string
      </td>

      <td>
        "X-CleverTap-Passcode: PASSCODE"
      </td>
    </tr>

    <tr>
      <td>
        Content-Type
      </td>

      <td>
        Request content-type is always set to application/json.
      </td>

      <td>
        string
      </td>

      <td>
        "Content-Type: application/json"
      </td>
    </tr>
  </tbody>
</Table>

## Body Parameters

The body is uploaded as a JSON payload. An empty payload \{} will return a real-time count of active users.

<Table align={["left","left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Description
      </th>

      <th>
        Required
      </th>

      <th>
        Type
      </th>

      <th>
        Example Value
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        user\_type
      </td>

      <td>
        If set to true, will return the split of users by type. Can be new, repeat, addressable or customer.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        session\_source
      </td>

      <td>
        If set to true, will return the split of users by session source. Can be direct, search, social, others or unavailable.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        browser
      </td>

      <td>
        If set to true, will return the split of users by browser.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        os
      </td>

      <td>
        If set to true, will return the split of users by OS.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        device
      </td>

      <td>
        If set to true, will return the split of users by device type. Can be desktop, mobile, or tablet.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>

    <tr>
      <td>
        new
      </td>

      <td>
        Set to true for only new users.
      </td>

      <td>
        optional
      </td>

      <td>
        boolean
      </td>

      <td>
        true
      </td>
    </tr>
  </tbody>
</Table>

Below is an example payload.

```json
{
	"user_type": true,
	"session_source": true,
	"browser": true,
	"os": true,
	"device": true,
	"new": true
}
```

## Example Request

```curl
curl -X POST -d '{"user_type":true,"session_source":true,"browser":true,"os":true,"device":true,"new":true}' "https://api.clevertap.com/1/now.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

## Example Response

```json
{
  "status": "success",
  "count": 214,
  "last_processed_ts": 1486536600
}
```

Here is an example response if you set all optional parameters to true.

```json
{
  "status": "success",
  "count": 214,
  "user_type": {
    "new": 58,
    "repeat": 25,
    "addressable": 38,
    "customer": 93
  },
  "session_source": {
    "direct": 122,
    "others": 115,
    "search": 18,
    "social": 2,
    "email": 1,
    "unavailable": 4
  },
  "browser": {
    "others": 4,
    "firefox": 2,
    "chrome": 52,
    "ie": 3,
    "mobileapp": 201
  },
  "os": {
    "others": 2,
    "android": 196,
    "ios": 35,
    "windows": 25,
    "mac": 3,
    "linux": 2
  },
  "device": {
    "desktop": 32,
    "mobile": 230,
    "tablet": 1
  },
  "new": {
    "session_source": {
      "direct": 18,
      "others": 50,
      "search": 12,
      "social": 2,
      "email": 1,
      "unavailable": 3
    },
    "browser": {
      "others": 1,
      "firefox": 0,
      "chrome": 36,
      "ie": 3,
      "mobileapp": 47
    },
    "os": {
      "others": 0,
      "android": 67,
      "ios": 2,
      "windows": 15,
      "mac": 2,
      "linux": 1
    },
    "device": {
      "desktop": 18,
      "mobile": 68,
      "tablet": 1
    }
  },
  "last_processed_ts": 1486536600
}
```

## Notes

The response will be a JSON object containing either the success or fail status.

If no payload was provided and the request was successful, the response will include a count of active users in real-time. The response will also include last\_processed\_ts, which is a UNIX epoch timestamp of the last processed event in your account.

```json
{
  "status": "success",
  "count": 214,
  "last_processed_ts": 1486536600
}
```
