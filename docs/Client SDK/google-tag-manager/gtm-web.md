---
title: CleverTap GTM Web Integration
excerpt: Learn how to send your website events to CleverTap via GTM
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

This SDK integration sends data from your website to your CleverTap account.

# Integration

The following are the four major steps to perform this integration:

1. [Initialize the SDK](doc:gtm-web#initialize-the-sdk).
2. [Call CleverTap Methods](doc:gtm-web#call-clevertap-methods).
3. [Create Variables for Passing Parameters](doc:gtm-web#create-variables-for-passing-parameters).
4. [Update Values of Variables before CleverTap Methods are Triggered](doc:gtm-web#update-values-of-variables-before-clevertap-methods-are-triggered)

## Initialize the SDK

Ensure you integrate the [Web SDK](doc:web-quickstart-guide#section-step-1-add-the-clever-tap-java-script-library-to-your-website) from the developer docs to all your web pages. One way to achieve this is by creating custom HTML tags. Refer to the example shown in the following figure:

<Image align="center" alt="Custom HTML Tags" border={true} caption="Custom HTML Tags" title="Custom HTML Tags" src="https://files.readme.io/013d66b-web_1.png" />

## Call CleverTap Methods

To implement CleverTap methods such as OnUserLogin(), event.push(), or profile.push(), you can create custom HTML tags with variables and define the appropriate triggers. Refer to the example shown in the following figure:

<Image align="center" alt="Call CleverTap Methods" border={true} caption="Call CleverTap Methods" title="Call CleverTap Methods" src="https://files.readme.io/10dce99-web_2.png" />

## Create Variables for Passing Parameters

You can use the data layer to pass variables to CleverTap functions. For instance, for the above custom HTML to work, you must create the variables first in your GTM container.

<Image align="center" alt="User-Defined Variables" border={true} caption="User-Defined Variables" title="User-Defined Variables" src="https://files.readme.io/4fd71dc-WEB_2.png" />

## Update Values of Variables before CleverTap Methods are Triggered

From your code, you must push these variables to the GTM dataLayer. For working with the GTM data layer, refer to the official [Google documentation](https://developers.google.com/tag-manager/devguide#datalayer).

For example, to update the email variable in the data layer, you must call that in your code.

```java
dataLayer.push({'email': 'satyajeet@clevertap.com'});
```
