# Module 4 — Sorting & Counting

This module focuses only on the requested MongoDB methods:

- `sort()`
- `limit()`
- `skip()`
- `countDocuments()`

## 1. Learning Objectives

After completing this module, students should be able to:

1. Sort MongoDB documents in ascending and descending order.
2. Limit the number of returned documents.
3. Skip a specific number of documents.
4. Count documents in a collection.
5. Combine `sort()`, `skip()`, and `limit()` for pagination.
6. Build practical queries for displaying ranked or paginated data.

---

## 2. Sample Collection

We will use a `students` collection.

```javascript
db.students.insertMany([
  {
    name: "Dara",
    age: 20,
    score: 85,
    city: "Phnom Penh"
  },
  {
    name: "Sokha",
    age: 22,
    score: 92,
    city: "Kampot"
  },
  {
    name: "Vanna",
    age: 19,
    score: 78,
    city: "Siem Reap"
  },
  {
    name: "Rina",
    age: 21,
    score: 95,
    city: "Phnom Penh"
  },
  {
    name: "Bora",
    age: 23,
    score: 88,
    city: "Kandal"
  }
])
```

Check the data:

```javascript
db.students.find()
```

---

## 3. `sort()`

The `sort()` method is used to control the order of documents returned by a query.

### Syntax

```javascript
db.collection.find().sort({ field: 1 })
```

or

```javascript
db.collection.find().sort({ field: -1 })
```

### Sorting Values

| Value | Meaning |
|---:|---|
| `1` | Ascending |
| `-1` | Descending |

### 3.1 Sort Ascending

Sort students by `score` from lowest to highest:

```javascript
db.students.find().sort({ score: 1 })
```

Result order:

```text
Vanna   78
Dara    85
Bora    88
Sokha   92
Rina    95
```

### 3.2 Sort Descending

Sort students by score from highest to lowest:

```javascript
db.students.find().sort({ score: -1 })
```

Result:

```text
Rina    95
Sokha   92
Bora    88
Dara    85
Vanna   78
```

### Practical Example

Find the highest-scoring students first:

```javascript
db.students.find().sort({ score: -1 })
```

---

## 4. `limit()`

The `limit()` method controls the maximum number of documents returned.

### Syntax

```javascript
db.collection.find().limit(number)
```

For example:

```javascript
db.students.find().limit(3)
```

This returns only **3 documents**.

### 4.1 Top 3 Students

Combine `sort()` and `limit()`:

```javascript
db.students
  .find()
  .sort({ score: -1 })
  .limit(3)
```

Result:

```text
Rina    95
Sokha   92
Bora    88
```

This is useful for queries such as:

- Top 3 students
- Top 5 products
- Latest 10 orders
- Highest 10 scores

---

## 5. `skip()`

The `skip()` method skips a specified number of documents.

### Syntax

```javascript
db.collection.find().skip(number)
```

Example:

```javascript
db.students.find().skip(2)
```

MongoDB skips the first two documents and returns the remaining documents.

---

## 6. Combining `sort()`, `skip()`, and `limit()`

These methods become especially useful when creating **pagination**.

Example:

```javascript
db.students
  .find()
  .sort({ score: -1 })
  .skip(2)
  .limit(2)
```

The process is:

```text
All Students
     ↓
Sort by score DESC
     ↓
95
92
88
85
78
     ↓
Skip 2
     ↓
88
85
78
     ↓
Limit 2
     ↓
88
85
```

So the result contains:

```text
Bora    88
Dara    85
```

---

## 7. Pagination

A common use of `skip()` and `limit()` is pagination.

Suppose we display **2 students per page**.

### Page 1

```javascript
db.students
  .find()
  .sort({ score: -1 })
  .skip(0)
  .limit(2)
```

Result:

```text
Rina
Sokha
```

### Page 2

```javascript
db.students
  .find()
  .sort({ score: -1 })
  .skip(2)
  .limit(2)
```

Result:

