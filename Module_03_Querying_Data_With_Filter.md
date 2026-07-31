# Module 3 -- Querying Data (MongoDB)

## Learning Objectives

After completing this lesson, students will be able to:

-   Query documents using comparison operators.
-   Combine multiple conditions with logical operators.
-   Query array fields.
-   Check field existence and data types.
-   Write efficient MongoDB queries.

------------------------------------------------------------------------

# 1. Sample Collection

``` javascript
use schoolDB
```

``` javascript
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

## Comparison Operators

  Operator   Description
  ---------- ---------------------------
  `$eq`      Equal
  `$ne`      Not Equal
  `$gt`      Greater Than
  `$gte`     Greater Than or Equal
  `$lt`      Less Than
  `$lte`     Less Than or Equal
  `$in`      Match any value in a list
  `$nin`     Not in a list

### Examples

``` javascript
db.students.find({ age: { $eq: 20 } })
db.students.find({ city: { $ne: "Phnom Penh" } })
db.students.find({ score: { $gt: 90 } })
db.students.find({ score: { $gte: 88 } })
db.students.find({ age: { $lt: 21 } })
db.students.find({ age: { $lte: 20 } })
db.students.find({ city: { $in: ["Phnom Penh","Kampot"] } })
db.students.find({ city: { $nin: ["Phnom Penh"] } })
```

## Logical Operators

  Operator   Description
  ---------- ------------------------------
  `$and`     All conditions must match
  `$or`      Any condition can match
  `$not`     Negates a condition
  `$nor`     None of the conditions match

``` javascript
db.students.find({
  $and: [
    { age: { $gt: 20 } },
    { score: { $gt: 90 } }
  ]
})

db.students.find({
  $or: [
    { city: "Kampot" },
    { score: { $gt: 90 } }
  ]
})

db.students.find({
  score: { $not: { $gt: 90 } }
})

db.students.find({
  $nor: [
    { city: "Phnom Penh" },
    { city: "Kampot" }
  ]
})
```

## Array Operators

  Operator       Description
  -------------- ---------------------
  `$all`         Contains all values
  `$elemMatch`   Match an element
  `$size`        Array length

``` javascript
db.students.find({ subjects: "Math" })

db.students.find({
  subjects: { $all: ["Math","Chemistry"] }
})

db.students.find({
  subjects: { $size: 2 }
})

db.orders.insertOne({
  customer: "John",
  items: [
    { name: "Keyboard", price: 40 },
    { name: "Mouse", price: 20 }
  ]
})

db.orders.find({
  items: {
    $elemMatch: {
      price: { $gt: 30 }
    }
  }
})
```

## Element Operators

  Operator    Description
  ----------- -----------------
  `$exists`   Field exists
  `$type`     Field data type

``` javascript
db.students.find({ score: { $exists: true } })
db.students.find({ phone: { $exists: false } })
db.students.find({ age: { $type: "int" } })
db.students.find({ city: { $type: "string" } })
```

## Combining Operators

``` javascript
db.students.find({
  score: { $gte: 85 },
  city: "Phnom Penh",
  graduated: true
})

db.students.find({
  subjects: "Math",
  age: { $lt: 23 },
  score: { $gt: 80 }
})
```

## Practice Exercises

1.  Find students whose score is greater than 80.
2.  Find students younger than 21.
3.  Find students from Phnom Penh.
4.  Find students whose city is not Kampot.
5.  Find students with scores between 80 and 90.
6.  Find students studying English.
7.  Find students studying both Math and English.
8.  Find students who graduated.
9.  Find students from Phnom Penh or Kampot.
10. Find documents without a phone field.

## Summary

  -----------------------------------------------------------------------
  Category                            Operators
  ----------------------------------- -----------------------------------
  Comparison                          `$eq`, `$ne`, `$gt`, `$gte`, `$lt`,
                                      `$lte`, `$in`, `$nin`

  Logical                             `$and`, `$or`, `$not`, `$nor`

  Array                               `$all`, `$elemMatch`, `$size`
  
  -----------------------------------------------------------------------
