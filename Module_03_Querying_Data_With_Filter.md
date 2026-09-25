# Module 3 — Querying Data with Filters in MongoDB

> **Topic:** MongoDB Querying  
> **Database:** `schoolDB`  
> **Collection:** `students`

---

# 1. Sample Collection

Before practicing the queries, create the database and sample data.

## 1.1 Select the Database

```javascript
use schoolDB
```

## 1.2 Insert Sample Students

```javascript
db.students.insertMany([
  {
    name: "dara",
    age: 20,
    score: 85,
    city: "Phnom Penh",
    subjects: ["Math", "English"],
    graduated: false
  },
  {
    name: "bora",
    age: 23,
    score: 92,
    city: "Siem Reap",
    subjects: ["Math", "Physics"],
    graduated: true
  },
  {
    name: "sopheak",
    age: 19,
    score: 78,
    city: "Battambang",
    subjects: ["Chemistry", "Biology"],
    graduated: false
  },
  {
    name: "vuthy",
    age: 22,
    score: 95,
    city: "Phnom Penh",
    subjects: ["Math", "Chemistry"],
    graduated: true
  },
  {
    name: "dana",
    age: 21,
    score: 88,
    city: "Kampot",
    subjects: ["English", "History"],
    graduated: false
  }
])
```

### Quick Check

```javascript
db.students.find()
```

---

# 2. Comparison Operators

Comparison operators are used when we want to compare a field with a value.

## 2.1 Operator Reference

| Operator | Meaning | Khmer | Example |
| --- | --- | --- | --- |
| `$eq` | Equal to | ស្មើនឹង | `age = 20` |
| `$ne` | Not equal to | មិនស្មើនឹង | `city ≠ "Phnom Penh"` |
| `$gt` | Greater than | ធំជាង | `score > 90` |
| `$gte` | Greater than or equal | ធំជាង ឬស្មើ | `score >= 88` |
| `$lt` | Less than | តូចជាង | `age < 21` |
| `$lte` | Less than or equal | តូចជាង ឬស្មើ | `age <= 20` |
| `$in` | Matches one of the values | ស្ថិតនៅក្នុងបញ្ជី | `city = Phnom Penh OR Kampot` |
| `$nin` | Does not match any listed value | មិនស្ថិតនៅក្នុងបញ្ជី | `city ≠ Phnom Penh` |

---

## 2.2 `$eq` — Equal To

Use `$eq` when a field must be equal to a specific value.

```javascript
db.students.find({
  age: { $eq: 20 }
})
```

**Meaning:** Find students whose age is exactly `20`.

---

## 2.3 `$ne` — Not Equal To

```javascript
db.students.find({
  city: { $ne: "Phnom Penh" }
})
```

**Meaning:** Find students whose city is not `"Phnom Penh"`.

---

## 2.4 `$gt` — Greater Than

```javascript
db.students.find({
  score: { $gt: 90 }
})
```

**Meaning:** Find students whose score is greater than `90`.

---

## 2.5 `$gte` — Greater Than or Equal

```javascript
db.students.find({
  score: { $gte: 88 }
})
```

**Meaning:** Find students whose score is `88` or higher.

---

## 2.6 `$lt` — Less Than

```javascript
db.students.find({
  age: { $lt: 21 }
})
```

**Meaning:** Find students younger than `21`.

---

## 2.7 `$lte` — Less Than or Equal

```javascript
db.students.find({
  age: { $lte: 20 }
})
```

**Meaning:** Find students whose age is `20` or younger.

---

## 2.8 `$in` — Match Any Value in a List

```javascript
db.students.find({
  city: { $in: ["Phnom Penh", "Kampot"] }
})
```

**Meaning:** Find students from either:

- `Phnom Penh`
- `Kampot`

### Think of `$in` as

```text
city = "Phnom Penh"
OR
city = "Kampot"
```

---

## 2.9 `$nin` — Not in a List

```javascript
db.students.find({
  city: { $nin: ["Phnom Penh"] }
})
```

**Meaning:** Find students whose city is not `"Phnom Penh"`.

---

# 3. Logical Operators

Logical operators allow us to combine or negate conditions.

## 3.1 Operator Reference

