---
title: Recommendations API
deprecated: false
hidden: true
metadata:
  robots: index
---
# Overview

The Recommendations API retrieves product recommendations through a single, unified endpoint. After you configure recommendation strategies in CleverTap, this API fetches recommended items based on those strategies. The API supports pagination, filtering, sorting, fallback strategies, and optional serving rules to control which items are returned.

Recommendations API supports the following recommendation algorithms:

| Algorithm Type            | Strategy                                                                  | Example                                                         |
| ------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------------------- |
| Popular                   | Popular, Best Sellers                                                     | Top 10 most viewed or purchased products this week              |
| Popular by Category       | Popular in Category, Best Sellers in Category                             | Top 10 most purchased shoes this week                           |
| Trending                  | Trending                                                                  | Products gaining rapid views in the last 24 hours               |
| Trending by Category      | Trending in Category                                                      | Electronics gaining rapid views in the last 24 hours            |
| Hybrid                    | Based on What You Viewed, Frequently Bought Together, Last Minute Add-ons | Personalized picks based on your browsing and purchase history  |
| Hybrid by Category        | Based on What You Viewed in category, Last Minute Add-ons in category     | Personalized picks within Sports based on your browsing history |
| Content-Based             | More Like This, Similar to What You Viewed, Premium Alternatives          | Items similar to the product you are currently viewing          |
| Content-Based by Category | More Like This in Category                                                | Items similar to what you are viewing within the same category  |

<Callout icon="📘" theme="info">
  #### Note

  All strategies use the same endpoint. What varies between strategies is the request body. Refer to the [Required Parameters by Strategy]() to understand what each algorithm type requires.
</Callout>

