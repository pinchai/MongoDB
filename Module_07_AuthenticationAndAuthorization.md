# MongoDB Authentication and Authorization on Windows OS

## 1. Introduction

MongoDB provides two important security mechanisms:

-   **Authentication:** Verifies who a user is (username and password).
-   **Authorization:** Determines what an authenticated user is allowed
    to do (roles and permissions).

For example, an administrator can manage databases and users, while a
normal user may only read or modify data in a specific database.

## 2. Authentication vs Authorization

  -----------------------------------------------------------------------
  Authentication                      Authorization
  ----------------------------------- -----------------------------------
  Verifies user identity              Controls user permissions

  Uses username and password          Uses roles and privileges

  Required before accessing protected Applied after authentication
  databases                           

  Example: Login as `admin`           Example: `readWrite` on `school_db`
  -----------------------------------------------------------------------

## 3. Enable MongoDB Authentication on Windows

### Step 1: Check MongoDB installation

Open Command Prompt or PowerShell:

``` powershell
mongod --version
mongosh --version
```

Make sure MongoDB Server and MongoDB Shell are installed.

### Step 2: Start MongoDB without authentication

If MongoDB is installed as a Windows service, it may already be running.

Open MongoDB Shell:

``` powershell
mongosh
```

Connect to the local MongoDB server:

``` text
mongodb://127.0.0.1:27017
```

### Step 3: Create an administrator user

Select the `admin` database:

``` javascript
use admin
```

Create an administrator account:

``` javascript
db.createUser({
  user: "admin",
  pwd: passwordPrompt(),
  roles: [
    {
      role: "root",
      db: "admin"
    }
  ]
})
```

The `passwordPrompt()` function asks for a password without displaying
it.

The `root` role provides broad administrative privileges across MongoDB.

### Step 4: Locate the MongoDB configuration file

On Windows, MongoDB commonly stores its configuration at a path similar
to:

``` text
C:\Program Files\MongoDB\Server\8.0\bin\mongod.cfg
```

The version number and configuration-file location may differ depending
on your installation and service configuration.

Open Notepad as Administrator, then open `mongod.cfg`.

A typical configuration looks like this:

``` yaml
storage:
  dbPath: C:\Program Files\MongoDB\Server\8.0\data

systemLog:
  destination: file
  logAppend: true
  path: C:\Program Files\MongoDB\Server\8.0\log\mongod.log

net:
  port: 27017
  bindIp: 127.0.0.1
```

Keep your existing storage and log paths. Do not replace them with
example paths unless they match your installation.

### Step 5: Enable authentication

Add the following configuration at the top level:

``` yaml
security:
  authorization: enabled
```

Save the file.

**Important:** YAML uses spaces for indentation, not tabs.

### Step 6: Restart MongoDB service

Open PowerShell as Administrator:

``` powershell
Restart-Service MongoDB
```

Alternatively, use Command Prompt as Administrator:

``` cmd
net stop MongoDB
net start MongoDB
```

If the service has a different name, check it using:

``` powershell
Get-Service *mongo*
```

## 4. Login with Authentication

After enabling authentication, a connection without credentials may
still open, but protected database operations will fail.

### Method 1: Login using MongoDB Shell

``` powershell
mongosh -u admin -p --authenticationDatabase admin
```

Enter the password when prompted.

### Method 2: Login using a connection string

``` text
mongodb://admin:<password>@localhost:27017/admin
```

Replace `<password>` with the actual password. Special characters in URI
passwords must be percent-encoded.

### Method 3: Authenticate inside MongoDB Shell

``` javascript
use admin

db.auth("admin", passwordPrompt())
```

Successful authentication returns:

``` javascript
{ ok: 1 }
```

## 5. MongoDB Authorization (Roles)

MongoDB uses Role-Based Access Control (RBAC).

Common built-in roles:

  Role                     Description
  ------------------------ --------------------------------------------
  `read`                   Read data
  `readWrite`              Read and modify data
  `dbAdmin`                Manage database administration tasks
  `userAdmin`              Manage users and roles in a database
  `dbOwner`                Combines readWrite, dbAdmin, and userAdmin
  `readAnyDatabase`        Read across databases
  `readWriteAnyDatabase`   Read and write across databases
  `root`                   Broad administrative access

## 6. Create Database Users

Login as administrator first.