| Operator | Meaning | Khmer |
| --- | --- | --- |
| `$and` | All conditions must be true | លក្ខខណ្ឌទាំងអស់ត្រូវតែពិត |
| `$or` | At least one condition must be true | យ៉ាងហោចណាស់មួយលក្ខខណ្ឌត្រូវពិត |
| `$not` | Negates a condition | បដិសេធលក្ខខណ្ឌ |
| `$nor` | None of the conditions can be true | គ្មានលក្ខខណ្ឌណាមួយត្រូវពិត |

---

## 3.2 `$and` — All Conditions Must Be True

```javascript
db.students.find({
  $and: [
    { age: { $gt: 20 } },
    { score: { $gt: 90 } }
  ]
})
```

### Read it as

```text
age > 20
AND
score > 90
```

Both conditions must be true.

---

## 3.3 `$or` — At Least One Condition Must Be True

```javascript
db.students.find({
  $or: [
    { city: "Kampot" },
    { score: { $gt: 90 } }
  ]
})
```

### Read it as

```text
city = "Kampot"
OR
score > 90
```

Only one of the conditions needs to be true.

---

## 3.4 `$not` — Negate a Condition

```javascript
db.students.find({
  score: { $not: { $gt: 90 } }
})
```

**Meaning:** Find students whose score is **not greater than `90`**.

---

## 3.5 `$nor` — None of the Conditions Are True

```javascript
db.students.find({
  $nor: [
    { city: "Phnom Penh" },
    { city: "Kampot" }
  ]
})
```

**Meaning:** Find students who are:

```text
NOT from Phnom Penh
AND
NOT from Kampot
```

---

# 4. Array Operators

MongoDB can query fields that contain arrays.

In our sample data, `subjects` is an array:

```javascript
subjects: ["Math", "English"]
```

## 4.1 Array Operator Reference

| Operator | Description |
| --- | --- |
| `$all` | Array contains all specified values |
| `$elemMatch` | Matches an array element based on conditions |
| `$size` | Matches an array with a specific length |

---

## 4.2 Find an Array Element

```javascript
db.students.find({
  subjects: "Math"
})
```

**Meaning:** Find students whose `subjects` array contains `"Math"`.

---

## 4.3 `$all` — Contains All Values

```javascript
db.students.find({
  subjects: {
    $all: ["Math", "Chemistry"]
  }
})
```

**Meaning:** Find students whose `subjects` array contains **both**:

- `Math`
- `Chemistry`

---

## 4.4 `$size` — Check Array Length

```javascript
db.students.find({
  subjects: {
    $size: 2
  }
})
```

**Meaning:** Find students whose `subjects` array contains exactly `2` elements.

---

## 4.5 `$elemMatch` — Match an Array Element

First, create an order document:

```javascript
db.orders.insertOne({
  customer: "John",
  items: [
    { name: "Keyboard", price: 40 },
    { name: "Mouse", price: 20 }
  ]
})
```

Then query the `items` array:

```javascript
db.orders.find({
  items: {
    $elemMatch: {
      price: { $gt: 30 }
    }
  }
})
```

**Meaning:** Find orders where at least one item has a price greater than `30`.

---

# 5. Element Operators

Element operators allow us to check whether a field exists or what data type it contains.

## 5.1 Operator Reference

| Operator | Description |
| --- | --- |
| `$exists` | Checks whether a field exists |
| `$type` | Checks the data type of a field |

---

## 5.2 `$exists` — Check Whether a Field Exists

### Field exists

```javascript
db.students.find({
  score: { $exists: true }
})
```

**Meaning:** Find documents that contain the `score` field.

### Field does not exist

```javascript
db.students.find({
  phone: { $exists: false }
})
```

**Meaning:** Find documents that do not contain the `phone` field.

---

## 5.3 `$type` — Check Data Type

```javascript
db.students.find({
  age: { $type: "int" }
})
```

Find documents where `age` has the integer data type.

```javascript
db.students.find({
  city: { $type: "string" }
})
```

Find documents where `city` has the string data type.

---

# 6. Combining Multiple Filters

MongoDB allows us to put multiple field conditions in the same query.

## Example 1 — Score + City + Graduation Status

```javascript
db.students.find({
  score: { $gte: 85 },
  city: "Phnom Penh",
  graduated: true
})
```

### Read it as

