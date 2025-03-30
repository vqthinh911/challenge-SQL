# SQL Review Guide

## 1. Basic SQL 🏗️

### 1.1. SELECT – Querying Data
```sql
SELECT column1, column2 FROM table_name;
```
- Select all columns:
  ```sql
  SELECT * FROM table_name;
  ```
- Filter data using `WHERE`:
  ```sql
  SELECT * FROM employees WHERE age > 30;
  ```
- Sort results:
  ```sql
  SELECT * FROM employees ORDER BY salary DESC;
  ```
- Limit results:
  ```sql
  SELECT * FROM employees LIMIT 10;
  ```

---

### 1.2. GROUP BY & HAVING – Grouping Data
```sql
SELECT department, COUNT(*) AS num_employees
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```
💡 **Note:** `HAVING` is used for filtering after `GROUP BY`.

---

### 1.3. JOIN – Combining Tables

- **INNER JOIN** (Matches only related rows):
  ```sql
  SELECT employees.name, departments.department_name
  FROM employees
  INNER JOIN departments ON employees.department_id = departments.id;
  ```
- **LEFT JOIN** (Keeps all rows from the left table):
  ```sql
  SELECT employees.name, departments.department_name
  FROM employees
  LEFT JOIN departments ON employees.department_id = departments.id;
  ```

---

## 2. Intermediate SQL 🚀

### 2.1. Subquery (Nested Query)
```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```
💡 **Use case:** Filtering based on another query.

---

### 2.2. Common Table Expression (CTE)
```sql
WITH HighSalary AS (
  SELECT name, salary FROM employees WHERE salary > 50000
)
SELECT * FROM HighSalary;
```
💡 **Advantage:** More readable and reusable than subqueries.

---

### 2.3. Window Functions
```sql
SELECT name, department,
       salary,
       RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank
FROM employees;
```
💡 **Use case:** Calculating values over a subset of data without removing original rows.

---

## 3. Advanced SQL 🔥

### 3.1. Indexing – Improving Performance
Create an index to speed up queries:
```sql
CREATE INDEX idx_employee_name ON employees(name);
```
💡 **Indexes speed up queries but use extra storage.**

---

### 3.2. Query Optimization Tips
✅ Use `EXPLAIN` to analyze queries:
  ```sql
  EXPLAIN SELECT * FROM employees WHERE salary > 50000;
  ```
✅ Avoid `SELECT *` if unnecessary.
✅ Use indexes when filtering large datasets.

---

### 3.3. Stored Procedures – SQL Programming
```sql
DELIMITER //

CREATE PROCEDURE GetHighSalaryEmployees()
BEGIN
    SELECT * FROM employees WHERE salary > 50000;
END //

DELIMITER ;
```
💡 **Useful for executing multiple SQL statements.**

---

### 3.4. Transactions – Ensuring Data Integrity
```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT; -- Use ROLLBACK if an error occurs
```
💡 **Ensures consistency in critical operations.**

---

## 4. Practice Exercises 🎯
💡 **Would you like a set of exercises from basic to advanced?** 🚀

