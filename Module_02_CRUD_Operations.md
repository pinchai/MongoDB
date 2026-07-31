# Module 2 -- CRUD Operations in MongoDB

This lesson introduces the basic **Create** and **Read** operations in
MongoDB using **MongoDB Shell (mongosh)**.

## Learning Objectives

After completing this lesson, students will be able to:

-   Understand CRUD operations
-   Insert a single document
-   Insert multiple documents
-   Retrieve documents
-   Retrieve one document
-   Display selected fields using Projection

## What is CRUD?

CRUD stands for the four basic database operations.

  Operation   MongoDB Method                  Description
  ----------- ------------------------------- --------------------
  Create      `insertOne()`, `insertMany()`   Add new documents
  Read        `find()`, `findOne()`           Retrieve documents
  Update      `updateOne()`, `updateMany()`   Modify documents
  Delete      `deleteOne()`, `deleteMany()`   Remove documents

In this lesson, we focus on **Create** and **Read**.

## Step 1: Create and Select a Database

Before performing any CRUD operations, create (or switch to) a database.

```javascript
use schoolDB
```

Verify the current database:

```javascript
db
```

> **Note:** MongoDB creates the database only after the first document is inserted.

## Step 2: Create a Collection

You can create a collection manually before inserting data.

```javascript
db.createCollection("students")
```

Verify the collection:

```javascript
show collections
```

After creating the collection, you can begin CRUD operations.

## Sample Collection

``` json
{
  "_id": "ObjectId(...)",
  "name": "John",
  "age": 20,
  "gender": "Male",
  "major": "IT",
  "gpa": 3.75
}
```

## insertOne()

**Syntax**

``` javascript
db.collection.insertOne(document)
```

**Example**

``` javascript
db.students.insertOne({
  name: "John",
  age: 20,
  gender: "Male",
  major: "IT",
  gpa: 3.75
})
```

Output:

``` javascript
{
  acknowledged: true,
  insertedId: ObjectId("...")
}
```

Another example:

``` javascript
db.students.insertOne({
  name: "Alice",
  age: 21,
  gender: "Female",
  major: "Computer Science",
  gpa: 3.90
})
```

## insertMany()

``` javascript
db.collection.insertMany([
  document1,
  document2,
  document3
])
```

Example:

``` javascript
db.students.insertMany([
  {
    name: "Bob",
    age: 19,
    gender: "Male",
    major: "Business",
    gpa: 3.20
  },
  {
    name: "Sophia",
    age: 22,
    gender: "Female",
    major: "Design",
    gpa: 3.60
  },
  {
    name: "David",
    age: 20,
    gender: "Male",
    major: "IT",
    gpa: 3.40
  }
])
```

## View All Documents

``` javascript
db.students.find()
```

Pretty output:

``` javascript
db.students.find().pretty()
```

## find()

Find all:

``` javascript
db.students.find()
```

Find IT students:

``` javascript
db.students.find({ major: "IT" })
```

Find students older than 20:

``` javascript
db.students.find({ age: { $gt: 20 } })
```

Find female students:

``` javascript
db.students.find({ gender: "Female" })
```

## findOne()

``` javascript
db.collection.findOne(query)
```

Example:

``` javascript
db.students.findOne({ name: "John" })
```

Find one IT student:

``` javascript
db.students.findOne({ major: "IT" })
```

## Projection

Syntax:

``` javascript
db.collection.find(query, projection)
```

Show only name:

``` javascript
db.students.find({}, { name: 1 })
```

Show name and GPA:

``` javascript
db.students.find({}, { name: 1, gpa: 1 })
```

Hide `_id`:

``` javascript
db.students.find({}, {
  _id: 0,
  name: 1,
  major: 1
})
```

Exclude GPA:

``` javascript
db.students.find({}, { gpa: 0 })
```

> You cannot mix inclusion (`1`) and exclusion (`0`) in the same
> projection, except for `_id`.

## find() vs findOne()

  find()                        findOne()
  ----------------------------- ---------------------------
  Returns multiple documents    Returns one document
  Returns a Cursor              Returns a single document
  Can iterate through results   Returns the first match

## Practice Exercises

1.  Insert one student named Emma (Age 22, Marketing, GPA 3.50).
2.  Insert three students using `insertMany()`.
3.  Display all students.
4.  Find all female students.
5.  Find students with GPA greater than 3.5.
6.  Display only Name and Major while hiding `_id`.
7.  Find one student named Alice.

## Summary

  Method           Purpose
  ---------------- -----------------------------
  `insertOne()`    Insert one document
  `insertMany()`   Insert multiple documents
  `find()`         Retrieve multiple documents
  `findOne()`      Retrieve one document
  Projection       Select displayed fields

## Homework

1.  Create a `books` collection.
2.  Insert 5 books using `insertMany()`.
3.  Display all books.
4.  Find one book by title.
5.  Show only the title and author fields.
6.  Hide `_id`.
7.  Find all books with a price greater than 20.
