---
title: Authentication
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
CleverTap uses a header based authentication model to authenticate requests to the API. 

# Overview

Every CleverTap API call should include both your *Account ID* and *Account Passcode* as the request headers. If your CleverTap admin has opted for *User-Passcode* instead of *Account Passcode*, you have to use your *User-Passcode* instead in the passcode header. The CleverTap API expects these values to be keyed as X-CleverTap-Account-Id and X-CleverTap-Passcode. 

# Getting Your Account Credentials

Your CleverTap *Account ID* and *Account/User Passcode* are available on the *Settings* page. To navigate to the *Settings* page, log in to [your CleverTap account](https://eu1.dashboard.clevertap.com/login.html), click on the gear icon on the bottom left navigation, and select *Settings* dashboard.

<Image title="1,20,29,33.png" alt={1088} className="border" border={true} src="https://files.readme.io/8e1be5f-1202933.png" />

<Image title="2,27,30,34.png" alt={1449} className="border" width="80%" border={true} src="https://files.readme.io/94a65ee-2273034.png" />

# Example

Here is an example cURL request to the *Events* API showing the headers needed to authenticate the request.

```curl
curl "https://location.api.clevertap.com/1/events.json?cursor=CURSOR_VALUE" \
-H "X-CleverTap-Account-Id: YOUR_ACCOUNT_ID" \
-H "X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE" OR "YOUR_USER_PASSCODE" \
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

> 📘 Note
>
> Use the URL based on your location:
>
> * India - in1.api.clevertap.com
> * Singapore - sg1.api.clevertap.com
> * U.S. - us1.api.clevertap.com

# Next Steps

Now that you understand how to authenticate with the CleverTap API, you are ready to make your first API call. 

Start with [Get Events API](doc:get-events-api) which shows you how to request *App Launch* events from CleverTap.

## User Passcode for APIs

You can enforce dashboard users to use *user passcode* rather than *account passcode* for API authentication. User passcode offers a better security standard while using CleverTap APIs. 

User passcodes are unique to each user and granted by admin to users.

## How To Enable User Passcode For a User

If you are an admin user, to enable using user passcode, go to *Settings* > *Users*. Pick a user from the list and click on **Grant** as shown below: 

<Image title="35.png" alt={2284} className="border" border={true} src="https://files.readme.io/10e7053-35.png" />

## User Passcode TTL (Time To Live)

When you grant the passcode to a user, you need to specify the time period until which the passcode remains valid. You can choose between *Finite* (1-365 days) and *Infinite* (never expire passcode).

Once you grant the user a user passcode, the user can see their user passcode on the *Settings* page as shown below:

<Image title="2,27,30,34,36.png" alt={1449} className="border" width="80%" border={true} src="https://files.readme.io/08f0429-227303436.png" />

## Resetting and Revoking User Passcode

An admin can reset or revoke an existing user passcode by going to the user's page and clicking on one of the following options:

<Image title="37.png" alt={2284} className="border" border={true} src="https://files.readme.io/9f0237e-37.png" />

![2284](https://files.readme.io/99a5814-38.png "38.png")

*Reset* passcode will generate a fresh new passcode for the user. Post resetting the user will have to incorporate the new passcode to APIs.\
*Revoke* passcode will invalidate the existing passcode for the user and the user can no longer fire API calls using their passcode.

> 📘 When to use user passcodes instead of account passcodes?
>
> There can be situations where it becomes risky to give away account passcode of your CleverTap account to people inside and outside of your organization. It exposes your account to security risks. Therefore, it is best to grant user passcodes to specific users who would use CleverTap APIs instead of account passcode.
