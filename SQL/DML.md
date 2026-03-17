# SQL DML (Data Manipulation Language) Guide

This document explains **DML in SQL** — the commands used to **insert, update, delete, and retrieve data** in a database.

---

## 1. What is DML?

**DML (Data Manipulation Language)** is a subset of SQL used to manipulate data stored in database tables.

| Command | Description |
|---------|-------------|
| `INSERT` | Adds new rows into a table |
| `UPDATE` | Modifies existing rows |
| `DELETE` | Removes rows from a table |
| `SELECT` | Retrieves data from a table |

> **Note:** `SELECT` is sometimes classified separately as DQL (Data Query Language), but it is commonly included under DML.

---

## 2. INSERT Statement

Used to add new records into a table.

### Insert a Single Row

```sql
INSERT INTO employees (id, name, department, salary)
VALUES (1, 'Murali', 'Engineering', 60000);
```

### Insert Multiple Rows

```sql
INSERT INTO employees (id, name, department, salary)
VALUES
  (2, 'Rahul', 'HR', 45000),
  (3, 'Anita', 'Engineering', 70000),
  (4, 'Priya', 'Finance', 55000);
```

### Insert Without Specifying Columns

> All columns must be provided in order.

```sql
INSERT INTO employees
VALUES (5, 'Kiran', 'Marketing', 48000);
```

---

## 3. UPDATE Statement

Used to modify existing records in a table.

### Update a Single Column

```sql
UPDATE employees
SET salary = 65000
WHERE id = 1;
```

### Update Multiple Columns

```sql
UPDATE employees
SET salary = 75000,
    department = 'Management'
WHERE id = 3;
```

### Update All Rows (No WHERE)

> ⚠️ This updates **every row** in the table. Use with caution.

```sql
UPDATE employees
SET salary = salary * 1.10;
```

This gives a 10% salary hike to all employees.

---

## 4. DELETE Statement

Used to remove records from a table.

### Delete a Specific Row

```sql
DELETE FROM employees
WHERE id = 2;
```

### Delete with a Condition

```sql
DELETE FROM employees
WHERE department = 'HR';
```

### Delete All Rows (No WHERE)

> ⚠️ This removes **all rows** but keeps the table structure.

```sql
DELETE FROM employees;
```

---

## 5. SELECT Statement (Recap)

Used to retrieve data from a table.

```sql
SELECT name, salary
FROM employees
WHERE salary > 50000
ORDER BY salary DESC;
```

> Refer to the **SQL Data Querying Guide** for full details on SELECT.

---

## 6. WHERE Clause in DML

The `WHERE` clause is critical in `UPDATE` and `DELETE` to target specific rows.

### Without WHERE — affects all rows:

```sql
UPDATE employees SET salary = 0;       -- Updates everyone
DELETE FROM employees;                 -- Deletes everyone
```

### With WHERE — affects specific rows:

```sql
UPDATE employees SET salary = 0 WHERE id = 3;
DELETE FROM employees WHERE id = 3;
```

---

## 7. Using Operators in DML

### Comparison Operators

```sql
DELETE FROM employees
WHERE salary < 40000;
```

### Logical Operators

```sql
UPDATE employees
SET salary = 80000
WHERE department = 'Engineering' AND salary < 70000;
```

### IN Operator

```sql
DELETE FROM employees
WHERE department IN ('HR', 'Marketing');
```

### BETWEEN Operator

```sql
UPDATE employees
SET salary = 50000
WHERE salary BETWEEN 40000 AND 49999;
```

---

## 8. INSERT with SELECT

You can insert data from another table using `SELECT`.

```sql
INSERT INTO archive_employees (id, name, department, salary)
SELECT id, name, department, salary
FROM employees
WHERE department = 'HR';
```

---

## 9. RETURNING Clause (PostgreSQL)

In PostgreSQL, `RETURNING` shows the affected rows after DML operations.

```sql
INSERT INTO employees (id, name, department, salary)
VALUES (6, 'Deepa', 'Finance', 52000)
RETURNING *;
```

```sql
UPDATE employees
SET salary = 90000
WHERE id = 1
RETURNING id, name, salary;
```

```sql
DELETE FROM employees
WHERE id = 5
RETURNING *;
```

---

## 10. Transactions in DML

DML statements can be wrapped in **transactions** to ensure data integrity.

| Command | Description |
|---------|-------------|
| `BEGIN` | Starts a transaction |
| `COMMIT` | Saves all changes permanently |
| `ROLLBACK` | Undoes changes since last BEGIN |

### Example

```sql
BEGIN;

UPDATE employees SET salary = 95000 WHERE id = 1;
DELETE FROM employees WHERE id = 4;

COMMIT;  -- Save changes
```

### Rollback Example

```sql
BEGIN;

DELETE FROM employees WHERE department = 'Engineering';

ROLLBACK;  -- Undo the delete
```

---

## 11. DML vs DDL vs DCL

| Category | Full Form | Commands |
|----------|-----------|----------|
| **DML** | Data Manipulation Language | `SELECT`, `INSERT`, `UPDATE`, `DELETE` |
| **DDL** | Data Definition Language | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| **DCL** | Data Control Language | `GRANT`, `REVOKE` |
| **TCL** | Transaction Control Language | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

---

## 12. Example Table

| id | name | department | salary |
|----|------|------------|--------|
| 1 | Murali | Engineering | 60000 |
| 2 | Rahul | HR | 45000 |
| 3 | Anita | Engineering | 70000 |
| 4 | Priya | Finance | 55000 |

### Insert a new employee

```sql
INSERT INTO employees (id, name, department, salary)
VALUES (5, 'Kiran', 'Marketing', 48000);
```

### Give Engineering employees a raise

```sql
UPDATE employees
SET salary = salary + 5000
WHERE department = 'Engineering';
```

### Remove employees earning below 50000

```sql
DELETE FROM employees
WHERE salary < 50000;
```

### View remaining records

```sql
SELECT * FROM employees ORDER BY salary DESC;
```

**Result:**

| id | name | department | salary |
|----|------|------------|--------|
| 3 | Anita | Engineering | 75000 |
| 1 | Murali | Engineering | 65000 |
| 4 | Priya | Finance | 55000 |