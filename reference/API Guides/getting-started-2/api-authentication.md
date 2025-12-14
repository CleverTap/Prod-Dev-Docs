---
title: API Authentication
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

CleverTap uses a header-based authentication model to authenticate requests to the API. Every CleverTap API call should include _Account ID_ and _Account Passcode_ as the request headers. Suppose your CleverTap admin has opted for _User-Passcode_ instead of _Account Passcode_. In that case, you must use your _User-Passcode_ in the passcode header. The CleverTap API expects these values to be keyed in as X-CleverTap-Account-Id and X-CleverTap-Passcode. 

# Account Passcode Vs. User Passcode

There can be situations where it becomes risky to give away the account passcode of your CleverTap account to people inside and outside your organization. It exposes your account to security risks. Therefore, it is best to grant user passcodes to specific users who would use CleverTap APIs instead of account passcode.

# Obtain Your Account Credentials

To obtain account credentials, log in to the [CleverTap dashboard](https://eu1.dashboard.clevertap.com/login.html) and navigate to _Settings_ > _Project_ page. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/273bd2c-Settings_icon.png",
        "Settings icon.png",
        1078
      ],
      "sizing": "80",
      "border": true
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a1bca28-Account_Passcode_navigation.png",
        "Account Passcode navigation.png",
        2238
      ],
      "sizing": "smart",
      "border": true
    }
  ]
}
[/block]

> 📘 Create Account Passcode
> 
> You can also create an account passcode from the _Projects_ page by clicking the **+Passcode** link. On clicking, you are navigated to the _Passcodes_ page. For more information about the steps involved, refer to [Create Account Passcode](https://developer.clevertap.com/docs/authentication_multiple-passcode#create-account-passcode).

# Example

Here is an example cURL request to the _Events_ API showing the headers needed to authenticate the request from the account in the India region.

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

## Account-level Passcode for APIs

You can have multiple account-level passcodes rather than having a single account passcode used across different partners. This offers better security when using CleverTap APIs. 

Account passcodes can be unique to each partner. Only admins or users with write access to the User Settings page can grant passcodes. They can create up to **100 passcodes**.

## Create Account Passcode

To create an account passcode:

1. Navigate to _Settings_ > _Passcodes_.
2. Click **+ Passcode**. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ba91ebd-Passcodes_page.png",
        "Passcodes page.png",
        2722
      ],
      "border": true
    }
  ]
}
[/block]

The _Generate Passcode_ page displays.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3072783-Generate_Passcode.png",
        "Generate Passcode.png",
        630
      ],
      "sizing": "smart",
      "border": true
    }
  ]
}
[/block]

3. Enter the following details:

| <p>Field</p>           | <p>Description</p>                                                                                                                                                                                                   |
| :--------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Passcode Name</p>   | <p>Enter the name to uniquely identify the passcode.</p>                                                                                                                                                             |
| <p>Set Expiry Date</p> | <p>Select from the available options:</p><ul><li>Set day(s): Enter the number of days after which the passcode will expire.</li><li>Forever: Select this option if you do not want the passcode to expire.</li></ul> |

4. Click **Create & View**. On clicking, the passcode displays.
5. Click **API Key** to copy it and then click **Done**. 

> 📘 Save Passcode for Later Use
> 
> For security reasons, the passcode cannot be shown again. Copy the key and save it for later use.

The new passcode is now visible under the _Passcodes_ page.

> 📘 Passcode Status
> 
> - An email notification is sent when the passcode is nearing expiration or has expired. 
> - The passcode status is displayed as _Expiring soon_ from the 30 days of expiration. 
> - An error message displays when using the expired passcode to authenticate with CleverTap API.
> - After the passcode expires, we recommend deleting the passcode.

## Edit Account Passcode

To edit account passcode:

1. Navigate to _Settings_ > _Passcodes_ and then click ![Edit](https://files.readme.io/dbb52d8-Edit_icon.png) icon for the passcode you want to edit. The _Edit Passcode_ page opens.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/389f1ff-Edit_Passcode.png",
        "Edit Passcode.png",
        578
      ],
      "sizing": "smart",
      "border": true
    }
  ]
}
[/block]

2. Modify the required fields and then click **Update**. 

## Delete Account Passcode

Navigate to _Settings_ > _Passcodes_ and then click ![Delete](https://files.readme.io/00c078d-Delete_icon.png) icon for the passcode you want to delete. On deleting the passcode, the APIs will stop working.

## User Passcode for APIs

For API authentication, you can enforce dashboard users to use _user passcode_ rather than _account passcode_. User passcode offers a better security standard while using CleverTap APIs. 

User passcodes are unique to each user and granted by the admin.

## Enable User Passcode For a User

1. If you are an admin user, go to _Settings_ > _Users_ to enable the user passcode. 
2. Select the user from the list and click **Grant**.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/10e7053-35.png",
        "35.png",
        2284
      ],
      "border": true
    }
  ]
}
[/block]

When you grant the passcode to a user, you need to specify the period for which the passcode remains valid.  
You can from the following options:

| Field    | Description                                                                  |
| :------- | :--------------------------------------------------------------------------- |
| Finite   | Indicates that the passcode can be valid for a specific period (1-365 days). |
| Infinite | Indicates that the passcode is valid for up to 10 years.                     |

After you grant the user a user passcode, the user can see their user passcode on the _Settings_ page, as shown in the following figure:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/996b211-User_Passcode.png",
        "User Passcode.png",
        2238
      ],
      "sizing": "80",
      "border": true
    }
  ]
}
[/block]

## Reset and Revoke User Passcode

An admin can reset or revoke an existing user passcode by navigating to the _Users_ page and selecting the required action.

- _Reset Passcode_ generates a fresh new passcode for the user. Post resetting, the user has to incorporate the new passcode into APIs.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/99a5814-38.png",
        "38.png",
        2284
      ],
      "border": true
    }
  ]
}
[/block]

- _Revoke Passcode_ invalidates the existing passcode for the user, and the user can no longer fire API calls using their passcode.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9f0237e-37.png",
        "37.png",
        2284
      ],
      "border": true
    }
  ]
}
[/block]

# Next Steps

Now that you understand how to authenticate with the CleverTap API, you are ready to make your first API call. 

Start with [Get User Profiles API](https://developer.clevertap.com/docs/get-user-profiles-api), which shows you how to request _User Profiles_ from CleverTap.