```text
score >= 85
AND
city = "Phnom Penh"
AND
graduated = true
```

---

## Example 2 — Subject + Age + Score

```javascript
db.students.find({
  subjects: "Math",
  age: { $lt: 23 },
  score: { $gt: 80 }
})
```

### Read it as

```text
studies Math
AND
age < 23
AND
score > 80
```

---

# 7. Practice Exercises

> **Goal:** Write the MongoDB query for each requirement.  
> **Important:** Try to solve the exercises yourself before looking at the answer key.

---

## Exercise 1 — Basic Comparison

Find students whose score is greater than `80`.

**Operator:** `$gt`

```javascript
// Write your query here
```

---

## Exercise 2 — Less Than

Find students younger than `21`.

**Operator:** `$lt`

```javascript
// Write your query here
```

---

## Exercise 3 — Exact Match

Find students from `Phnom Penh`.

```javascript
// Write your query here
```

---

## Exercise 4 — Not Equal

Find students whose city is not `Kampot`.

**Operator:** `$ne`

```javascript
// Write your query here
```

---

## Exercise 5 — Range Query

Find students with scores between `80` and `90`, including both `80` and `90`.

**Hint:** Use `$gte` and `$lte`.

```javascript
// Write your query here
```

---

## Exercise 6 — Array Query

Find students studying `English`.

```javascript
// Write your query here
```

---

## Exercise 7 — Array Contains All

Find students studying both `Math` and `English`.

**Hint:** Use `$all`.

```javascript
// Write your query here
```

---

## Exercise 8 — Boolean Filter

Find students who have graduated.

```javascript
// Write your query here
```

---

## Exercise 9 — Multiple Possible Values

Find students from either `Phnom Penh` or `Kampot`.

**Hint:** Use `$in` or `$or`.

```javascript
// Write your query here
```

---

## Exercise 10 — Field Does Not Exist

Find documents that do not have a `phone` field.

**Hint:** Use `$exists`.

```javascript
// Write your query here
```

---

# 8. Challenge Exercises

These exercises require you to combine multiple filters.

## Challenge 1 — Age + Score

Find students who:

- are older than `20`
- AND have a score greater than `85`

```javascript
// Write your query here
```

---

## Challenge 2 — City + Graduation

Find students who:

- live in `Phnom Penh`
- AND have graduated

```javascript
// Write your query here
```

---

## Challenge 3 — Subject + Score

Find students who:

- study `Math`
- AND have a score greater than `90`

```javascript
// Write your query here
```

---

## Challenge 4 — Multiple Conditions

Find students who:

- are younger than `23`
- AND have a score greater than `80`
- AND study `Math`

```javascript
// Write your query here
```

---

## Challenge 5 — `$or`

Find students who:

- are from `Kampot`
- OR have a score greater than `90`

```javascript
// Write your query here
```

---

## Challenge 6 — `$and`

Find students who:

- are older than `20`
- AND have a score greater than `90`

Use the `$and` operator explicitly.

```javascript
// Write your query here
```

---

## Challenge 7 — `$nor`

Find students who are:

- NOT from `Phnom Penh`
- AND NOT from `Kampot`

Use `$nor`.

```javascript
// Write your query here
```

---

## Challenge 8 — Array Size

Find students who have exactly `2` subjects.

**Operator:** `$size`

```javascript
// Write your query here
```

---

## Challenge 9 — `$all`

Find students whose subjects contain both:

- `Math`
- `Chemistry`

```javascript
// Write your query here
```

---

## Challenge 10 — Combined Query

Find students who:

- are from `Phnom Penh`
- AND have a score of at least `85`
- AND have graduated

```javascript
// Write your query here
```

---

# 9. Mini Lab — Student Filtering

## Task

Using the `students` collection, create **10 MongoDB queries**.

Your queries must include:

| Requirement | Operator |
| --- | --- |
| Greater than | `$gt` |
| Less than | `$lt` |
| Greater than or equal | `$gte` |
| Not equal | `$ne` |
| Multiple values | `$in` |
| AND conditions | `$and` |
| OR conditions | `$or` |
| Array contains all | `$all` |
| Array size | `$size` |
| Field existence | `$exists` |

### Rules

