# MongoDB Learning Path (Beginner to Advanced)

## Learning Roadmap

  ------------------------------------------------------------------------------
  Step             Topic                     Description
  ---------------- ------------------------- -----------------------------------
  1                Introduction to MongoDB   Understand NoSQL, Document
                                             Database, Collections, Documents

  2                Connect to MongoDB        Start `mongod`, open `mongosh`,
                                             connect to the server

  3                `show dbs`                Display all databases

  4                `use database_name`       Create or switch databases

  5                `show collections`        Display collections in the current
                                             database

  6                `db.createCollection()`   Create a collection manually
                   *(Optional)*              

  7                `insertOne()`             Insert a single document

  8                `insertMany()`            Insert multiple documents

  9                `find()`                  Retrieve matching documents

  10               `findOne()`               Retrieve the first matching
                                             document

  11               Projection                Return only selected fields

  12               Comparison Operators      `$gt`, `$lt`, `$gte`, `$lte`,
                                             `$eq`, `$ne`

  13               Logical Operators         `$and`, `$or`, `$not`, `$nor`

  14               Array Operators           `$in`, `$nin`, `$all`

  15               Element Operators         `$exists`, `$type`

  16               `sort()`                  Sort query results

  17               `limit()`                 Limit returned documents

  18               `skip()`                  Pagination

  19               `countDocuments()`        Count matching documents

  20               `updateOne()`             Update one document

  21               `updateMany()`            Update many documents

  22               Update Operators          `$set`, `$unset`, `$inc`,
                                             `$rename`, `$push`, `$pull`

  23               `replaceOne()`            Replace an entire document

  24               `deleteOne()`             Delete one document

  25               `deleteMany()`            Delete many documents

  26               `drop()`                  Delete a collection

  27               `dropDatabase()`          Delete a database

  28               Indexes                   Create and manage indexes

  29               Aggregation Framework     `aggregate()` pipeline

  30               Text Search               `$text` queries

  31               Data Validation           Schema validation

  32               Backup & Restore          `mongodump`, `mongorestore`

  33               Authentication & Security Users and roles

  34               Transactions *(Advanced)* Multi-document transactions

  35               MongoDB Compass           GUI management

  36               MongoDB Driver            Python, Node.js, C#, Java, etc.
  ------------------------------------------------------------------------------

------------------------------------------------------------------------

# Course Modules

## Module 1 -- MongoDB Basics

-   What is MongoDB?
-   NoSQL vs SQL
-   Documents
-   Collections
-   Databases

## Module 2 -- CRUD Operations

-   `insertOne()`
-   `insertMany()`
-   `find()`
-   `findOne()`
-   Projection

## Module 3 -- Querying Data

-   Comparison operators
-   Logical operators
-   Array operators
-   Element operators

## Module 4 -- Sorting & Pagination

-   `sort()`
-   `limit()`
-   `skip()`
-   `countDocuments()`

## Module 5 -- Updating Data

-   `updateOne()`
-   `updateMany()`
-   `$set`
-   `$inc`
-   `$unset`
-   `$rename`
-   `$push`
-   `$pull`

## Module 6 -- Deleting Data

-   `deleteOne()`
-   `deleteMany()`
-   `drop()`
-   `dropDatabase()`

## Module 7 -- Indexes

-   Single-field indexes
-   Compound indexes
-   Unique indexes
-   Text indexes

## Module 8 -- Aggregation

-   `$match`
-   `$group`
-   `$project`
-   `$sort`
-   `$limit`
-   `$lookup`
-   `$unwind`

## Module 9 -- Administration

-   Backup
-   Restore
-   Authentication
-   Roles
-   Security

## Module 10 -- Real Projects

-   Student Management System
-   Library Management System
-   Inventory System
-   E-commerce Database
-   Blog Database
-   Employee Management System

------------------------------------------------------------------------

# Final Learning Roadmap

``` text
Introduction
      │
      ▼
Installation
      │
      ▼
Database Commands
(show dbs → use → show collections)
      │
      ▼
CRUD
(Insert → Read → Update → Delete)
      │
      ▼
Query Operators
($gt, $lt, $in, $or...)
      │
      ▼
Sorting & Pagination
(sort → limit → skip)
      │
      ▼
Indexes
      │
      ▼
Aggregation Framework
      │
      ▼
Security
      │
      ▼
Backup & Restore
      │
      ▼
Application Development
(Python / Node.js / C#)
      │
      ▼
Real Projects
```
