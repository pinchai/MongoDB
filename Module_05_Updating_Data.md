# Module 5 — Updating Data

## Learning Objectives

By the end of this module, students should be able to:

- Use `updateOne()` to update a single document.
- Use `updateMany()` to update multiple documents.
- Use `$set` to change field values.
- Use `$inc` to increase or decrease numeric values.
- Use `$unset` to remove fields.
- Use `$rename` to rename fields.
- Use `$push` to add values to arrays.
- Use `$pull` to remove values from arrays.

---

## 1. `updateOne()`

`updateOne()` is used to update **one document** that matches a filter.

### Syntax

```javascript
db.collection.updateOne(
  { filter },
  { update }
)
```

### Example

```javascript
db.students.updateOne(
  { name: "Dara" },
  { $set: { age: 21 } }
)
```

Only one matching document is updated.

---

## 2. `updateMany()`

`updateMany()` is used to update **multiple documents** that match a filter.

### Example

```javascript
db.students.updateMany(
  { city: "Phnom Penh" },
  { $set: { status: "active" } }
)
```

Every matching student receives:

```javascript
status: "active"
```

---

## 3. `$set`

`$set` changes the value of a field.

### Example

```javascript
db.students.updateOne(
  { name: "Dara" },
  {
    $set: {
      age: 22,
      city: "Kampot"
    }
  }
)
```

### Before

```javascript
{
  name: "Dara",
  age: 21,
  city: "Phnom Penh"
}
```

### After

```javascript
{
  name: "Dara",
  age: 22,
  city: "Kampot"
}
```

If the field does not exist, `$set` creates it.

---

## 4. `$inc`

`$inc` increases or decreases a numeric value.

### Increase

```javascript
db.students.updateOne(
  { name: "Dara" },
  { $inc: { score: 5 } }
)
```

If:

```text
score = 80
```

After the update:

```text
score = 85
```

### Decrease

```javascript
db.students.updateOne(
  { name: "Dara" },
  { $inc: { score: -5 } }
)
```

---

## 5. `$unset`

`$unset` removes a field from a document.

### Example

```javascript
db.students.updateOne(
  { name: "Dara" },
  { $unset: { temporaryField: "" } }
)
```

The `temporaryField` is removed.

> The value assigned to `$unset` is normally `""`; the important part is the field name.

---

## 6. `$rename`

`$rename` changes the name of a field.

### Example

```javascript
db.students.updateOne(
  { name: "Dara" },
  {
    $rename: {
      "fullname": "name"
    }
  }
)
```

### Before

```javascript
{
  fullname: "Dara"
}
```

### After

```javascript
{
  name: "Dara"
}
```

---

## 7. `$push`

`$push` adds a new value to an **array**.

### Example

```javascript
db.students.updateOne(
  { name: "Dara" },
  {
    $push: {
      courses: "MongoDB"
    }
  }
)
```

### Before

```javascript
{
  name: "Dara",
  courses: ["JavaScript", "Python"]
}
```

### After

```javascript
{
  name: "Dara",
  courses: ["JavaScript", "Python", "MongoDB"]
}
```

---

## 8. `$pull`

`$pull` removes a value from an **array**.

### Example

```javascript
db.students.updateOne(
  { name: "Dara" },
  {
    $pull: {
      courses: "Python"
    }
  }
)
```

### Before

```javascript
{
  name: "Dara",
  courses: ["JavaScript", "Python", "MongoDB"]
}
```

### After

```javascript
{
  name: "Dara",
  courses: ["JavaScript", "MongoDB"]
}
```

---

# 9. Complete Example

Create sample student data:

```javascript
db.students.insertMany([
  {
    name: "Dara",
    age: 20,
    score: 80,
    city: "Phnom Penh",
    courses: ["JavaScript", "Python"]
  },
  {
    name: "Sokha",
    age: 21,
    score: 85,
    city: "Phnom Penh",
    courses: ["Java", "SQL"]
  },
  {
    name: "Vanna",
    age: 22,
    score: 90,
    city: "Kampot",
    courses: ["Python", "MongoDB"]
  }
])
```

### Update one student's score

```javascript
db.students.updateOne(
  { name: "Dara" },
  { $inc: { score: 5 } }
)
```

### Update all students in Phnom Penh

```javascript
db.students.updateMany(
  { city: "Phnom Penh" },
  { $set: { status: "active" } }
)
```

### Add a course

```javascript
db.students.updateOne(
  { name: "Dara" },
  { $push: { courses: "MongoDB" } }
)
```

### Remove a course

```javascript
db.students.updateOne(
  { name: "Dara" },
  { $pull: { courses: "Python" } }
)
```

---

# 10. Summary

| Method / Operator | Purpose |
| --- | --- |
| `updateOne()` | Update one matching document |
| `updateMany()` | Update multiple matching documents |
| `$set` | Set or change a field value |
| `$inc` | Increase or decrease a number |
| `$unset` | Remove a field |
| `$rename` | Rename a field |
| `$push` | Add an item to an array |
| `$pull` | Remove an item from an array |

---

## Practice

1. Update Dara's age to `25`.
2. Increase Dara's score by `10`.
3. Decrease Dara's score by `5`.
4. Add a `status` field with the value `"active"` to all students.
5. Remove the `temporaryField` from Dara.
6. Rename `fullname` to `name`.
7. Add `"MongoDB"` to Dara's `courses` array.
8. Remove `"Python"` from Dara's `courses` array.
9. Use `updateMany()` to change the city of all students from `"Phnom Penh"` to `"Kandal"`.
10. Write an update query that increases the score of all students by `5`.
