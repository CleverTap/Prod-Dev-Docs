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
Every CleverTap API call should include both your Account ID and Account Passcode as the request headers. The CleverTap API expects these values to be keyed as X-CleverTap-Account-Id and X-CleverTap-Passcode. 

# Getting Your Account Credentials 
Your CleverTap Account ID and Account Passcode are available on the Settings page. To navigate to the Settings page, log into [your CleverTap account](https://eu1.dashboard.clevertap.com/login.html), click on the gear icon on the top right navigation, and select Settings dashboard.


[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/661a84e-image2.png",
        "image2.png",
        489,
        258,
        "#e9e9ef"
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
        "https://files.readme.io/4301b9d-image1.png",
        "image1.png",
        769,
        425,
        "#eaebf2"
      ],
      "border": true
    }
  ]
}
[/block]
# Example
Here is an example cURL request to the Events API showing the headers needed to authenticate the request.
[block:code]
{
  "codes": [
    {
      "code": "curl \"https://api.clevertap.com/1/events.json?cursor=CURSOR_VALUE\" \\\n-H \"X-CleverTap-Account-Id: YOUR_ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    },
    {
      "code": "require 'net/http'\nrequire 'uri'\nuri = URI.parse(\"https://api.clevertap.com/1/events.json?cursor=CURSOR_VALUE\")\nrequest = Net::HTTP::Get.new(uri)\nrequest.content_type = \"application/json\"\nrequest[\"X-Clevertap-Account-Id\"] = \"YOUR_ACCOUNT_ID\"\nrequest[\"X-Clevertap-Passcode\"] = \"YOUR_ACCOUNT_PASSCODE\"\nreq_options = {\n  use_ssl: uri.scheme == \"https\",\n}\nresponse = Net::HTTP.start(uri.hostname, uri.port, req_options) do |http|\n  http.request(request)\nend\nputs response.body",
      "language": "ruby"
    }
  ]
}
[/block]
# Next Steps
Now that you understand how to authenticate with the CleverTap API, you are ready to make your first API call. 

Start with the [Get Events API](doc:get-events-api), which will show you how to request App Launch events from CleverTap.