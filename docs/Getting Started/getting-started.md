---
title: CleverTap Overview for Developers
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
# Overview

CleverTap is a customer retention platform that provides the functionality to integrate app analytics and marketing. The platform helps customers increase user engagement in three ways: 

* Tracks actions users are taking and analyzes how people use the product. 
* [Segment](doc:segment) users based on their actions and run targeted [campaigns](https://docs.clevertap.com/docs/intro-to-campaigns) to these segments. 
* [Analyze](https://docs.clevertap.com/docs/intro-to-reports) each campaign to understand their effect on user engagement and business metrics.

# Platform Features

The CleverTap platform has five parts: 

* CleverTap's dashboard provides the functionality to [segment](doc:segment) users based on their actions and profile properties, run [targeted campaigns](https://docs.clevertap.com/docs/intro-to-campaigns) to these segments, and [analyze](https://docs.clevertap.com/docs/intro-to-reports)  each campaign’s performance. 
* [SDKs](https://developer.clevertap.com/docs/clevertap-sdks) track users’ actions within mobile apps and websites. CleverTap's SDKs also provide the ability to personalize apps by giving customers access to user profile data.
* [APIs](https://developer.clevertap.com/docs/api-overview) push user profile or event data from any source to CleverTap. Our APIs also export user data from CleverTap for analysis in BI tools and enrich customer information in CRMs.
* Integrations with communication platforms such as [SendGrid](doc:sendgrid) and [Twilio](doc:twilio), attributions providers such as [Branch](doc:branch), [Appsflyer](doc:appsflyer), and remarketing platforms such as [Facebook Audience Network](https://docs.clevertap.com/docs/facebook-audience-network-1). 
* [Webhooks](doc:webhooks) trigger workflows in backend systems as soon as qualifying events occur.

# Core Concepts

Users, events, segments, campaigns, and reports are central to CleverTap, so it is important to understand each role. 

* Users: After [integrating the CleverTap SDK](https://developer.clevertap.com/docs), a [user profile](doc:user-profiles) is created for each person who launches the app or visits your website. The CleverTap user profile has a set of default fields, such as device and location. You can also extend the default user profile data model by adding custom fields specific to their business.
* Events: The CleverTap SDK tracks the actions users perform in your app or website, such as a user viewing a product, listening to a song, or making a purchase. [Events](doc:concepts-events) are associated with a user profile.
* Segments: CleverTap provides the ability for users to create segments which are a group of users whose actions or user profile properties match a set of criteria you have defined. Once a user has created a segment, they can target them with a campaign or create a report to analyze them. 
* Campaigns: CleverTap [campaigns](https://docs.clevertap.com/docs/intro-to-campaigns) communicate with your users at scale. CleverTap offers 13 different messaging channels to reach your users on the optimal channel.
* Reports: CleverTap provides the ability to build [reports](https://docs.clevertap.com/docs/intro-to-reports) to understand the effect of campaigns on your users. You can use these reports to analyze your user engagement and guide product decisions.

# Usage Scenario

The following is an example of how a CleverTap customer in the retail industry is using our platform to increase sales:

When a person launches the company’s app for the first time, we automatically create a CleverTap [user profile](doc:user-profiles) for the person with our SDK. As the person navigates through product pages in the app, we log these actions as [events](doc:concepts-events) associated with the user’s profile. 

The user’s interaction with the company extends beyond the app. To complete a purchase, the user contacts the company’s call center, and the company adds this purchase event to the user’s profile with our [server-side API](https://developer.clevertap.com/docs/api-overview). 

Now, the marketer at the company wants to reward its most loyal users and incentivize them to buy more. Through CleverTap’s dashboard, the marketer can [create a campaign](https://docs.clevertap.com/docs/intro-to-campaigns) to show an  [in-app message](https://docs.clevertap.com/docs/inapp-campaigns) to users who recently made a purchase above a certain price. The next time the user launches the app, they will see a personalized message thanking them for buying that specific product and a discount code for their next purchase. 

To close this loop, the marketer can create a [report](https://docs.clevertap.com/docs/intro-to-reports) in the CleverTap dashboard to measure the campaign's impact in terms of both user engagement and product sales. 

# Getting Started

The best place to get started with CleverTap is our quick start guides which cover how to install our SDK and view a user event in the CleverTap dashboard in less than 10 minutes.

* [iOS Quick Start Guide](doc:ios-quickstart-guide)
* [Android Quick Start Guide](doc:android-quickstart-guide) 
* [Web Quick Start Guide](doc:web-quickstart-guide)
