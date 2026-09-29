# Module 6 — Deleting Data

## Learning Objectives

By the end of this module, students should be able to:

- Explain how MongoDB deletes data.
- Use `deleteOne()` to delete one document.
- Use `deleteMany()` to delete multiple documents.
- Use `drop()` to delete a collection.
- Use `dropDatabase()` to delete a database.
- Understand the difference between deleting documents, collections, and databases.
- Apply safe practices when using delete commands.

---

## 1. `deleteOne()`

`deleteOne()` is used to delete **one document** that matches a condition.

### Syntax

```javascript
db.collection.deleteOne(filter)
```

### Example

Suppose the `students` collection contains:

```javascript
db.students.find()
```

Example data:

```text
[
  { name: "Dara", age: 20, city: "Phnom Penh" },
  { name: "Sokha", age: 21, city: "Kampot" },
  { name: "Rina", age: 22, city: "Siem Reap" }
]
```

Delete the student named `Dara`:

```javascript
db.students.deleteOne({
  name: "Dara"
})
```

Possible result:

```javascript
{
  acknowledged: true,
  deletedCount: 1
}
```

### Important

If multiple documents match the condition, `deleteOne()` deletes **only one** matching document.

Example:

```javascript
db.students.deleteOne({
  age: 20
})
```

---

## 2. `deleteMany()`

`deleteMany()` is used to delete **all documents that match a condition**.

### Syntax

```javascript
db.collection.deleteMany(filter)
```

### Example

Delete all students from Kampot:

```javascript
db.students.deleteMany({
  city: "Kampot"
})
```

Possible result:

```javascript
{
  acknowledged: true,
  deletedCount: 3
}
```

If three documents match the condition, all three are deleted.

### Another Example

Delete students whose age is greater than 25:

```javascript
db.students.deleteMany({
  age: { $gt: 25 }
})
```

---

## 3. `drop()`

`drop()` is used to **delete an entire collection**.

### Syntax

```javascript
db.collection.drop()
```

### Example

```javascript
db.students.drop()
```

This removes the entire `students` collection and all documents inside it.

Possible result:

```text
true
```

After that, check the collections:

```javascript
show collections
```

The `students` collection will no longer appear.

### Important Difference

These two commands have different effects:

```javascript
db.students.deleteMany({})
```

This deletes **all documents**, but keeps the collection.

```javascript
db.students.drop()
```

This deletes the **entire collection**.

---

## 4. `dropDatabase()`

`dropDatabase()` is used to delete an **entire database**.

### Syntax

```javascript
db.dropDatabase()
```

### Example

Select the database:

```javascript
use school
```

Then delete it:

```javascript
db.dropDatabase()
```

Possible result:

```javascript
{
  ok: 1,
  dropped: "school"
}
```

The entire `school` database is removed, including its collections and documents.

---

## 5. Comparison

| Command | Deletes | Example |
|---|---|---|
| `deleteOne()` | One document | `db.students.deleteOne({...})` |
| `deleteMany()` | Multiple documents | `db.students.deleteMany({...})` |
| `drop()` | Entire collection | `db.students.drop()` |
| `dropDatabase()` | Entire database | `db.dropDatabase()` |

---

## 6. Important Safety Rules

Be careful when using delete commands.

### Delete One Document

```javascript
db.students.deleteOne({
  name: "Dara"
})
```

Deletes one matching document.

### Delete Multiple Documents

```javascript
db.students.deleteMany({
  city: "Phnom Penh"
})
```

Deletes all matching documents.

### Delete All Documents

```javascript
db.students.deleteMany({})
```

**Warning:** The empty filter `{}` matches every document in the collection.

### Delete a Collection

```javascript
db.students.drop()
```

**Warning:** The entire collection is removed.

### Delete a Database

```javascript
db.dropDatabase()
```

**Warning:** The entire current database is removed.

---

## 7. Recommended Practice

Create a test database:

```javascript
use school
```

Create sample data:

```javascript
db.students.insertMany([
  {
    name: "Dara",
    age: 20,
    city: "Phnom Penh"
  },
  {
    name: "Sokha",
    age: 21,
    city: "Kampot"
  },
  {
    name: "Rina",
    age: 22,
    city: "Kampot"
  },
  {
    name: "Vanna",
    age: 25,
    city: "Siem Reap"
  }
])
```

### Exercise 1

Delete the student named `Dara`.

```javascript
// Write your command
```

### Exercise 2

Delete all students from `Kampot`.

```javascript
// Write your command
```

### Exercise 3

Create a collection called `test`.

```javascript
// Write your command
```

Then delete the collection using:

```javascript
// Write your command
```

### Exercise 4

Create a database called `test_school`, add one collection, then delete the entire database.

```javascript
// Write your commands
```

### Exercise 5 — Challenge

Insert 5 students and then:

1. Delete one student.
2. Delete students older than 20.
3. Display the remaining documents.
4. Delete the collection.
5. Verify that the collection no longer exists.

---

## Summary

MongoDB provides several commands for deleting data at different levels:

- `deleteOne()` → deletes one document.
- `deleteMany()` → deletes multiple matching documents.
- `drop()` → deletes an entire collection.
- `dropDatabase()` → deletes an entire database.

Always verify your filter before running a delete operation, especially when using `{}` or commands that remove entire collections or databases.
