---
title: Report APIs
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
CleverTap provides aggregate metrics for users, events, and campaigns. You can use these metrics to analyse your user engagement and guide product decisions. CleverTap customers often use the endpoints below to include users, events, and campaign metrics into an internal dashboard, so they get an entire view of all the systems they are using. 

Here are the API endpoints available to interact with the report object:
* [Get Message Reports](doc:get-message-reports-api) 
The Get Message Reports API lets you download a list of messages sent by CleverTap. For example, you can this API to get a report of how many in-app messages and push notifications were sent to users in the past week. 
* [Real-Time Counts](doc:real-time-counts-api) 
The Real-Time Counts API can be used to get a real-time count of active users in the past five minutes including information about those users, such as UTM source.
* [Top Property Counts](doc:top-property-counts-api) 
The Top Property Counts API lets you get counts for the most and least frequently occurring properties for a particular event in a specified duration.
* [Trends](doc:trends-api) 
The Trends API lets you get a daily, weekly, and monthly event trends in a specified duration.