For more information on Recommendations and the Strategies, refer to [Recommendations Agent](https://docs.clevertap.com/docs/recommendations-agent#/).

# Method

`POST`

# Endpoint

`/api/v1/recommendations`

# Base URL

`https://<region>.recommendation.clevertap.com/api/v1/recommendations`

# Region

To know the region of your account, refer to [Region](doc:common-api-components#region).

# Headers

These headers must be included with every API request. A valid `Authorization` token is required to authenticate the request.

| Header                 | Description                                              | Required |
| ---------------------- | -------------------------------------------------------- | -------- |
| X-CleverTap-Account-Id | CleverTap account identifier                             | Yes      |
| X-CleverTap-Passcode   | CleverTap passcode for authentication                    | Yes      |
| Content-Type           | Content type of the request. Must be `application/json`. | Yes      |

# Request Parameters

The following parameters control the recommendation strategy, scoping, filtering, and pagination behavior:

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th>
        Type
      </th>

      <th>
        Required
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        id
      </td>

      <td>
        Integer
      </td>

      <td>
        Yes
      </td>

      <td>
        Unique identifier of the recommendation strategy to be applied.
      </td>
    </tr>

    <tr>
      <td>
        email
      </td>

      <td>
        String
      </td>

      <td>
        Conditional
      </td>

      <td>
        User email address. Required for Hybrid strategies when identifying a user by email.
      </td>
    </tr>

    <tr>
      <td>
        identity
      </td>

      <td>
        String
      </td>

      <td>
        Conditional
      </td>

      <td>
        Custom user identity. Required for Hybrid strategies when identifying a user by a custom ID.
      </td>
    </tr>

    <tr>
      <td>
        objectId
      </td>

      <td>
        String
      </td>

      <td>
        Conditional
      </td>

      <td>
        CleverTap object ID. Required for Hybrid strategies when identifying a user by objectId.
      </td>
    </tr>

    <tr>
      <td>
        userId
      </td>

      <td>
        Integer
      </td>

      <td>
        Conditional
      </td>

      <td>
        CleverTap internal user ID. Required for Hybrid strategies when the userId is already known.
      </td>
    </tr>

    <tr>
      <td>
        productId
      </td>

      <td>
        String
      </td>

      <td>
        Conditional
      </td>

      <td>
        Catalog item identifier. Required for Content-Based strategies when seeding recommendations from a specific product.
      </td>
    </tr>

    <tr>
      <td>
        scope
      </td>

      <td>
        Object
      </td>

      <td>
        Conditional
      </td>

      <td>
        Defines the scope of recommendations using catalog attributes (for example, `category: "Shoes"`).

        Required for Popular by Category, Trending by Category, Hybrid by Category, and Content-Based by Category strategies.
      </td>
    </tr>

    <tr>
      <td>
        filters
      </td>

      <td>
        Object
      </td>

      <td>
        No
      </td>

      <td>
        Filtering rules are applied to the catalog to include or exclude items.

        Supports combining conditions using logical operators.

        If not provided, no additional filtering is applied.
      </td>
    </tr>

    <tr>
      <td>
        fallback
      </td>

      <td>
        Integer
      </td>

      <td>
        No
      </td>

      <td>
        An identifier of an alternate strategy to use if the primary strategy returns no results.

        Fallback items are deduplicated against primary results before being returned.
      </td>
    </tr>

    <tr>
      <td>
        responseColumns
      </td>

      <td>
        Array of Strings
      </td>

      <td>
        No
      </td>

      <td>
        List of catalog fields to include for each recommended item. If not provided, the response includes only default fields (`name`, `identity`, `image_url`).
      </td>
    </tr>

    <tr>
      <td>
        count
      </td>

      <td>
        Integer
      </td>

      <td>
        No
      </td>

      <td>
        Number of recommendations to return. If not provided, a default number of items is returned.
      </td>
    </tr>

    <tr>
      <td>
        page
      </td>

      <td>
        Object
      </td>

      <td>
        No
      </td>

      <td>
        Pagination settings used to fetch a specific page of results.
      </td>
    </tr>

    <tr>
      <td>
        page.pageNumber
      </td>

      <td>
        Integer
      </td>

      <td>
        No
      </td>

      <td>
        Page number to return. If not provided, the first page is returned.
      </td>
    </tr>

    <tr>
      <td>
        page.limit
      </td>

      <td>
        Integer
      </td>

      <td>
        No
      </td>

      <td>
        Number of items to return per page. If not provided, a default page size is used.
      </td>
    </tr>

    <tr>
      <td>
        sort
      </td>

      <td>
        Array of Objects
      </td>

      <td>
        No
      </td>

      <td>
        Sorting rules applied to the result set. Sorting is applied in the order provided in the request. If not provided, the strategy-defined sort order is used.
      </td>
    </tr>

    <tr>
      <td>
        includeMetadata
      </td>

      <td>
        Boolean
      </td>

      <td>
        No
      </td>

      <td>
        Indicates whether full item details must be included in the response. Default is `false`.
      </td>
    </tr>

    <tr>
      <td>
        name
      </td>

      <td>
        String
      </td>

      <td>
        No
      </td>

      <td>
        Optional label for the request. This parameter does not affect recommendation results.
      </td>
    </tr>
  </tbody>
</Table>

## Required Parameters by Strategy

All strategies require the id parameter. The following table shows the additional parameters mandatory per strategy:

| Strategy                  | `id` is Required? | Is the user identifier required?                             | `scope` is Required? |
| ------------------------- | ----------------- | ------------------------------------------------------------ | -------------------- |
| Popular                   | Yes               | No                                                           | No                   |
| Popular by Category       | Yes               | No                                                           | Yes                  |
| Trending                  | Yes               | No                                                           | No                   |
| Trending by Category      | Yes               | No                                                           | Yes                  |
| Hybrid                    | Yes               | Yes (`email`, `identity`, or `objectId`)                     | No                   |
| Hybrid by Category        | Yes               | Yes (`email`, `identity`, or `objectId`)                     | Yes                  |
| Content-Based             | Yes               | Yes (`productId` or  one of `email`, `identity`, `objectId`) | No                   |
| Content-Based by Category | Yes               | Yes (`productId` or one of `email`, `identity`, `objectId`)  | Yes                  |

<Callout icon="📘" theme="info">
  #### Note

  Only one identifier is allowed per request. For Content-Based strategies, use `productId` to seed recommendations from a specific product, or provide a user identifier (`email`, `identity`, or `objectId`) for personalized recommendations. User identifiers are resolved to an internal ID automatically, you need not to pass it directly.
</Callout>

# Example Requests

The following code blocks provide sample requests for each of the 8 recommendation algorithms:

```json Popular Top 30
{
    "id": <POPULAR_STRATEGY_ID>,
    "name": "PopularTop30",
    "filters": {
        "logic": "OR",
        "clauses": [
            { "attr": "price", "op": "GT", "value": 10 },
            { "attr": "weight", "op": "LT", "value": 10 }
        ]
    },
    "responseColumns": ["price", "name"],
    "sort": [{ "attr": "price", "order": "DESC" }],
    "count": 20,
    "page": { "pageNumber": 1, "limit": 10 },
    "includeMetadata": true
    }
```
```json Popular By Category
{
    "id": <POPULAR_BY_CATEGORY_STRATEGY_ID>,
    "name": "PopularByCategory",
    "scope": { "genre": "Horror", "language": "Hindi" },
    "filters": {
        "logic": "OR",
        "clauses": [
            { "attr": "price", "op": "GT", "value": 10 },
            { "attr": "weight", "op": "LT", "value": 10 }
        ]
    },
    "responseColumns": ["cast", "genre"],
    "sort": [{ "attr": "price", "order": "DESC" }],
    "count": 20,
    "page": { "pageNumber": 1, "limit": 10 },
    "includeMetadata": true
    }
```
```json Trending Top 30
{{
    "id": <TRENDING_STRATEGY_ID>,
    "name": "TrendingTop30",
    "filters": {
        "logic": "OR",
        "clauses": [
            { "attr": "price", "op": "GT", "value": 10 },
            { "attr": "weight", "op": "LT", "value": 10 }
        ]
    },
    "responseColumns": ["price", "name"],
    "sort": [{ "attr": "price", "order": "DESC" }],
    "count": 20,
    "page": { "pageNumber": 1, "limit": 10 },
    "includeMetadata": true
    }
```
```json Trending By Category
{
    "id": <TRENDING_BY_CATEGORY_STRATEGY_ID>,
    "name": "TrendingByCategory",
    "scope": { "genre": "Horror", "language": "Hindi" },
    "filters": {
        "logic": "OR",
        "clauses": [
            { "attr": "price", "op": "GT", "value": 10 },
            { "attr": "weight", "op": "LT", "value": 10 }
        ]
    },
   "responseColumns": ["cast", "genre"],
    "sort": [{ "attr": "price", "order": "DESC" }],
    "count": 20,
    "page": { "pageNumber": 1, "limit": 10 },
    "includeMetadata": true
    }
```
```json Hybrid
{
    "id": <HYBRID_STRATEGY_ID>,
    "identity": "<USER_IDENTITY>",
    "fallback": 1773210729642,
    "responseColumns": ["cast", "tags"],
    "count": 20,
    "page": { "pageNumber": 1, "limit": 10 },
    "includeMetadata": true
    }
```
```json Content Based
{
    "id": <CONTENT_BASED_STRATEGY_ID>,
    "productId": "CTTDOC002",
    "responseColumns": ["price", "name"],
    "count": 10,
    "page": { "pageNumber": 1, "limit": 10 },
    "includeMetadata": true
    }
```

<Callout icon="📘" theme="info">
  #### Actual Values Required

  Replace all placeholder values (strategy IDs, product IDs, user identifiers, and auth headers) with real values from your CleverTap account before running these requests. Strategy IDs must correspond to active recommendation strategies.
</Callout>

# Example Responses

The following code blocks provide sample responses for each of the 8 recommendation strategies:

```json Popular Top 30
{
    "id": <POPULAR_STRATEGY_ID>,
    "name": "PopularTop30",
    "page": { "pageNumber": 1, "limit": 10 },
    "items": [
        {
            "identity": "PROD_001",
            "data": { "name": "AirRun Pro", "price": 2299 },
            "metaData": {
                "identity": "PROD_001",
                "image_url": "https://sample.jpg",
                "name": "AirRun Pro"
            }
        }
    ],
    "fallback_count": 0
    }
```
```json Popular By Category
{
    "id": <POPULAR_BY_CATEGORY_STRATEGY_ID,
    "name": "PopularByCategory",
    "scope": { "genre": "Horror", "language": "Hindi" },
    "page": { "pageNumber": 1, "limit": 10 },
    "items": [
        {
            "identity": "PROD_002",
            "data": { "cast": "Actor A, Actor B", "genre": "Horror" },
            "metaData": {
                "identity": "PROD_002",
                "image_url": "https://sample.jpg",
                "name": "The Silent Hour"
            }
        }
    ],
    "fallback_count": 0
    }
```
```json Trending Top 30
{
    "id": <TRENDING_STRATEGY_ID>,
    "name": "TrendingTop30",
    "page": { "pageNumber": 1, "limit": 10 },
    "items": [
        {
            "identity": "PROD_003",
            "data": { "name": "CloudStep X", "price": 3499 },
            "metaData": {
                "identity": "PROD_003",
                "image_url": "https://sample.jpg",
                "name": "CloudStep X"
            }
        }
    ],
    "fallback_count": 0
    }
```
```json Trending By Category
{
    "id": <TRENDING_BY_CATEGORY_STRATEGY_ID>,
    "name": "TrendingByCategory",
    "scope": { "genre": "Horror", "language": "Hindi" },
    "page": { "pageNumber": 1, "limit": 10 },
    "items": [
        {
            "identity": "PROD_004",
            "data": { "cast": "Actor A, Actor B", "genre": "Horror" },
            "metaData": {
                "identity": "PROD_004",
                "image_url": "https://sample.jpg",
                "name": "Midnight Run"
            }
        }
    ],
    "fallback_count": 0
    }
```
```json Hybrid
{
    "id": <HYBRID_STRATEGY_ID>,
    "page": { "pageNumber": 1, "limit": 10 },
    "items": [
        {
            "identity": "PROD_005",
            "data": { "cast": "Actor A, Actor B", "tags": "action, thriller" },
            "metaData": {
                "identity": "PROD_005",
                "image_url": "https://sample.jpg",
                "name": "Dark Horizon"
            }
        }
    ],
    "fallback_count": 2
    }
```
```json Content Based
{
    "id": <CONTENT_BASED_STRATEGY_ID>,
    "page": { "pageNumber": 1, "limit": 10 },
    "items": [
        {
            "identity": "PROD_007",
            "data": { "cast": "Actor A, Actor B", "genre": "Horror" },
            "metaData": {
                "identity": "PROD_007",
                "image_url": "https://sample.jpg",
                "name": "SwiftRun Elite"
            }
        }
    ],
    "fallback_count": 0
    }
```

# Response Parameters

The following table describes the fields returned in the API response:

| Parameter        | Type             | Description                                                                                                    |
| ---------------- | ---------------- | -------------------------------------------------------------------------------------------------------------- |
| id               | Integer          | Unique identifier of the recommendation strategy used to generate the results.                                 |
| name             | String           | Name of the recommendation strategy.                                                                           |
| scope            | Object           | Scope attributes applied to generate the recommendations. Present only for category-based strategies.          |
| page             | Object           | Pagination details for the returned result set.                                                                |
| page.pageNumber  | Integer          | Current page number in the result set.                                                                         |
| page.limit       | Integer          | Number of items returned per page.                                                                             |
| items            | Array of objects | List of recommended items.                                                                                     |
| items[].identity | String           | Primary identifier of the catalog item.                                                                        |
| items[].data     | Object           | Catalog fields returned based on the requested `responseColumns`.                                              |
| items[].metaData | Object           | Default catalog fields such as `name`, `image_url`, and `identity`, returned when `includeMetadata` is `true`. |
| fallback_count   | Integer          | Number of items in the response that came from the fallback strategy. `0` when no fallback was triggered.      |

# Filter

This API supports advanced catalog filtering and sorting using predefined operators.

## Filter Operators

The following filter operators are supported for catalog filtering:

| Operator | Description                                                                                                                                              |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| EQ       | Checks if the value equals the specified value (`==`)                                                                                                    |
| NE       | Checks if the value does not equal the specified value (`!=`)                                                                                            |
| GT       | Checks if the value is greater than the specified value (`>`)                                                                                            |
| GTE      | Checks if the value is greater than or equal to the specified value (`>=`)                                                                               |
| LT       | Checks if the value is less than the specified value (`<`)                                                                                               |
| LTE      | Checks if the value is less than or equal to the specified value (`<=`)                                                                                  |
| CNT      | Checks if the value is in the specified list. Accepts an array: `["a", "b", "c"]`                                                                        |
| DNCNT    | Checks if the value is not in the specified list. Accepts an array: `["a", "b", "c"]`                                                                    |
| BET      | Checks if the value falls within the specified range. Accepts an array:`[min, max]`                                                                      |
| INL      | Checks if the value is null. `true` returns rows with null values.`false` returns rows with non-null values.                                             |
| SWITH    | Checks if the value starts with the specified string. For example, ("attr": "sku", "op": "SWITH", "value": "ELEC") matches ELEC001, ELEC_PRO, and so on. |
| EWITH    | Checks if the value ends with the specified string. For example, ("attr": "sku", "op": "EWITH", "value": "_PRO") matches SHOE_PRO, ELEC_PRO, and so on.  |

## Logical Operators

The following logical operators are supported to combine multiple filter clauses:

| Operator | Description                                         |
| -------- | --------------------------------------------------- |
| AND      | Checks if all clauses in the filter match           |
| OR       | Checks if at least one clause in the filter matches |

## Filter Examples

The following examples demonstrate the use of the filters object across common use cases:

```json
json
// Single filter
{
  "filters": {
    "logic": "AND",
    "clauses": [
      {"attr": "price", "op": "GTE", "value": 1000}
    ]
  }
}

// Multiple filters with AND logic
{
  "filters": {
    "logic": "AND",
    "clauses": [
      {"attr": "price", "op": "GTE", "value": 1000},
      {"attr": "rating", "op": "GT", "value": 4.0},
      {"attr": "stock", "op": "EQ", "value": "IN_STOCK"}
    ]
  }
}

// Multiple filters with OR logic
{
  "filters": {
    "logic": "OR",
    "clauses": [
      {"attr": "category", "op": "EQ", "value": "electronics"},
      {"attr": "category", "op": "EQ", "value": "computers"}
    ]
  }
}

// List membership using CNT
{
  "filters": {
    "logic": "AND",
    "clauses": [
      {"attr": "brand", "op": "CNT", "value": ["Apple", "Samsung", "Sony"]}
    ]
  }
}

// Range filter using BET
{
  "filters": {
    "logic": "AND",
    "clauses": [
      {"attr": "price", "op": "BET", "value": [100, 500]}
    ]
  }
}

// String prefix matching using SWITH
{
  "filters": {
    "logic": "AND",
    "clauses": [
      {"attr": "sku", "op": "SWITH", "value": "ELEC"}
    ]
  }
}
```

# Error Responses

This API may return the following errors:

| HTTP Code | Message                                                  | Description                                                                                     |
| --------- | -------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| 400       | Invalid request body                                     | The request payload is malformed, missing required fields, or contains invalid data types.      |
| 400       | Only one identifier allowed                              | More than one identifier was provided. Only one is allowed per request.                         |
| 400       | Invalid id                                               | The provided strategy ID is invalid or does not meet validation requirements.                   |
| 400       | Unsupported strategy type                                | The strategy type is not supported or recognized by the API.                                    |
| 400       | Missing scope for category-based strategy                | A category-based strategy was requested, but no `scope` value was provided in the request body. |
| 404       | Recommendation strategy not found                        | The requested recommendation strategy does not exist or is not configured.                      |
| 404       | User not found                                           | No user record was found for the provided identifier.                                           |
| 500       | Hybrid strategy requires a user identifier               | A Hybrid strategy was requested, but no user identifier was provided.                           |
| 500       | User resolution failed                                   | The system failed while resolving the user profile due to an internal processing issue.         |
| 500       | User resolution service unavailable                      | The user resolution service is temporarily unavailable or not responding.                       |
| 500       | Failed to fetch recommendations. Please try again later. | The system could not retrieve recommendations due to an internal error or dependency failure.   |
| 500       | Unexpected error occurred                                | An unhandled internal server error occurred.                                                    |
