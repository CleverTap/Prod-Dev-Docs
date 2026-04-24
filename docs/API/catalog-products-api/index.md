---
title: Catalog Products API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

The Catalog Products API allows you to create, retrieve, update, and delete products within a catalog. Understand how products are structured and validated before using the APIs.

Refer to the following endpoints:

* [Create Single Product API](doc:create-single-product-api)
* [Create Products (Bulk) API](doc:create-products-bulk-api)
* [List Products API](doc:list-products-api)
* [Get Single Product API](doc:get-single-product-api)
* [Update Single Product API](doc::update-single-product-api)
* [Update Products (Bulk) API](doc:update-products-bulk-api)
* [Delete Single Product API](doc:delete-products-api)
* [Delete Products (Bulk) API](doc:delete-products-bulk-api)

# Product Model

A product consists of the following attributes:

* `identity` (required), unique identifier for the product
* `name` (required), display name
* `imageUrl` (required), publicly accessible image URL
* `customAttributes` (optional), key-value map defined by the catalog schema

**Example**

```json
{
  "identity": "PROD-001",
  "name": "Running Shoes",
  "imageUrl": "https://example.com/images/shoes.png",
  "customAttributes": {
    "brand": "Nike",
    "price": 99.99,
    "category": "Shoes",
    "in_stock": true
  }
}
```

# Fixed Fields and Custom Attributes

* Fixed fields (`identity`, `name`, `imageUrl`) are always required
* Custom attributes are defined in the catalog schema and validated during write operations
* Attributes can be marked as filterable for querying

# Validation

When creating or updating products:

* Required fields must be present
* Custom attributes must match schema data types
* Invalid products may fail individually in bulk requests

# Querying Products

Listing products supports:

* Pagination (`page`, `size`)
* Sorting (`sortBy`, `sortOrder`)
* Filtering using JSON string

**Example**

```text
{"brand":"Nike","price_gt":50}
```

Only attributes marked as filterable can be used in filters.

# Bulk Operations

All product APIs support bulk operations.

* Maximum **100 products per request**
* Maximum payload size **500 KB**

# File Upload vs Products API

* Use **Signed URL upload** for bulk ingestion
* Use **Products API** for incremental or real-time updates

# Best Practices

* Use stable product identifiers
* Mark only necessary attributes as filterable
* Keep batch sizes small
* Track and retry failed records
