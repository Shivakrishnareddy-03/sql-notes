# ORDER BY & LIMIT

**Topic:** _ORDER BY, LIMIT_ · **Date:** _2026-07-04_ · **Difficulty:** ⭐

---

## What it is

_This clause is mainly used for sorting the data and extracting the top rows from it._

- **ORDER BY**
  - Its job is to sort the data in ASC or DESC order.
  - By default, it uses ASC.
  - We can use it on multiple columns at the same time.

```sql
SELECT * FROM EMPLOYEES
ORDER BY CITY, SALARY DESC;
```

- When sorting by multiple columns, the first column sorts first, and the second one breaks ties _within_ it. Above: city sorts A–Z, and inside each city, salary goes high → low.

- **LIMIT**
  - It says how many rows need to be returned.
  - Almost always paired with its buddy **ORDER BY**, because this combo solves the most common question in data work: _top N of something_ (e.g. top highest paid).

```sql
SELECT * FROM EMPLOYEES
ORDER BY SALARY DESC
LIMIT 3;
```

## Why / when to use it

_When we need to extract the top or the least rows of a specific table, we use this clause._

## Syntax

```sql
SELECT column1, column2
FROM table_name
WHERE condition
ORDER BY column1 DESC
LIMIT 5;
```

## Example

**Q: Show the top 2 highest-paid employees (name and salary).**

```sql
SELECT NAME, SALARY FROM EMPLOYEES
ORDER BY SALARY DESC
LIMIT 2;
```

**Result:**

```
name   | salary
-------+--------
Meera  | 110000
Lena   |  99000
```

## In my own words

_**ORDER BY** is used when we need to sort data DESC or ASC. **LIMIT** is for extracting the top rows. Using this combo we can pull the top (or least) rows from a table._

## Gotchas / things that tripped me up

- **The keyword order is fixed:** `SELECT → FROM → WHERE → ORDER BY → LIMIT`. Put `ORDER BY` before `WHERE` and it errors.
- **`LIMIT` isn't universal:** it works in SQLite, MySQL and PostgreSQL. SQL Server uses `TOP`, Oracle uses `FETCH`.
- **"Second highest" with OFFSET has a catch:** `LIMIT 1 OFFSET 1` works, but if two people are _tied_ for top salary it can give the wrong answer. There's a cleaner `DENSE_RANK()` way I'll learn later (window functions, note 07).

## Solved Practice Question

> **Q:** Show the employee with the **second-highest** salary.

```sql
SELECT * FROM EMPLOYEES
ORDER BY SALARY DESC
LIMIT 1 OFFSET 1;
```

**Result:**

```
id | name | city      | role           | salary
---+------+-----------+----------------+--------
5  | Lena | Bangalore | Data Scientist |  99000
```

_Logic: sort salaries high → low, skip the top one (`OFFSET 1`), then take one (`LIMIT 1`)._

## Practice

_Solve each one yourself on the `employees` table._

### Part A — Today's topic (ORDER BY & LIMIT)

> 1.  List all employees sorted by salary, lowest -> highest.
> 2.  Show the top 3 highest-paid employees (name and salary).
> 3.  Sort employees by role (A–Z), and within each role, salary highest first.

### Part B — Combined with note 01 (SELECT + WHERE + sorting)

> 4.  Show the name and salary of everyone in Bangalore, highest salary first.
> 5.  Show the single highest-paid Data Scientist.
> 6.  List all Data Analysts sorted by salary low -> high (name and salary).
> 7.  🌶️ Find the second-lowest paid employee.
