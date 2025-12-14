---
title: Android Custom Proxy Domain
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Custom Proxy Domain

A custom proxy domain allows you to proxy all events raised from the CleverTap SDK through your required domain. If you want to use your application server, use a proxy domain to handle and/or relay CleverTap events. We provide custom domain support for Push Impression event handling.

## Why is it useful?

Using  a custom proxy domain has the following benefits:

1. Use your own domain to collect data.
2. Have fine control over filtering, auditing, and/or cleaning data before sending it to CleverTap.
3. Turn event collection on or off across all platforms quickly.
4. If you want to use your own application server, you can use a proxy domain to handle and/or relay CleverTap events.

You must have a domain and access to DNS site settings to set up the proxy domain. To create an AWS Certificate and CloudFront distribution for the proxy domain, perform the steps listed in the following sections, and then integrate CleverTap SDK with your proxy domain configuration.

## AWS Certificate Manager

To create a certificate using ACM in the required region:

1. Go to *Certificate Manager* in AWS.
2. Click **Request Certificate** and select *Request a public certificate*.

<Image title="Request a Public Certificate" alt={2793} align="center" border={true} src="https://files.readme.io/f02e9fa-Screenshot_2022-09-21_at_12.42.43_AM.png">
  Request a Public Certificate
</Image>

3. Add the proxy domain name that you want to use. This proxy domain name relays events to CleverTap's origin domain. For example, *subdomain.domain.com*. 

<Image title="Add the Proxy Domain Name" alt={2782} align="center" border={true} src="https://files.readme.io/1a223f1-Screenshot_2022-09-21_at_12.44.19_AM.png">
  Add the Proxy Domain Name
</Image>

4. Select a validation method based on your domain account permission.

<Image title="Select Validation Method" alt={2786} align="center" border={true} src="https://files.readme.io/0bf22e6-Screenshot_2022-09-21_at_12.44.56_AM.png">
  Select Validation Method
</Image>

5. After review, confirm and request the certificate.
6. Copy the *CNAME record* from AWS Certificate Manager (ACM) details to add it to your domain registrar settings.

<Image title="CNAME Record in AWS Certificate Manager (ACM)" alt={2794} align="center" border={true} src="https://files.readme.io/237e869-Screenshot_2022-09-21_at_12.45.40_AM.png">
  CNAME Record in AWS Certificate Manager (ACM)
</Image>

7. Add the CNAME record in DNS Settings as shown below:

<Image title="Add CNAME in DNS" alt={2795} align="center" border={true} src="https://files.readme.io/ee9b7a2-Screenshot_2022-09-21_at_12.46.03_AM.png">
  Add CNAME in DNS
</Image>

The ACM Validation status should update from `Pending to Success` in some time. This certificate is used for creating CloudFront distribution.

## AWS CloudFront Distribution

### Origin

1. Enter Origin Domain: **eu1.clevertap-prod.com**.
2. Select Protocol: *HTTPS*.

<Image title="Configure Origin Domain and Protocol" alt={1436} align="center" border={true} src="https://files.readme.io/af56f72-custom_proxy_domain_1.png">
  Configure Origin Domain and Protocol
</Image>

### Default Cache Behaviour:

1. Select *Redirect HTTP to HTTPS* as the *Viewer* protocol policy.
2. Select *GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE* as the *Allowed HTTP methods*. 

<Image title="Configure Viewer" alt={1280} align="center" border={true} src="https://files.readme.io/4946727-custom_proxy_domain_2.png">
  Configure Viewer Section
</Image>

3. Under *Cache key and origin requests*, select *Cache policy and origin request policy (recommended)* option and select *CachingOptimized* from the list.

<Image title="Configure Cache Key and Origin Requests" alt={1405} align="center" border={true} src="https://files.readme.io/99e3d54-custom_proxy_domain_3.png">
  Configure Cache Key and Origin Requests
</Image>

### Settings

1. Add proxy name in *Alternate domain name (CNAME) - optional*: *analytics.sdktesting.xyz*. This is the same proxy used earlier to create the AWS certificate.

2. Select the certificate created earlier in *Custom SSL certificate - optional*. 

<Image title="Configure Settings Section" alt={1202} align="center" border={true} src="https://files.readme.io/66adb32-custom_proxy_domain_4.png">
  Configure Alternate Domain Name (CNAME) and Custom SSL Certificate
</Image>

3. Select the recommended option under *Security policy*.

<Image title="Configure Security Policy" alt={1421} align="center" border={true} src="https://files.readme.io/ccbf668-custom_proxy_domain_5.png">
  Configure Security Policy
</Image>

4. Keep the other settings at default selection and click *Create Distribution*. 

<Image title="Create Distribution" alt={1405} align="center" border={true} src="https://files.readme.io/be108ef-custom_proxy_domain_6.png">
  Create Distribution
