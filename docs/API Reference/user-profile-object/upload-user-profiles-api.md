---
title: Upload User Profiles API
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
The Upload User Profiles API enables you to create or update user profiles in CleverTap. 

For example, you can use this API to update a user profile to include information, such as gender, phone, and customer type.

Adding more information to user profiles in CleverTap enables you to create richer segments and run targeted campaigns.  

# Overview

Uploading user profiles to CleverTap requires a POST request with a JSON payload specifying the user profile information. 100 user profiles records can be sent per API call. 

## Base URL

[https://api.clevertap.com/1/upload](https://api.clevertap.com/1/upload)

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
        Request content-type is always set to application/json; charset=utf-8.
      </td>

      <td>
        string
      </td>

      <td>
        "Content-Type: application/json; charset=utf-8"
      </td>
    </tr>
  </tbody>
</Table>

## Body Parameters

User profiles are uploaded as a JSON payload. The payload is an object keyed with “d” whose value is an array describing the user profile.

User profile attribute keys are limited to 120 bytes and their values are limited to 512 bytes. The maximum number of custom user profile keys is 256.

For each user profile, a user identifier is required. This is the key that CleverTap uses to find the user whose profile needs to be updated. You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId. If this identity is not found a new user profile will be created. 

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
        type
      </td>

      <td>
        Set to profile. Required.
      </td>

      <td>
        string
      </td>

      <td>
        “profile”
      </td>
    </tr>

    <tr>
      <td>
        profileData
      </td>

      <td>
        User profile properties. Passed as key/value pairs. Required.\
        profileData[“Phone”] should be formatted as +[country code][phone number].
      </td>

      <td>
        object
      </td>

      <td>
        "profileData": \{\
                "Name": "Jack Montana",\
                "Email": "[jack@gmail.com](mailto:jack@gmail.com)",\
                "Phone": "+14155551234",\
                "Gender": "M",\
                "Employed": "Y",\
                "Education": "Graduate",\
                "Married": "Y",\
                "MSG-sms": false,\
                "DOB": "$D*1487576752",    // UNIX epoch timestamp with $D* prefix for recording date properties (string)\
                "Customer Type": "Platinum"\
          }
      </td>
    </tr>

    <tr>
      <td>
        ts
      </td>

      <td>
        Date and time when the event occured, Formatted as a UNIX epoch. Defaults to the current timestamp if omitted. Optional.
      </td>

      <td>
        ts
      </td>

      <td>
        1468308340
      </td>
    </tr>

    <tr>
      <td>
        identity
      </td>

      <td>
        Identity to recognize a unique user. It can be the user’s email, a phone number, or any other identifier that you are using to tag your users.\
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
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
        FBID
      </td>

      <td>
        Identify a unique user by Facebook ID.\
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “34322423"
      </td>
    </tr>

    <tr>
      <td>
        GPID
      </td>

      <td>
        Identify a unique user by Google Plus ID.\
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “34322424"
      </td>
    </tr>

    <tr>
      <td>
        objectId
      </td>

      <td>
        Identify a unique user by CleverTap Global Object ID.\
        You have to set a value for one of these parameters to identify the user: identity, FBID, GPID, or objectId.
      </td>

      <td>
        string
      </td>

      <td>
        “34322425"
      </td>
    </tr>
  </tbody>
</Table>

Here is an example JSON payload.

```json
{
  "d": [
    {
      "identity": "1189549",  // customer ID, email or any other ID that uniquely identifies your user
      "ts": 1468308340,       // user creation date, or just leave this field out to set the time has current
      "type": "profile",
      "profileData": {
        "Name": "Jack Montana",
        "Email": "jack@gmail.com",
        "Phone": "+14155551234",
        "Gender": "M",
        "Employed": "Y",
        "Education": "Graduate",
        "Married": "Y",
        "MSG-sms": false,
        "DOB": "$D_1487576752",    // UNIX epoch timestamp with $D_ prefix for recording date properties (string)
        "Customer Type": "Platinum"
      }
    }
  ]
}
```

## Example Request

```curl
curl -X POST -d '{"d":[{"objectId":"25b08803c1af4e00839f530264dac6f8","type":"profile","profileData":{"Name": "Jack Test"}}]}' "https://api.clevertap.com/1/upload" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json; charset=utf-8"
```

## Example Response

```json
{
    "status": "success",
    "processed": 1,
    "unprocessed": []
}
```

## Debugging

Requests with processing errors will be returned in the API call response as shown in the example below.

```json
{
    "status":<success, partial, fail>, 
    "processed":<count>, 
    "unprocessed": [{"status":"fail", "code":<error code>, "error":<error msg>, "record":<record>}]
}
```

To test if your data will be submitted without errors, you can add the parameter dryRun=1 to the URL. This will validate the input without submitting the data to CleverTap.

A list of errors code for this API can be found on [this page](doc:api-errors).

## Notes

* Calls to this endpoint are synchronous. We recommend making individual calls, waiting for our response, and then make another request.
* Concurrent requests are limited to 3 per account. Requests that exceed the limit will return a 429 HTTP response code.

# Next Steps

Now that you’ve learned how to upload user profiles with the CleverTap API, learn how to use the [Get Profile Count API](doc:get-profile-count-api) that lets you get a count of profiles that match a specific query.
