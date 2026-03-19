# SQL Filtering — WHERE Condition Guide

This document explains **Filtering Data in SQL** using the `WHERE` clause with all types of operators and conditions.

---

## 1. What is Filtering?

Filtering means **retrieving only specific rows** from a table that match a given condition.

The `WHERE` clause is used to filter records in `SELECT`, `UPDATE`, and `DELETE` statements.

### Syntax

```sql
SELECT column1, column2
FROM table_name
WHERE condition;
```

---

## 2. WHERE Condition

The `WHERE` clause specifies the condition that rows must satisfy to be included in the result.

### Example Table — `employees`

| id | name | department | salary | email |
|----|------|------------|--------|-------|
| 1 | Murali | Engineering | 60000 | murali@mail.com |
| 2 | Rahul | HR | 45000 | NULL |
| 3 | Anita | Engineering | 70000 | anita@mail.com |
| 4 | Priya | Finance | 55000 | priya@mail.com |
| 5 | Kiran | Marketing | 48000 | NULL |
| 6 | Meena | HR | 52000 | meena@mail.com |

### Basic WHERE Example

```sql
SELECT *
FROM employees
WHERE department = 'Engineering';
```

**Result:**

| id | name | department | salary |
|----|------|------------|--------|
| 1 | Murali | Engineering | 60000 |
| 3 | Anita | Engineering | 70000 |

---

## 3. Comparison Operators

Used to compare a column's value against a specific value.

| Operator | Meaning | Example |
|----------|---------|---------|
| `=` | Equal to | `salary = 60000` |
| `!=` or `<>` | Not equal to | `department != 'HR'` |
| `>` | Greater than | `salary > 50000` |
| `<` | Less than | `salary < 50000` |
| `>=` | Greater than or equal | `salary >= 55000` |
| `<=` | Less than or equal | `salary <= 55000` |

### Examples

**Equal to:**
```sql
SELECT * FROM employees
WHERE department = 'HR';
```

**Not equal to:**
```sql
SELECT * FROM employees
WHERE department != 'HR';
```

**Greater than:**
```sql
SELECT name, salary FROM employees
WHERE salary > 50000;
```

**Less than or equal:**
```sql
SELECT name, salary FROM employees
WHERE salary <= 55000;
```

**Result** (`salary <= 55000`):

| name | salary |
|------|--------|
| Rahul | 45000 |
| Priya | 55000 |
| Kiran | 48000 |

---

## 4. Logical Operators

Used to **combine multiple conditions** in a `WHERE` clause.

| Operator | Description |
|----------|-------------|
| `AND` | Both conditions must be true |
| `OR` | At least one condition must be true |
| `NOT` | Reverses / negates the condition |

### AND — Both conditions must be true

```sql
SELECT * FROM employees
WHERE department = 'Engineering'
AND salary > 60000;
```

**Result:**

| id | name | department | salary |
|----|------|------------|--------|
| 3 | Anita | Engineering | 70000 |

---

### OR — At least one condition must be true

```sql
SELECT * FROM employees
WHERE department = 'HR'
OR department = 'Finance';
```

**Result:**

| id | name | department | salary |
|----|------|------------|--------|
| 2 | Rahul | HR | 45000 |
| 4 | Priya | Finance | 55000 |
| 6 | Meena | HR | 52000 |

---

### NOT — Negates the condition

```sql
SELECT * FROM employees
WHERE NOT department = 'Engineering';
```

**Result:** All employees except those in Engineering.

---

### Combining AND + OR

Use **parentheses** to control precedence.

```sql
SELECT * FROM employees
WHERE (department = 'Engineering' OR department = 'Finance')
AND salary > 54000;
```

**Result:**

| id | name | department | salary |
|----|------|------------|--------|
| 1 | Murali | Engineering | 60000 |
| 3 | Anita | Engineering | 70000 |
| 4 | Priya | Finance | 55000 |

---

## 5. Range Filtering (BETWEEN)

`BETWEEN` filters values within a **specific range** (inclusive on both ends).

### Syntax

```sql
WHERE column BETWEEN value1 AND value2;
```

### Example — Salary Range

```sql
SELECT * FROM employees
WHERE salary BETWEEN 48000 AND 65000;
```

**Result:**

| id | name | department | salary |
|----|------|------------|--------|
| 1 | Murali | Engineering | 60000 |
| 4 | Priya | Finance | 55000 |
| 5 | Kiran | Marketing | 48000 |
| 6 | Meena | HR | 52000 |

### NOT BETWEEN — Outside the range

```sql
SELECT * FROM employees
WHERE salary NOT BETWEEN 48000 AND 65000;
```

**Result:**

