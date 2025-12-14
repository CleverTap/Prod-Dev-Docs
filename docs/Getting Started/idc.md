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

```java India Region
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="in1"/>
```
```text U.S Region
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="us1"/>
```
```javascript Singapore Region
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="sg1"/>
```
```javascript South Korea Region
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="sk1"/>
```

## iOS

To integrate CleverTap in your iOS application, follow the [iOS Quick Start Guide](doc:ios-quickstart-guide).

To enable the CleverTap Region, complete the following step. In your applications’ Info.plist file, add a new key under the Information Property List with the type string. The name for key is CleverTapRegion and the value is `in1` for **India**, `sg1` for **Singapore**, `sk1` for **South Korea**, and `us1` for **U.S**.

## KaiOS

To integrate CleverTap in your KaiOS application, follow the [KaiOS integration guide](https://developer.clevertap.com/docs/kaios).

To enable the CleverTap region, complete the following step. In your applications’ Info.plist file, add a new key under the Information Property List with the type string. The name for key is CleverTapRegion and the value is `in1` for **India**, `sg1` for **Singapore**, `sk1` for **South Korea**, and `us1` for **U.S**.

```javascript Indian Region
clevertap.init("<Project ID>", "in1")
```
```javascript Singapore Region
clevertap.init("<Project ID>", "sg1")
```
```javascript South Korea Region
clevertap.init("<Project ID>", "sk1")
```
```javascript U.S. Region
clevertap.init("<Project ID>", "us1")
```

## Web

To integrate CleverTap in your website, follow the [Web Quick Start Guide](doc:web-quickstart-guide).

To enable the CleverTap Region, complete the following step. In line 2 of the CleverTap JavaScript snippet, you will need to add a new element to the clevertap object. The new element's key is region and value is `in1` for **India**, `sg1` for **Singapore**, `sk1` for **South Korea**, and `us1` for **U.S** .

```javascript India Region
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'in1', account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript Singapore Region
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'sg1', account:[], onUserLogin:[], notifications:[],privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript South Korea Region
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'sk1', account:[], onUserLogin:[], notifications:[],privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript U.S Region
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'us1', account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/a.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

## API

To get started with the CleverTap API, please review [this page](https://developer.clevertap.com/docs/api-overview).

To enable the CleverTap Region for usage with our API, complete the following step. Replace the default base URL (api.clevertap.com) with` in1.api.clevertap.com` for **India**, `sg1.api.clevertap.com` for **Singapore**, `sk1.api.clevertap.com` for **South Korea**, and `us1.api.clevertap.com` for **U.S**.

Here is a cURL example showing how to make an API call to CleverTap's Indian data center.

```curl India Region
curl "https://in1.api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```curl Singapore Region
curl "https://sg1.api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```curl South Korea Region
curl "https://sk1.api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```curl U.S Region
curl "https://us1.api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