```text
Bora
Dara
```

### Page 3

```javascript
db.students
  .find()
  .sort({ score: -1 })
  .skip(4)
  .limit(2)
```

Result:

```text
Vanna
```

### Pagination Formula

If:

```text
page = 3
limit = 2
```

Then:

```text
skip = (page - 1) × limit
```

Therefore:

```text
skip = (3 - 1) × 2
     = 4
```

MongoDB query:

```javascript
db.students
  .find()
  .sort({ score: -1 })
  .skip(4)
  .limit(2)
```

---

## 8. `countDocuments()`

The `countDocuments()` method counts documents that match a query.

### Syntax

```javascript
db.collection.countDocuments()
```

Count all students:

```javascript
db.students.countDocuments()
```

Result:

```text
5
```

### 8.1 Count With a Filter

Count students whose score is greater than `80`:

```javascript
db.students.countDocuments({
  score: { $gt: 80 }
})
```

Result:

```text
4
```

Because these students have scores above 80:

```text
Dara    85
Sokha   92
Rina    95
Bora    88
```

### 8.2 Count Students From a City

```javascript
db.students.countDocuments({
  city: "Phnom Penh"
})
```

Result:

```text
2
```

---

## 9. Combining Query Methods

These methods can be combined to create useful queries.

### Example 1 — Top 3 Scores

```javascript
db.students
  .find()
  .sort({ score: -1 })
  .limit(3)
```

### Example 2 — Lowest 2 Scores

```javascript
db.students
  .find()
  .sort({ score: 1 })
  .limit(2)
```

### Example 3 — Second Page

```javascript
db.students
  .find()
  .sort({ score: -1 })
  .skip(2)
  .limit(2)
```

### Example 4 — Count Students Older Than 20

```javascript
db.students.countDocuments({
  age: { $gt: 20 }
})
```

### Example 5 — Count Students in Phnom Penh

```javascript
db.students.countDocuments({
  city: "Phnom Penh"
})
```

---

## 10. Method Summary

| Method | Purpose | Example |
|---|---|---|
| `sort()` | Order documents | `.sort({score: -1})` |
| `limit()` | Restrict results | `.limit(5)` |
| `skip()` | Skip documents | `.skip(10)` |
| `countDocuments()` | Count matching documents | `.countDocuments({age: 20})` |

### Common Combination

```javascript
db.students
  .find()
  .sort({ score: -1 })
  .skip(10)
  .limit(5)
```

Meaning:

> Sort students by score from highest to lowest, skip the first 10 students, then return the next 5 students.

---

## 11. Practice Exercise

Using the `students` collection, write MongoDB queries for the following:

### Exercise 1
Display all students sorted by `age` from youngest to oldest.

### Exercise 2
Display all students sorted by `score` from highest to lowest.

### Exercise 3
Display only the top 3 students by score.

### Exercise 4
Display only the 2 students with the lowest scores.

### Exercise 5
Skip the first 2 students and display the remaining students.

### Exercise 6
Sort students by score descending, skip the first 2 students, and display the next 2 students.

### Exercise 7
Count all students.

### Exercise 8
Count students whose score is greater than `80`.

### Exercise 9
Count students from `"Phnom Penh"`.

### Exercise 10
Create a pagination query that displays **3 students per page**.

Write queries for:

```text
Page 1
Page 2
Page 3
```

---

## 12. Key Points to Remember

```text
sort()
   ↓
Controls the order

limit()
   ↓
Controls how many documents are returned

skip()
   ↓
Skips documents before returning results

countDocuments()
   ↓
Counts documents matching a condition
```

### Quick Reference

| Method | Main Use |
|---|---|
| `sort()` | Sort query results |
| `limit()` | Limit returned documents |
| `skip()` | Skip documents |
| `countDocuments()` | Count matching documents |

A common pagination pattern is:

```javascript
db.students
  .find()
  .sort({ score: -1 })
  .skip((page - 1) * limit)
  .limit(limit)
```
