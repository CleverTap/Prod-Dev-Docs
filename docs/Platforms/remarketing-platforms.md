---
title: Remarketing Platforms
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
## Facebook Remarketing

CleverTap uses the following FB API endpoints to create, update, associate a custom audience to an adset :

1. CleverTap creates the custom audience using the FB API: [https://developers.facebook.com/docs/marketing-api/reference/ad-account/customaudiences/#Creating](https://developers.facebook.com/docs/marketing-api/reference/ad-account/customaudiences/#Creating)

2. The segmentation is run using CleverTap’s segmentation engine and we add encoded (EMAIL\_SHA256) email addresses to the created custom audience using FB API: [https://developers.facebook.com/docs/marketing-api/reference/custom-audience/users/#Updating](https://developers.facebook.com/docs/marketing-api/reference/custom-audience/users/#Updating)

3. The custom audience is associated with an existing adset using the targeting\_spec: [https://developers.facebook.com/docs/marketing-api/targeting-specs#custom\_audiences](https://developers.facebook.com/docs/marketing-api/targeting-specs#custom_audiences) and the FB API: [https://developers.facebook.com/docs/marketing-api/reference/ad-campaign#Updating](https://developers.facebook.com/docs/marketing-api/reference/ad-campaign#Updating)
