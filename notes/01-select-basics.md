# [ SELECT, FROM, WHERE ] — Retrieving and Filtering Rows

**Topic:** _Basics/Filtering_ · **Date:** _2026-07-03_ · **Difficulty:** ⭐

---

## What it is

_**The Three Keywords that begins almost every SQL query:**_

- **Select** - Which **_columns_** you want.
- **From** - Which **_table_** they come from.
- **Where** - Which **_rows_** to keep (_the filter_).

**_On a Simple Note_** - _"**SELECT**" these columns "**FROM**" This Table "**WHERE**" this condition is True._

## Why / when to use it

This is the foundation for every query you will ever write. Anytime you need to pull specific data from a table - "show me the customers in Brazil", "Give me the names and salaries of the data analysts". We are using SELECT / FROM / WHERE.Everything more advanced (Joins, Group By, window functions) built on top of this base.

## Syntax

```sql
SELECT column1, column2
FROM table_name
WHERE condition;
```

- `SELECT *` grabs _all_ columns — handy for a quick look, but in real work you name the columns you actually need.
- The `WHERE` line is optional. Leave it off and you get every row.

## Example

Sample table `employees`:

| id  | name  | city      | role           | salary |
| --- | ----- | --------- | -------------- | ------ |
| 1   | Aisha | Hyderabad | Data Analyst   | 62000  |
| 2   | Rohan | Bangalore | Data Scientist | 95000  |
| 3   | Meera | Hyderabad | ML Engineer    | 110000 |
| 4   | Sam   | Pune      | Data Analyst   | 58000  |
| 5   | Lena  | Bangalore | Data Scientist | 99000  |

**Get everyone's name and salary:**

```sql
SELECT name, salary
FROM employees;
```

**Result:** all 5 rows, but only the `name` and `salary` columns.

**Get only the people in Hyderabad:**

```sql
SELECT *
FROM employees
WHERE city = 'Hyderabad';
```

**Result:** Aisha and Meera.

## In my own words

_"**SELECT**" Picks the columns(Vertical slices), "**WHERE**" picks the rows(Horizontal slices). And last but not least "**FROM**" just names the table we want to play. So simply the Query is pointing towards the grid and saying: "Give me **these** columns and **these** rows._

## Gotchas / things that tripped me up

- **Text needs single quotes:** `WHERE city = 'Hyderabad'`. Numbers don't: `WHERE salary > 90000`.
- **Equality is `=`, not `==`** in SQL — different from Python, easy to slip on.
- **WHERE filters rows, not columns.** SELECT controls which columns show; WHERE controls which rows survive.
- **The semicolon `;`** ends a statement — some tools insist on it.
- **Case sensitivity** on text matching varies by database — `'hyderabad'` may not match `'Hyderabad'`.

## Practice

**Worked example:**

> **Q:** Show the names of everyone who is a Data Scientist.

```sql
SELECT name
FROM employees
WHERE role = 'Data Scientist';
```

**My turn — I'll solve these myself before committing:**

> **Q1:** Show the `name` and `salary` of everyone.
>
> **Q2:** Show only the people in Hyderabad.
>
> **Q3:** Show everyone earning more than 90000.
