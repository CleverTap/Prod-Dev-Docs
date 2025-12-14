---
title: Developer Go Live Checklist
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
## Overview

It is always a good idea to have a checklist before you begin the *Go-Live* process. This can provide clarity about the task while preventing any last-minute issues and eliminating any repetitive human errors. This checklist is meant for the development of team members.

Following are the five steps that will mark the readiness of your application before you make it live to your users.

1. [Plan](https://developer.clevertap.com/docs/go-live-checklist#section-plan)
2. [Define](https://developer.clevertap.com/docs/go-live-checklist#section-define)
3. [Integrate and Test](https://developer.clevertap.com/docs/go-live-checklist#section-integrate-and-test)
4. [Launch](https://developer.clevertap.com/docs/go-live-checklist#section-launch)
5. [Review and Optimize](https://developer.clevertap.com/docs/go-live-checklist#section-review-and-optimize)

# Plan

1. Change CleverTap project ID, project token, and passcode with the production project credentials. Navigate to *Organization > Project* to check that you are using the correct project.
2. Having the right set of people on board is essential. Invite your team members to your CleverTap account dashboard from *Settings > Users > Add User*.

# Define

1. Set up *User Identity Management*. CleverTap allows you to identify your users per your requirement. Configure your user identification under *Settings > User Identity*. 
2. Assign the correct roles. Different team members invited on the CleverTap dashboard will have different access roles and permissions to read/write access. Check that the proper role is assigned to the users. You can manage roles under *Settings > Roles*.

# Integrate and Test

1. Check that the CleverTap SDK is initialized in your application.
2. Check that Google AdID/IDFA complies with your company’s [GDPR compliance policy](https://developer.clevertap.com/docs/sdk-changes-for-gdpr-compliance#section-restrict-tracking-with-google-ad-id).
3. Check that [multiple user profiles from the same device](https://developer.clevertap.com/docs/concepts-user-profiles#section-maintaining-multiple-user-profiles-on-the-same-device) are maintained per your requirement.
4. Check that [Push Impression event tracking](https://developer.clevertap.com/docs/android#section-push-impressions) is configured in your code and enabled from the CleverTap dashboard.
5. Check that the correct data types for the user properties and event properties are passed to CleverTap. For example,  date properties such as birth date, subscription date, and so on must be passed in the date format( using the Date object), and amount, price, quantity must be passed in the Integer format.
6. Check that the user and events properties are passed only with valid values to user profiles. Any *null* or empty strings must be avoided. 
7. Add a privacy policy. Declare to your users that you’re sharing data with third parties such as CleverTap. Refer to our [App store privacy policy section](https://developer.clevertap.com/docs/app-store-connect-app-privacy) for iOS apps. The [CleverTap privacy policy](https://clevertap.com/privacy-policy/) is available on our Website. 
8. Check that the current [CleverTap IP](https://developer.clevertap.com/docs/clevertap-ip-ranges) ranges are allowed. 

# Launch

1. Publish your app on the Playstore/Appstore.
2. Verify that the app is approved and live for users.

# Review and optimize

This section of the checklist is meant for the business team members.

## Review implementation

1. Upload the app icon by clicking *Settings > Project > Project Avatar* .
2. Check that *Uninstall Tracking* is enabled on the CleverTap dashboard under *Settings > Uninstall Tracking*.

## Review results of initial use case

1. Monitor the *Today* dashboard and check the *Activity stream* to observe the event counts. 
2. Open a few profiles to check if the user properties and events are being passed correctly. 
3. Set up *Custom Analytics* boards to monitor important business metrics. 
4. Monitor *Mobile app* dashboard for metrics such as MAU, DAU, WAU.

## Review campaign results and launch new campaigns

1. Enable daily and weekly campaign reports under *Settings > Setup > Reports*
2. Set up Journeys and Campaigns.

## Review and compare overall KPI metrics

1. Create and save a segment of all users that launched the app in the past 30 days.
2. Prepare a basic 4 step funnel to monitor conversions and dropoffs. 
3. Pin this funnel on a custom board.
4. Observe trends for at least three important events on the events dashboard. 
5. Navigate to *Events dashboard > Trends and Properties* and pin the trend.

## Identify other areas of improvement

Check if any other custom changes are required.
