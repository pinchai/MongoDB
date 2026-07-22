# MongoDB Data Types

MongoDB stores data as **BSON (Binary JSON)**, which supports more data
types than standard JSON.

## Common MongoDB Data Types

  -----------------------------------------------------------------------------------
  Data Type         BSON Type         Example                    Description
  ----------------- ----------------- -------------------------- --------------------
  String            String            `"John"`                   Text data

  Integer           Int32 / Int64     `25`, `1000000`            Whole numbers

  Double            Double            `99.95`                    Decimal numbers

  Decimal           Decimal128        `NumberDecimal("99.99")`   High-precision
                                                                 decimal (financial
                                                                 data)

  Boolean           Boolean           `true`, `false`            True or False

  Date              Date              `new Date()`               Date and time

  Array             Array             `["Java", "Python"]`       List of values

  Object            Object            `{ city: "Phnom Penh" }`   Embedded document

  ObjectId          ObjectId          `ObjectId()`               Unique document
                                                                 identifier

  Null              Null              `null`                     Empty value

  Binary            BinData           Binary data                Images, files, etc.

  Timestamp         Timestamp         `Timestamp()`              Internal timestamp

  Regular           Regex             `/^A/`                     Pattern matching
  Expression                                                     

  JavaScript        JavaScript        `function(){}`             JavaScript code
                                                                 (rarely used)
  -----------------------------------------------------------------------------------

## String

``` javascript
{
    name: "Alice"
}
```

## Integer

``` javascript
{
    age: 25
}
```

## Double

``` javascript
{
    price: 19.99
}
```

## Decimal128

``` javascript
{
    salary: NumberDecimal("1500.75")
}
```

## Boolean

``` javascript
{
    active: true
}
```

## Date

``` javascript
{
    createdAt: new Date()
}
```

Output:

``` javascript
{
    createdAt: ISODate("2026-07-22T09:30:00Z")
}
```

## Array

``` javascript
{
    skills: ["HTML", "CSS", "JavaScript"]
}
```

``` javascript
{
    scores: [80, 90, 95]
}
```

## Embedded Object

``` javascript
{
    address: {
        city: "Phnom Penh",
        country: "Cambodia"
    }
}
```

## ObjectId

``` javascript
{
    _id: ObjectId("6880c6d4c54b2e8b5e8d1234")
}
```

Create one manually:

``` javascript
ObjectId()
```

## Null

``` javascript
{
    middleName: null
}
```

## Binary

``` javascript
{
    file: BinData(...)
}
```

## Timestamp

``` javascript
{
    updated: Timestamp()
}
```

## Regular Expression

``` javascript
{
    pattern: /^A/
}
```

Query example:

``` javascript
db.students.find({
    name: /^A/
})
```

## Example Document

``` javascript
db.students.insertOne({
    name: "John",
    age: 20,
    gpa: 3.75,
    active: true,
    birthday: new Date("2005-05-12"),
    skills: ["JavaScript", "MongoDB", "Node.js"],
    address: {
        city: "Phnom Penh",
        country: "Cambodia"
    },
    salary: NumberDecimal("1200.50"),
    middleName: null
})
```

## SQL vs MongoDB

  SQL         MongoDB
  ----------- ------------------
  VARCHAR     String
  CHAR        String
  TEXT        String
  INT         Int32 / Int64
  BIGINT      Int64
  FLOAT       Double
  DECIMAL     Decimal128
  BOOLEAN     Boolean
  DATE        Date
  DATETIME    Date
  TIMESTAMP   Timestamp / Date
  JSON        Object / Array
  BLOB        Binary
  NULL        Null

## Key Difference

SQL requires defining column data types in advance:

``` sql
CREATE TABLE Student (
    id INT,
    name VARCHAR(100),
    age INT
);
```

MongoDB collections are schema-flexible:

``` javascript
// Document 1
{
    name: "Alice",
    age: 20
}

// Document 2
{
    name: "Bob",
    age: "Twenty",
    phone: "012345678"
}
```

For production systems, use schema validation to enforce consistent data
types.
