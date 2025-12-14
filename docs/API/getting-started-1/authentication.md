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


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8e1be5f-1202933.png",
        "1,20,29,33.png",
        1088,
        840,
        "#efeff0"
      ],
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
        "https://files.readme.io/94a65ee-2273034.png",
        "2,27,30,34.png",
        1449,
        1419,
        "#f9f9fb"
      ],
      "border": true,
      "sizing": "80"
    }
  ]
}
[/block]
# Example
Here is an example cURL request to the *Events* API showing the headers needed to authenticate the request.
[block:code]
{
  "codes": [
    {
      "code": "curl \"https://location.api.clevertap.com/1/events.json?cursor=CURSOR_VALUE\" \\\n-H \"X-CleverTap-Account-Id: YOUR_ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE\" OR \"YOUR_USER_PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nuri = URI.parse(\"https://api.clevertap.com/1/events.json?cursor=CURSOR_VALUE\")\nrequest = Net::HTTP::Get.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"YOUR_ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"YOUR_ACCOUNT_PASSCODE\"\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend\nputs response.body",
      "language": "ruby"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Use the URL based on your location:\n  * India - in1.api.clevertap.com\n  * Singapore - sg1.api.clevertap.com\n  * U.S. - us1.api.clevertap.com"
}
[/block]



# Next Steps
Now that you understand how to authenticate with the CleverTap API, you are ready to make your first API call. 

Start with [Get Events API](doc:get-events-api) which shows you how to request *App Launch* events from CleverTap.
[block:api-header]
{
  "title": "User Passcode for APIs"
}
[/block]
You can enforce dashboard users to use *user passcode* rather than *account passcode* for API authentication. User passcode offers a better security standard while using CleverTap APIs. 

User passcodes are unique to each user and granted by admin to users.

## How To Enable User Passcode For a User
If you are an admin user, to enable using user passcode, go to *Settings* > *Users*. Pick a user from the list and click on **Grant** as shown below: 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/10e7053-35.png",
        "35.png",
        2284,
        174,
        "#f1f0f1"
      ],
      "border": true
    }
  ]
}
[/block]
## User Passcode TTL (Time To Live)
When you grant the passcode to a user, you need to specify the time period until which the passcode remains valid. You can choose between *Finite* (1-365 days) and *Infinite* (never expire passcode).

Once you grant the user a user passcode, the user can see their user passcode on the *Settings* page as shown below:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/08f0429-227303436.png",
        "2,27,30,34,36.png",
        1449,
        1419,
        "#f9f9fb"
      ],
      "border": true,
      "sizing": "80"
    }
  ]
}
[/block]
## Resetting and Revoking User Passcode
An admin can reset or revoke an existing user passcode by going to the user's page and clicking on one of the following options:
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9f0237e-37.png",
        "37.png",
        2284,
        174,
        "#f0eff0"
      ],
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
        "https://files.readme.io/99a5814-38.png",
        "38.png",
        2284,
        174,
        "#f1f0f1"
      ]
    }
  ]
}
[/block]
*Reset* passcode will generate a fresh new passcode for the user. Post resetting the user will have to incorporate the new passcode to APIs.
*Revoke* passcode will invalidate the existing passcode for the user and the user can no longer fire API calls using their passcode.
[block:callout]
{
  "type": "info",
  "title": "When to use user passcodes instead of account passcodes?",
  "body": "There can be situations where it becomes risky to give away account passcode of your CleverTap account to people inside and outside of your organization. It exposes your account to security risks. Therefore, it is best to grant user passcodes to specific users who would use CleverTap APIs instead of account passcode."
}
[/block]