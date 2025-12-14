---
title: Import Historical Data
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
Using CleverTap’s [APIs](doc:api-overview) you can import important historical data that you have collected in your own internal systems or from other analytical/engagement tools you might have been using previously.

# Step 1: Uploading User Profiles

You first need to send information on who your customers are to CleverTap. i.e. their profile information, things like their name, age, email id and any other custom tags you might have assigned a user.

Check out our API Documentation on [uploading user profiles](doc:upload-user-profiles-api).

# Step 2: Uploading Events

Once user profiles have been uploaded into CleverTap, you can add events to these profiles i.e you can add past transactions or important actions that a user might have performed.

Check out our API Documentation on [uploading user events](doc:upload-events-api).

# Tips
  * Always upload a sample set of data in your Test Account to ensure that data is consistent. Information once accepted by your production account can never be altered or deleted, your test account on the other hand can be reset
  * First upload profiles and then upload events against those profiles.
  * While uploading events, ensure you upload them oldest information to the latest information
  * Only data that comes in with a timestamp that occurred in the last 365 days will show up in the CleverTap Dashboard