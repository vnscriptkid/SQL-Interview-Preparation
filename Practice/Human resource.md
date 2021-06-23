#### 1. Get the highest, lowest, average, total salary of employees in all departments
```sql
SELECT 
	department_id,
	ROUND(MAX(salary), 2) AS highest_salary,
	ROUND(MIN(salary), 2) AS lowest_salary,
	ROUND(AVG(salary), 2) AS avg_salary,
	ROUND(SUM(salary), 2) AS total_salary
FROM employees
GROUP BY department_id;
```

#### 2. Find the difference between highest and lowest salary for each job_id
```sql
SELECT 
	department_id,
	ROUND(MAX(salary), 2) AS highest_salary,
	ROUND(MIN(salary), 2) AS lowest_salary,
	ROUND(AVG(salary), 2) AS avg_salary,
	ROUND(SUM(salary), 2) AS total_salary
FROM employees
GROUP BY department_id;
```

#### 3. Lowest salary paid by each manager
```sql
SELECT
	MIN(salary),
	manager_id AS paid_by_manager
FROM employees
GROUP BY manager_id
ORDER BY 1;
```

#### 4. Average salary of departments that have more than 10 employees
```sql
SELECT
	department_id,
	ROUND(AVG(salary), 2) AS average_salary,
	COUNT(*) AS num_of_employees
FROM
	employees
GROUP BY 1
HAVING COUNT(*) >= 10;
```

#### 5. Get average salary of all posts excluding ones from 'IT_PROG'
```sql
SELECT
	job_id,
	AVG(salary)
FROM employees
WHERE job_id <> 'IT_PROG'
GROUP BY job_id;
```

#### 6. Get list of average salary of every post that is >= 5000
```sql
SELECT
	job_id,
	MAX(salary)
FROM
	employees
GROUP BY job_id
HAVING MAX(salary) >= 5000
ORDER BY 2;
```

#### 7. Display employee name (first_name, last_name) using alias ("First Name", "Last Name")
```sql
SELECT
	first_name AS "First Name",
	last_name AS "Last Name"
FROM employees;
```

#### 8. Get first name, last name, salary, 15% of salary for every employee
```sql
SELECT
	first_name AS "First Name",
	last_name AS "Last Name",
	salary,
	ROUND(salary * .15, 2) AS "15% of salary"
FROM employees
ORDER BY salary DESC;
```

#### 9. Query job_id and all employee ids within that job_id group
```sql
SELECT
	job_id,
	ARRAY_AGG(employee_id)
FROM employees
GROUP BY job_id;
```

#### 10. Get all employees, discard the last 4 characters from email
```sql
SELECT
	employee_id,
	email,
	SUBSTR(email, 1, LENGTH(email) - 4)
FROM
	employees;
```

#### 11. Find all employees whose first_name starts with A, C or M
```sql
SELECT
	employee_id,
	first_name
FROM employees
WHERE first_name LIKE 'A%'
OR first_name LIKE 'C%'
OR first_name LIKE 'M%'
ORDER BY 2;
```

#### 12. Get first name of employees in uppercase, lowercase, and capitalized
```sql
SELECT
	first_name,
	UPPER(first_name),
	LOWER(first_name),
	INITCAP(first_name)
FROM employees;
```

#### 13. Get first 3 characters from first_name of all employees
```sql
SELECT
	first_name,
	SUBSTR(first_name, 1, 3)
FROM employees;
```

#### 14.  Get the number of unique job_ids in employees table
```sql
SELECT
	COUNT(DISTINCT job_id) AS "Unique designations"
FROM employees;
```

#### 15. Get first_name, last_name, department_id of employees who work in department_id 30 or 100
```sql
SELECT
	first_name,
	last_name,
	department_id
FROM employees
WHERE department_id IN (30, 100)
ORDER BY department_id;
```

#### 16. Display name, salary of employees who do not work in department 30 or 100 and salary is from 10000 and 20000
```sql
SELECT
	first_name,
	last_name,
	department_id,
	salary
FROM employees
WHERE 
	department_id NOT IN (30, 100)
	AND salary NOT BETWEEN 10000 AND 20000;
```

#### 17. Find letter "C" in last_name at 3th or greater position
```sql
SELECT
	last_name,
	POSITION('C' IN last_name)
FROM employees
WHERE POSITION('C' IN last_name) > 2;
```

#### 18. Update phone numbers that contain '123' with '888'
```sql
UPDATE employees
SET phone_number = REPLACE(phone_number, '123', '888')
WHERE phone_number LIKE '%123%';
```

#### 19. Get monthly salary of each employee
```sql
SELECT
	employee_id,
	ROUND(salary / 12, 2)
FROM
	employees
ORDER BY 2 DESC;
```

#### 20. Find average salary and number of employees
```sql
SELECT
	COUNT(*) AS "Number of employees",
	ROUND(AVG(salary), 2) AS "Average salary"
FROM employees;
```

#### 21. Display last name of employees whose last name contains exactly 6 characters
```sql
-- One way
SELECT
	last_name,
	LENGTH(last_name)
FROM employees
WHERE LENGTH(last_name) = 6

-- The other
SELECT
	last_name
FROM employees
WHERE last_name LIKE '______';
```
