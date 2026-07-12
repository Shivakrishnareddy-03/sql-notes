# SQL Grand Test — Answer Key

Correct solutions with the key idea behind each. Back to the [questions](grand-test.md).

> Many questions have more than one valid answer — if yours returns the right rows with clean, correct syntax, it counts.

---

**1. Employees earning more than 90000.**

```sql
SELECT name, salary FROM employees
WHERE salary > 90000;
```

**2. The 3 most recently hired.**

```sql
SELECT name, hire_date FROM employees
ORDER BY hire_date DESC
LIMIT 3;
```

_(Dates in `YYYY-MM-DD` text sort correctly.)_

**3. Total salary bill.**

```sql
SELECT SUM(salary) AS total_salary_bill FROM employees;
```

**4. Employees per department.**

```sql
SELECT dept_id, COUNT(emp_id) AS no_of_emp FROM employees
GROUP BY dept_id
ORDER BY no_of_emp DESC;
```

**5. Average salary per department, rounded, highest first.**

```sql
SELECT dept_id, ROUND(AVG(salary)) AS avg_sal FROM employees
GROUP BY dept_id
ORDER BY avg_sal DESC;
```

**6. Departments with more than 3 employees.**

```sql
SELECT dept_id, COUNT(*) AS total_emp FROM employees
GROUP BY dept_id
HAVING COUNT(*) > 3;
```

_Filtering on an aggregate → HAVING, not WHERE._

**7. Each employee alongside their department name.**

```sql
SELECT e.name, d.dept_name FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
```

_INNER join — Nisha (no dept) is correctly excluded._

**8. Every employee + dept name, including those with no department.**

```sql
SELECT e.name, d.dept_name FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id;
```

_"Every employee" → LEFT JOIN keeps Nisha (dept shows NULL). More precise/portable than FULL JOIN here._

**9. Every department's name + employee count, including zero-employee depts.**

```sql
SELECT d.dept_name, COUNT(e.name) AS total_emp FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.dept_id
GROUP BY d.dept_name;
```

_RIGHT join keeps all departments; `COUNT(e.name)` (not `COUNT(*)`) makes Operations correctly show 0._

**10. Each department's name + average salary, highest first.**

```sql
SELECT d.dept_name, ROUND(AVG(e.salary)) AS avg_sal FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.dept_id
GROUP BY d.dept_name
ORDER BY avg_sal DESC;
```

_Answer exactly what's asked — average only._

**11. Employees earning above the company average.**

```sql
SELECT name, salary FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

_Scalar subquery: the inner query returns one value, compared with `>`._

**12. Employees in Bangalore departments (subquery).**

```sql
SELECT name FROM employees
WHERE dept_id IN (SELECT dept_id FROM departments WHERE city = 'Bangalore');
```

_Inner query returns multiple dept_ids → use `IN`, never `=`. Don't forget the `WHERE city = 'Bangalore'` filter inside._

**13. Each employee's rank within their own department by salary.**

```sql
SELECT name, dept_id, salary,
       RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS salary_rank
FROM employees;
```

_`PARTITION BY` = which group, `ORDER BY` (inside OVER) = ranked by what. Both are required — without the ORDER BY, the rank is meaningless. Use `DENSE_RANK`/`ROW_NUMBER` instead depending on how you want ties handled._

**14. Top earner in each department (one row per department).**

```sql
SELECT * FROM (
    SELECT name, dept_id, salary,
           ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rn
    FROM employees
) AS ranked
WHERE rn = 1;
```

_Rank inside a subquery, filter outside (you can't filter a window function in WHERE). `ROW_NUMBER` guarantees exactly one winner per department — `RANK` could return two if the top salaries tie._

**15. Total hours logged per department, highest first.**

```sql
SELECT d.dept_name, SUM(a.hours) AS total_hrs
FROM assignments a
JOIN employees e ON a.emp_id = e.emp_id
JOIN departments d ON e.dept_id = d.dept_id
GROUP BY d.dept_name
ORDER BY total_hrs DESC;
```

_Chain three tables (assignments → employees → departments), then GROUP BY + SUM. Operations has no assignments, so INNER joins correctly drop it._
