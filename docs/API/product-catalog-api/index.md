---
title: Product Catalog API
deprecated: false
hidden: false
metadata:
  robots: index
---
# Overview

The Catalog API allows you to programmatically upload and manage catalogs in CleverTap. Catalogs are structured datasets, such as product lists or store locations, that can be referenced in campaigns, journeys, and personalization.

CleverTap supports [Product Catalog](https://docs.clevertap.com/docs/product-catalog), a collection of items, such as products in an e-commerce app or shows on a streaming platform.

The Catalog API provides endpoints to manage the full lifecycle of catalogs, including creation, updates, data uploads, and attribute configuration. The following are the available APIs:

- [Get Signed URL API](doc:pre-signed-url-api)
- [Mark a Signed URL API](doc:mark-a-signed-url-api)
- [Create Catalog API](doc:create-catalog-api)
- [List Catalogs API](doc:list-catalogs-api)
- [Get Catalog API ](doc:get-catalog-api)
- [Update Catalog API](doc:update-catalog-api)
- [Delete Catalog API](doc:delete-catalog-api)
- [Mark Attribute as Filterable API  ](doc:mark-attribute-as-filterable-api)

<Callout icon="📘" theme="info">
  ### Note

  The Catalog API v2 endpoints require the Catalogs feature to be enabled on your account and plan. If the feature is not enabled, requests fail with a 401 or 403 status and a message such as "Catalog feature is not enabled for this account." To enable the feature, contact your Customer Success Manager.
</Callout>