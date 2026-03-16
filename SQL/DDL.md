# DDL (Data Definition Language) Commands in SQL

DDL commands are used to **define, modify, and delete the structure of database objects** such as databases, tables, schemas, indexes, and constraints.

These commands change the **database structure (metadata)** rather than the data stored inside tables.

DDL operations are usually **auto-committed**, meaning the changes are permanently saved immediately.

---

# Common DDL Commands

| Command | Purpose |
|--------|---------|
| CREATE | Creates database objects like tables, databases, schemas |
| ALTER | Modifies the structure of existing objects |
| DROP | Deletes database objects permanently |
| TRUNCATE | Removes all records from a table but keeps structure |
| RENAME | Renames database objects |

---

# 1. CREATE

The `CREATE` command is used to create new database objects.

## Create Database

```sql
CREATE DATABASE company_db;
```
## Create Table
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2)
);
```
## Create Schema
```sql
CREATE SCHEMA sales;
```
# 2. ALTER

The ALTER command is used to modify an existing database object.

## Add Column
```sql
ALTER TABLE employees
ADD email VARCHAR(100);
```

## Modify Column
```sql
ALTER TABLE employees
ALTER COLUMN salary TYPE DECIMAL(12,2);
```

## Drop Column
```sql
ALTER TABLE employees
DROP COLUMN email;
```

# 3. DROP

The DROP command removes database objects permanently.

## Drop Table
```sql 
DROP TABLE employees;
```
## Drop Database

```sql
DROP DATABASE company_db;
```
## Drop Schema

```sql
DROP SCHEMA sales;
```

# 4. TRUNCATE

The TRUNCATE command removes all rows from a table but keeps the table structure.

It is faster than DELETE because it does not log each row deletion.

```sql
TRUNCATE TABLE employees;
```

Example:

Before:

| id | name   | salary |
| -- | ------ | ------ |
| 1  | Murali | 60000  |
| 2  | Rahul  | 50000  |


After TRUNCATE:

Table exists but contains no rows.

# 5. RENAME

The RENAME command changes the name of a table or other database object.

```sql
ALTER TABLE employees
RENAME TO staff;
```

## Key Characteristics of DDL

- Used to define database structure

- Affects tables, schemas, indexes, constraints

- Changes metadata

- Usually auto-committed

- Cannot easily rollback in many databases

