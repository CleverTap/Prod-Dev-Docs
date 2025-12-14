---
title: API FAQs
excerpt: Understand the common queries and concerns related to CleverTap APIs.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This section answers common questions related to the functionalities, usage, and limitations of CleverTap APIs.

## Q. What are the rate limits for CleverTap APIs?

A. CleverTap does not enforce rate limits based on a specific time frame (minute, hour, or day). It supports concurrent request limits for its APIs. For more details on concurrent requests, refer to [API Request Limit](https://developer.clevertap.com/docs/api-request-limit).

## Q. What do 15 concurrent requests for CleverTap APIs mean?

A.  It means 15 API requests can be processed simultaneously. It may take one or more seconds to complete the processing. For more details, refer to [API Request Limit](https://developer.clevertap.com/docs/api-request-limit).

## Q. What is a CORS (Cross-Origin Resource Sharing) error?

A. CORS errors (Cross-Origin Resource Sharing) can occur when accessing an API endpoint from a different domain or origin. If you encounter a CORS error while making API requests, the API you are trying to access is not configured to accept requests from your domain.

> 📘 Note
> 
> Only web browsers enforce CORS errors. You will not encounter CORS errors when accessing an API using cURL or Postman.

## Q. Has CleverTap adopted the CORS standard for its APIs?

A. Yes. CleverTap has adopted the CORS standard for its APIs. This enables web applications running on different domains to request and use resources from the CleverTap API securely.

## Q. Can customers send 1000 events for 1000 different profiles in the same [Upload Events](https://developer.clevertap.com/docs/upload-events-api) API request?

A. Yes. Customers can send 1000 events for 1000 profiles in a single [Upload Events](https://developer.clevertap.com/docs/upload-events-api) API request.

## Q. What format is used for `date` fields in CleverTap APIs for event/user properties?

A. Always use Epoch seconds. The following is an example of the `date` format:

```
"DOB": "$D_1487576752",// Pass the date format value as Epoch in seconds.`
```

## Q. Is `FBID` a mandatory parameter in the [Upload Events](https://developer.clevertap.com/docs/upload-events-api) API?

A. No, `FBID` is an optional parameter in [Upload Events](https://developer.clevertap.com/docs/upload-events-api) API. You must provide a value for at least one of the following parameters to identify the user: `identity, FBID, GPID`, or `objectId`.

## Q. How long does it take for CleverTap API to process 1 million profiles?

A. The processing time can range from 1 minute to 20 minutes depending on various factors, such as the size and complexity of the profile data, the API endpoint used, the network speed, and the server load.

## Q. How long does CleverTap take to demerge a user profile?

A. A user profile demerge may take up to 24-48 hours after it is marked for demerge from the CleverTap dashboard. 

## Q. How many profiles can be demerged using the [Demerge User Profile](https://developer.clevertap.com/docs/demerge-user-profile-api) API in a day?

A. It depends on the query threads available to perform the queries. CleverTap initiates the demerge process only during the customer's non-business hours (4 AM to 7 AM).

## Q. How many profiles can be deleted daily using the [Delete User Profile](https://developer.clevertap.com/docs/delete-user-profile-api) API?

A. CleverTap can delete up to 1 million profiles daily using the Delete User Profile API. 

## Q. How much time does CleverTap take to delete a profile?

A. A profile deletion may take up to 48 hours after it is marked for deletion from the CleverTap dashboard.

## Q. How can I confirm that the user profile will be deleted?

A. In the CleverTap dashboard, you can navigate to the user profile by selecting _Segment > Find People > By identity_. The profile marked for deletion will appear with a red banner and a message saying _The profile has been marked for deletion_.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/15bd1f4-Deleted_Profile.png",
        "User Profile marked for Deletion",
        1171
      ],
      "align": "center",
      "border": true,
      "caption": "User Profile marked for Deletion"
    }
  ]
}
[/block]


## Q. Does CleverTap restore the profile and its event data after deleting it?

A. No, CleverTap does not restore the data. Unfortunately, once you delete profiles, along with their event data, they cannot be restored. When you make a deletion request, CleverTap processes it during non-business hours, resulting in the permanent removal of this information. 

CleverTap suggests using the Delete API only in the following two cases:

- If you push incorrect data.
- If you opt for your data deletion under the [General Data Protection Regulation (GDPR)](https://docs.clevertap.com/docs/gdpr) Act.

However, please be aware that proceeding with this request leads to losing all your data.

CleverTap follows a strict data deletion policy, making data irretrievable once a deletion request is executed. Therefore, we strongly recommend exercising caution before proceeding with any deletion requests. To mitigate the risk of unintentional data loss, we recommend implementing robust backup and archiving procedures at your end. This extra layer of protection ensures that you have the means to recover essential data if the need arises in the future.