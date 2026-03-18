# SQL Joins Guide

This document explains **Joins in SQL** — used to combine rows from two or more tables based on a related column.

---

## 1. What is a JOIN?

A **JOIN** clause is used to retrieve data from multiple tables by linking them using a common column (usually a primary key and foreign key).

### Why Use Joins?

Databases are **normalized** — data is split across multiple tables to avoid redundancy. Joins allow you to bring that data back together when querying.

---

## 2. Sample Tables

We will use these two tables throughout this guide.

### `employees` Table

| id | name   | department_id | salary |
|----|--------|---------------|--------|
| 1  | Murali | 101           | 60000  |
| 2  | Rahul  | 102           | 45000  |
| 3  | Anita  | 101           | 70000  |
| 4  | Priya  | 103           | 55000  |
| 5  | Kiran  | NULL          | 48000  |

### `departments` Table

| id  | department_name |
|-----|-----------------|
| 101 | Engineering     |
| 102 | HR              |
| 103 | Finance         |
| 104 | Marketing       |

> `employees.department_id` links to `departments.id`

---

## 3. Types of Joins

| Join Type | Description |
|-----------|-------------|
| `INNER JOIN` | Returns only matching rows from both tables |
| `LEFT JOIN` | Returns all rows from left table + matched rows from right |
| `RIGHT JOIN` | Returns all rows from right table + matched rows from left |
| `FULL OUTER JOIN` | Returns all rows from both tables |
| `CROSS JOIN` | Returns all combinations of rows from both tables |
| `SELF JOIN` | Joins a table with itself |

---

## 4. INNER JOIN

Returns **only the rows** where there is a match in **both** tables.

```sql
SELECT employees.name, departments.department_name
FROM employees
INNER JOIN departments
ON employees.department_id = departments.id;
```

**Result:**

| name   | department_name |
|--------|-----------------|
| Murali | Engineering     |
| Rahul  | HR              |
| Anita  | Engineering     |
| Priya  | Finance         |

> Kiran is excluded because `department_id` is NULL (no match).  
> Marketing is excluded because no employee belongs to it.

---

## 5. LEFT JOIN (LEFT OUTER JOIN)

Returns **all rows from the left table** and matched rows from the right table.  
If no match, right side columns return `NULL`.

```sql
SELECT employees.name, departments.department_name
FROM employees
LEFT JOIN departments
ON employees.department_id = departments.id;
```

**Result:**

| name   | department_name |
|--------|-----------------|
| Murali | Engineering     |
| Rahul  | HR              |
| Anita  | Engineering     |
| Priya  | Finance         |
| Kiran  | NULL            |

> Kiran is included with `NULL` because they have no matching department.

---

## 6. RIGHT JOIN (RIGHT OUTER JOIN)

Returns **all rows from the right table** and matched rows from the left table.  
If no match, left side columns return `NULL`.

```sql
SELECT employees.name, departments.department_name
FROM employees
RIGHT JOIN departments
ON employees.department_id = departments.id;
```

**Result:**

| name   | department_name |
|--------|-----------------|
| Murali | Engineering     |
| Anita  | Engineering     |
| Rahul  | HR              |
| Priya  | Finance         |
| NULL   | Marketing       |

> Marketing is included with `NULL` because no employee belongs to it.

---

## 7. FULL OUTER JOIN

Returns **all rows from both tables**.  
Non-matching rows get `NULL` for the missing side.

```sql
SELECT employees.name, departments.department_name
FROM employees
FULL OUTER JOIN departments
ON employees.department_id = departments.id;
```

**Result:**

| name   | department_name |
|--------|-----------------|
| Murali | Engineering     |
| Rahul  | HR              |
| Anita  | Engineering     |
| Priya  | Finance         |
| Kiran  | NULL            |
| NULL   | Marketing       |

> Both unmatched rows (Kiran and Marketing) are included.

---

## 8. CROSS JOIN

Returns the **Cartesian product** — every row from the left table paired with every row from the right table.

```sql
SELECT employees.name, departments.department_name
FROM employees
CROSS JOIN departments;
```

> If `employees` has 5 rows and `departments` has 4 rows → result has **5 × 4 = 20 rows**.

Use with caution — can produce very large result sets.

---

## 9. SELF JOIN

A table is **joined with itself**. Useful for hierarchical data like employee-manager relationships.

### Example: Find employees and their managers

Assume the `employees` table has a `manager_id` column:

| id | name   | manager_id |
|----|--------|------------|
| 1  | Murali | NULL       |
| 2  | Rahul  | 1          |
| 3  | Anita  | 1          |
| 4  | Priya  | 2          |

```sql
SELECT e.name AS employee, m.name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.id;
```

**Result:**

| employee | manager |
|----------|---------|
| Murali   | NULL    |
| Rahul    | Murali  |
| Anita    | Murali  |
| Priya    | Rahul   |

---

## 10. Using Aliases in Joins

Aliases make join queries shorter and more readable.

```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.id;
```

---

## 11. Joining More Than Two Tables

You can chain multiple joins together.

```sql
SELECT e.name, d.department_name, p.project_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
INNER JOIN projects p ON e.id = p.employee_id;
```

---

## 12. JOIN with WHERE, ORDER BY, and GROUP BY

### Filter after joining

```sql
SELECT e.name, d.department_name, e.salary
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
WHERE e.salary > 50000;
```

### Sort joined results

```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
ORDER BY e.name ASC;
```

### Aggregate with GROUP BY

```sql
SELECT d.department_name, COUNT(e.id) AS employee_count
FROM employees e
INNER JOIN departments d ON e.department_id = d.id
GROUP BY d.department_name;
```

**Result:**

| department_name | employee_count |
|-----------------|----------------|
| Engineering     | 2              |
| HR              | 1              |
| Finance         | 1              |

---

## 13. Visual Summary of Joins

```
Table A     Table B
  [ A ]       [ B ]

INNER JOIN  →  [ A ∩ B ]         (only matching rows)
LEFT JOIN   →  [ A + A∩B ]       (all of A, matched B)
RIGHT JOIN  →  [ B + A∩B ]       (all of B, matched A)
FULL JOIN   →  [ A + A∩B + B ]   (everything)
CROSS JOIN  →  [ A × B ]         (every combination)
```

---

## 14. Quick Reference

| Join | Includes Unmatched from Left | Includes Unmatched from Right |
|------|------------------------------|-------------------------------|
| `INNER JOIN` | ❌ | ❌ |
| `LEFT JOIN` | ✅ | ❌ |
| `RIGHT JOIN` | ❌ | ✅ |
| `FULL OUTER JOIN` | ✅ | ✅ |
| `CROSS JOIN` | N/A (all combinations) | N/A |