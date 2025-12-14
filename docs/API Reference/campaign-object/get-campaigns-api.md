---
title: Get Campaigns API
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

The Get Campaigns API lets you get a list of campaigns created using the API.

# Base URL

[https://api.clevertap.com/1/targets/list.json](https://api.clevertap.com/1/targets/list.json)

# HTTP Method

POST

# Headers

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

# Body Parameters

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
        from
      </td>

      <td>
        Start of date range for events needed. Value specified in format YYYYMMDD.
      </td>

      <td>
        int
      </td>

      <td>
        20171201
      </td>
    </tr>

    <tr>
      <td>
        to
      </td>

      <td>
        End of date range for events needed. Value specified in format YYYYMMDD.
      </td>

      <td>
        int
      </td>

      <td>
        20171225
      </td>
    </tr>
  </tbody>
</Table>

Below is an example payload.

```json
{
    "from": 20160101,
    "to": 20160101
}
```

# Example Request

```curl
curl -X POST -d '{"from": 20160101,"to": 20160101}' "https://api.clevertap.com/1/targets/list.json" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

# Example Response

```json
{
    "status": "success",
    "targets": [
        {
            "id": 1457429935,
            "name": "My API Campaign",
            "scheduled_on": 201703081508,
            "status": "pending"
        },
        {
            "id": 1457432766,
            "name": "My API Campaign 2",
            "scheduled_on": 201603081556,
            "status": "completed"
        }
    ]
}
```