1. Do not copy the examples from the lesson.
2. Use different field values where possible.
3. Write the query first.
4. Run the query in MongoDB.
5. Check whether the returned documents match your requirement.

---

# 10. Exercise Answer Key

> **Teacher section:** Students should attempt the exercises before reading this section.

## Exercise 1

```javascript
db.students.find({
  score: { $gt: 80 }
})
```

## Exercise 2

```javascript
db.students.find({
  age: { $lt: 21 }
})
```

## Exercise 3

```javascript
db.students.find({
  city: "Phnom Penh"
})
```

## Exercise 4

```javascript
db.students.find({
  city: { $ne: "Kampot" }
})
```

## Exercise 5

```javascript
db.students.find({
  score: {
    $gte: 80,
    $lte: 90
  }
})
```

## Exercise 6

```javascript
db.students.find({
  subjects: "English"
})
```

## Exercise 7

```javascript
db.students.find({
  subjects: {
    $all: ["Math", "English"]
  }
})
```

## Exercise 8

```javascript
db.students.find({
  graduated: true
})
```

## Exercise 9

```javascript
db.students.find({
  city: {
    $in: ["Phnom Penh", "Kampot"]
  }
})
```

## Exercise 10

```javascript
db.students.find({
  phone: { $exists: false }
})
```

---

# 11. Challenge Answer Key

## Challenge 1

```javascript
db.students.find({
  age: { $gt: 20 },
  score: { $gt: 85 }
})
```

## Challenge 2

```javascript
db.students.find({
  city: "Phnom Penh",
  graduated: true
})
```

## Challenge 3

```javascript
db.students.find({
  subjects: "Math",
  score: { $gt: 90 }
})
```

## Challenge 4

```javascript
db.students.find({
  age: { $lt: 23 },
  score: { $gt: 80 },
  subjects: "Math"
})
```

## Challenge 5

```javascript
db.students.find({
  $or: [
    { city: "Kampot" },
    { score: { $gt: 90 } }
  ]
})
```

## Challenge 6

```javascript
db.students.find({
  $and: [
    { age: { $gt: 20 } },
    { score: { $gt: 90 } }
  ]
})
```

## Challenge 7

```javascript
db.students.find({
  $nor: [
    { city: "Phnom Penh" },
    { city: "Kampot" }
  ]
})
```

## Challenge 8

```javascript
db.students.find({
  subjects: { $size: 2 }
})
```

## Challenge 9

```javascript
db.students.find({
  subjects: {
    $all: ["Math", "Chemistry"]
  }
})
```

## Challenge 10

```javascript
db.students.find({
  city: "Phnom Penh",
  score: { $gte: 85 },
  graduated: true
})
```

---

# 12. Quick Operator Cheat Sheet

## Comparison

| Operator | Use |
| --- | --- |
| `$eq` | Equal |
| `$ne` | Not equal |
| `$gt` | Greater than |
| `$gte` | Greater than or equal |
| `$lt` | Less than |
| `$lte` | Less than or equal |
| `$in` | Match one of several values |
| `$nin` | Exclude several values |

## Logical

| Operator | Use |
| --- | --- |
| `$and` | All conditions must be true |
| `$or` | At least one condition is true |
| `$not` | Negate a condition |
| `$nor` | None of the conditions are true |

## Array

| Operator | Use |
| --- | --- |
| `$all` | Array contains all specified values |
| `$elemMatch` | Match an array element |
| `$size` | Match array length |

## Element

| Operator | Use |
| --- | --- |
| `$exists` | Check whether a field exists |
| `$type` | Check field data type |

---

# 13. Summary

In this module, we learned how to filter MongoDB documents using different types of operators.

### The main groups are

```text
Comparison
├── $eq
├── $ne
├── $gt
├── $gte
├── $lt
├── $lte
├── $in
└── $nin

Logical
├── $and
├── $or
├── $not
└── $nor

Array
├── $all
├── $elemMatch
└── $size

Element
├── $exists
└── $type
```

---

# Module 03 — MongoDB Lab Exercises

## Querying Data with Filters

**Database:** `schoolDB`
**Collection:** `students`

---

## Part 1 — Basic Filtering

### Question 1

Find all students whose age is exactly `20`.

### Question 2

Find students whose score is greater than `90`.

### Question 3

