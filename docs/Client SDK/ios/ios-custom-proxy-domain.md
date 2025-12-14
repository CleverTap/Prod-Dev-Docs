---
title: iOS Custom Proxy Domain
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

1. Go to _Certificate Manager_ in AWS.
2. Click **Request Certificate** and select _Request a public certificate_.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f02e9fa-Screenshot_2022-09-21_at_12.42.43_AM.png",
        "Request a Public Certificate",
        2793
      ],
      "align": "center",
      "border": true,
      "caption": "Request a Public Certificate"
    }
  ]
}
[/block]



3. Add the proxy domain name that you want to use. This proxy domain name relays events to CleverTap's origin domain. For example, _subdomain.domain.com_. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1a223f1-Screenshot_2022-09-21_at_12.44.19_AM.png",
        "Add the Proxy Domain Name",
        2782
      ],
      "align": "center",
      "border": true,
      "caption": "Add the Proxy Domain Name"
    }
  ]
}
[/block]



4. Select a validation method based on your domain account permission.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0bf22e6-Screenshot_2022-09-21_at_12.44.56_AM.png",
        "Select Validation Method",
        2786
      ],
      "align": "center",
      "border": true,
      "caption": "Select Validation Method"
    }
  ]
}
[/block]



5. After review, confirm and request the certificate.
6. Copy the _CNAME record_ from AWS Certificate Manager (ACM) details to add it to your domain registrar settings.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/237e869-Screenshot_2022-09-21_at_12.45.40_AM.png",
        "CNAME Record in AWS Certificate Manager (ACM)",
        2794
      ],
      "align": "center",
      "border": true,
      "caption": "CNAME Record in AWS Certificate Manager (ACM)"
    }
  ]
}
[/block]



7. Add the CNAME record in DNS Settings as shown below:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ee9b7a2-Screenshot_2022-09-21_at_12.46.03_AM.png",
        "Add CNAME in DNS",
        2795
      ],
      "align": "center",
      "border": true,
      "caption": "Add CNAME in DNS"
    }
  ]
}
[/block]



The ACM Validation status should update from `Pending to Success` in some time. This certificate is used for creating CloudFront distribution.

## AWS CloudFront Distribution

### Origin

1. Enter Origin Domain: **eu1.clevertap-prod.com**.
2. Select Protocol: _HTTPS_.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/af56f72-custom_proxy_domain_1.png",
        "Configure Origin Domain and Protocol",
        1436
      ],
      "align": "center",
      "border": true,
      "caption": "Configure Origin Domain and Protocol"
    }
  ]
}
[/block]



### Default Cache Behaviour:

1. Select _Redirect HTTP to HTTPS_ as the _Viewer_ protocol policy.
2. Select _GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE_ as the _Allowed HTTP methods_. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4946727-custom_proxy_domain_2.png",
        "Configure Viewer",
        1280
      ],
      "align": "center",
      "border": true,
      "caption": "Configure Viewer Section"
    }
  ]
}
[/block]



3. Under _Cache key and origin requests_, select _Cache policy and origin request policy (recommended)_ option and select _CachingOptimized_ from the list.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/99e3d54-custom_proxy_domain_3.png",
        "Configure Cache Key and Origin Requests",
        1405
      ],
      "align": "center",
      "border": true,
      "caption": "Configure Cache Key and Origin Requests"
    }
  ]
}
[/block]



### Settings

1. Add proxy name in _Alternate domain name (CNAME) - optional_: _analytics.sdktesting.xyz_. This is the same proxy used earlier to create the AWS certificate.

2. Select the certificate created earlier in _Custom SSL certificate - optional_. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/66adb32-custom_proxy_domain_4.png",
        "Configure Settings Section",
        1202
      ],
      "align": "center",
      "border": true,
      "caption": "Configure Alternate Domain Name (CNAME) and Custom SSL Certificate"
    }
  ]
}
[/block]



3. Select the recommended option under _Security policy_.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ccbf668-custom_proxy_domain_5.png",
        "Configure Security Policy",
        1421
      ],
      "align": "center",
      "border": true,
      "caption": "Configure Security Policy"
    }
  ]
}
[/block]



