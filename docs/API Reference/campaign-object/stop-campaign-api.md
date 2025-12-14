---
title: Stop Scheduled Campaign API
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

The Stop Scheduled Campaign API lets you stop a campaign scheduled through the API.

## Base URL

[https://api.clevertap.com/1/targets/stop.json](https://api.clevertap.com/1/targets/stop.json)

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

## Body Parameter

The body is uploaded as a JSON payload.

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
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
        id
      </td>

      <td>
        CleverTap ID of the scheduled campaign you want to stop. You can find this id in your CleverTap dashboard under the push section.
      </td>

      <td>
        int
      </td>

      <td>
        1457432766
      </td>
    </tr>
  </tbody>
</Table>

Below is an example payload.

```json
{
    "id": 1457432766
}
```

## Example Request

```curl
curl -X POST -d '{ "id": 1457432766}' "https://api.clevertap.com/1/targets/stop.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

## Example Response

```json
{
    "status": "success"
}
```

# Next Steps

Now that you’ve learned how to stop a scheduled campaign with the CleverTap API, learn how to get a campaign report using the [Get Campaign Report API](doc:get-campaign-report-api).
