---
title: Configuration Guide for CleverTap Regions
excerpt: Understand how to configure CleverTap data center regions.
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

Data centers help store, process, or distribute large amounts of data. We offer data processing and storage flexibility in **Europe**, **India**, the **United States**, **Singapore** for our customers who need in-region storage and processing for regulatory compliance.

In addition to these regions, CleverTap has recently set up the infrastructure in **Indonesia** and **Middle East (UAE)** AWS regions to help Indonesia and Middle East customers in the Fintech and Healthtech space to have their data processed and stored in these regions.

# Enable CleverTap Region

To enable the processing and storage in our regional data center, perform the following two steps:

1. Contact [CleverTap](https://help.clevertap.com/hc/en-us/requests/new) to enable your account's data center region feature.
2. Complete the configuration steps for each platform you use CleverTap with.
   * [Android](doc:idc#android)
   * [iOS](doc:idc#ios)
   * [KaiOS](doc:idc#kaios)
   * [Web](doc:idc#web)
   * [API](doc:idc#api)

## Android

To integrate CleverTap into your Android application, refer to the [Android Quick Start Guide](doc:android-quickstart-guide). 

To enable the CleverTap region, add the following snippet in your `AndroidManifest.xml` file between the `<application></application>` tags:

```java India
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="in1"/>
```
```javascript Singapore
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="sg1"/>
```
```java United States
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="us1"/>
```
```java Indonesia
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="aps3"/>
```
```java Middle East (UAE)
<meta-data 
android:name="CLEVERTAP_REGION" 
android:value="mec1"/>
```
```text Europe (default region)
No action needed.
```

## iOS

To integrate CleverTap into your iOS application, refer to the [iOS Quick Start Guide](doc:ios-quickstart-guide).

To enable the CleverTap region, in your `Info.plist` file, add a new `CleverTapRegion` key of type string under the `Information Property List`.

<Image title="Add the CleverTapRegion Key in iOS" alt={1316} align="center" border={true} src="https://files.readme.io/594c9b1-2023-02-01_10-48-33.png">
  Add the 'CleverTapRegion' Key
</Image>

Refer the following table for `CleverTapRegion` key values:

| Region                  | Value                                          |
| :---------------------- | :--------------------------------------------- |
| India                   | `in1`                                          |
| Singapore               | `sg1`                                          |
| United States (U.S)     | `us1`                                          |
| Indonesia               | `aps3`                                         |
| Middle East (UAE)       | `mec1`                                         |
| Europe (default region) | You can skip adding the `CleverTapRegion` key. |

## KaiOS

To integrate CleverTap into your KaiOS application, refer to the [KaiOS Integration Guide](https://developer.clevertap.com/docs/kaios).

To enable the CleverTap region, use the following code: 

```javascript India
clevertap.init("<Project ID>", "in1")
```
```javascript Singapore
clevertap.init("<Project ID>", "sg1")
```
```javascript United States
clevertap.init("<Project ID>", "us1")
```
```javascript Indonesia
clevertap.init("<Project ID>", "aps3")
```
```javascript Middle East (UAE)
clevertap.init("<Project ID>", "mec1")
```
```text Europe (default region)
No action needed.
```

## Web

To integrate CleverTap into your website, refer to the [Web Quick Start Guide](doc:web-quickstart-guide).

To enable the CleverTap region, add a `region` element to the `clevertap` object as shown in the following code (second line):

```javascript India
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'in1', account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript Singapore
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'sg1', account:[], onUserLogin:[], notifications:[],privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript United States
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'us1', account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript Indonesia
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'aps3', account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript Middle East (UAE)
<script type="text/javascript">
     var clevertap = {event:[], profile:[], region : 'mec1', account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```
```javascript Europe (default region)
<script type="text/javascript">
     var clevertap = {event:[], profile:[], account:[], onUserLogin:[], notifications:[], privacy:[]};
 clevertap.account.push({"id": "CLEVERTAP_ACCOUNT_ID"});
 (function () {
 var wzrk = document.createElement('script');
 wzrk.type = 'text/javascript';
 wzrk.async = true;
 wzrk.src = ('https:' == document.location.protocol ? 'https://d2r1yp2w7bby2u.cloudfront.net' : 'http://static.clevertap.com') + '/js/clevertap.min.js';
 var s = document.getElementsByTagName('script')[0];
 s.parentNode.insertBefore(wzrk, s);
  })();
</script>
```

Refer the following table for `region` key values:

| Region                  | Value                                 |
| :---------------------- | :------------------------------------ |
| India                   | `in1`                                 |
| Singapore               | `sg1`                                 |
| United States (U.S)     | `us1`                                 |
| Indonesia               | `aps3`                                |
| Middle East (UAE)       | `mec1`                                |
| Europe (default region) | You can skip adding the `region` key. |

## API

To get started with the CleverTap APIs, refer to [API Overview](doc:api-overview).

The following table lists the API endpoints based on your data center region:

| Region                  | API Endpoint             | CleverTap Dashboard URL                                                                              |
| :---------------------- | :----------------------- | :--------------------------------------------------------------------------------------------------- |
| India                   | `in1.api.clevertap.com`  | [https://in1.dashboard.clevertap.com/login.html](https://in1.dashboard.clevertap.com/login.html#/)   |
| Singapore               | `sg1.api.clevertap.com`  | [https://sg1.dashboard.clevertap.com/login.html](https://sg1.dashboard.clevertap.com/login.html#/)   |
| United States           | `us1.api.clevertap.com`  | [https://us1.dashboard.clevertap.com/login.html](https://us1.dashboard.clevertap.com/login.html#/)   |
| Indonesia               | `aps3.api.clevertap.com` | [https://aps3.dashboard.clevertap.com/login.html](https://aps3.dashboard.clevertap.com/login.html#/) |
| Middle East (UAE)       | `mec1.api.clevertap.com` | [https://mec1.dashboard.clevertap.com/login.html](https://mec1.dashboard.clevertap.com/login.html#/) |
| Europe (default region) | `api.clevertap.com`      | [https://eu1.dashboard.clevertap.com/login.html](https://eu1.dashboard.clevertap.com/login.html#/)   |

Here is a cURL example showing how to make an API call to CleverTap's data center regions:

```curl India
curl "https://in1.api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```curl Singapore
curl "https://sg1.api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```curl Unites States
curl "https://us1.api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```curl Indonesia
curl "https://aps3.api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```curl Middle East (UAE)
curl "https://mec1.api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
```curl Europe (default region)
curl "https://api.clevertap.com/1/events.json?cursor=CURSOR" \
-H "X-CleverTap-Account-Id: ACCOUNT_ID" \
-H "X-CleverTap-Passcode: PASSCODE" \
-H "Content-Type: application/json"
```
