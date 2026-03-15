# SQL Data Querying Guide

This document explains **Querying Data in SQL** using the **SELECT statement** and related clauses.

---

## 1. What is Querying Data?

Querying data means **retrieving data from a database table** using SQL statements.

The primary command used is:

```sql
SELECT
```

It allows you to retrieve specific columns, filter rows, sort results, and aggregate data.

---

## 2. Basic SELECT Statement

The most basic query retrieves all columns from a table.

```sql
SELECT * 
FROM employees;
```

**Explanation:**
- `SELECT` → specifies the columns to retrieve
- `*` → means all columns
- `FROM` → specifies the table

---

## 3. Selecting Specific Columns

Instead of retrieving all columns, you can select specific ones.

```sql
SELECT id, name, salary
FROM employees;
```

This returns only: `id`, `name`, `salary`

---

## 4. Filtering Data using WHERE

The `WHERE` clause filters rows based on conditions.

```sql
SELECT *
FROM employees
WHERE department = 'Engineering';
```

Example with multiple conditions:

```sql
SELECT name, salary
FROM employees
WHERE salary > 50000;
```

---

## 5. Comparison Operators

| Operator | Description |
|----------|-------------|
| `=` | Equal |
| `!=` or `<>` | Not equal |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

```sql
SELECT *
FROM employees
WHERE salary >= 60000;
```

---

## 6. Logical Operators

Used to combine multiple conditions.

| Operator | Description |
|----------|-------------|
| `AND` | Both conditions must be true |
| `OR` | At least one condition must be true |
| `NOT` | Reverses the condition |

```sql
SELECT *
FROM employees
WHERE department = 'Engineering'
AND salary > 50000;
```

---

## 7. Sorting Data (ORDER BY)

`ORDER BY` sorts the result.

**Ascending order (default):**

```sql
SELECT *
FROM employees
ORDER BY salary ASC;
```

**Descending order:**

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

---

## 8. Limiting Results (LIMIT / TOP)

Used to restrict the number of returned rows.

**MySQL / PostgreSQL:**

```sql
SELECT *
FROM employees
LIMIT 5;
```

**SQL Server:**

```sql
SELECT TOP 5 *
FROM employees;
```

---

## 9. Removing Duplicate Values (DISTINCT)

`DISTINCT` removes duplicate records.

```sql
SELECT DISTINCT department
FROM employees;
```

---

## 10. Aggregate Functions

Aggregate functions perform calculations on data.

| Function | Description |
|----------|-------------|
| `COUNT()` | Counts rows |
| `SUM()` | Adds values |
| `AVG()` | Calculates average |
| `MIN()` | Finds smallest value |
| `MAX()` | Finds largest value |

```sql
SELECT COUNT(*) 
FROM employees;
```

```sql
SELECT AVG(salary)
FROM employees;
```

---

## 11. GROUP BY

`GROUP BY` groups rows with the same values.

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
```

This counts employees per department.

---

## 12. HAVING Clause

`HAVING` filters grouped results.

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

This returns departments with more than 5 employees.

---

## 13. IN Operator

Used to match multiple values.

```sql
SELECT *
FROM employees
WHERE department IN ('HR', 'Engineering');
```

---

## 14. BETWEEN Operator

Used to filter values within a range.

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 40000 AND 80000;
```

---

## 15. LIKE Operator

Used for pattern matching.

| Pattern | Meaning |
|---------|---------|
| `%` | Any number of characters |
| `_` | Single character |

```sql
SELECT *
FROM employees
WHERE name LIKE 'M%';
```

Returns names starting with **M**.

---

## 16. IS NULL

Used to check missing values.

```sql
SELECT *
FROM employees
WHERE email IS NULL;
```

---

## 17. Aliases (AS)

Aliases rename columns or tables in results.

```sql
SELECT name AS employee_name,
       salary AS employee_salary
FROM employees;
```

---

## Example Table

| id | name | department | salary |
|----|------|------------|--------|
| 1 | Murali | Engineering | 60000 |
| 2 | Rahul | HR | 45000 |
| 3 | Anita | Engineering | 70000 |

**Example query:**

```sql
SELECT name, salary
FROM employees
WHERE salary > 50000
ORDER BY salary DESC;
```

**Result:**

| name | salary |
|------|--------|
| Anita | 70000 |
| Murali | 60000 |