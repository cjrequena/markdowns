---
layout: default
title: mongo-reference-guideline
parent: software-engineering
nav_order: 20
---
# Mongo Reference Guideline
{: .no_toc }
MongoDB queries are used to interact with the data stored in MongoDB collections. This guide covers CRUD operations, 
query operators, aggregation, indexes, and more.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 1. Basic Query Syntax

```javascript
db.collection.find(query, projection)
```

* `query`: Filters the documents.
* `projection`: Optional. Specifies fields to return.

### Example:

```javascript
db.users.find({ age: { $gte: 18 } }, { name: 1, age: 1 })
```

---

## 2. CRUD Operations

### Create

```javascript
db.collection.insertOne(document)
db.collection.insertMany([document1, document2, ...])
```

### Read

```javascript
db.collection.find(query, projection)
db.collection.findOne(query)
```

### Update

```javascript
db.collection.updateOne(filter, update)
db.collection.updateMany(filter, update)
db.collection.replaceOne(filter, replacement)
```

* Update operators: `$set`, `$inc`, `$push`, `$addToSet`, `$unset`

### Delete

```javascript
db.collection.deleteOne(filter)
db.collection.deleteMany(filter)
```

---

## 3. Query Operators

### Comparison Operators

| Operator | Description      |
| -------- | ---------------- |
| `$eq`    | Equals           |
| `$ne`    | Not equal        |
| `$gt`    | Greater than     |
| `$gte`   | Greater or equal |
| `$lt`    | Less than        |
| `$lte`   | Less or equal    |
| `$in`    | In array         |
| `$nin`   | Not in array     |

```javascript
db.products.find({ price: { $gte: 50, $lte: 100 } })
```

### Logical Operators

| Operator | Description |
| -------- | ----------- |
| `$and`   | Logical AND |
| `$or`    | Logical OR  |
| `$not`   | Logical NOT |
| `$nor`   | NOR         |

```javascript
db.users.find({
  $or: [{ age: { $lt: 18 } }, { isActive: true }]
})
```

### Element Operators

| Operator  | Description         |
| --------- | ------------------- |
| `$exists` | Field exists or not |
| `$type`   | Field is of a type  |

```javascript
db.users.find({ phone: { $exists: true } })
```

---

## 4. Projection

Specify fields to include (`1`) or exclude (`0`):

```javascript
db.users.find({}, { name: 1, _id: 0 })
```

---

## 5. Sorting, Limiting, Skipping

```javascript
db.collection.find().sort({ field: 1 })  // 1 = ASC, -1 = DESC
db.collection.find().limit(n)
db.collection.find().skip(n)
```

---

## 6. Basic Aggregation Framework

Used for advanced data processing:

```javascript
db.collection.aggregate([
  { $match: { status: "active" } },
  { $group: { _id: "$category", total: { $sum: "$amount" } } },
  { $sort: { total: -1 } }
])
```

### Common Aggregation Stages:

| Stage      | Description                  |
| ---------- | ---------------------------- |
| `$match`   | Filter data                  |
| `$group`   | Group and aggregate          |
| `$project` | Shape the output             |
| `$sort`    | Sort results                 |
| `$limit`   | Limit the number of results  |
| `$skip`    | Skip documents               |
| `$unwind`  | Deconstruct arrays           |
| `$lookup`  | Join with another collection |

---

## 7. Array Queries

### Match array elements:

```javascript
db.orders.find({ items: "apple" })
```

### Match specific array conditions:

```javascript
db.orders.find({ qty: { $all: [5, 10] } })
db.orders.find({ qty: { $elemMatch: { $gt: 5, $lt: 20 } } })
```

---

## 8. Regular Expressions

```javascript
db.users.find({ name: /john/i })  // Case-insensitive match
```

---

## 9. Basic Text Search

1. Sample Document
    ```jsonld=
    {
      _id: ObjectId("..."),
      name: "Samsung Galaxy S21 Ultra",
      description: "Flagship smartphone with 108MP camera and 5000mAh battery",
      category: "Smartphones",
      brand: "Samsung"
    }
    ```

1. Create a text index:

    ```javascriptd=
    db.products.createIndex({
      name: "text",
      description: "text",
      brand: "text"
    })
    ```

3. Perform a text search:

   **Basic Search**

    ```javascriptd=
    db.products.find({
      $text: { $search: "samsung galaxy" }
    })
    ```

   **Phrase Search**

    ```javascriptd=
    db.products.find({
      $text: { $search: "\"Galaxy S21 Ultra\"" }
    })
    ```

   **Exclude Terms**

    ```javascriptd=
    db.products.find({
      $text: { $search: "galaxy -case" }
    })
    ```

   **Ranked by Relevance**

    ```javascriptd=
    db.products.find(
      { $text: { $search: "flagship smartphone" } },
      { score: { $meta: "textScore" } }
    ).sort({ score: { $meta: "textScore" } })
    ```

### **How It Works:**

MongoDB builds an internal searchable word map from these fields.

