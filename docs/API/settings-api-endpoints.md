---
title: Settings API
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
These APIs allow you to both fetch data about dashboard settings and also allow you to set up/update existing dashboard settings.

# Subscription Groups

You can use this API to get the list of all subscription groups created on the dashboard

### Base Method

Here is an example base URL from the account in the India region:  
<https://in1.api.clevertap.com/category-groups>

### Region

Refer [Region](https://developer.clevertap.com/docs/common-api-components#region) for more details.

### HTTP Method

GET

### Headers

Refer [Headers](https://developer.clevertap.com/docs/common-api-components#headers) for more details.

### Example Response

```json
//response will be in the below format
{
   "list": [
       {
           "name": "Newsletter",
           "key": 1,
           "description": "Newslettter",
           "channels": [
               "Email"
           ],
           "updated_on": 1597812749,
           "created_on": 1597812749
       },
       {
           "name": "Product Updates",
           "key": 2,
           "description": "Product Updates",
           "channels": [
               "Email"
           ],
           "updated_on": 1597826038,
           "created_on": 1597826038
       }
   ],
   "status": "success"
}
```

To understand the common queries and concerns related to CleverTap APIs, refer to [API FAQs](doc:api-faqs).