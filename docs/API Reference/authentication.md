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

<Image title="image2.png" alt={489} className="border" border={true} src="https://files.readme.io/661a84e-image2.png" />

<Image title="image1.png" alt={769} className="border" border={true} src="https://files.readme.io/4301b9d-image1.png" />

# Example

Here is an example cURL request to the Events API showing the headers needed to authenticate the request.

```curl
curl "https://api.clevertap.com/1/events.json?cursor=CURSOR_VALUE" \
-H "X-CleverTap-Account-Id: YOUR_ACCOUNT_ID" \
-H "X-CleverTap-Passcode: YOUR_ACCOUNT_PASSCODE" \
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

# Next Steps

Now that you understand how to authenticate with the CleverTap API, you are ready to make your first API call. 

Start with the [Get Events API](doc:get-events-api), which will show you how to request App Launch events from CleverTap.
