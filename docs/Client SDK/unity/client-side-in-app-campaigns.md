---
title: Client-Side In-App Campaigns
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

The Client-side in-app triggering mechanic is designed to fetch in-app campaigns at the beginning of a new user session with the App Launched event. This ensures that the user receives the latest in-app campaigns from the start of a session. This also allows you to quickly display in-app campaigns in real time when the in-app campaign trigger event occurs during the session.

## OnUserLogin

An *App Launched* event is recorded when a user logs in using the `OnUserLogin` method. This means that in-apps get automatically fetched for the logged-in user. For example, *User1* is currently logged in on a device. *User2* is logged in using OnUserLogin. User2's in-app campaigns get automatically fetched. The same happens if another user logs in or *User1* logs in again.

Note that this does not happen on the first login to the app. A user profile is automatically created for every user launching your mobile application. Initially, the user profile starts as *Anonymous*. It indicates that it does not have identifiable information about the user. When users log in using the`OnUserLogin` for the first time, they become identified (non-anonymous). Since this is practically the same user, in-apps are not re-fetched mid-session.

## Session Reset

In-app campaigns are automatically fetched once the session expires and a new session starts. The session is reset when the app is brought to the foreground after 20 minutes of inactivity in the background.

## fetchInApps Method

Most use cases can be best handled with the automatic fetching on App Launched.  However, in certain cases, you must update which campaigns a user should receive mid-session. Examples of such use cases include:

* when a user performs a critical event in the app (Event change)
  * the user completes a game
  * the user performs their first purchase
* when a user profile is updated, resulting in user experience change (Profile Property change)
  * the user upgrades from a silver member to a gold member

In these cases, you can fetch in-apps from CleverTap mid-session by calling the `CleverTap.fetchInApps` method. This method returns the latest in-apps the user is eligible for.

## Image Preloading

The images used in the campaigns are preloaded when using client-side in-app campaigns. The images are downloaded when the in-app campaigns are fetched. The downloaded images are saved to the device storage. 

This allows the in-app to appear instantly upon the trigger because all required resources are already available. The SDK automatically tracks which images are used actively by campaigns. Inactive images are deleted every 14 days. 

Call the `CleverTap.clearInAppResources(bool expiredOnly)` method to clear the in-app resources manually. You can specify whether to clear only the inactive resources at that time or delete all.