</Image>

5. Add another CNAME in DNS Settings to point your subdomain (analytics in this case) to the CloudFront distribution, as shown in the following image:

<Image title="Add CNAME in DNS Settings" alt={1912} align="center" border={true} src="https://files.readme.io/a47b677-7.png">
  Add CNAME in DNS Settings
</Image>

6. After the CloudFront distribution is deployed, enter the *proxy domain* in the browser address to check if the settings are working correctly. 

# Integrate CleverTap to use Proxy Domain

## Using Manifest File

1. Add your CleverTap Account credentials in the Manifest file against the `CLEVERTAP_ACCOUNT_ID` and `CLEVERTAP_TOKEN` keys.
2. Add the `CLEVERTAP_PROXY_DOMAIN` key with the proxy domain value for handling events through the custom proxy domain.
3. Add the `CLEVERTAP_SPIKY_PROXY_DOMAIN` key with the spiky proxy domain value for handling push impression events.

```xml
<meta-data
    android:name="CLEVERTAP_ACCOUNT_ID"
    android:value="YOUR_ACCOUNT_ID" />
<meta-data
    android:name="CLEVERTAP_TOKEN"
    android:value="YOUR_ACCOUNT_TOKEN" />
<meta-data
    android:name="CLEVERTAP_PROXY_DOMAIN"
    android:value="YOUR_PROXY_DOMAIN" />   <!-- e.g., analytics.sdktesting.xyz -->
<meta-data
    android:name="CLEVERTAP_SPIKY_PROXY_DOMAIN"
    android:value="YOUR_SPIKY_PROXY_DOMAIN" /> <!-- e.g., spiky-analytics.sdktesting.xyz -->
```

4. Use CleverTap's defaultInstance to raise the custom events.

```java
clevertapDefaultInstance.pushEvent("Product viewed");
```
```kotlin
clevertapDefaultInstance?.pushEvent("Product viewed")
```

## Using `changeCredentials` API

1. Call the `changeCredentials(String accountID, String token, String proxyDomain, String spikyProxyDomain)` method.

```java Java
CleverTapAPI.changeCredentials(
     "YOUR_CLEVERTAP_ACCOUNT_ID",
     "YOUR_CLEVERTAP_TOKEN",
     "YOUR_PROXY_DOMAIN",       //e.g., analytics.sdktesting.xyz
     "YOUR_SPIKY_PROXY_DOMAIN"  //e.g., spiky-analytics.sdktesting.xyz
);
```
```kotlin Kotlin
CleverTapAPI.changeCredentials(
     "YOUR CLEVERTAP ACCOUNT ID",
     "YOUR CLEVERTAP TOKEN",
     "YOUR PROXY DOMAIN",       //e.g., analytics.sdktesting.xyz
     "YOUR SPIKY PROXY DOMAIN"  //e.g., spiky-analytics.sdktesting.xyz
)
```

2. Use CleverTap's defaultInstance to log the events.

```java
clevertapDefaultInstance.pushEvent("Product viewed");
```
```kotlin
clevertapDefaultInstance?.pushEvent("Product viewed")
```

## Using CleverTap's Additional Instance

1. Create an instance of `CleverTapInstanceConfig` class :

```java
CleverTapInstanceConfig cleverTapInstanceConfig = CleverTapInstanceConfig.createInstance(
        applicationContext,
        "YOUR_CLEVERTAP_ACCOUNT_ID", 
        "YOUR_CLEVERTAP_TOKEN"       
);
```
```kotlin
val cleverTapInstanceConfig = CleverTapInstanceConfig.createInstance(
    applicationContext,
    "YOUR_CLEVERTAP_ACCOUNT_ID", 
    "YOUR_CLEVERTAP_TOKEN"       
)
```

2. Set the proxy domain(s) with the `CleverTapInstanceConfig` object you created earlier.

```java
cleverTapInstanceConfig.setProxyDomain("YOUR_PROXY_DOMAIN");   //e.g., analytics.sdktesting.xyz
cleverTapInstanceConfig.setSpikyProxyDomain("YOUR_SPIKY_PROXY_DOMAIN"); //e.g., spiky-analytics.sdktesting.xyz
```
```kotlin
cleverTapInstanceConfig.proxyDomain = "YOUR_PROXY_DOMAIN"
cleverTapInstanceConfig.spikyProxyDomain = "YOUR_SPIKY_PROXY_DOMAIN"
```

3. Use CleverTap's defaultInstance to log the events.

```java
clevertapDefaultInstance.pushEvent("Product viewed");
```
```kotlin
clevertapDefaultInstance?.pushEvent("Product viewed")
```

## Debug and Test

After integration, the logged events will be accessible on the CleverTap dashboard. Use `CleverTapAPI.setDebugLevel(VERBOSE)` to debug network requests and responses from the CleverTap SDK.
