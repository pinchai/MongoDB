# MongoDB Midterm Exam

**Student Course Management System**

## Exam Objective

Students must design and implement a MongoDB database for managing students, courses, and enrollments. The project evaluates students' ability to create collections, insert documents, perform CRUD operations, and write MongoDB queries.

---

# 1. Project Overview

Develop a **Student Course Management System** using MongoDB.

The system should manage:

- Students
- Courses
- Enrollments

The system should allow users to:

- Register students
- Create courses
- Enroll students in courses
- Search for students and courses
- Query data using MongoDB Query Language (MQL)

### Database Name

```javascript
student_management
```

---

# 2. Database Design

Design the database using MongoDB's document-based model.

Create the following collections:

```text
student_management
│
├── students
├── courses
└── enrollments
```

### Students

Each student should contain information such as:

```javascript
{
    studentId: "ST001",
    name: "Dara",
    gender: "Male",
    age: 20,
    email: "dara@example.com",
    major: "Computer Science",
    gpa: 3.5
}
```

### Courses

Each course should contain:

```javascript
{
    courseId: "CS101",
    courseName: "Database Server Application",
    instructor: "Mr. Chan",
    credits: 3
}
```

### Enrollments

Each enrollment should contain:

```javascript
{
    studentId: "ST001",
    courseId: "CS101",
    semester: "2026-S1",
    score: 85,
    status: "Passed"
}
```

Students should choose appropriate MongoDB data types for fields where appropriate.

---

# 3. Collections

Create the three required collections:

```text
students
courses
enrollments
```

### Requirements

**Students**

- Insert at least **10 students**.
- Students must have different ages, majors, and GPAs.
- At least one student should have a GPA greater than 3.5.

**Courses**

- Insert at least **5 courses**.
- Courses must have different instructors and credit values.

**Enrollments**

- Insert at least **15 enrollment documents**.
- Students should be enrolled in different courses.
- Include different scores and statuses.

---

# 4. CRUD Operations

Students must demonstrate all four CRUD operations.

## Create

Insert:

- At least 10 students
- At least 5 courses
- At least 15 enrollments

Example:

```javascript
db.students.insertOne({
    studentId: "ST001",
    name: "Dara",
    age: 20,
    major: "Computer Science",
    gpa: 3.5
})
```

## Read

Display:

- All students
- All courses
- All enrollments

Example:

```javascript
db.students.find()
```

---

# 5. Query Operations

Students must write MongoDB queries to answer the following questions.

## Basic Queries

1. Display all students.
2. Display all courses.
3. Find a student by `studentId`.
4. Find students whose major is **Computer Science**.

## Comparison Queries

1. Find students whose age is greater than 20.
2. Find students whose GPA is less than 3.0.
3. Find students whose GPA is equal to 3.5.

## Projection

1. Display only:

```text
name
major
gpa
```

Do not display the default `_id` field
---
