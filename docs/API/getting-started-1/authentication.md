---
title: API Authentication
excerpt: Understand how CleverTap authenticates API requests
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

CleverTap uses a header-based authentication model to authenticate requests to the API. Every CleverTap API call should include *Account ID* and *Account Passcode* as the request headers.   If your CleverTap admin has opted for *User-Passcode* instead of *Account Passcode*, you must use your *User-Passcode* in the `X-CleverTap-Passcode` header. The CleverTap API expects these values to be keyed in as `X-CleverTap-Account-Id` and `X-CleverTap-Passcode`. 

# Obtain Your Account Credentials

To obtain your account credentials, refer to [Get CleverTap Account Credentials](https://developer.clevertap.com/docs/api-quickstart-guide#get-clevertap-account-credentials-to-authenticate-api-requests).

# Authorize Partners

If you are a CleverTap customer trying to connect your partner dashboard to the CleverTap dashboard, you must use `connect` API. This API enables partners to verify customer account credentials, ensuring that customers enter the correct information when connecting to CleverTap via their partner dashboard.

<Image alt="Authorize an Email Template" align="center" border={true} src="https://files.readme.io/8ace922f7dea492a90f2b90569882a1ed85cda0a91d84843009931edee9663cf-Authorize_Partner_Credentials_.png">
  Authorize Customer Account Credentials
</Image>

## Base URL

Here is an example base URL from the account in the India region:

[https://in1.api.clevertap.com/v1/connect?partner=parntername](https://in1.api.clevertap.com/v1/connect?partner=parntername)

### Region

Refer to [Region](doc:common-api-components#region) for more details.

## HTTP Method

GET

## Headers

Refer to [Headers](doc:common-api-components#headers) for more details.

## Body Parameters

The following body parameter is sent in the URL of a GET request:

| Name    | Description                                             | Type   | Sample Value | Optional/Required |
| :------ | :------------------------------------------------------ | :----- | :----------- | :---------------- |
| partner | Name of the partner who wants to connect with CleverTap | String | adjust       | Required          |

## Example Request

Here is an example cURL request showing the headers needed to authenticate the request from the account in the India region.

```curl
curl --location 'https://us1.api.clevertap.com/v1/connect?partner=partnername' \
--header 'X-CleverTap-Account-Id: YOUR_ACCOUNT_ID' \
--header 'X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE OR YOUR_USER_PASSCODE'
```
```ruby
require "uri"
require "net/http"

url = URI("https://us1.api.clevertap.com/v1/connect?partner=partnername")

https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true

request = Net::HTTP::Get.new(url)
request["X-CleverTap-Account-Id"] = "YOUR_ACCOUNT_ID"
request["X-CleverTap-Passcode"] = "YOUR_ACCOUNT_PASSCODE"

response = https.request(request)
puts response.read_body
```

For more information on the API endpoints for the region of your account, refer to [CleverTap Regions](doc:common-api-components#region).

## Example Response

```json
{
    "message": "Authorised Successfully.",
    "status": "success"
}
```

## Error Codes

If there are errors, you will receive a response in the following format:

```json
{
    "status": "fail",
    "error": "Partner not whitelisted: patnername",
    "code": 403
}
```

The following are the possible error codes:

| Error Code | Error Message           | Description                                                                                                                                                           |
| :--------- | :---------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 400        | Invalid Credentials     | Ensure that the customers enter the correct Account ID and Passcode.                                                                                                  |
| 403        | Partner not whitelisted | Before you use this API, the partner name must be whitelisted. To get yourself whitelisted, write to [integrations@clevertap.com](mailto:integrations@clevertap.com). |
| 503, 504   | Server Error            | Indicates server-side errors.                                                                                                                                         |

# Authorize Customers

If you are a CleverTap customer looking to make API requests, you must use this authorization method. This section explains how CleverTap customers are authorized before making any API requests.

## Example cURL Request

Here is an example cURL request to the *Events* API showing the headers needed to authenticate the request from the account in the India region.

```curl
curl "https://in1.api.clevertap.com/1/events.json?cursor=CURSOR_VALUE" \
-H "X-CleverTap-Account-Id: YOUR_ACCOUNT_ID" \
-H "X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE OR YOUR_USER_PASSCODE" \
-H "Content-Type: application/json"
```
```ruby
require 'net/http'
require 'uri'
uri = URI.parse("https://api.clevertap.com/1/events.json?cursor=CURSOR_VALUE")
request = Net::HTTP::Get.new(uri)
request.content_type = "application/json"
request["X-Clevertap-Account-Id"] = "YOUR_ACCOUNT_ID"
request["X-Clevertap-Passcode"] = "YOUR_ACCOUNT_PASSCODE"
req_options = {
  use_ssl: uri.scheme == "https",
}
response = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|
  http.request(request)
end
puts response.body
```

For more information on the API endpoints for the region of your account, refer to [CleverTap Regions](doc:common-api-components#region).

# Account Passcode Vs User Passcode

There can be situations where it becomes risky to give away the account passcode of your CleverTap account to people inside and outside your organization. It exposes your account to security risks. Therefore, granting user passcodes to specific users using CleverTap APIs instead of account passcodes is best.

## Account-Level Passcode

You can have multiple account-level passcodes rather than having a single account passcode used across different partners. This offers better security when using CleverTap APIs. 

Account passcodes can be unique to each partner. Only admins or users with write access to the User Settings page can grant passcodes. They can create up to **100 passcodes**.

### Create Account Passcode

To create an account passcode:

1. Navigate to *Settings* > *Passcodes*.
2. Click **+ Passcode**. 

<Image title="Generate New Passcode Page" alt={2722} align="center" border={true} src="https://files.readme.io/ba91ebd-Passcodes_page.png">
  Generate Passcode
</Image>

3. The *Generate Passcode* page displays.

<Image title="Add Passcode Details Page" alt={630} align="center" width="40% " border={true} src="https://files.readme.io/3072783-Generate_Passcode.png">
  Add Passcode Details
</Image>

4. Enter the following details:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        <p>Field</p>
      </th>

      <th>
        <p>Description</p>
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        <p>Passcode Name</p>
      </td>

      <td>
        <p>Enter the name to uniquely identify the passcode.</p>
      </td>
    </tr>

    <tr>
      <td>
        <p>Set Expiry Date</p>
      </td>

      <td>
        <p>Select from the available options:</p><ul><li>Set day(s): Enter the number of days after which the passcode will expire.</li><li>Forever: Select this option if you do not want the passcode to expire.</li></ul>
      </td>
    </tr>
  </tbody>
</Table>

5. Click **Create & View**. On clicking, the passcode displays.
6. Click **API Key** to copy it and then click **Done**. 

> 📘 Save Passcode
>
> For security reasons, the passcode cannot be shown again. Copy the key and save it for later use.

7. The new passcode is now visible under the *Passcodes* page.

> 📘 Passcode Expiry
>
> * An email notification is sent when the passcode is nearing expiration or has expired. 
> * The passcode status is displayed as *Expiring soon* from the 30 days of expiration. 
> * An error message displays when using the expired passcode to authenticate with CleverTap API.
> * After the passcode expires, we recommend deleting the passcode.

### Edit Account Passcode

To edit account passcode:

1. Navigate to *Settings* > *Passcodes* and then click ![Edit](https://files.readme.io/dbb52d8-Edit_icon.png) icon for the passcode you want to edit. The *Edit Passcode* page opens.

<Image title="Edit Passcode Details Page" alt={578} align="center" width="300rt" border={true} src="https://files.readme.io/389f1ff-Edit_Passcode.png">
  Edit the Existing Passcode
</Image>

2. Modify the required fields and then click **Update**. 

### Delete Account Passcode

Navigate to *Settings* > *Passcodes* and then click ![Delete](https://files.readme.io/00c078d-Delete_icon.png) icon for the passcode you want to delete. On deleting the passcode, the APIs will stop working.

## User Passcode

For API authentication, you can enforce dashboard users to use *user passcode* rather than *account passcode*. User passcode offers a better security standard while using CleverTap APIs. 

User passcodes are unique to each user and granted by the admin.

### Enable User Passcode for a User

1. If you are an admin user, go to *Settings* > *Users* to enable the user passcode. 
2. Select the user from the list and click **Grant**.

<Image title="Grant User Access" alt={2284} align="center" border={true} src="https://files.readme.io/10e7053-35.png">
  Enable User Passcode
</Image>

When you grant the passcode to a user, you need to specify the period for which the passcode remains valid.\
You can specify the period from the following options:

| Field    | Description                                                                  |
| :------- | :--------------------------------------------------------------------------- |
| Finite   | Indicates that the passcode can be valid for a specific period (1-365 days). |
| Infinite | Indicates that the passcode is valid for up to 10 years.                     |

3. After you grant the user a user passcode, the users can see their user passcode on the *Settings* page, as shown below:

<Image alt="View User Passcode" align="center" border={true} src="https://files.readme.io/8e864e4-Password_view.png">
  View User Passcode
</Image>

### Reset and Revoke User Passcode

An admin can reset or revoke an existing user passcode by navigating to the *Users* page and selecting the required action.

* *Reset Passcode* generates a fresh new passcode for the user. Post resetting, the user has to incorporate the new passcode into APIs.

<Image title="Reset Passcode" alt={2284} align="center" border={true} src="https://files.readme.io/99a5814-38.png">
  Reset User Passcode
</Image>

* *Revoke Passcode* invalidates the existing passcode for the user, and the user can no longer fire API calls using their passcode.

<Image title="Revoke User Access" alt={2284} align="center" border={true} src="https://files.readme.io/9f0237e-37.png">
  Revoke User Access to the Passcode
</Image>

# Next Step

Now that you understand how to authenticate with the CleverTap API, you are ready to make your first API call. 

Start with [Get User Profiles](https://developer.clevertap.com/docs/get-user-profiles-api) API, which shows you how to request *User Profiles* from CleverTap.
