---
title: API Request Limit
excerpt: Understand the concurrent request limit for CleverTap APIs.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Concurrent Requests

Concurrent requests are the maximum number of parallel API requests you can send to CleverTap. Think of this as the system's maximum capacity to handle multiple requests from users simultaneously without causing delays or failures.

When interacting with CleverTap APIs, you might need to perform several operations simultaneously, such as uploading device tokens, retrieving campaign data, or managing user profiles. Each operation is an API request. CleverTap limits concurrent API requests per user account to ensure smooth and efficient processing and maintain service performance and stability.

Here is a practical example to help you understand concurrent requests:

**Uploading Device Tokens:**Suppose you have a batch of device tokens to upload. The concurrent request limit for this operation is 15, meaning you can send up to 15 upload requests at the same time. If you exceed this limit, the system rejects the extra requests, and you receive an **HTTP 429** error with the message _Too many concurrent requests_.

# Importance of Concurrent Request Limits

* **Performance:**  Limits ensure the system handles requests efficiently without overloading, providing consistent and reliable performance.
* **Fair Usage:** Prevents one account from using all the available resources.
* **Stability:**  Maintains overall service stability and availability by managing the load.

Understanding concurrent request limits helps you manage your API usage, avoid exceeding limits, and ensure smooth application performance.

# Concurrent Request Limits for APIs

The following table lists the maximum number of concurrent requests allowed per user account for an API:

> 📘 Note
>
> CleverTap does not enforce rate limits based on specific time frames (minute, hour, or day), making our APIs more accessible.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        API
      </th>

      <th>
        Maximum

        Concurrent Requests
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        [Catalog API](doc:catalog-api-endpoints)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Create Campaign](doc:get-campaigns-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Create Segment](doc:create-segment-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Update Segment](doc:update-segment-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Custom List API](doc:custom-list-endpoints)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Delete User Profile](doc:delete-user-profile-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Demerge User Profile](doc:demerge-user-profile-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [External Trigger API](https://developer.clevertap.com/docs/external-trigger-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Get Campaign Report](doc:get-campaign-report-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Get Campaigns](doc:get-campaigns-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Get Event Count](doc:get-event-count-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Get Events](doc:get-events-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Get Message Reports](doc:get-message-reports-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Get Profile Count](doc:get-profile-count-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Get User Profiles](doc:get-user-profiles-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Raise a Bulletin](doc:bulletins-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Real-Time Counts](doc:real-time-counts-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Stop Scheduled Campaign](doc:stop-campaign-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Top Property Counts](doc:top-property-counts-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Trends](doc:trends-api)
      </td>

      <td>
        3
      </td>
    </tr>

    <tr>
      <td>
        [Upload Device Tokens](doc:upload-device-tokens-api)
      </td>

      <td>
        15
      </td>
    </tr>

    <tr>
      <td>
        [Upload Events](doc:upload-events-api)
      </td>

      <td>
        15
      </td>
    </tr>

    <tr>
      <td>
        [Upload User Profiles](doc:upload-user-profiles-api)
      </td>

      <td>
        15
      </td>
    </tr>
  </tbody>
</Table>