Find students whose score is greater than or equal to `88`.

### Question 4

Find students whose age is less than `21`.

### Question 5

Find students whose age is less than or equal to `21`.

---

## Part 2 — Not Equal and Multiple Values

### Question 6

Find students who are not from `Phnom Penh`.

### Question 7

Find students who are from either `Phnom Penh` or `Siem Reap`.

### Question 8

Find students who are not from `Phnom Penh` or `Kampot`.

---

## Part 3 — Range Queries

### Question 9

Find students whose score is between `80` and `90`, including both values.

### Question 10

Find students whose age is between `20` and `22`.

### Question 11

Find students whose score is greater than `85` but less than `95`.

---

## Part 4 — Logical Operators

### Question 12

Find students who are older than `20` **AND** have a score greater than `90`.

### Question 13

Find students who are from `Kampot` **OR** have a score greater than `90`.

### Question 14

Find students who are younger than `23` **AND** have a score greater than `80`.

### Question 15

Find students who are younger than `23`, have a score greater than `80`, and study `Math`.

### Question 16

Find students whose score is **not greater than `90`**.

### Question 17

Find students who are **not** from `Phnom Penh` and **not** from `Kampot`.

---

## Part 5 — Array Queries

### Question 18

Find all students who study `Math`.

### Question 19

Find all students who study `English`.

### Question 20

Find students who study both `Math` and `Chemistry`.

### Question 21

Find students whose subjects contain both `English` and `History`.

### Question 22

Find students who have exactly `2` subjects.

---

## Part 6 — Field Existence

### Question 23

Find students that have the `score` field.

### Question 24

Find students that do not have a `phone` field.

### Question 25

Find students that do not have an `email` field.

---

## Part 7 — Data Type Queries

### Question 26

Find documents where the `age` field has the integer data type.

### Question 27

Find documents where the `city` field has the string data type.

---

## Part 8 — Combined Filters

### Question 28

Find students who live in `Phnom Penh` and have a score greater than `80`.

### Question 29

Find students who live in `Phnom Penh` and have graduated.

### Question 30

Find students who study `Math` and have a score greater than `90`.

### Question 31

Find students who are older than `20`, have a score greater than `85`, and have graduated.

### Question 32

Find students who live in `Phnom Penh`, study `Math`, and have a score greater than `80`.

---

# Part 9 — Challenge Questions

### Challenge 1

Find students whose score is greater than or equal to `90`.

### Challenge 2

Find students whose age is less than `22` and who have not graduated.

### Challenge 3

Find students from `Phnom Penh`, `Siem Reap`, or `Kampot`.

### Challenge 4

Find students whose city is not `Battambang`.

### Challenge 5

Find students whose score is greater than `80` and less than `95`.

### Challenge 6

Find students who study both `Math` and `Chemistry`.

### Challenge 7

Find students who are older than `20`, have a score of at least `88`, and have graduated.

### Challenge 8

Find students who are from `Kampot` or have a score of at least `95`.

---

# Part 10 — `$elemMatch` Questions

Use the following `orders` collection:

```javascript
db.orders.insertMany([
  {
    customer: "John",
    items: [
      { name: "Keyboard", price: 40 },
      { name: "Mouse", price: 20 }
    ]
  },
  {
    customer: "Dara",
    items: [
      { name: "Monitor", price: 150 },
      { name: "Mouse", price: 25 }
    ]
  },
  {
    customer: "Bora",
    items: [
      { name: "Keyboard", price: 35 },
      { name: "Headset", price: 60 }
    ]
  }
])
```

### Question 33

Find orders where at least one item has a price greater than `30`.

### Question 34

Find orders where at least one item is a `Keyboard` with a price greater than `30`.

### Question 35

Find orders where at least one item is a `Monitor` with a price greater than `100`.

---

# Part 11 — Real-World Student Search

### Question 36

Find all high-performing students whose score is at least `90`.

### Question 37

Find all students whose score is less than `80`.

### Question 38

Find graduated students from `Phnom Penh`.

### Question 39

Find students from `Phnom Penh` or `Siem Reap` who have a score greater than `85`.

### Question 40

Find students who study `Math` and have graduated.

### Question 41

Find students who are not from `Phnom Penh` and not from `Kampot`.

---
