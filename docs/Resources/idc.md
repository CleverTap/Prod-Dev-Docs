---
title: Configuration Guide for CleverTap IDC
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: noindex
next:
  description: ''
---
# Overview

By default, we process and store customer data in our EU data center. For CleverTap customers based in India who need in-region storage and processing for regulatory compliance, we offer an Indian data center. 

 To enable processing and storage in our Indian data center, please complete these two steps:
  * Contact the [CleverTap support team](doc:developer-support) to enable this feature for your account.
  * Complete the technical configuration steps described below.

# Configuration Steps to Enable CleverTap IDC

For each platform that you are using CleverTap with, there is a quick step you need to complete to enable usage of CleverTap's Indian data center.

## Android

To integrate CleverTap in your Android application, follow [this quickstart guide](doc:android-quickstart-guide). 

To enable the CleverTap IDC, complete the following step. Add the snippet below in your AndroidManifest.xml file between the <application></application> tags.
[block:code]
{
  "codes": [
    {
      "code": "<meta-data \nandroid:name=\"CLEVERTAP_REGION\" \nandroid:value=\"in\"/>",
      "language": "java"
    }
  ]
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a887f33-android1.png",
        "android1.png",
        1213,
        412,
        "#363738"
      ],
      "border": true
    }
  ]
}
[/block]
## iOS 

To integrate CleverTap in your iOS application, follow [this quickstart guide](doc:ios-quickstart-guide).

To enable the CleverTap IDC, complete the following step. In your applications’s Info.plist file, add a new key under the Information Property List with the type string. The name for key is CleverTapRegion and the value is 'in'. 
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f8050bc-ios1.png",
        "ios1.png",
        906,
        454,
        "#f0ecec"
      ],
      "border": true
    }
  ]
}
[/block]
## Web

To integrate CleverTap in your website, follow [this quickstart guide](doc:web-quickstart-guide).

To enable the CleverTap IDC, complete the following step. In line 2 of the CleverTap JavaScript snippet, you will need to add a new element to the clevertap object. The new element's key is region and value is  'in'.
[block:code]
{
  "codes": [
    {
      "code": "<script type=\"text/javascript\">\n     var clevertap = {event:[], profile:[], region : 'in', account:[], onUserLogin:[], notifications:[]};\n clevertap.account.push({\"id\": \"CLEVERTAP_ACCOUNT_ID\"});\n (function () {\n var wzrk = document.createElement('script');\n wzrk.type = 'text/javascript';\n wzrk.async = true;\n wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';\n var s = document.getElementsByTagName('script')[0];\n s.parentNode.insertBefore(wzrk, s);\n  })();\n</script>",
      "language": "javascript"
    }
  ]
}
[/block]
## API

To get started with the CleverTap API, please review [this page](https://developer.clevertap.com/docs/api-overview).

To enable the CleverTap IDC for usage with our API, complete the following step. Replace the default base URL (api.clevertap.com) with in.api.clevertap.com.

Here is a cURL example showing how to make an API call to CleverTap's Indian data center.
[block:code]
{
  "codes": [
    {
      "code": "curl \"https://in.api.clevertap.com/1/events.json?cursor=CURSOR\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n -H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl"
    }
  ]
}
[/block]