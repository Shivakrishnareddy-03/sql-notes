# GROUP BY & HAVING

**Topic:** _Grouping & group-filtering_ · **Date:** _2026-07-07_ · **Difficulty:** ⭐⭐

---

## What it is

- **GROUP BY** bundles rows into groups and runs an aggregate on _each group separately_. It turns "one summary for everything" into "one summary **per group**."
- **HAVING** is like WHERE, but for _groups_. it filters on aggregate results, _after_ the grouping happens.

Mental model for GROUP BY: whenever you hear **"for each"** i.e., average salary _for each_ role, count _for each_ city — that's GROUP BY.

## Why / when to use it

- **GROUP BY:** when you want a summary broken down per category, not one number for the whole table.
- **HAVING:** when you need to filter based on an aggregate (e.g. "only cities with more than 1 employee") something WHERE can't do.

## Syntax

```sql
SELECT column, AGG(column) AS alias
FROM table_name
WHERE row_condition        -- filters rows BEFORE grouping
GROUP BY column
HAVING agg_condition       -- filters GROUPS after grouping
ORDER BY alias;
```

## Key details (with examples)

**GROUP BY**

```sql
SELECT city, COUNT(*) AS num_employees
FROM employees
GROUP BY city;
```

```
city      | num_employees
----------+--------------
Bangalore | 2
Hyderabad | 2
Pune      | 1
```

Now every city gets its own count — a real answer, not one meaningless total.

**HAVING — filter the groups:**

```sql
SELECT city, COUNT(*) AS num_employees
FROM employees
GROUP BY city
HAVING COUNT(*) > 1;
```

Returns only Bangalore and Hyderabad (Pune has 1, drops out).

**All together — read the execution order right off it:**

```sql
SELECT role, AVG(salary) AS avg_sal
FROM employees
WHERE salary > 50000        -- 1. filter rows
GROUP BY role               -- 2. group them
HAVING AVG(salary) > 90000  -- 3. filter groups
ORDER BY avg_sal DESC;      -- 4. sort
```

`WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY` — exactly the order from note 01.

## In my own words

> _**Group By** is for grouping the rows which almost always pair with aggregation. Coming to the **Having** its works same as **Where**. But **Having** filters after grouping the rows._

## Gotchas / things that tripped me up

- **The golden rule:** every column in SELECT must _either_ be in the GROUP BY _or_ inside an aggregate. A naked column that's neither (e.g. `SELECT city, role, COUNT(*) ... GROUP BY city`) is a bug — strict databases (PostgreSQL, SQL Server) **throw an error**; lenient ones (SQLite, old MySQL) **silently pick one value at random** (the `Aisha | 5` lie). Fix: add the column to GROUP BY, or aggregate it.
- **WHERE vs HAVING:** WHERE filters _rows_ (before grouping); HAVING filters _groups_ (after). Filtering on a plain column → WHERE. Filtering on an aggregate (COUNT/SUM/AVG) → HAVING.
- **HAVING without GROUP BY** technically works (treats the whole table as one group), but if you're reaching for HAVING with no GROUP BY, you probably want WHERE.
- **Repeat the full aggregate in HAVING / ORDER BY** (e.g. `HAVING AVG(salary) > 90000`, not `HAVING avg_sal`). The alias may not exist yet when HAVING runs (execution order), so repeating the aggregate is the safe, portable habit.

---

## Practice

_Solve each one yourself on the `employees` table._

1. Count the number of employees in each city.
2. Show the average salary for each role (clean column name, rounded).
3. Show the total salary paid per city, highest total first.
4. Show only the roles that have more than one employee.
5. Show the cities where the average salary is above 80000.
6. For each city, show the highest salary, sorted highest → lowest.
7. Show the average salary per role, but only for roles whose average is above 90000, sorted descending.
8. Count how many employees earn more than 60000, grouped by city, showing only cities with at least one such employee.
9. 🧠 **Diagnose**: why does `SELECT city, role, COUNT(*) FROM employees GROUP BY city;` fail (or mislead)? What rule does it break?
