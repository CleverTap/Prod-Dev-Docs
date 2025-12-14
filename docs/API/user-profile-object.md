---
title: Profile APIs
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
After you integrate the CleverTap SDK in your app, we automatically create a user profile for each person using your app. You can get information about user profiles or enrich CleverTap user profiles with our API. You can also create user profiles for people who haven’t used your app yet.

Each user profile has standard properties like name and email. You can extend the user profile data model with your own custom properties. 

Here are the API endpoints available to interact with the user profile object:

* [Get User Profiles](doc:get-user-profiles-api)\
  The Get User Profiles API let you download user profiles from CleverTap. There are two ways to get user profiles. The first way is to download user profiles that have performed a specific event. For example, you can download a list of users that have launched your app in the past day. The second way to download user profiles is by requesting the specific users needed.
* [Upload User Profiles](doc:upload-user-profiles-api)\
  The Upload User Profiles API enables you to create or update user profiles in CleverTap. For example, you can use this API to update a user profile to include information, such as gender, phone, and customer type. Adding more information to user profiles in CleverTap enables you to create richer segments and run targeted campaigns. 
* [Upload Device Tokens](doc:upload-device-tokens-api)\
  The Upload Device Tokens API enables developers to add an existing device token to a CleverTap user profile. For example, you can use this API to add a GCM or APNS token to a user profile. Adding this information will let you send push notifications to that device. 
* [Get Profile Count](doc:get-profile-count-api)\
  The Get Profile Count lets you get a count of profiles that match a specific query.
* [Delete User Profile](doc:delete-user-profile-api)\
  The Delete User Profile API lets you delete a specific user profile.
* [Demerge User Profile](doc:demerge-user-profile-api)\
  The Demerge User Profile API lets you demerge a specific user profile.
