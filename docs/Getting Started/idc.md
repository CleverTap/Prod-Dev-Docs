---
title: Configuration Guide for CleverTap Regions
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

By default, we process and store customer data in our EU data center. For CleverTap customers based in India or any other region who need in-region storage and processing for regulatory compliance, we offer U.S, India, Singapore & South Korea as a data center. 

 To enable processing and storage in our regional data center, please complete these two steps:
  * Contact [CleverTap](https://clevertap.com/contact-us/) to enable this feature for your account.
  * Complete the technical configuration steps described below.

# Configuration Steps to Enable CleverTap Region

For each platform that you are using CleverTap with, there is a quick step you need to complete, to enable usage of CleverTap's regional data center.

## Android

To integrate CleverTap in your Android application, follow the [Android Quick Start Guide](doc:android-quickstart-guide). 

To enable the CleverTap Region, complete the following step. Add the following snippet according to your location in your AndroidManifest.xml file between the <application></application> tags.
[block:code]
{
  "codes": [
    {
      "code": "<meta-data \nandroid:name=\"CLEVERTAP_REGION\" \nandroid:value=\"in1\"/>",
      "language": "java",
      "name": "India Region"
    },
    {
      "code": "<meta-data \nandroid:name=\"CLEVERTAP_REGION\" \nandroid:value=\"us1\"/>",
      "language": "text",
      "name": "U.S Region"
    },
    {
      "code": "<meta-data \nandroid:name=\"CLEVERTAP_REGION\" \nandroid:value=\"sg1\"/>",
      "language": "javascript",
      "name": "Singapore Region"
    },
    {
      "code": "<meta-data \nandroid:name=\"CLEVERTAP_REGION\" \nandroid:value=\"sk1\"/>",
      "language": "javascript",
      "name": "South Korea Region"
    }
  ]
}
[/block]
## iOS 

To integrate CleverTap in your iOS application, follow the [iOS Quick Start Guide](doc:ios-quickstart-guide).

To enable the CleverTap Region, complete the following step. In your applications’ Info.plist file, add a new key under the Information Property List with the type string. The name for key is CleverTapRegion and the value is `in1` for **India**, `sg1` for **Singapore**, `sk1` for **South Korea**, and `us1` for **U.S**.

## KaiOS 

To integrate CleverTap in your KaiOS application, follow the [KaiOS integration guide](https://developer.clevertap.com/docs/kaios).

To enable the CleverTap region, complete the following step. In your applications’ Info.plist file, add a new key under the Information Property List with the type string. The name for key is CleverTapRegion and the value is `in1` for **India**, `sg1` for **Singapore**, `sk1` for **South Korea**, and `us1` for **U.S**.
[block:code]
{
  "codes": [
    {
      "code": "clevertap.init(\"<Project ID>\", \"in1\")",
      "language": "javascript",
      "name": "Indian Region"
    },
    {
      "code": "clevertap.init(\"<Project ID>\", \"sg1\")",
      "language": "javascript",
      "name": "Singapore Region"
    },
    {
      "code": "clevertap.init(\"<Project ID>\", \"sk1\")",
      "language": "javascript",
      "name": "South Korea Region"
    },
    {
      "code": "clevertap.init(\"<Project ID>\", \"us1\")",
      "language": "javascript",
      "name": "U.S. Region"
    }
  ]
}
[/block]

## Web

To integrate CleverTap in your website, follow the [Web Quick Start Guide](doc:web-quickstart-guide).

To enable the CleverTap Region, complete the following step. In line 2 of the CleverTap JavaScript snippet, you will need to add a new element to the clevertap object. The new element's key is region and value is `in1` for **India**, `sg1` for **Singapore**, `sk1` for **South Korea**, and `us1` for **U.S **.
[block:code]
{
  "codes": [
    {
      "code": "<script type=\"text/javascript\">\n     var clevertap = {event:[], profile:[], region : 'in1', account:[], onUserLogin:[], notifications:[], privacy:[]};\n clevertap.account.push({\"id\": \"CLEVERTAP_ACCOUNT_ID\"});\n (function () {\n var wzrk = document.createElement('script');\n wzrk.type = 'text/javascript';\n wzrk.async = true;\n wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';\n var s = document.getElementsByTagName('script')[0];\n s.parentNode.insertBefore(wzrk, s);\n  })();\n</script>",
      "language": "javascript",
      "name": "India Region"
    },
    {
      "code": "<script type=\"text/javascript\">\n     var clevertap = {event:[], profile:[], region : 'sg1', account:[], onUserLogin:[], notifications:[],privacy:[]};\n clevertap.account.push({\"id\": \"CLEVERTAP_ACCOUNT_ID\"});\n (function () {\n var wzrk = document.createElement('script');\n wzrk.type = 'text/javascript';\n wzrk.async = true;\n wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';\n var s = document.getElementsByTagName('script')[0];\n s.parentNode.insertBefore(wzrk, s);\n  })();\n</script>",
      "language": "javascript",
      "name": "Singapore Region"
    },
    {
      "code": "<script type=\"text/javascript\">\n     var clevertap = {event:[], profile:[], region : 'sk1', account:[], onUserLogin:[], notifications:[],privacy:[]};\n clevertap.account.push({\"id\": \"CLEVERTAP_ACCOUNT_ID\"});\n (function () {\n var wzrk = document.createElement('script');\n wzrk.type = 'text/javascript';\n wzrk.async = true;\n wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';\n var s = document.getElementsByTagName('script')[0];\n s.parentNode.insertBefore(wzrk, s);\n  })();\n</script>",
      "language": "javascript",
      "name": "South Korea Region"
    },
    {
      "code": "<script type=\"text/javascript\">\n     var clevertap = {event:[], profile:[], region : 'us1', account:[], onUserLogin:[], notifications:[], privacy:[]};\n clevertap.account.push({\"id\": \"CLEVERTAP_ACCOUNT_ID\"});\n (function () {\n var wzrk = document.createElement('script');\n wzrk.type = 'text/javascript';\n wzrk.async = true;\n wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';\n var s = document.getElementsByTagName('script')[0];\n s.parentNode.insertBefore(wzrk, s);\n  })();\n</script>",
      "language": "javascript",
      "name": "U.S Region"
    }
  ]
}
[/block]
## API

To get started with the CleverTap API, please review [this page](https://developer.clevertap.com/docs/api-overview).

To enable the CleverTap Region for usage with our API, complete the following step. Replace the default base URL (api.clevertap.com) with` in1.api.clevertap.com` for **India**, `sg1.api.clevertap.com` for **Singapore**, `sk1.api.clevertap.com` for **South Korea**, and `us1.api.clevertap.com` for **U.S**.

Here is a cURL example showing how to make an API call to CleverTap's Indian data center.
[block:code]
{
  "codes": [
    {
      "code": "curl \"https://in1.api.clevertap.com/1/events.json?cursor=CURSOR\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl",
      "name": "India Region"
    },
    {
      "code": "curl \"https://sg1.api.clevertap.com/1/events.json?cursor=CURSOR\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl",
      "name": "Singapore Region"
    },
    {
      "code": "curl \"https://sk1.api.clevertap.com/1/events.json?cursor=CURSOR\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl",
      "name": "South Korea Region"
    },
    {
      "code": "curl \"https://us1.api.clevertap.com/1/events.json?cursor=CURSOR\" \\\n-H \"X-CleverTap-Account-Id: ACCOUNT_ID\" \\\n-H \"X-CleverTap-Passcode: PASSCODE\" \\\n-H \"Content-Type: application/json\"",
      "language": "curl",
      "name": "U.S Region"
    }
  ]
}
[/block]