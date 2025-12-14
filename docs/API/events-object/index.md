---
title: Event APIs
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
Events represent the actions users take with your product. Examples are standard events like app launches and custom events you define like product views.

<Callout icon="📘" theme="info">
  **Events Used for Live Campaigns**

  Any events used for _live_ (triggered) campaigns must be in real-time or as close as possible (i.e., 2 hours in the past and 2 minutes in the future).
</Callout>

Here are the API endpoints available to interact with the event object:

* [Get Events](doc:get-events-api)  
  The Get Events API lets you download user events from CleverTap. For example, you can use this API to get a list of Purchase events in the past week.
* [Upload Events](doc:upload-events-api)  
  The Upload Events API enables you to upload user events from external systems to CleverTap. You can use this API to store user events, such as phone calls and purchases. Storing user events in CleverTap enables you to create richer segments and run targeted campaigns.
* [Get Event Count](doc:get-event-count-api)  
  This Get Event Count lets you get a count of a specific event that occurred during a time period you define.
