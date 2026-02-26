---
title: CleverTap Postman Collection
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
# Overview

You can use our Postman Collection to quickly test CleverTap API endpoints without building requests from scratch.

To import and configure this collection into Postman, perform the following steps:

1. Download the [Zip folder](https://github.com/CleverTap/clevertap-snippets/raw/master/Clevertap%20API%20Postman%20Collection.zip) and unzip it.
2. Open Postman and click **Import**.
3. Select the collection file (CleverTap API.postman_collection.json) and the environment file for your region. For example, you will select CleverTap API - IN.postman_environment.json if your account region is India.
4. From the top-right dropdown in Postman, select the imported environment.
5. Click the eye icon and enter the following credentials:
   * id: Your CleverTap Account ID
   * passcode → Your CleverTap Passcode
     You can find these details by navigating to _Settings_ > _Project_ > _Overview_ or refer to [API Quickstart Guide](doc:api-quickstart-guide#get-clevertap-account-credentials-to-authenticate-api-requests).

After completing these steps, open any request from the collection and click **Send** to test the API.