### Example 1: Create a read-only user

``` javascript
use school_db

db.createUser({
  user: "student_reader",
  pwd: passwordPrompt(),
  roles: [
    {
      role: "read",
      db: "school_db"
    }
  ]
})
```

The user can read data but cannot insert, update, or delete documents.

### Example 2: Create a read/write user

``` javascript
use school_db

db.createUser({
  user: "school_staff",
  pwd: passwordPrompt(),
  roles: [
    {
      role: "readWrite",
      db: "school_db"
    }
  ]
})
```

The user can perform CRUD operations.

### Example 3: Create a database administrator

``` javascript
use school_db

db.createUser({
  user: "school_manager",
  pwd: passwordPrompt(),
  roles: [
    {
      role: "dbOwner",
      db: "school_db"
    }
  ]
})
```

The user can manage data, users, and database administration tasks
within `school_db`.

## 7. Test User Permissions

### Login as a read-only user

Open another terminal:

``` powershell
mongosh -u student_reader -p --authenticationDatabase school_db
```

Select the database:

``` javascript
use school_db
```

Read data:

``` javascript
db.students.find()
```

**Expected result:** Allowed.

Try inserting data:

``` javascript
db.students.insertOne({
  name: "Dara",
  age: 20
})
```

**Expected result:** Not allowed. MongoDB returns an authorization error
similar to:

``` text
MongoServerError: not authorized on school_db
to execute command
```

## 8. Manage Users and Roles

Run these commands while authenticated as an administrator with the
required permissions.

### View all users in the current database

``` javascript
use school_db

db.getUsers()
```

### View a specific user

``` javascript
db.getUser("school_staff")
```

### Change user password

``` javascript
db.changeUserPassword(
  "school_staff",
  passwordPrompt()
)
```

### Grant a new role

``` javascript
db.grantRolesToUser(
  "student_reader",
  [
    {
      role: "readWrite",
      db: "school_db"
    }
  ]
)
```

Now `student_reader` has both `read` and `readWrite` roles. Since
`readWrite` already includes read access, the separate `read` role is
redundant.

### Revoke a role

``` javascript
db.revokeRolesFromUser(
  "student_reader",
  [
    {
      role: "readWrite",
      db: "school_db"
    }
  ]
)
```

### Delete a user

``` javascript
db.dropUser("school_staff")
```

## 9. MongoDB Compass Authentication on Windows

To connect using MongoDB Compass:

1.  Open MongoDB Compass.

2.  Select **New Connection**.

3.  Enter the connection string:

    ``` text
    mongodb://localhost:27017
    ```

4.  Open advanced connection settings and choose Username/Password
    authentication.

5.  Enter the following information:

  Setting                   Value
  ------------------------- -----------------------------
  Authentication Method     Username / Password
  Username                  admin
  Password                  Your administrator password
  Authentication Database   admin

6.  Click **Connect**.

Alternatively, use a connection URI with the username, password, and
authentication database.

## 10. MongoDB Security Best Practices

-   Enable `security.authorization` for databases that require access
    control.
-   Use strong, unique passwords.
-   Assign the minimum required privileges to each user.
-   Avoid using the `root` account in applications.
-   Keep MongoDB bound to `127.0.0.1` for local-only development.
-   Use TLS for remote connections.
-   Restrict network access with Windows Firewall.
-   Keep MongoDB updated and back up databases regularly.

## 11. Practical Exercise for Students

**Exercise: MongoDB Authentication and Authorization**

Create a database named `company_db` with a collection named
`employees`.

Create three users:

  Username          Role          Database
  ----------------- ------------- --------------
  `company_admin`   `dbOwner`     `company_db`
  `hr_staff`        `readWrite`   `company_db`
  `auditor`         `read`        `company_db`

Tasks:

1.  Enable MongoDB authentication on Windows.
2.  Create the three users.
3.  Login using each account.
4.  Insert five employees using `hr_staff`.
5.  Retrieve employees using `auditor`.
6.  Try deleting an employee using `auditor`.
7.  Explain why MongoDB rejects the delete operation.
8.  Change `auditor` to have `readWrite` permission.
9.  Test the delete operation again.
10. Revoke the `readWrite` role from `auditor`.

**Expected learning outcome:** Students understand how to configure
MongoDB authentication on Windows, create users, assign roles, and
enforce database access permissions.
