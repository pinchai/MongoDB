# MongoDB Relationships and `$lookup`

MongoDB is a **NoSQL document database**, so relationships are handled differently from SQL databases.

There are two main approaches:

1. **Embedding** — store related data inside the same document.
2. **Referencing** — store the `_id` of another document and use `$lookup` when you need to combine them.

---

## 1. Embedding

For example, an order can contain its items directly:

```js
{
  _id: 1,
  customer: "Chai",
  items: [
    {
      product: "Laptop",
      price: 1000,
      quantity: 1
    },
    {
      product: "Mouse",
      price: 20,
      quantity: 2
    }
  ]
}
```

### Advantages

- Fast to read
- No `$lookup` required
- Good when related data is usually accessed together

### Disadvantages

- Documents can become very large
- Updating duplicated data can be difficult

---

# 2. Referencing

Instead of embedding products inside orders, we can create separate collections.

### `customers`

```js
{
  _id: ObjectId("64a111"),
  name: "Chai"
}
```

### `orders`

```js
{
  _id: ObjectId("64b111"),
  customerId: ObjectId("64a111"),
  total: 1020
}
```

This is similar to a foreign-key relationship in SQL.

```text
customers
   │
   │ customerId
   ▼
orders
```

---

# 3. `$lookup`

MongoDB's `$lookup` is similar to a SQL **JOIN**.

### Customers

```js
{
  _id: 1,
  name: "Chai"
}
```

### Orders

```js
{
  _id: 101,
  customerId: 1,
  total: 500
}
```

We can join them:

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customer"
    }
  }
])
```

Result:

```js
{
  _id: 101,
  customerId: 1,
  total: 500,
  customer: [
    {
      _id: 1,
      name: "Chai"
    }
  ]
}
```

Notice that `$lookup` returns an **array**.

---

# 4. `$lookup` + `$unwind`

If each order belongs to only one customer, you may want the customer to be an object rather than an array.

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customer"
    }
  },
  {
    $unwind: "$customer"
  }
])
```

Result:

```js
{
  _id: 101,
  customerId: 1,
  total: 500,
  customer: {
    _id: 1,
    name: "Chai"
  }
}
```

---

# 5. One-to-Many Relationship

A common example:

```text
Customer
   │
   ├── Order 1
   ├── Order 2
   └── Order 3
```

### `customers`

```js
{
  _id: 1,
  name: "Chai"
}
```

### `orders`

```js
{
  _id: 101,
  customerId: 1,
  total: 500
}
```

```js
{
  _id: 102,
  customerId: 1,
  total: 800
}
```

```js
{
  _id: 103,
  customerId: 1,
  total: 300
}
```

Get customers with their orders:

```js
db.customers.aggregate([
  {
    $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "customerId",
      as: "orders"
    }
  }
])
```

Result:

```js
{
  _id: 1,
  name: "Chai",
  orders: [
    {
      _id: 101,
      customerId: 1,
      total: 500
    },
    {
      _id: 102,
      customerId: 1,
      total: 800
    },
    {
      _id: 103,
      customerId: 1,
      total: 300
    }
  ]
}
```

This is similar to:

```sql
SELECT *
FROM customers
LEFT JOIN orders
    ON customers.id = orders.customer_id;
```

---

# 6. Many-to-Many Relationship

For example:

```text
Students
   │
   ├──── Courses
   │
   └──── Courses
```

A student can take many courses, and a course can have many students.

You can use a relationship collection.

### `students`

```js
{
  _id: 1,
  name: "Dara"
}
```

### `courses`

```js
{
  _id: 10,
  name: "MongoDB"
}
```

### `enrollments`

```js
{
  studentId: 1,
  courseId: 10
}
```

Then use `$lookup` multiple times:

```js
db.enrollments.aggregate([
  {
    $lookup: {
      from: "students",
      localField: "studentId",
      foreignField: "_id",
      as: "student"
    }
  },
  {
    $lookup: {
      from: "courses",
      localField: "courseId",
      foreignField: "_id",
      as: "course"
    }
  },
  {
    $unwind: "$student"
  },
  {
    $unwind: "$course"
  }
])
```

Result:

```js
{
  student: {
    _id: 1,
    name: "Dara"
  },
  course: {
    _id: 10,
    name: "MongoDB"
  }
}
```

---

# 7. `$lookup` with a Pipeline

For more complex relationships, `$lookup` can contain a pipeline.

Example:

```js
db.customers.aggregate([
  {
    $lookup: {
      from: "orders",
      let: {
        customerId: "$_id"
      },
      pipeline: [
        {
          $match: {
            $expr: {
              $eq: ["$customerId", "$$customerId"]
            }
          }
        },
        {
          $sort: {
            total: -1
          }
        },
        {
          $limit: 5
        }
      ],
      as: "orders"
    }
  }
])
```

This lets you:

- Filter related documents
- Sort related documents
- Limit results
- Project fields
- Perform additional aggregation

---

# 8. `$lookup` with Different Field Names

Suppose:

```js
// orders
{
  _id: 101,
  customerId: 1
}
```

and:

```js
// customers
{
  _id: 1,
  name: "Chai"
}
```

Use:

```js
$lookup: {
  from: "customers",
  localField: "customerId",
  foreignField: "_id",
  as: "customer"
}
```

Think of it as:

```text
orders.customerId
       ↓
customers._id
```

---

# 9. Important: `$lookup` Is Not Always Needed

MongoDB is designed around **data access patterns**.

Don't automatically design MongoDB like a relational SQL database.

### Embed when

Use embedding when:

- One document contains related data
- The related data is relatively small
- The related data is usually accessed together
- The related data does not need independent querying

Example:

```text
Customer
└── orders
    ├── order 1
    └── order 2
```

### Reference when

Use references when:

- Related data is large
- Data changes independently
- Data is shared by many documents
- Data needs independent queries

Example:

```text
customers                 orders
┌──────────┐              ┌─────────────┐
│ _id: 1   │◄─────────────│ customerId:1│
│ Chai     │              │ total: 500  │
└──────────┘              └─────────────┘
                               ▲
                               │
                           $lookup
```

---

# 10. SQL vs MongoDB

| SQL | MongoDB |
|---|---|
| Table | Collection |
| Row | Document |
| Column | Field |
| Primary Key | `_id` |
| Foreign Key | Reference field |
| JOIN | `$lookup` |
| JOIN + WHERE | `$lookup` + pipeline |
| Nested data | Embedded document |
| One-to-many | Reference or embedding |
| Many-to-many | Reference collection |

---

# Summary

The key idea is:

```text
Embedding
Customer
└── orders
    ├── order 1
    └── order 2
```

versus:

```text
Referencing

customers                 orders
┌──────────┐              ┌─────────────┐
│ _id: 1   │◄─────────────│ customerId:1│
│ Chai     │              │ total: 500  │
└──────────┘              └─────────────┘
                               ▲
                               │
                           $lookup
```

**`$lookup` is MongoDB's mechanism for combining related documents during an aggregation.**
