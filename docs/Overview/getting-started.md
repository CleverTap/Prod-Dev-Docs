---
title: CleverTap Overview for Developers
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
CleverTap enables you to integrate app analytics and marketing. 

Our platform helps you increase user engagement in three ways: 
  * You can track actions users are taking in your app to analyze how people use your product. 
  * You can segment users based on their actions and run targeted campaigns to these segments. 
  * You can analyze each of your campaigns to understand their impact on user engagement and your business metrics.

# Platform Features

There are five parts of the CleverTap platform: 

* [SDKs](doc:clevertap-sdks) that let you track users’ actions within your mobile apps and websites. Our SDKs also enable you to [personalize your app](doc:app-personalization) by giving you access to user profile data.
* [APIs](doc:api-overview) that let you push user profile or event data from any source to CleverTap. Our APIs also enable you to export your data from CleverTap for analysis in BI tools and enrich customer information in CRMs.
* [Dashboard](https://docs.clevertap.com/docs/getting-started) where you can segment your users based on their actions, run targeted campaigns to these segments, and analyze each campaign’s performance. 
* Integrations with communication platforms like SendGrid and Twilio, [attribution providers](doc:attribution) like Branch and Tune, and remarketing platforms like Facebook Audience Network.
* [Webhooks](doc:webhooks) that let you trigger workflows in your backend systems as soon as qualifying events occur.

# Core Concepts

Users, events, segments, campaigns, and reports are central to the CleverTap experience, so it is important to understand each one's role. 

* **Users:** After you integrate the CleverTap SDK, a user profile will be created for each person who launches your app or visits your website. The CleverTap user profile has a set of default fields, such as device and location. You can also extend the default user profile data model by adding custom fields that are specific to your business.
* **Events:** The CleverTap SDK lets you track what actions users perform in your app or website, such as a user viewing a product, listening to a song, or making a purchase. Events are associated with a user profile.
* **Segments:** CleverTap enables you to create segments, which are a group of users whose behaviors, activities, or user profile properties meet a set of criteria you’ve defined. Once you’ve identified segments of interest, you can save them, target them with a campaign, and then analyze them over time. 
* **Campaigns:** CleverTap campaigns enable you to communicate with your users at scale. CleverTap offers 9 different messaging channels to reach your users with the right message at the right time.
* **Reports:** CleverTap lets you build reports to understand the impact of your campaigns on your users. You can use these reports to analyze your user engagement and guide product decisions.

# Example Use Case

Let’s walk through an example of how a CleverTap customer in the retail industry is using our platform to increase sales.

When a person launches the company’s app for the first time, we automatically create a CleverTap [user profile](doc:user-profile-object) for the person with our SDK. As the person navigates through product pages in the app, we log these actions as [events](doc:events-object) associated with the user’s profile. 

The user’s interaction with the company extends beyond the app though. To complete a purchase the user contacts the company’s call center, the company adds this purchase event to the user’s profile with our [server-side API](doc:api-overview). 

Now the marketer at the company wants to rewards its most loyal users and incentivize them to buy more. Through CleverTap’s dashboard, the marketer can create a campaign to show an in-app notification to users who recently made a purchase above a certain price. Next time the user launches the app, they will see a personalized message thanking them for buying that specific product and a discount code for their next purchase. 

To close this loop, the marketer can measure the impact of that campaign to see how well it performed in terms of both user engagement and product sales. 

# Getting Started

The best place to get started with CleverTap is our quickstart guides, which will show you how to install our SDK and see a user event in the CleverTap dashboard in less than 10 minutes.

** [iOS Quickstart Guide](doc:ios-quickstart-guide) **

** [Android Quickstart Guide](doc:android-quickstart-guide)  **

** [Web Quickstart Guide](doc:web-quickstart-guide) **