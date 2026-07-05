# Aggregate Functions (COUNT, SUM, AVG, MIN, MAX)

**Topic:** _Aggregates_ · **Date:** _2026-07-05_ · **Difficulty:** ⭐⭐

---

## What it is

Aggregate functions **squish many rows into a single summary number.**

The five core functions:

- **COUNT()** → how many rows
- **SUM()** → adds up a column
- **AVG()** → the average of a column
- **MIN()** → the smallest value
- **MAX()** → the largest value

## Why / when to use it

When you don't want the raw rows — you want a _summary_: a total, an average, a count, a highest/lowest. "How many customers?", "What's our total revenue?", "What's the average order value?" all live here.

## Syntax

```sql
SELECT COUNT(*) AS total,
AVG(column) AS average
FROM table_name
WHERE condition;
```

## Key details (with examples)

**COUNT(\*) vs COUNT(column)**

```sql
SELECT COUNT(*) AS total_rows FROM employees;   -- counts ALL rows -> 5
SELECT COUNT(salary) AS with_salary FROM employees;  -- counts rows where salary IS NOT NULL
```

`COUNT(*)` counts every row. `COUNT(column)` skips rows where that column is NULL.

**Aggregates ignore NULLs.** `AVG`, `SUM`, `MIN`, `MAX` all skip blanks — a NULL is _not_ treated as zero. `AVG` of `[100, NULL]` is 100, not 50.

**COUNT(DISTINCT ...) counts unique values**

```sql
SELECT COUNT(DISTINCT city) AS num_cities FROM employees;  -- -> 3
```

**Always alias your aggregates with AS** — the raw output name (`COUNT(*)`) is ugly:

```sql
SELECT ROUND(AVG(salary), 0) AS average_salary FROM employees;  -- -> 84800
```

**Aggregates + WHERE** — WHERE filters the rows first, then the aggregate summarises what survives:

```sql
SELECT AVG(salary) AS avg_ds
FROM employees
WHERE role = 'Data Scientist';   -- -> 97000
```

## In my own words

> _On a simple note aggregations are something which gives the overall summary like maximum salary, total no. of employees from HYD etc.._

## Gotchas / things that tripped me up

- **Mixing a plain column with an aggregate misleads you:** `SELECT name, COUNT(*) FROM employees;` returns a _random_ name next to the total (e.g. `Aisha | 5`) — meaningless. To pair a column with an aggregate properly, you need **`GROUP BY`** (note 04).
- **Multi-word aliases need an underscore or quotes:** `AS average_salary` ✅ or `AS "Average Salary"` ✅. A bare space — `AS AVERAGE SALARY` — breaks the query.
- **Commas between columns:** listing two aggregates? `MAX(salary) AS max_salary, MIN(salary) AS min_salary` — the comma is required.
- **Keywords are case-insensitive, but quoted VALUES are case-sensitive:** `select`/`SELECT` and `SALARY`/`salary` are fine, but `'Data Scientist'` ≠ `'DATA SCIENTIST'`. A wrong-case value silently returns nothing.

---

## Practice

_Solve each one yourself on the `employees` table._

1. Count the total number of employees.
2. Find the total of all salaries.
3. Show the average salary, rounded to a whole number, with a clean column name.
4. Show the highest and lowest salary in a single query.
5. How many distinct cities are in the table?
6. How many distinct roles are in the table?
7. What's the average salary of Data Scientists only?
8. How many employees are in Hyderabad?
9. What's the total salary paid to everyone in Bangalore?
10. What's the average salary of employees earning more than 60000?
