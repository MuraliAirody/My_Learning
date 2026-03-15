# SQL Basics

## What is SQL?
SQL (Structured Query Language) is a standard programming language used to manage and manipulate relational databases. It allows you to perform operations like querying data, inserting records, updating information, and defining database structures.

## Key Concepts

### Database
A database is an organized collection of structured data stored electronically, designed for efficient storage, retrieval, and management.

### DBMS
DBMS (Database Management System) is software that provides tools to create, maintain, and interact with databases, handling tasks like data storage, security, and querying (e.g., MySQL, PostgreSQL, Oracle).

### Tables
Tables are the basic structure in a relational database, consisting of rows and columns.

### Basic SQL Commands

#### CREATE TABLE
```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);
```

#### INSERT
```sql
INSERT INTO users (id, name, email) VALUES (1, 'John Doe', 'john@example.com');
```

#### SELECT
```sql
SELECT * FROM users;
SELECT name, email FROM users WHERE id = 1;
```

#### UPDATE
```sql
UPDATE users SET email = 'john.doe@example.com' WHERE id = 1;
```

#### DELETE
```sql
DELETE FROM users WHERE id = 1;
```

## Data Types
- INT: Integer numbers
- VARCHAR(n): Variable-length string (up to n characters)
- DATE: Date values
- DECIMAL: Decimal numbers

## Constraints
- PRIMARY KEY: Uniquely identifies each record
- FOREIGN KEY: Links to primary key in another table
- NOT NULL: Ensures column cannot have NULL value
- UNIQUE: Ensures all values in column are unique

## Joins
- INNER JOIN: Returns records with matching values in both tables
- LEFT JOIN: Returns all records from left table and matching records from right table
- RIGHT JOIN: Returns all records from right table and matching records from left table
- FULL JOIN: Returns all records when there is a match in either table

## Example Query
```sql
SELECT u.name, o.order_date
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE u.name LIKE 'J%';
```


# DDL and DML in SQL

SQL (Structured Query Language) commands are categorized based on what they do in the database. Two important categories are **DDL (Data Definition Language)** and **DML (Data Manipulation Language)**.

---

# 1. DDL (Data Definition Language)

DDL commands are used to **define or modify the structure of database objects** such as tables, schemas, indexes, etc.

These commands change the **database structure**, not the data itself.

## Common DDL Commands

| Command | Description |
|--------|-------------|
| CREATE | Creates database objects like tables, schemas, indexes |
| ALTER | Modifies the structure of existing objects |
| DROP | Deletes database objects permanently |
| TRUNCATE | Removes all records from a table but keeps the structure |
| RENAME | Renames a database object |

## Examples

### Create Table
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2)
);
```

### Alter Table
```sql
ALTER TABLE employees
ADD email VARCHAR(100);
```

### Drop Table
```sql
DROP TABLE employees;
Truncate Table
TRUNCATE TABLE employees;
```

# 2. DML (Data Manipulation Language)

DML commands are used to manipulate the data inside database tables.

These commands allow you to insert, update, delete, and retrieve records.

## Common DML Commands
Command	Description
```sql
INSERT	Adds new records into a table
UPDATE	Modifies existing records
DELETE	Removes records from a table
SELECT	Retrieves data from a table
```
Examples

```sql
Insert Data
INSERT INTO employees (id, name, department, salary)
VALUES (1, 'Murali', 'Engineering', 60000);
Select Data
SELECT * FROM employees;
Update Data
UPDATE employees
SET salary = 70000
WHERE id = 1;
Delete Data
DELETE FROM employees
WHERE id = 1;

```
# Key Differences Between DDL and DML
| Feature     | DDL                        | DML                        |
| ----------- | -------------------------- | -------------------------- |
| Purpose     | Defines database structure | Manipulates table data     |
| Works On    | Tables, schemas, indexes   | Records/rows               |
| Examples    | CREATE, ALTER, DROP        | INSERT, UPDATE, DELETE     |
| Transaction | Usually auto-commit        | Supports COMMIT / ROLLBACK |
