# Module 1 – MongoDB Basics

## 🎯 Learning Objectives

By the end of this module, students will be able to:

- Understand what MongoDB is.
- Explain the difference between SQL and NoSQL databases.
- Understand Databases, Collections, and Documents.
- Create and visualize MongoDB data structure.
- Know when MongoDB is a good choice.

---

# 1. What is MongoDB?

MongoDB is a **NoSQL document database** that stores data in **JSON-like documents (BSON)** instead of tables.

It is designed for:

- High performance
- High availability
- Scalability
- Flexible schema

Unlike traditional databases, MongoDB does **not require predefined table structures**.

## Example

```json
{
  "_id": "ObjectId(...)",
  "name": "John",
  "age": 20
}
```

Each student is a **Document**.

## Why MongoDB?

- Flexible schema
- Fast development
- Easy to scale
- Stores complex data easily
- Supports nested objects and arrays
- Excellent for modern web applications

## SQL vs NoSQL

| SQL | MongoDB (NoSQL) |
|------|-----------------|
| Table | Collection |
| Row | Document |
| Column | Field |
| Schema required | Flexible schema |
| JOIN supported | Embedding / Reference |
| Uses SQL | MongoDB Query Language |

## MongoDB Structure

```text
Database
└── Collection
    └── Document
        └── Field
```

## Database

A **Database** contains one or more **Collections**.

Example:

- students
- teachers
- courses
- payments

## Collection

A **Collection** stores many documents.

## Document

A **Document** is a single record stored in BSON format.

```json
{
  "_id": "ObjectId(...)",
  "name": "Alice",
  "age": 22,
  "address": {
    "city": "Phnom Penh",
    "country": "Cambodia"
  }
}
```

## Fields

Each key inside a document is called a **Field**.

Example:

```json
{
  "name": "John",
  "age": 20,
  "email": "john@gmail.com"
}
```

Fields:

- name
- age
- email

## MongoDB Hierarchy

```text
MongoDB Server
│
└── Database
    └── Collection
        └── Document
            └── Field
```

## SQL vs MongoDB Mapping

| SQL | MongoDB |
|------|----------|
| Database | Database |
| Table | Collection |
| Row | Document |
| Column | Field |
| Primary Key | _id |
| Foreign Key | Reference / Embedded Document |

## Summary

Students should understand:

- What MongoDB is
- NoSQL vs SQL
- Database
- Collection
- Document
- Field
- MongoDB hierarchy
- Flexible schema

---

# 📝 Practice Exercises

1. What is MongoDB?
2. Explain SQL vs NoSQL.
3. What is a Collection?
4. What is a Document?
5. What is a Field?
6. Draw the MongoDB hierarchy.
7. Match: Table→?, Row→?, Column→?
8. Create a student JSON document.
9. Create a document with an address object.
10. Create a document with an array of programming languages.

---

# 🎯 Quiz

## Multiple Choice

1. MongoDB stores data as:
   - A. Tables
   - **B. Documents ✅**
   - C. Rows
   - D. Sheets

2. A Collection is similar to:
   - A. Row
   - **B. Table ✅**
   - C. Column
   - D. Database

3. MongoDB is a:
   - A. Relational Database
   - **B. NoSQL Database ✅**
   - C. Graph Database
   - D. Spreadsheet

4. Every MongoDB document contains:
   - A. id
   - **B. _id ✅**
   - C. key
   - D. uuid

5. MongoDB stores documents internally as:
   - A. XML
   - B. CSV
   - **C. BSON ✅**
   - D. TXT

## True / False

1. MongoDB requires a fixed schema. **False**
2. A Collection contains Documents. **True**
3. A Database contains Collections. **True**
4. A Document can contain nested objects. **True**
5. MongoDB uses tables and rows. **False**

---

# 💻 Lab Activity

Create a `students` collection with five documents.

```json
{
  "name": "",
  "age": 0,
  "gender": "",
  "phone": "",
  "skills": [],
  "address": {
    "city": "",
    "country": ""
  }
}
```

**Challenge:** Add an extra field such as `email`, `grade`, or `hobbies` to demonstrate MongoDB's flexible schema.
