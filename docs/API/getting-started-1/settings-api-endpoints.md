---
title: Settings API Endpoints
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
These endpoints allow you to both fetch data about dashboard settings and also allow you to setup/update existing dashboard settings

# Subscription Groups

You can use this API to get the list of all subscription groups created on the dashboard

### Base Method

[https://api.clevertap.com/category-groups](https://api.clevertap.com/category-groups)

### HTTP Method

GET

### Headers

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
        String
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
        String
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
        String
      </td>

      <td>
        "Content-Type: application/json"
      </td>
    </tr>
  </tbody>
</Table>

### Example Response

```json
//response will be in the below format
{
   "list": [
       {
           "name": "Newsletter",
           "key": 1,
           "description": "Newslettter",
           "channels": [
               "Email"
           ],
           "updated_on": 1597812749,
           "created_on": 1597812749
       },
       {
           "name": "Product Updates",
           "key": 2,
           "description": "Product Updates",
           "channels": [
               "Email"
           ],
           "updated_on": 1597826038,
           "created_on": 1597826038
       }
   ],
   "status": "success"
}
```
