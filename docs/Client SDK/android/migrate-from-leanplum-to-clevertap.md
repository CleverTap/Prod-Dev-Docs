---
title: Migrate from Leanplum to CleverTap
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
This guide provides you with the information and resources to migrate to CleverTap. A well-planned and performed transfer can result in more effective product utilization. As a result, the CleverTap team is dedicated to supporting you in achieving this objective.

The Leanplum JS SDK v 1.11.0 wraps the CleverTap SDK so that you can duplicate events and profiles without code changes.\
To migrate to the CleverTap platform:

1. Upgrade the Leanplum JS SDK to the latest version
2. Request traffic duplication from the support team
3. Monitor the data that has arrived in your CleverTap instance
4. Migrate campaigns to the CleverTap platform
5. Request for our support team to stop ingesting data in Leanplum. This action cannot be undone.

> 📘 Note
>
> Web Push campaigns can work with only one platform at a time because push token registrations are bound to a single application server. The push token is migrated to CleverTap after step 5 completes, at which time you can migrate your campaigns to CleverTap.

CleverTap does not collect IP data by default. You can opt-in to collect device IPs by adding the following snippet:

```javascript
Leanplum.addStartResponseHandler(() => {
    if (window.clevertap) {
        clevertap.privacy.push({ useIP: true });
    }
});
```
