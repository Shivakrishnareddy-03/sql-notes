# SQL Grand Test — the whole core

A mixed, **unlabeled** exam covering everything: SELECT/WHERE, ORDER BY/LIMIT, aggregates, GROUP BY/HAVING, JOINs, subqueries, and window functions.

No topic hints — each question just states a problem, and _you_ decide which tools it needs. Try to solve all 15 before peeking at the [answer key](grand-test-answers.md).

**Golden rule:** write → **run** → read the output → confirm it answers the exact question → then check.

---

## Setup — paste this into your SQL editor first

```sql
CREATE TABLE departments (
  dept_id INTEGER, dept_name TEXT, city TEXT, budget INTEGER
);
INSERT INTO departments VALUES
(1,'Engineering','Bangalore',500000),
(2,'Analytics','Hyderabad',300000),
(3,'Marketing','Mumbai',200000),
(4,'Research','Bangalore',400000),
(5,'Operations','Chennai',150000);

CREATE TABLE employees (
  emp_id INTEGER, name TEXT, dept_id INTEGER, role TEXT,
  salary INTEGER, hire_date TEXT, manager_id INTEGER
);
INSERT INTO employees VALUES
(1,'Aarav',1,'Engineering Manager',120000,'2019-03-15',NULL),
(2,'Diya',1,'Software Engineer',85000,'2020-06-01',1),
(3,'Kabir',1,'Software Engineer',85000,'2021-02-10',1),
(4,'Meera',2,'Analytics Manager',110000,'2018-09-20',NULL),
(5,'Rohan',2,'Data Scientist',95000,'2020-01-15',4),
(6,'Ananya',2,'Data Analyst',72000,'2021-07-30',4),
(7,'Vikram',2,'Data Analyst',72000,'2022-03-12',4),
(8,'Isha',3,'Marketing Manager',90000,'2019-11-05',NULL),
(9,'Arjun',3,'Marketing Specialist',60000,'2021-05-18',8),
(10,'Priya',4,'Research Lead',115000,'2017-08-22',NULL),
(11,'Karan',4,'Research Scientist',98000,'2020-10-01',10),
(12,'Sara',4,'Research Scientist',98000,'2021-12-15',10),
(13,'Nisha',NULL,'Contractor',65000,'2023-01-10',NULL),
(14,'Dev',1,'Junior Developer',58000,'2023-06-01',1);

CREATE TABLE projects (
  project_id INTEGER, project_name TEXT, dept_id INTEGER, budget INTEGER, status TEXT
);
INSERT INTO projects VALUES
(1,'Apollo',1,150000,'Active'),
(2,'Beacon',2,90000,'Active'),
(3,'Cobalt',2,60000,'Completed'),
(4,'Delta',4,120000,'Active'),
(5,'Echo',3,40000,'On Hold'),
(6,'Falcon',1,80000,'Completed');

CREATE TABLE assignments (
  emp_id INTEGER, project_id INTEGER, hours INTEGER
);
INSERT INTO assignments VALUES
(2,1,120),(3,1,90),(14,1,60),
(5,2,100),(6,2,80),(7,3,110),
(11,4,130),(12,4,95),(10,4,40),
(9,5,50),(2,6,70),(3,6,85);
```

---

## The questions

1. List the `name` and `salary` of all employees earning more than 90000.
2. Show the 3 most recently hired employees (`name` and `hire_date`).
3. What is the total salary bill across the whole company?
4. Show how many employees are in each `dept_id`.
5. Show the average salary per `dept_id`, rounded, highest first.
6. Show only the `dept_id`s that have more than 3 employees.
7. List each employee's `name` alongside their department `dept_name`.
8. List **every** employee's `name` and their `dept_name`, including anyone with no department.
9. Show every department's `dept_name` and how many employees it has — **including departments with zero employees.**
10. Show each department's `dept_name` and its average salary, highest average first.
11. List the `name` and `salary` of employees who earn more than the **company-wide average** salary.
12. List the names of employees who work in departments located in **Bangalore** (use a subquery).
13. Show each employee's `name`, `dept_id`, `salary`, and their **rank within their own department** by salary (highest first).
14. Show the **top earner in each department** — one row per department (`name`, `dept_id`, `salary`).
15. For each department, show the `dept_name` and the **total hours** its employees have logged across all projects, highest first.
