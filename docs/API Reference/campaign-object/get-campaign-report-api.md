---
title: Get Campaign Report API
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

The Get Campaign Report API lets you get campaign metrics.

To get a campaign report, you specify the id of the report needed. If the campaign has not completed yet, you will receive an error message indicating that status.

## Base URL

[https://api.clevertap.com/1/targets/result.json](https://api.clevertap.com/1/targets/result.json)

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
        CleverTap campaign id of the campaign whose metrics you want to view. You can find this id in your CleverTap dashboard under the Push section
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
curl -X POST -d '{ "id": 1457432766}' "https://api.clevertap.com/1/targets/result.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

## Example Response

```json
{
    "result": {
        "clicked": 28,
        "sent": 82
    },
    "status": "success"
}
```

## Notes

If the campaign has not been completed yet, you will receive the error response below.

```json
{
    "error": "This target hasn't been completed",
    "status": "fail"
}
```

# Next Steps

Now that you’ve learned how to get a campaign report from the CleverTap API, learn how to get events using the [Get Events API](doc:get-events-api).
