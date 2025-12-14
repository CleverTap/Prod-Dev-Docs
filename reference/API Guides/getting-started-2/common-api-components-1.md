---
title: Common API Components
excerpt: Understand the common components used in CleverTap APIs.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
CleverTap APIs contain several common components in their design and usage.\
This page details the common components found within CleverTap APIs.

# Region

The following table lists your API endpoint based on the region of your account:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th>
        Region
      </th>

      <th>
        API Endpoint
      </th>

      <th>
        CleverTap Dashboard URL
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        India
      </td>

      <td>
        `in1.api.clevertap.com`
      </td>

      <td>
        [https://in1.dashboard.clevertap.com/login.html](https://in1.dashboard.clevertap.com/login.html#/)
      </td>
    </tr>

    <tr>
      <td>
        Singapore
      </td>

      <td>
        `sg1.api.clevertap.com`
      </td>

      <td>
        [https://sg1.dashboard.clevertap.com/login.html](https://sg1.dashboard.clevertap.com/login.html#/)
      </td>
    </tr>

    <tr>
      <td>
        United States
      </td>

      <td>
        `us1.api.clevertap.com`
      </td>

      <td>
        [https://us1.dashboard.clevertap.com/login.html](https://us1.dashboard.clevertap.com/login.html#/)
      </td>
    </tr>

    <tr>
      <td>
        Indonesia
      </td>

      <td>
        `aps3.api.clevertap.com`
      </td>

      <td>
        [https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html#/)
      </td>
    </tr>

    <tr>
      <td>
        Middle East (UAE)
      </td>

      <td>
        `mec1.api.clevertap.com`
      </td>

      <td>
        [https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html#/)
      </td>
    </tr>

    <tr>
      <td>
        Europe (default region)
      </td>

      <td>
        `api.clevertap.com`
      </td>

      <td>
        [https://eu1.dashboard.clevertap.com/login.html](https://eu1.dashboard.clevertap.com/login.html#/)
      </td>
    </tr>
  </tbody>
</Table>

# API Headers

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