4. Keep the other settings at default selection and click _Create Distribution_. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/be108ef-custom_proxy_domain_6.png",
        "Create Distribution",
        1405
      ],
      "align": "center",
      "border": true,
      "caption": "Create Distribution"
    }
  ]
}
[/block]



5. Add another CNAME in DNS Settings to point your subdomain (analytics in this case) to the CloudFront distribution, as shown in the following image:

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a47b677-7.png",
        "Add CNAME in DNS Settings",
        1912
      ],
      "align": "center",
      "border": true,
      "caption": "Add CNAME in DNS Settings"
    }
  ]
}
[/block]



6. After the CloudFront distribution is deployed, enter the _proxy domain_ in the browser address to check if the settings are working correctly. 

# Integrate CleverTap to use Proxy Domain

## Using `Autointegrate` API

1. Add your CleverTap credentials in the _Info.plist_ file of your application. 
2. Insert the _Account ID_ and _Account Token_ values from your CleverTap account against the `CleverTapAccountID` and `CleverTapToken` keys.
3. Add the `CleverTapProxyDomain` key with the proxy domain value for handling events through the custom proxy domain.
4. Add the `CleverTapSpikyProxyDomain` key with proxy domain value for handling push impression events.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/898f686-8.png",
        "Integrate CleverTap to Use Proxy Domain",
        942
      ],
      "align": "center",
      "border": true,
      "caption": "Integrate CleverTap to Use Proxy Domain"
    }
  ]
}
[/block]



5. Import the CleverTap SDK in your _AppDelegate_ file and call CleverTap's `autoIntegrate` method in the `didFinishLaunchingWithOptions:` method.

```swift
CleverTap.autoIntegrate()
```



6. Use CleverTap's `sharedInstance` to log events.

```swift
CleverTap.sharedInstance()?.recordEvent("Product viewed")
```



## Using `setCredentials` API

1. Import the CleverTap SDK and call either the `setCredentialsWithAccountID:token:proxyDomain:` method or the `setCredentialsWithAccountID:token:proxyDomain:spikyProxyDomain:` method.

```swift setCredentialsWithAccountID:token:proxyDomain:
CleverTap.setCredentialsWithAccountID(accountID: ACCOUNT_ID, token: ACCOUNT_TOKEN, proxyDomain: "analytics.sdktesting.xyz")
```
```swift setCredentialsWithAccountID:token:proxyDomain:spikyProxyDomain:
CleverTap.setCredentialsWithAccountID(accountID: ACCOUNT_ID, token: ACCOUNT_TOKEN, proxyDomain: "analytics.sdktesting.xyz", spikyProxyDomain: "spiky-analytics.sdktesting.xyz")
```



2. Use CleverTap's `sharedInstance` to log events.

```swift
CleverTap.sharedInstance()?.recordEvent("Product viewed")
```



## Create CleverTap's Additional Instance

1. Call `CleverTapInstanceConfig` constructor with either of the following APIs:

```swift setCredentialsWithAccountID:token:proxyDomain:
let ctConfig = CleverTapInstanceConfig(accountId: ACCOUNT_ID, accountToken: ACCOUNT_TOKEN, proxyDomain: "analytics.sdktesting.xyz")
```
```swift setCredentialsWithAccountID:token:proxyDomain:spikyProxyDomain:
let ctConfig = CleverTapInstanceConfig(accountId: ACCOUNT_ID, accountToken: ACCOUNT_TOKEN, proxyDomain: "analytics.sdktesting.xyz", spikyProxyDomain: "spiky-analytics.sdktesting.xyz")
```



2. Start the CleverTap instance by calling the `CleverTap.instance(with:)` method with the `CleverTapInstanceConfig` object you created earlier.

```swift
let cleverTapProxyInstance = CleverTap.instance(with: ctConfig)
```



3. Use this instance to log events.

```swift
cleverTapProxyInstance.recordEvent("Product viewed")
```



## Debug and Test

After integration, the logged events are available on the CleverTap dashboard. Use CleverTapInstanceConfig's logLevel instance variable or `CleverTap.setDebugLevel(1)` to debug requests and response.