This index supports efficient keyword and phrase matching.

###  **What this Does:**

Finds documents where the title or content contains "MongoDB", "indexing", or both.

Uses natural language processing (NLP) like stemming and stop word filtering.

| Feature                 | `$text` (Basic Text Search) | Atlas Search (`$search`)  |
| ----------------------- | --------------------------- | ------------------------- |
| Supports custom paths?  | ❌ No                        | ✅ Yes                     |
| Supports nested fields? | ❌ No                        | ✅ Yes                     |
| Flexible indexing?      | ❌ One text index only       | ✅ Fully configurable      |
| Use case                | Simple keyword search       | Advanced full-text search |

### **MongoDB Built-in Text Search Overview**

**Supported:**
- Searching strings in one or more fields
- Phrase search
- Logical operators (AND, OR, NOT)
- Relevance scoring (textScore)

**Not Supported:**
- Autocomplete
- Fuzzy search (typos)
- Nested fields or advanced analyzers

### **Limitations (Compared to Atlas Search)**

| Feature             | Text Index | Atlas Search |
| ------------------- | ---------- | ------------ |
| Tokenization        | Basic      | Advanced     |
| Fuzzy Match         | ❌          | ✅            |
| Autocomplete        | ❌          | ✅            |
| Synonyms            | ❌          | ✅            |
| Score Boosting      | Limited    | Advanced     |
| Nested Field Search | ❌          | ✅            |
| Language Support    | Basic      | Extensive    |

---

## 10. Indexing

### Create Index:

```javascript
db.collection.createIndex({ field: 1 })
```

### Compound Index:

```javascript
db.collection.createIndex({ field1: 1, field2: -1 })
```

### View Indexes:

```javascript
db.collection.getIndexes()
```

---

## 11. Data Types (BSON Types)

| Type        | Number |
| ----------- | ------ |
| Double      | 1      |
| String      | 2      |
| Object      | 3      |
| Array       | 4      |
| Binary Data | 5      |
| ObjectId    | 7      |
| Boolean     | 8      |
| Date        | 9      |
| Null        | 10     |
| Regex       | 11     |

Example:

```javascript
db.collection.find({ field: { $type: "string" } })
```

---

## 12. Miscellaneous

### Count Documents:

```javascript
db.collection.countDocuments({ field: value })
```

### Distinct Values:

```javascript
db.collection.distinct("field", { query })
```

### Rename a Field:

```javascript
db.collection.updateMany({}, { $rename: { "oldField": "newField" } })
```

---

## 13. Tips & Best Practices

* Always index fields used in queries or `$lookup`.
* Avoid unbounded `$regex` on unindexed fields.
* Use `explain()` to analyze query performance.
* Use `projection` to reduce payload size.
* Normalize or embed documents depending on access pattern.


---

## 14. Sample Collection: `users`


```json
[
  {
    "_id": "1",
    "name": "Alice",
    "age": 28,
    "email": "alice@example.com",
    "isActive": true,
    "address": {
      "city": "New York",
      "zip": "10001"
    },
    "roles": ["admin", "editor"],
    "lastLogin": ISODate("2025-05-20T10:00:00Z")
  },
  {
    "_id": "2",
    "name": "Bob",
    "age": 35,
    "email": "bob@example.com",
    "isActive": false,
    "address": {
      "city": "Chicago",
      "zip": "60601"
    },
    "roles": ["viewer"],
    "lastLogin": ISODate("2025-05-25T14:30:00Z")
  },
  {
    "_id": "3",
    "name": "Carol",
    "age": 22,
    "email": "carol@example.com",
    "isActive": true,
    "address": {
      "city": "Los Angeles",
      "zip": "90001"
    },
    "roles": ["editor"],
    "lastLogin": ISODate("2025-05-28T08:00:00Z")
  }
]
```
### Mongo Play Ground

