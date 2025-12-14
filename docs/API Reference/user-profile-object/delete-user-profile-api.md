---
title: Delete User Profile API
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

This endpoint enables you to delete a user profile.

## Base URL

[http://eu1.wzrkt.com/1/delete/profiles.json](http://eu1.wzrkt.com/1/delete/profiles.json)

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

The body is uploaded as a JSON payload. 

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
        identity
      </td>

      <td>
        Custom user identity defined by you.
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>
        “5555555555”
      </td>
    </tr>

    <tr>
      <td>
        objectId
      </td>

      <td>
        CleverTap ID.
      </td>

      <td>
        optional
      </td>

      <td>
        string
      </td>

      <td>
        “1a063854f83a4c6484285039ecff87cb”
      </td>
    </tr>
  </tbody>
</Table>

Below is an example payload.

```json
{
    "guid":"df2e224d90874887b4d61153ef3a2508"
}
```

## Example Response

```json
{
  "status": "success",
  "count": 7138
}
```
