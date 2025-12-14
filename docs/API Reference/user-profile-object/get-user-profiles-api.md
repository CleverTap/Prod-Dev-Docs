---
title: Get User Profiles API
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
The Get User Profiles API lets you download user profiles from CleverTap. 

There are two ways to get user profiles. The first way is to download user profiles that have performed a specific event. For example, you can download a list of users that have launched your app in the past day. The second way to download user profiles is by requesting the specific users needed.

# Get User Profiles by Events

Getting a list of user profiles by events requires two steps:

* With your first API call you will specify the event name, date range, and batch size needed. This request will return a cursor, which you will use to get the list of user profile records in step 2. 
* You will make a second API call using the cursor from step 1. This call will return the first batch of user profiles and and another cursor to get the second batch of user profiles. You will repeat this process until the cursor is not returned in the response, which means there are no more matching user profiles.

## Step 1: Get Cursor by Specifying Events for User Profiles

In this request, you will specify the event name, date range, and batch size needed. This request will return a cursor, which you will use to get the list of user profiles in the step 2.

### Base URL

[https://api.clevertap.com/1/profiles.json](https://api.clevertap.com/1/profiles.json)

### HTTP Method

POST

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

### URL Parameter

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
        batch\_size
      </td>

      <td>
        The number of results to return in each API call. The maximum is 5000.
      </td>

      <td>
        int
      </td>

      <td>
        50
      </td>
    </tr>
  </tbody>
</Table>

### Body Parameters

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
        event\_name
      </td>

      <td>
        The event type needed. You can get both standard and custom events from this endpoint. The standard events include App Launched, App Installed, App Uninstalled, Charged, Notification Viewed, Notification Sent, Product Viewed, and UTM Visited.
      </td>

      <td>
        string
      </td>

      <td>
        “App Launched”
      </td>
    </tr>

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

### Example Request

```curl
curl -X POST -d '{"event_name":"App Launched","from":20171201,"to":20171225}' "https://api.clevertap.com/1/profiles.json?batch_size=50" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

### Example Response

```json
{
 "cursor":"AfljfgIJBgBnamF5Kz8NegcBAwxhbCe%2Fbmhhe04BBAVlYjT4YG5reQEATQQrai57K2oue04FAUhnd38%3D",
 "status":"success"
}
```

## Step 2: Get User Profiles Using Cursor

Using the cursor from step 1, you will now make an API call that will return the first batch of user profiles and and another cursor to get the second batch of user profiles. You will repeat this process until a cursor is not returned in the response, which means there are no more matching user profiles.

### Base URL

[https://api.clevertap.com/1/profiles.json](https://api.clevertap.com/1/profiles.json)

### HTTP Method

GET

### Headers

The headers described below are all required. The X-CleverTap-Account-Id and X-CleverTap-Passcode are used to authenticate the request. Please see the [authentication guide](doc:authentication) to see how to get their values.

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

### URL Parameter

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
        cursor
      </td>

      <td>
        Required parameter. Specifies what set of user profiles to return.
      </td>

      <td>
        string
      </td>

      <td>
        ZyZjfwYEAgdjYmZyKz8NegYFAwxmamF%2FZ21meU4BBQFlYmN7ZG5ifAYCTQQrai57K2ouegJMABl6
      </td>
    </tr>
  </tbody>
</Table>

### Example Request

```curl
curl "https://api.clevertap.com/1/profiles.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

### Example Response

If there are more user profiles available, next\_cursor will be returned. The value for next\_cursor is used to get the next batch of user profiles. 

User profiles will be returned in a records object. The schema for the user profile object is documented in the section below.

```json
{
  "status": "success",
  "previous_cursor": "ZiZiNwMEBQBkaWZzY2MuO20BBQFlYmN7ZG5hewYBTQVjb2BzZmphfwEABQUra2Jmeg==",
  "next_cursor": "ZiZjNwMEBQBkaWZzY2MuO20BBQFlYmN7ZG5hewYBTQVjb2BzZmphfwEABQUra2Jmeg%3D%3D",
  "records": [
    {
      "identity": "5555555555",
      "profileData": {
        "favoriteFood": "pizza"
      },
      "platformInfo": [
        {
          "platform": "iOS",
          "os_version": "10.2",
          "app_version": "6.1.3",
          "make": "Apple",
          "model": "iPhone7,2",
          "push_token": "95f98af6ad9a5e714a56c5bf527a78cb1e3eda18d2f23bc8591437d0d8ae71a3",
          "objectId": "-1a063854f83a4c6484285039ecff87cb"
        },
        {
          "platform": "Web",
          "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097"
        }
      ]
    }
  ]
}
```

If there are no more user profiles available, next\_cursor will not be returned and a success response will be returned.

```json
{ "status" : "success"}
```

## User Profile Object

Here is the schema for the User Profile object.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Key
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        email
      </td>

      <td>
        User’s email address.
      </td>
    </tr>

    <tr>
      <td>
        profileData
      </td>

      <td>
        User’s custom profile properties.
      </td>
    </tr>

    <tr>
      <td>
        identity
      </td>

      <td>
        Custom user identity provided by you.
      </td>
    </tr>

    <tr>
      <td>
        name
      </td>

      <td>
        User’s name.
      </td>
    </tr>

    <tr>
      <td>
        phone
      </td>

      <td>
        User’s phone.
      </td>
    </tr>

    <tr>
      <td>
        platformInfo
      </td>

      <td>
        Array containing all platform names with the corresponding objectId (CleverTap ID), push\_token, app\_version, os\_version, make (device make), model (device model) if available.
      </td>
    </tr>
  </tbody>
</Table>

# Download User Profiles by ID

You can download user profiles by requesting the specific user needed.

### Overview

You can retrieve a user’s profile and all the events performed by a specific user. Supported identity values are email, a custom identity, or the CleverTap objectId.

### Base URL

[https://api.clevertap.com/1/profile.json](https://api.clevertap.com/1/profile.json)

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

### URL Parameters

One of the three parameters documented below needs to be included in your request to specify the user profile needed.

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
        email
      </td>

      <td>
        User’s email address.
      </td>

      <td>
        string
      </td>

      <td>
        “[jack@gmail.com](mailto:jack@gmail.com)”
      </td>
    </tr>

    <tr>
      <td>
        identity
      </td>

      <td>
        Custom user identity defined by you.
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
        string
      </td>

      <td>
        “1a063854f83a4c6484285039ecff87cb”
      </td>
    </tr>
  </tbody>
</Table>

### Example Request

```curl
curl "https://api.clevertap.com/1/profile.json?email=jack@gmail.com" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```

### Example Response

The User profile will be returned in a record object. The schema for the user profile object is documented below.

```json
{
  "status": "success",
  "record": {
    "email": "jack@gmail.com",
    "profileData": {
      "Last Score": 308,
      "High Score": 308,
      "Replayed": true
    },
    "events": {
      "App Launched": {
        "count": 10,
        "first_seen": 1457271567,
        "last_seen": 1458041215
      },
      "Charged": {
        "count": 6,
        "first_seen": 1457962417,
        "last_seen": 1458041276
      }
    },
    "platformInfo": [
      {
        "platform": "iOS",
        "os_version": "10.2",
        "app_version": "6.1.3",
        "make": "Apple",
        "model": "iPhone7,2",
        "push_token": "95f98af6ad9a5e714a56c5bf527a78cb1e3eda18d2f23bc8591437d0d8ae71a3",
        "objectId": "-1a063854f83a4c6484285039ecff87cb"
      },
      {
        "platform": "Web",
        "objectId": "a8ffcbc9-a747-4ee3-a791-c5e58ad03097"
      }
    ]
  }
}
```

### Additional Notes

* API errors are described on [this page](doc:api-errors).
* Requests are limited to 3 concurrent requests. If you exceed this limit, we will return a 429 error. Please try again after 10 minutes.

# Next Steps

Now that you’ve learned how to get user profiles from the CleverTap API, learn how to upload user profiles using the [Upload User Profiles API](doc:upload-user-profiles-api).