[mongoplayground](https://mongoplayground.net/)
[search-playground](https://search-playground.mongodb.com/)

---

### Basic Find

```js
db.users.find()  // All documents
db.users.find({ name: "Alice" })  // Filter by exact match
```

---

### Comparison

```js
db.users.find({ age: { $gt: 25 } })
db.users.find({ age: { $gte: 28, $lte: 35 } })
db.users.find({ age: { $ne: 22 } })
```

---

### Boolean Fields

```js
db.users.find({ isActive: true })
```

---

### Embedded Documents

```js
db.users.find({ "address.city": "Chicago" })
db.users.find({ "address.zip": { $in: ["10001", "60601"] } })
```

---

### Arrays

```js
db.users.find({ roles: "admin" })  // Element match
db.users.find({ roles: { $all: ["admin", "editor"] } })
db.users.find({ roles: { $size: 2 } })
```

---

### Projections

```js
db.users.find({}, { name: 1, email: 1, _id: 0 })
```

---

### Sorting & Limiting

```js
db.users.find().sort({ age: -1 }).limit(2)
```

---

### Dates

```js
db.users.find({ lastLogin: { $gt: ISODate("2025-05-21T00:00:00Z") } })
```

---

### Regex & Text

```js
db.users.find({ name: /a/i })  // Case-insensitive match
```

> **Optional**: Create a text index and use full-text search

```js
db.users.createIndex({ name: "text", email: "text" })
db.users.find({ $text: { $search: "Alice" } })
```

---

### Logical Operators

```js
db.users.find({
  $and: [{ isActive: true }, { age: { $gt: 25 } }]
})

db.users.find({
  $or: [{ "address.city": "Chicago" }, { "address.city": "Los Angeles" }]
})

db.users.find({
  age: { $not: { $gt: 30 } }
})
```

---

### Aggregation Examples

```js
// Count users by city
db.users.aggregate([
  { $group: { _id: "$address.city", count: { $sum: 1 } } }
])

// Average age of active users
db.users.aggregate([
  { $match: { isActive: true } },
  { $group: { _id: null, avgAge: { $avg: "$age" } } }
])
```

---

### Updates

```js
db.users.updateOne(
  { name: "Bob" },
  { $set: { isActive: true } }
)

db.users.updateMany(
  { isActive: false },
  { $set: { lastLogin: new Date() } }
)

db.users.updateOne(
  { name: "Alice" },
  { $push: { roles: "superadmin" } }
)
```

---

### Deletes

```js
db.users.deleteOne({ name: "Carol" })
db.users.deleteMany({ age: { $lt: 25 } })
```

---

### Count, Distinct, Exists

```js
db.users.countDocuments({ isActive: true })
db.users.distinct("address.city")
db.users.find({ "phone": { $exists: false } })
```

---

## 15. Aggregation Framework

The aggregation framework processes data records and returns computed results. It’s like SQL’s `GROUP BY`, `JOIN`, and `SELECT` functions — but more powerful and pipeline-based.

---

### Basic Syntax

```js
db.collection.aggregate([
  { stage1 },
  { stage2 },
  ...
])
```

Each **stage** transforms the documents in some way.

---

### Common Aggregation Stages

**1. `$match`: Filter documents**

```js
{ $match: { status: "active" } }
```

Like `WHERE` in SQL.

---

**2. `$group`: Group and aggregate**

```js
{
  $group: {
    _id: "$category",
    total: { $sum: "$price" },
    avgPrice: { $avg: "$price" }
  }
}
```

Like `GROUP BY`.

---

**3. `$project`: Shape documents**

```js
{
  $project: {
    name: 1,
    priceWithTax: { $multiply: ["$price", 1.1] }
  }
}
```

Transforms or renames fields.

---

**4. `$sort`: Sort documents**

```js
{ $sort: { price: -1 } } // Descending
```

---

**5. `$limit` and `$skip`: Pagination**

```js
{ $skip: 10 },
{ $limit: 5 }
```

---

**6. `$lookup`: Join another collection**

```js
{
  $lookup: {
    from: "orders",
    localField: "_id",
    foreignField: "userId",
    as: "orders"
  }
}
```

---

**7. `$unwind`: Flatten arrays**

```js
{ $unwind: "$tags" }
```

---

**8. `$facet`: Multiple parallel pipelines**

```js
{
  $facet: {
    countByCategory: [
      { $group: { _id: "$category", count: { $sum: 1 } } }
    ],
    recentItems: [
      { $sort: { createdAt: -1 } },
      { $limit: 5 }
    ]
  }
}
```

---

**9. `$addFields`: Add computed fields**

```js
{ $addFields: { total: { $add: ["$price", "$tax"] } } }
```

---

**10. `$set`: Alias for `$addFields`**

```js
{ $set: { isActive: true } }
```

---

**11. `$count`: Count documents**

```js
{ $count: "total" }
```

---

**12. `$bucket` & `$bucketAuto`: Group by ranges**

```js
{
  $bucket: {
    groupBy: "$price",
    boundaries: [0, 100, 500, 1000],
    default: "Other",
    output: { count: { $sum: 1 } }
  }
}
```

---

### Expressions (Used in `$project`, `$addFields`, etc.)

* `$add`, `$subtract`, `$multiply`, `$divide`
* `$concat`, `$toUpper`, `$toLower`, `$substr`
* `$year`, `$month`, `$dayOfWeek`, `$dateToString`
* `$cond`, `$ifNull`, `$switch`

---

### Example Aggregation Pipeline

**Goal: Get average order amount per user with order count**

```js
db.orders.aggregate([
  {
    $group: {
      _id: "$userId",
      totalSpent: { $sum: "$amount" },
      avgSpent: { $avg: "$amount" },
      orderCount: { $sum: 1 }
    }
  },
  {
    $sort: { totalSpent: -1 }
  }
])
```

---

### Performance Tips

* Use `$match` and `$project` early to reduce data volume.
* Always index fields used in `$match`, especially before `$lookup`.
* Use `$merge` to write results to a new collection.
* Use `$out` or `$merge` for reporting workflows.

---

### Summary Table of Key Stages

| Stage      | Use Case                         |
| ---------- | -------------------------------- |
| `$match`   | Filter input                     |
| `$group`   | Aggregate by key                 |
| `$project` | Shape output                     |
| `$sort`    | Sort result                      |
| `$limit`   | Limit documents                  |
| `$skip`    | Skip N documents                 |
| `$lookup`  | Join with another collection     |
| `$unwind`  | Flatten arrays                   |
| `$facet`   | Multiple result sets in one pass |
| `$count`   | Count documents                  |
| `$bucket`  | Bin documents by range           |


---

## 16. Detailed Aggregation Stages


### `$lookup` – Aggregation Joins

**Purpose**

`$lookup` allows you to **join documents from another collection** into your aggregation pipeline.

**Sample Collections**

**Collection: `users`**

```json
[
  {
    "_id": 1,
    "name": "Alice",
    "age": 28
  },
  {
    "_id": 2,
    "name": "Bob",
    "age": 35
  }
]
```

**Collection: `orders`**

```json
[
  {
    "_id": 101,
    "userId": 1,
    "item": "Laptop",
    "amount": 1200
  },
  {
    "_id": 102,
    "userId": 1,
    "item": "Mouse",
    "amount": 25
  },
  {
    "_id": 103,
    "userId": 2,
    "item": "Monitor",
    "amount": 300
  }
]
```

**Basic `$lookup`**

```js
db.users.aggregate([
  {
    $lookup: {
      from: "orders",          // foreign collection
      localField: "_id",       // field in users
      foreignField: "userId",  // field in orders
      as: "userOrders"         // output array field
    }
  }
])
```

**Output:**

```json
[
  {
    "_id": 1,
    "name": "Alice",
    "age": 28,
    "userOrders": [
      { "_id": 101, "userId": 1, "item": "Laptop", "amount": 1200 },
      { "_id": 102, "userId": 1, "item": "Mouse", "amount": 25 }
    ]
  },
  {
    "_id": 2,
    "name": "Bob",
    "age": 35,
    "userOrders": [
      { "_id": 103, "userId": 2, "item": "Monitor", "amount": 300 }
    ]
  }
]
```

**`$lookup` + `$unwind`**

To **flatten the joined array** into individual documents:

```js
db.users.aggregate([
  {
    $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "userId",
      as: "userOrders"
    }
  },
  { $unwind: "$userOrders" }
])
```

**`$lookup` with `$group`**

To calculate total spend per user:

```js
db.users.aggregate([
  {
    $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "userId",
      as: "orders"
    }
  },
  {
    $project: {
      name: 1,
      totalSpent: { $sum: "$orders.amount" }
    }
  }
])
```

**Advanced `$lookup` (Pipeline Form) – MongoDB 3.6+**

Allows filtering or projecting within the join.

```js
db.users.aggregate([
  {
    $lookup: {
      from: "orders",
      let: { userId: "$_id" },
      pipeline: [
        { $match: { $expr: { $eq: ["$userId", "$$userId"] } } },
        { $match: { amount: { $gt: 100 } } },
        { $project: { item: 1, amount: 1, _id: 0 } }
      ],
      as: "bigOrders"
    }
  }
])
```

**$lookup Best Practices**

* Index `foreignField` in the joined collection for performance.
* Avoid joining large collections in real-time user queries.
* Use `$unwind` and `$project` for formatting the output cleanly.

**$lookup Summary**

| Feature                       | SQL Equivalent       |
| ----------------------------- | -------------------- |
| `$lookup`                     | `JOIN`               |
| `localField` + `foreignField` | `ON` clause          |
| `$unwind`                     | Row expansion        |
| `$group`, `$project`          | `GROUP BY`, `SELECT` |


### `$facet` -  multiple sub-pipelines in parallel on the same input documents and outputs

`$facet` runs **multiple sub-pipelines** in parallel on the **same input documents** and outputs their results together. It’s incredibly useful for **multi-dimensional analysis**, such as paginated results + counts, or filtering + faceted navigation (like on e-commerce sites).


**Syntax**

```js
db.collection.aggregate([
  {
    $facet: {
      output1: [ pipeline1 ],
      output2: [ pipeline2 ],
      ...
    }
  }
])
```

Each field (e.g., `output1`) defines a **sub-pipeline** that processes the same documents independently.


**Example: `products` Collection**

```json
[
  { _id: 1, name: "Phone", category: "Electronics", price: 499 },
  { _id: 2, name: "Laptop", category: "Electronics", price: 1299 },
  { _id: 3, name: "Shoes", category: "Clothing", price: 79 },
  { _id: 4, name: "Watch", category: "Accessories", price: 199 },
  { _id: 5, name: "Headphones", category: "Electronics", price: 199 }
]
```

---

**Sample `$facet` Query**

**Goal: Get a count by category, and a list of items priced under \$500.**

```js
db.products.aggregate([
  {
    $facet: {
      "categoryCounts": [
        {
          $group: {
            _id: "$category",
            names: {
              $push: "$name"
            },
            count: {
              $sum: 1
            }
          }
        },
        {
          $project: {
            _id: 0,
            category: "$_id",
            names: 1,
            count: 1
          }
        }
      ],
      "under500": [
        {
          $match: {
            price: {
              $lt: 500
            }
          }
        },
        {
          $project: {
            name: 1,
            price: 1,
            _id: 1
          }
        }
      ]
    }
  }
])
```

**Output:**

```json
[
  {
    "categoryCounts": [
      {
        "category": "Clothing",
        "count": 1,
        "names": [
          "Shoes"
        ]
      },
      {
        "category": "Accessories",
        "count": 1,
        "names": [
          "Watch"
        ]
      },
      {
        "category": "Electronics",
        "count": 3,
        "names": [
          "Phone",
          "Laptop",
          "Headphones"
        ]
      }
    ],
    "under500": [
      {
        "_id": 1,
        "name": "Phone",
        "price": 499
      },
      {
        "_id": 3,
        "name": "Shoes",
        "price": 79
      },
      {
        "_id": 4,
        "name": "Watch",
        "price": 199
      },
      {
        "_id": 5,
        "name": "Headphones",
        "price": 199
      }
    ]
  }
]
```

**$facet Use Cases**

| Use Case                 | Example                                         |
| ------------------------ | ----------------------------------------------- |
| Faceted navigation       | E-commerce filters (e.g., size, brand, color)   |
| Pagination + total count | Page of results + total available documents     |
| Multi-metric reporting   | Avg price, count by category, top-selling items |
| Complex dashboards       | Multiple charts/tables from one aggregation run |


**Nested Facets**

You can even **nest** `$facet` within a facet sub-pipeline, though it's advanced and should be used cautiously for performance reasons.

**$facet Best Practices**

* Use projections early in pipelines to reduce document size.
* Avoid too many sub-pipelines if performance is a concern.
* Use `$match` outside the `$facet` to pre-filter if possible.


### `$bucket` - Groups documents into buckets (bins)

**Purpose:**

The `$bucket` stage **groups documents into buckets (bins)** based on a field’s value. It’s used to create **histogram-like groupings**, such as price ranges or age brackets.

**Example Input Collection: `products`**

```json
[
  { "name": "Pencil", "price": 1 },
  { "name": "Notebook", "price": 50 },
  { "name": "Keyboard", "price": 150 },
  { "name": "Monitor", "price": 300 },
  { "name": "Laptop", "price": 800 },
  { "name": "Phone", "price": 999 },
  { "name": "TV", "price": 1100 }
]
```

**Aggregation Query:**

```js
db.products.aggregate([
  {
    $bucket: {
      groupBy: "$price",                      // Field to group by
      boundaries: [0, 100, 500, 1000],        // Bucket ranges (inclusive lower, exclusive upper)
      default: "Other",                       // Bucket for out-of-range values
      output: {
        count: { $sum: 1 },                   // Count per bucket
        items: { $push: "$name" }             // Optional: List item names
      }
    }
  }
])
```

**How Buckets Are Built:**

| Bucket Range        | Matches           |
| ------------------- | ----------------- |
| 0 ≤ price < 100     | Pencil, Notebook  |
| 100 ≤ price < 500   | Keyboard, Monitor |
| 500 ≤ price < 1000  | Laptop, Phone     |
| Other (≥1000 or <0) | TV                |


**Output:**

```json
[
  {
    "_id": 0,
    "count": 2,
    "items": ["Pencil", "Notebook"]
  },
  {
    "_id": 100,
    "count": 2,
    "items": ["Keyboard", "Monitor"]
  },
  {
    "_id": 500,
    "count": 2,
    "items": ["Laptop", "Phone"]
  },
  {
    "_id": "Other",
    "count": 1,
    "items": ["TV"]
  }
]
```

---

## 17. Atlas Search Index

**MongoDB Atlas Search** enables full-text search capabilities within your database using **Lucene-powered** search indexes. It integrates seamlessly with the aggregation framework using the `$search` stage.

See: [search-playground](https://search-playground.mongodb.com/tools/search-demo-builder/snapshots/new)


### Search Index

* A **Search Index** is a Lucene-style index defined at the collection level.
* Enables fast, advanced search features (e.g., text relevance, autocomplete, faceted search).
* Accessed using the `$search` stage in an aggregation pipeline.


### How to Create a Search Index

**Using Atlas UI:**

1. Go to your cluster in MongoDB Atlas.
2. Click **Search** tab.
3. Choose **Create Search Index**.
4. Select **Fields** to index or use the **Dynamic Mapping**.

**Example: JSON Index Definition**

```json
{
  "mappings": {
    "dynamic": false,
    "fields": {
      "title": { "type": "string" },
      "description": { "type": "string" },
      "price": { "type": "number" },
      "tags": { "type": "string" }
    }
  }
}
```

### Basic `$search` Syntax

```js
db.products.aggregate([
  {
    $search: {
      index: "default",  // Optional if using default index
      text: {
        query: "laptop",
        path: ["title", "description"]
      }
    }
  }
])
```

### Common `$search` Operators

| Operator           | Description                                  |
| ------------------ | -------------------------------------------- |
| `text`             | Full-text search                             |
| `autocomplete`     | Partial word matching                        |
| `compound`         | Combine multiple search clauses (AND/OR/NOT) |
| `phrase`           | Match exact phrases                          |
| `wildcard`         | Pattern-based matching (e.g., `lap*`)        |
| `regex`            | Regular expression matching                  |
| `range`            | Numeric/date range search                    |
| `embeddedDocument` | Search inside subdocuments                   |
| `exists`           | Match documents that contain a field         |


### Score Boosting and Control

* `$meta: "searchScore"` gives relevance score.
* Use `score` inside operators to boost relevance.

### Example: Boost `title` matches higher

```js
text: {
  query: "laptop",
  path: ["title", "description"],
  score: {
    boost: {
      value: 3,
      path: { value: "title" }
    }
  }
}
```

### Search with Aggregation

Combine `$search` with aggregation stages like `$match`, `$project`, `$facet`, `$limit`.

```js
db.products.aggregate([
  {
    $search: {
      text: {
        query: "smartphone",
        path: ["title"]
      }
    }
  },
  { $match: { price: { $lt: 1000 } } },
  { $project: { title: 1, price: 1, score: { $meta: "searchScore" } } },
  { $sort: { score: -1 } },
  { $limit: 5 }
])
```


### Facets and Highlighting

**`$searchMeta` with `facet`**

```js
db.products.aggregate([
  {
    $searchMeta: {
      facet: {
        operator: {
          text: { query: "laptop", path: "title" }
        },
        facets: {
          categoryFacet: {
            type: "string",
            path: "category"
          },
          priceRanges: {
            type: "number",
            path: "price",
            boundaries: [0, 500, 1000],
            default: "Other"
          }
        }
      }
    }
  }
])
```

**Highlighting (in `$search` only)**

```js
$search: {
  text: {
    query: "watch",
    path: "description"
  },
  highlight: {
    path: "description"
  }
}
```

Then use:

```js
{ $project: { description: 1, highlights: { $meta: "searchHighlights" } } }
```

### Search Index Definition Features

| Feature         | Type     | Description                      |
| --------------- | -------- | -------------------------------- |
| `string`        | text     | Full-text, fuzzy, exact matching |
| `number`        | numeric  | Range search (price, rating)     |
| `date`          | date     | Supports ISODate filtering       |
| `boolean`       | boolean  | True/false filtering             |
| `array`         | mixed    | Indexed array contents           |
| `dynamic: true` | wildcard | Auto-index all fields            |
| `analyzers`     | advanced | Custom tokenization and stemming |


### Summary of Operators

| Operator       | Use Case                        |
| -------------- | ------------------------------- |
| `text`         | Full-text match                 |
| `autocomplete` | Suggest-as-you-type             |
| `phrase`       | Exact match for phrases         |
| `range`        | Price/date filters              |
| `compound`     | Combine multiple logic branches |
| `facet`        | Count by field or range         |
| `highlight`    | Highlight match snippets        |


---

### Demo Setup

**Create a collection**

**Database:** `shop`
**Collection:** `products`

Run this in **MongoDB Shell**, Compass, or Atlas UI:

```js
use shop;

db.products.insertMany([
  {
    name: "Laptop",
    description: "Powerful and portable computing device",
    translations: {
      en: {
        name: "Laptop",
        description: "Powerful and portable computing device"
      },
      es: {
        name: "Portátil",
        description: "Dispositivo informático potente y portátil"
      },
      fr: {
        name: "Ordinateur portable",
        description: "Appareil informatique puissant et portable"
      },
      de: {
        name: "Laptop",
        description: "Leistungsstarkes und tragbares Computergerät"
      }
    },
    category: "Electronics",
    price: 1200,
    defaultLanguage: "en"
  },
  {
    name: "Phone",
    description: "Fast, sleek smartphone",
    translations: {
      en: {
        name: "Phone",
        description: "Fast, sleek smartphone"
      },
      es: {
        name: "Teléfono",
        description: "Teléfono inteligente rápido y elegante"
      },
      fr: {
        name: "Téléphone",
        description: "Smartphone rapide et élégant"
      },
      de: {
        name: "Telefon",
        description: "Schnelles, elegantes Smartphone"
      }
    },
    category: "Electronics",
    price: 499,
    defaultLanguage: "en"
  },
  {
    name: "Shoes",
    description: "Comfortable running shoes",
    translations: {
      en: {
        name: "Shoes",
        description: "Comfortable running shoes"
      },
      es: {
        name: "Zapatos",
        description: "Zapatos cómodos para correr"
      },
      fr: {
        name: "Chaussures",
        description: "Chaussures de course confortables"
      },
      de: {
        name: "Schuhe",
        description: "Bequeme Laufschuhe"
      }
    },
    category: "Clothing",
    price: 79,
    defaultLanguage: "en"
  },
  {
    name: "Watch",
    description: "Stylish analog watch",
    translations: {
      en: {
        name: "Watch",
        description: "Stylish analog watch"
      },
      es: {
        name: "Reloj",
        description: "Reloj analógico elegante"
      },
      fr: {
        name: "Montre",
        description: "Montre analogique élégante"
      },
      de: {
        name: "Uhr",
        description: "Stilvolle analoge Uhr"
      }
    },
    category: "Accessories",
    price: 199,
    defaultLanguage: "en"
  },
  {
    name: "Headphones",
    description: "Noise-cancelling headphones",
    translations: {
      en: {
        name: "Headphones",
        description: "Noise-cancelling headphones"
      },
      es: {
        name: "Auriculares",
        description: "Auriculares con cancelación de ruido"
      },
      fr: {
        name: "Écouteurs",
        description: "Écouteurs à réduction de bruit"
      },
      de: {
        name: "Kopfhörer",
        description: "Geräuschunterdrückende Kopfhörer"
      }
    },
    category: "Electronics",
    price: 199,
    defaultLanguage: "en"
  }
]);
```

**Create a Search Index**

If you're using MongoDB **Atlas**, go to:

**Cluster > Collections > shop.products > Search > Create Index**

Choose **JSON Editor** and paste:

```json
{
  "mappings": {
    "dynamic": false,
    "fields": {
      "name": {
        "type": "autocomplete",
        "analyzer": "lucene.english",
        "foldDiacritics": true,
        "maxGrams": 12,
        "minGrams": 3,
        "tokenization": "edgeGram"
      },
      "description": {
        "type": "string",
        "analyzer": "lucene.english"
      },
      "category": {
        "type": "string"
      },
      "price": {
        "type": "number"
      },
      "translations": {
        "type": "document",
        "fields": {
          "en": {
            "type": "document",
            "fields": {
              "name": {
                "type": "autocomplete",
                "analyzer": "lucene.english",
                "foldDiacritics": true,
                "maxGrams": 12,
                "minGrams": 3,
                "tokenization": "edgeGram"
              },
              "description": {
                "type": "string",
                "analyzer": "lucene.english"
              }
            }
          },
          "es": {
            "type": "document",
            "fields": {
              "name": {
                "type": "autocomplete",
                "analyzer": "lucene.spanish",
                "foldDiacritics": true,
                "maxGrams": 12,
                "minGrams": 3,
                "tokenization": "edgeGram"
              },
              "description": {
                "type": "string",
                "analyzer": "lucene.spanish"
              }
            }
          },
          "fr": {
            "type": "document",
            "fields": {
              "name": {
                "type": "autocomplete",
                "analyzer": "lucene.french",
                "foldDiacritics": true,
                "maxGrams": 12,
                "minGrams": 3,
                "tokenization": "edgeGram"
              },
              "description": {
                "type": "string",
                "analyzer": "lucene.french"
              }
            }
          },
          "de": {
            "type": "document",
            "fields": {
              "name": {
                "type": "autocomplete",
                "analyzer": "lucene.german",
                "foldDiacritics": true,
                "maxGrams": 12,
                "minGrams": 3,
                "tokenization": "edgeGram"
              },
              "description": {
                "type": "string",
                "analyzer": "lucene.german"
              }
            }
          }
        }
      }
    }
  }
}
```

Or enable `dynamic: true` to auto-index everything for now.

**Search Query Set**

**1. Basic Full-Text Search**

```js
db.products.aggregate([
  {
    $search: {
      text: {
        query: "smartphone",
        path: ["name", "description"]
      }
    }
  }
])
```


**2. Search + Price Filter**

```js
db.products.aggregate([
  {
    $search: {
      text: {
        query: "watch",
        path: ["name", "description"]
      }
    }
  },
  {
    $match: { price: { $lt: 500 } }
  }
])
```

**3. Highlighting Matches**

```js
db.products.aggregate([
  {
    $search: {
      text: {
        query: "noise",
        path: "description"
      },
      highlight: { path: "description" }
    }
  },
  {
    $project: {
      name: 1,
      description: 1,
      highlights: { $meta: "searchHighlights" }
    }
  }
])
```

**4. Faceted Search with `$searchMeta`**

```js
db.products.aggregate([
  {
    $searchMeta: {
      facet: {
        operator: {
          text: {
            query: "headphones",
            path: "description"
          }
        },
        facets: {
          categoryFacet: {
            type: "string",
            path: "category"
          },
          priceRanges: {
            type: "number",
            path: "price",
            boundaries: [0, 100, 500, 1000],
            default: "Other"
          }
        }
      }
    }
  }
])
```

**5. Autocomplete (needs special index)**

```js
db.products.aggregate([
  {
    $search: {
      index: "autocomplete_index",
      autocomplete: {
        query: "lap",
        path: "name", // Simple root-level search
        fuzzy: { maxEdits: 2 }
      }
    }
  }
])
```

```js
db.products.aggregate([
  {
    $search: {
      index: "autocomplete_index",
      autocomplete: {
        query: "por",
        path: "translations.es.name",
        fuzzy: { maxEdits: 2 }
      }
    }
  }
])
```

**Ready to Use**

You can test all these queries directly in:

* **MongoDB Atlas > Aggregation Builder**
* **MongoDB Compass > Aggregation Tab**
* **mongosh or MongoDB Shell**

---

## 18. MongoDB vs Relational Database - Complete Comparison

| **Concept** | **Relational Database** | **MongoDB** | **Description/Analogy** |
|-------------|-------------------------|-------------|-------------------------|
| **Database** | Database | Database | Both: Container for all data - like a library building |
| **Schema** | Fixed Schema (DDL) | Flexible Schema | **Relational**: Strict blueprint that must be followed<br>**MongoDB**: Flexible guidelines that can evolve |
| **Data Structure** | Tables | Collections | **Relational**: Organized shelves with identical slots<br>**MongoDB**: Themed sections with varied content |
| **Data Records** | Rows/Records | Documents | **Relational**: Standardized catalog cards<br>**MongoDB**: Flexible information packets (JSON-like) |
| **Data Fields** | Columns | Fields | **Relational**: Pre-defined categories<br>**MongoDB**: Customizable attributes |
| **Primary Key** | Primary Key | _id (ObjectId) | Both: Unique identifier for each record |
| **Relationships** | Foreign Keys + JOINs | Embedded Docs or References | **Relational**: Separate linked tables<br>**MongoDB**: Nested data or manual references |
| **Data Types** | Strict SQL Data Types | BSON Data Types | **Relational**: varchar, int, date, etc.<br>**MongoDB**: string, number, array, object, etc. |
| **Queries** | SQL | MQL (MongoDB Query Language) | **Relational**: SELECT, FROM, WHERE<br>**MongoDB**: find(), aggregate(), match() |
| **Transactions** | ACID Transactions | Multi-Document Transactions | Both support, but different implementation approaches |
| **Indexing** | B-Tree Indexes | Various Index Types | **Relational**: Standard B-tree<br>**MongoDB**: Compound, text, geospatial, etc. |
| **Scaling** | Vertical Scaling (Scale Up) | Horizontal Scaling (Scale Out) | **Relational**: Bigger, more powerful servers<br>**MongoDB**: More servers in cluster |
| **Clustering** | Master-Slave Replication | Replica Sets | **Relational**: Primary-secondary setup<br>**MongoDB**: Automatic failover groups |
| **Sharding** | Manual/Complex | Built-in Sharding | **Relational**: Difficult to partition<br>**MongoDB**: Automatic data distribution |
| **Consistency** | Strong Consistency | Eventual Consistency (configurable) | **Relational**: Immediate consistency<br>**MongoDB**: Tunable consistency levels |
| **Storage Engine** | Various (InnoDB, MyISAM) | WiredTiger, MMAPv1 | Different underlying storage mechanisms |
| **Joins** | Native SQL JOINs | $lookup (aggregation) | **Relational**: Efficient cross-table queries<br>**MongoDB**: Aggregation pipeline joins |
| **Normalization** | Normalized (1NF, 2NF, 3NF) | Denormalized | **Relational**: Data split across tables<br>**MongoDB**: Data often stored together |
| **Aggregation** | GROUP BY, HAVING | Aggregation Pipeline | **Relational**: SQL aggregation functions<br>**MongoDB**: Multi-stage data processing |
| **Views** | Database Views | Views | Both: Virtual tables/collections based on queries |
| **Stored Procedures** | Stored Procedures/Functions | Server-side JavaScript | **Relational**: SQL-based procedures<br>**MongoDB**: JavaScript functions |
| **Triggers** | Database Triggers | Change Streams | **Relational**: Automatic actions on data changes<br>**MongoDB**: Real-time change notifications |
| **Backup** | SQL Dumps, Binary Logs | mongodump, Filesystem Snapshots | Different backup and recovery strategies |
| **Administration** | Database Administrator (DBA) | Database Administrator | Both require administration, different skill sets |
| **Data Modeling** | Entity-Relationship (ER) | Document-Oriented | **Relational**: Tables with relationships<br>**MongoDB**: Nested document structures |
| **Use Cases** | OLTP, Complex Queries, Reports | Content Management, Real-time Analytics | **Relational**: Financial systems, ERP<br>**MongoDB**: Social media, IoT, catalogs |
| **Examples** | MySQL, PostgreSQL, Oracle, SQL Server | MongoDB, Amazon DocumentDB | Popular implementations of each approach |

### Key Decision Factors

**Choose Relational DB when:**
- Complex relationships between data
- Need ACID transactions
- Well-defined, stable schema
- Complex reporting and analytics
- Strong consistency requirements

**Choose MongoDB when:**
- Rapidly evolving data structures
- Need horizontal scaling
- Working with semi-structured data
- Agile development environment
- Geographic distribution requirements
