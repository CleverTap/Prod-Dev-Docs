---
title: API Overview
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
We provide a REST API that lets you create, read, update, and delete data in CleverTap.

This page describes our authentication model, API endpoints, webhook feature, server-side SDKs, and error handling.

# Make Your First API Call
The best place to get started with the CleverTap API is the [API Quick Start Guide](doc:api-quickstart-guide).

# API Use Cases

Here are some use cases with our API:

* **Uploading user profile and event data from external systems to CleverTap.** For example, if a user contacted your call center about a product issue. You can store this as an event on that user’s profile, which allows you to send a follow-up note about the conversation as an email/SMS or push notification. Check out the Upload User Profiles API and the Upload Events API to learn more. 
* **Getting user profile and event data from CleverTap to enrich customer information in external systems.** For example, if a salesperson wants to know what products a user has viewed before contacting calling them. You can get this information from CleverTap and include it in your CRM within that customer’s account record. Check out the Get User Profiles API and the Get Events API to learn more.
* **Getting user metrics from CleverTap for analysis in external systems.** For example, let’s say you have an internal dashboard where you track all your marketing metrics. Our API enables you to include user, event, and campaign data for your app on that dashboard. Check out the Trends API and the Real-Time Counts API to learn more.
* **Triggering campaigns in external systems based on events monitored by CleverTap.** For example, let’s say a user abandons their cart during a high-value transaction. You can set up a webhook in CleverTap to monitor for this specific event, get notified when it happens, and then trigger a call center workflow to contact the user to solve any issues and complete the purchase. Check out our webhooks feature to learn more.
* **Creating campaigns in CleverTap based on events in external systems.** For example, you can create a campaign to send a price drop alert as a push notification to users who have viewed a particular product in the past week. Check out our Create Campaigns API to learn more.

# Authentication
CleverTap uses a basic access authentication model to authenticate requests to the API. Please see the [authentication page](doc:authentication) for more details.

# API Endpoints

The API endpoints documented below describe how to create, read, update, and delete data in CleverTap.

## User Profile API Endpoints

After you integrate the CleverTap SDK in your app, we automatically create a user profile for each person using your app. You can get information about user profiles or enrich CleverTap user profiles with our API. You can also create user profiles for people who haven’t used your app yet.

Each user profile has standard properties like name and email. You can extend the user profile data model with your custom properties. 

Here are the API endpoints available to interact with the user profile object:
* [Get User Profiles API](doc:get-user-profiles-api) 
The Get User Profiles API lets you download user profiles from CleverTap. There are two ways to get user profiles. The first way is to download user profiles that have performed a specific event. For example, you can download a list of users that have launched your app in the past day. The second way to download user profiles is by requesting the specific users needed.
* [Upload User Profiles API](doc:upload-user-profiles-api) 
The Upload User Profiles API enables you to create or update user profiles in CleverTap. For example, you can use this API to update a user profile to include information, such as gender, phone, and customer type. Adding more information to user profiles in CleverTap enables you to create richer segments and run targeted campaigns. 
* [Upload Device Tokens API](doc:upload-device-tokens-api) 
The Upload Device Tokens API enables developers to add an existing device token to a CleverTap user profile. For example, you can use this API to add a GCM or APNS token to a user profile. Adding this information will let you send push notifications to that device. 
* [Get Profile Count API](doc:get-profile-count-api) 
The Get Profile Count lets you get a count of profiles that match a specific query.
* [Delete User Profile API](doc:delete-user-profile-api) 
The Delete User Profile API lets you delete a specific user profile.

## Event API Endpoints
Events represent the actions users take with your product. Examples are standard events like app launches and custom events you define like product views. 
[block:callout]
{
  "type": "info",
  "body": "Any events used for *live* (triggered) campaigns must be in real-time or as close as possible (i.e., 2 hours in the past and 2 minutes in the future).",
  "title": "Events Used for Live Campaigns"
}
[/block]
Here are the API endpoints available to interact with the event object:
* [Get Events API](doc:get-events-api) 
The Get Events API lets you download user events from CleverTap. For example, you can use this API to get a list of Purchase events in the past week.
* [Upload Events API](doc:upload-events-api) 
The Upload Events API enables you to upload user events from external systems to CleverTap. You can use this API to store user events, such as phone calls and purchases. Storing user events in CleverTap enables you to create richer segments and run targeted campaigns.  
* [Get Event Count API](doc:get-event-count-api) 
This Get Event Count lets you get a count of a specific event that occurred during a time period you define. 

## Campaign API Endpoints

CleverTap campaigns enable you to communicate with your users at scale. For example, you can set up a campaign to send a push notification with a discount code to users who have viewed a certain product.

Here are the API endpoints available to interact with the campaign object:
* [Create Campaign API](doc:create-campaign-api) 
The Create Campaign API lets you create campaigns in CleverTap. For example, you can use this endpoint to send an in-app notification to a specific set of users. You can also target your campaigns based on segments that match user profile properties you define. 
* [Stop Campaign API](doc:stop-campaign-api) 
The Stop Campaign API enables you to stop campaigns scheduled with the API. 
* [Get Campaign Report API](doc:get-campaign-report-api) 
The Get Campaign Report API lets you get performance metrics about a specific campaign.
* [Get Campaigns API](doc:get-campaigns-api)
The Get Campaigns API lets you get a list of campaigns created using the API.

## Report API Endpoints

CleverTap provides aggregate metrics for users, events, and campaigns. You can use these metrics to analyze your user engagement and guide product decisions. CleverTap customers often use the endpoints below to include users, events, and campaigns metrics into an internal dashboard, so they get an entire view of all the systems they are using. 

Here are the API endpoints available to interact with the report object:
* [Get Message Reports API](doc:get-message-reports-api) 
The Get Message Reports API lets you download a list of messages sent by CleverTap. For example, you can this API to get a report of how many in-app messages and push notifications were sent to users in the past week. 
* [Real-Time Counts API](doc:real-time-counts-api) 
The Real-Time Counts API can be used to get a real-time count of active users in the past five minutes including information about those users, such as UTM source.
* [Top Property Counts API](doc:top-property-counts-api) 
The Top Property Counts API lets you get counts for the most and least frequently occurring properties for a particular event in a specified duration.
* [Trends API](doc:trends-api) 
The Trends API lets you get a daily, weekly, and monthly event trends in a specified duration.

# Webhooks
CleverTap webhooks enable developers to monitor specific user events and receive push notifications to their server when those events happen. You can use these notifications to trigger workflows in your backend systems as soon as qualifying events occur. Please see the [webhooks page](doc:webhooks) for more details.

# SDKs
We provide server-side libraries that will help you build more quickly with our API. Please see our [SDKs page](doc:clevertap-sdks) for links to our server-side libraries.

# Postman Collection
Postman is a tool that lets you build and test HTTP requests. We created [a Postman Collection](https://developer.clevertap.com/docs/clevertap-postman-collection) that you can use to test making requests to the CleverTap API. 

#Configure Regions
You must enable the CleverTap Region to enable the usage of CleverTap's regional data center. For more information, see [Configuration Guide for CleverTap Regions](doc:idc).
# Errors
If your CleverTap API request fails, you will receive an error response including a response code and a message explaining the reason for the error. Error codes are documented on [this page](doc:api-errors).