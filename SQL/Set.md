# SQL Set Operators Guide

This document explains **Set Operators in SQL** — used to combine results from two or more `SELECT` queries.

---

## 1. What are Set Operators?

Set operators **combine the results of two or more SELECT statements** into a single result set.

| Operator | Description |
|----------|-------------|
| `UNION` | Combines results, removes duplicates |
| `UNION ALL` | Combines results, keeps duplicates |
| `INTERSECT` | Returns only common rows |
| `EXCEPT` / `MINUS` | Returns rows from first query not in second |

### Rules for Set Operators

> All queries combined with set operators must follow these rules:

- Each `SELECT` must have the **same number of columns**
- Corresponding columns must have **compatible data types**
- Column names in the result are taken from the **first SELECT**
- `ORDER BY` can only be used **once at the end**

---

## 2. Example Tables

### `employees_2023`

| id | name | department | salary |
|----|------|------------|--------|
| 1 | Murali | Engineering | 60000 |
| 2 | Rahul | HR | 45000 |
| 3 | Anita | Engineering | 70000 |
| 4 | Priya | Finance | 55000 |

### `employees_2024`

| id | name | department | salary |
|----|------|------------|--------|
| 3 | Anita | Engineering | 70000 |
| 4 | Priya | Finance | 55000 |
| 5 | Kiran | Marketing | 48000 |
| 6 | Meena | HR | 52000 |

---

## 3. UNION

`UNION` combines results of two queries and **removes duplicate rows**.

### Syntax

```sql
SELECT column1, column2 FROM table1
UNION
SELECT column1, column2 FROM table2;
```

### Example

```sql
SELECT name, department FROM employees_2023
UNION
SELECT name, department FROM employees_2024;
```

**Result:** (duplicates like Anita and Priya appear only once)

| name | department |
|------|------------|
| Murali | Engineering |
| Rahul | HR |
| Anita | Engineering |
| Priya | Finance |
| Kiran | Marketing |
| Meena | HR |

> ✅ Use `UNION` when you want a clean, deduplicated combined list.

---

## 4. UNION ALL

`UNION ALL` combines results of two queries and **keeps all duplicate rows**.

### Syntax

```sql
SELECT column1, column2 FROM table1
UNION ALL
SELECT column1, column2 FROM table2;
```

### Example

```sql
SELECT name, department FROM employees_2023
UNION ALL
SELECT name, department FROM employees_2024;
```

**Result:** (Anita and Priya appear twice)

| name | department |
|------|------------|
| Murali | Engineering |
| Rahul | HR |
| Anita | Engineering |
| Priya | Finance |
| Anita | Engineering |
| Priya | Finance |
| Kiran | Marketing |
| Meena | HR |

> ✅ Use `UNION ALL` when duplicates are meaningful or for better performance (no dedup step).

---

## 5. INTERSECT

`INTERSECT` returns only the rows that are **common to both queries**.

### Syntax

```sql
SELECT column1, column2 FROM table1
INTERSECT
SELECT column1, column2 FROM table2;
```

### Example

```sql
SELECT name, department FROM employees_2023
INTERSECT
SELECT name, department FROM employees_2024;
```

**Result:** (only rows present in both tables)

| name | department |
|------|------------|
| Anita | Engineering |
| Priya | Finance |

> ✅ Use `INTERSECT` to find employees who appear in **both** years.

---

## 6. EXCEPT / MINUS

`EXCEPT` returns rows from the **first query that do not appear in the second query**.

- `EXCEPT` → used in **PostgreSQL, SQL Server, SQLite**
- `MINUS` → used in **Oracle**

### Syntax

```sql
SELECT column1, column2 FROM table1
EXCEPT
SELECT column1, column2 FROM table2;
```

### Example — Who was in 2023 but NOT in 2024?

```sql
SELECT name, department FROM employees_2023
EXCEPT
SELECT name, department FROM employees_2024;
```

**Result:**

| name | department |
|------|------------|
| Murali | Engineering |
| Rahul | HR |

### Reverse — Who joined in 2024 but was NOT in 2023?

```sql
SELECT name, department FROM employees_2024
EXCEPT
SELECT name, department FROM employees_2023;
```

**Result:**

| name | department |
|------|------------|
| Kiran | Marketing |
| Meena | HR |

> ✅ Use `EXCEPT` to find rows **unique to one dataset**.

---

## 7. ORDER BY with Set Operators

`ORDER BY` is applied **once at the end** of the entire combined query.

```sql
SELECT name, salary FROM employees_2023
UNION
SELECT name, salary FROM employees_2024
ORDER BY salary DESC;
```

**Result:**

| name | salary |
|------|--------|
| Anita | 70000 |
| Murali | 60000 |
| Priya | 55000 |
| Meena | 52000 |
| Kiran | 48000 |
| Rahul | 45000 |

---

## 8. Set Operators with WHERE

You can use `WHERE` in each individual `SELECT` before combining.

```sql
SELECT name, department FROM employees_2023
WHERE salary > 50000
UNION
SELECT name, department FROM employees_2024
WHERE salary > 50000;
```

**Result:**

| name | department |
|------|------------|
| Murali | Engineering |
| Anita | Engineering |
| Priya | Finance |
| Meena | HR |

---

## 9. UNION vs UNION ALL — Performance

| Feature | UNION | UNION ALL |
|---------|-------|-----------|
| Removes duplicates | ✅ Yes | ❌ No |
| Performance | Slower (extra dedup step) | Faster |
| Use when | Data may have duplicates | Duplicates are expected or irrelevant |

> ✅ Prefer `UNION ALL` when you're sure there are no duplicates or duplicates don't matter — it's faster.

---

## 10. Database Support

| Operator | MySQL | PostgreSQL | SQL Server | Oracle | SQLite |
|----------|-------|------------|------------|--------|--------|
| `UNION` | ✅ | ✅ | ✅ | ✅ | ✅ |
| `UNION ALL` | ✅ | ✅ | ✅ | ✅ | ✅ |
| `INTERSECT` | ✅ (8.0+) | ✅ | ✅ | ✅ | ✅ |
| `EXCEPT` | ✅ (8.0+) | ✅ | ✅ | ❌ | ✅ |
| `MINUS` | ❌ | ❌ | ❌ | ✅ | ❌ |

---

## 11. Quick Reference Summary

| Operator | What it Returns | Duplicates |
|----------|-----------------|------------|
| `UNION` | All rows from both queries | Removed |
| `UNION ALL` | All rows from both queries | Kept |
| `INTERSECT` | Only rows common to both | Removed |
| `EXCEPT` / `MINUS` | Rows in first but not in second | Removed |

---

## 12. Full Combined Example

```sql
-- All employees across both years (no duplicates)
SELECT name, department, salary FROM employees_2023
UNION
SELECT name, department, salary FROM employees_2024
ORDER BY salary DESC;

-- Employees present in both years
SELECT name FROM employees_2023
INTERSECT
SELECT name FROM employees_2024;

-- Employees who left (in 2023 but not 2024)
SELECT name FROM employees_2023
EXCEPT
SELECT name FROM employees_2024;

-- New employees (in 2024 but not 2023)
SELECT name FROM employees_2024
EXCEPT
SELECT name FROM employees_2023;
```