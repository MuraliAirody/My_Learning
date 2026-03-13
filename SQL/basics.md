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