| id | name | department | salary |
|----|------|------------|--------|
| 2 | Rahul | HR | 45000 |
| 3 | Anita | Engineering | 70000 |

### BETWEEN with Dates

```sql
SELECT * FROM orders
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';
```

> ✅ `BETWEEN` is **inclusive** — both boundary values are included in the result.

---

## 6. Membership Filtering (IN)

`IN` checks if a column's value matches **any value in a list**.

### Syntax

```sql
WHERE column IN (value1, value2, value3, ...);
```

### Example

```sql
SELECT * FROM employees
WHERE department IN ('HR', 'Finance', 'Marketing');
```

**Result:**

| id | name | department | salary |
|----|------|------------|--------|
| 2 | Rahul | HR | 45000 |
| 4 | Priya | Finance | 55000 |
| 5 | Kiran | Marketing | 48000 |
| 6 | Meena | HR | 52000 |

### NOT IN — Exclude specific values

```sql
SELECT * FROM employees
WHERE department NOT IN ('HR', 'Finance');
```

**Result:**

| id | name | department | salary |
|----|------|------------|--------|
| 1 | Murali | Engineering | 60000 |
| 3 | Anita | Engineering | 70000 |
| 5 | Kiran | Marketing | 48000 |

### IN with Numbers

```sql
SELECT * FROM employees
WHERE id IN (1, 3, 5);
```

### IN with Subquery

```sql
SELECT * FROM employees
WHERE department IN (
    SELECT department FROM departments WHERE location = 'Bangalore'
);
```

---

## 7. Pattern Search (LIKE)

`LIKE` is used for **pattern matching** in string columns.

### Wildcard Characters

| Wildcard | Meaning |
|----------|---------|
| `%` | Matches any number of characters (including zero) |
| `_` | Matches exactly one character |

### Syntax

```sql
WHERE column LIKE 'pattern';
```

---

### Starts With

```sql
SELECT * FROM employees
WHERE name LIKE 'M%';
```

**Result:** Murali, Meena

---

### Ends With

```sql
SELECT * FROM employees
WHERE name LIKE '%a';
```

**Result:** Anita, Priya, Meena

---

### Contains

```sql
SELECT * FROM employees
WHERE name LIKE '%ra%';
```

**Result:** Rahul, Priya (contains "ra")

---

### Exactly N Characters

```sql
SELECT * FROM employees
WHERE name LIKE '_____';
```

Returns names with exactly 5 characters — e.g., `Rahul`, `Anita`, `Priya`, `Kiran`, `Meena`

---

### Single Character Wildcard

```sql
SELECT * FROM employees
WHERE name LIKE 'M___a';
```

Returns names starting with M, ending with a, with exactly 3 characters in between — e.g., `Meena`

---

### NOT LIKE

```sql
SELECT * FROM employees
WHERE name NOT LIKE 'M%';
```

Returns all employees whose name does **not** start with M.

---

## 8. NULL Filtering (IS NULL / IS NOT NULL)

`NULL` represents a **missing or unknown value**. You cannot use `=` to check for NULL.

### IS NULL — Find missing values

```sql
SELECT * FROM employees
WHERE email IS NULL;
```

**Result:**

| id | name | department |
|----|------|------------|
| 2 | Rahul | HR |
| 5 | Kiran | Marketing |

### IS NOT NULL — Find existing values

```sql
SELECT * FROM employees
WHERE email IS NOT NULL;
```

**Result:** All employees who have an email address.

---

## 9. Combining All Filters — Full Example

```sql
SELECT name, department, salary
FROM employees
WHERE department IN ('Engineering', 'Finance')
AND salary BETWEEN 50000 AND 75000
AND name LIKE 'A%'
AND email IS NOT NULL
ORDER BY salary DESC;
```

**What this does:**
- Picks employees from Engineering or Finance
- With salary between 50,000 and 75,000
- Whose name starts with A
- Who have a valid email
- Sorted by salary (highest first)

**Result:**

| name | department | salary |
|------|------------|--------|
| Anita | Engineering | 70000 |

---

## 10. Quick Reference Summary

| Filter Type | Operator | Example |
|-------------|----------|---------|
| Comparison | `=`, `!=`, `>`, `<`, `>=`, `<=` | `salary > 50000` |
| Logical | `AND`, `OR`, `NOT` | `dept = 'HR' AND salary > 45000` |
| Range | `BETWEEN ... AND ...` | `salary BETWEEN 40000 AND 70000` |
| Membership | `IN (...)` | `dept IN ('HR', 'Finance')` |
| Pattern Search | `LIKE` | `name LIKE 'M%'` |
| NULL Check | `IS NULL`, `IS NOT NULL` | `email IS NULL` |