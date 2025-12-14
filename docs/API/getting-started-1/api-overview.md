---
title: API Overview
excerpt: Understand the basic overview of CleverTap APIs.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
We provide a REST API that lets you create, read, update, and delete data in CleverTap.

This page describes our authentication model, API endpoints, webhook feature, server-side SDKs, and error handling.

# Make Your First API Call

The best place to get started with the CleverTap API is the [API Quick Start Guide](doc:api-quickstart-guide).

# API Use Cases

Here are some use cases with our API:

- **Uploading user profile and event data from external systems to CleverTap.** For example, if a user contacted your call center about a product issue. You can store this as an event on that user’s profile, which allows you to send a follow-up note about the conversation as an email/SMS or push notification. Check out the [Upload User Profiles](https://developer.clevertap.com/docs/upload-user-profiles-api) and the [Upload Events](https://developer.clevertap.com/docs/upload-events-api) APIs to learn more. 
- **Getting user profile and event data from CleverTap to enrich customer information in external systems.** For example, if a salesperson wants to know what products a user has viewed before contacting calling them. You can get this information from CleverTap and include it in your CRM within that customer’s account record. Check out the [Get User Profiles](https://developer.clevertap.com/docs/get-user-profiles-api) and the [Get Events](https://developer.clevertap.com/docs/get-events-api) APIs to learn more.
- **Getting user metrics from CleverTap for analysis in external systems.** For example, let’s say you have an internal dashboard where you track all your marketing metrics. Our API enables you to include user, event, and campaign data for your app on that dashboard. Check out the [Trends](https://developer.clevertap.com/docs/trends-api) and the [Real-Time Counts](https://developer.clevertap.com/docs/real-time-counts-api) APIs to learn more.
- **Triggering campaigns in external systems based on events monitored by CleverTap.** For example, let’s say a user abandons their cart during a high-value transaction. You can set up a webhook in CleverTap to monitor this specific event, get notified when it happens, and then trigger a call centre workflow to contact the user to solve any issues and complete the purchase. Check out our [webhooks](https://docs.clevertap.com/docs/webhooks-overview) feature to learn more.
- **Creating campaigns in CleverTap based on events in external systems.** For example, you can create a campaign to send a price drop alert as a push notification to users who have viewed a particular product in the past week. Check out our [Create Campaigns](https://developer.clevertap.com/docs/create-campaign-api) API to learn more.

# API Authentication

CleverTap uses a basic access authentication model to authenticate requests to the API. Please refer the [API Authentication](doc:authentication) for more details.

# API Endpoints

APIs are used to describe how to create, read, update, and delete data in CleverTap. For more information on each APIs, refer [API Reference](https://developer.clevertap.com/docs/api-reference).

# Webhooks

CleverTap webhooks enable developers to monitor specific user events and receive push notifications to their server when those events happen. You can use these notifications to trigger workflows in your backend systems as soon as qualifying events occur. Please see the [Webhooks](doc:webhooks) for more details.

# SDKs

We provide server-side libraries that will help you integrate more quickly with our API. Please see our [SDKs](doc:clevertap-sdks) for links to our server-side libraries.

# Postman Collection

Postman is a tool that lets you build and test HTTP requests. We created a [Postman Collection](https://developer.clevertap.com/docs/clevertap-postman-collection) that you can use to test making requests to the CleverTap API. 

# Configure Regions

You must enable the CleverTap Region to use the CleverTap's regional data center. For more information, see [Configuration Guide for CleverTap Regions](doc:idc).

# API Errors

If your CleverTap API request fails, you will receive an error response including a response code and a message explaining the reason for the error. For more details on error codes, refer [API Errors](doc:api-errors).

# API FAQs

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).