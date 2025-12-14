---
title: Import Historical Data
excerpt: Understand how you can import historical data on the CleverTap dashboard.
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

Using CleverTap’s [APIs](doc:api-overview) and [SFTP Import](https://developer.clevertap.com/docs/imports-via-sftp), you can import important historical data that you have collected in your internal systems or from other analytical/engagement tools you might have been using previously.

# Upload User Profiles

First, you must send information about your customers to CleverTap, i.e., their profile information, name, age, email ID, and any other custom tags you might have assigned a user. For more information, refer to [Upload User Profiles API](https://developer.clevertap.com/docs/upload-user-profiles-api) and [SFTP Import](https://developer.clevertap.com/docs/imports-via-sftp).

# Upload Events

Once user profiles have been uploaded into CleverTap, you can add events to these profiles, i.e., past transactions or important actions that a user might have performed. To know more about this, refer to [Upload User Events](https://developer.clevertap.com/docs/upload-events-api) and [Events SFTP Import](https://developer.clevertap.com/docs/imports-via-sftp#sample-csv-files-for-upload).

# Key Points to Remember

* Always upload a sample data set in your Test Account to ensure that data is consistent. After the information is accepted by your production account, you can never alter or delete it. Your test account, on the other hand, can be reset.
* Begin by uploading user profiles, followed by uploading events related to those profiles.
* While uploading events, start with uploading the oldest data first and progress to the recent data.
* Data with a timestamp is available on the CleverTap Dashboard and can be imported based on the [Data Retention Policy (DRP)](https://docs.clevertap.com/docs/account-and-data-management#data-retention-policy).
