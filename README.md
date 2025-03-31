# SQL Review Guide

# Hướng Dẫn Cơ Bản về SQL'

## 0. Cấu trúc truy vấn SQL
  ```sql
SELECT column
FROM table
WHERE dk1,..
GROUP BY column
HAVING dk2
ORDER BY column
```

## 1. Câu lệnh SELECT – Lấy dữ liệu từ bảng
Câu lệnh `SELECT` được sử dụng để truy vấn dữ liệu từ cơ sở dữ liệu.
```sql
SELECT column1, column2 FROM table_name;
```
- Lấy tất cả các cột:
  ```sql
  SELECT * FROM table_name;
  ```
- Lọc dữ liệu với `WHERE`:
  ```sql
  SELECT * FROM employees WHERE age > 30;
  ```
  **Lưu ý:** `WHERE` dùng để lọc dữ liệu trước khi truy vấn.
- Sắp xếp kết quả với `ORDER BY`:
  ```sql
  SELECT * FROM employees ORDER BY salary DESC;
  ```
  **Lưu ý:** `ASC` sắp xếp tăng dần (mặc định), `DESC` sắp xếp giảm dần.
- Giới hạn số dòng trả về với `LIMIT`:
  ```sql
  SELECT * FROM employees LIMIT 10;
  ```
  **Lưu ý:** `LIMIT` giúp giới hạn số lượng kết quả trả về để tối ưu hiệu suất.

---

## 2. GROUP BY & HAVING – Nhóm dữ liệu
Câu lệnh `GROUP BY` được sử dụng để nhóm các hàng có cùng giá trị lại với nhau.
```sql
SELECT department, COUNT(*) AS num_employees
FROM employees
GROUP BY department;
```
- Lọc nhóm dữ liệu bằng `HAVING`:
  ```sql
  SELECT department, COUNT(*) AS num_employees
  FROM employees
  GROUP BY department
  HAVING COUNT(*) > 5;
  ```
  **Lưu ý:** `HAVING` lọc sau khi nhóm dữ liệu, còn `WHERE` lọc trước khi nhóm.

---

## 3. Sự khác nhau giữa ORDER BY, HAVING, GROUP BY và WHERE

### 🔹 `WHERE`
- Dùng để lọc dữ liệu trước khi nhóm (`GROUP BY`) hoặc sắp xếp (`ORDER BY`).
- Không thể dùng với hàm tổng hợp như `COUNT()`, `SUM()`, `AVG()`.
- Ví dụ:
  ```sql
  SELECT * FROM employees WHERE salary > 3000;
  ```

### 🔹 `GROUP BY`
- Nhóm dữ liệu theo một hoặc nhiều cột, thường đi kèm với các hàm tổng hợp (`COUNT()`, `SUM()`, `AVG()`).
- Ví dụ:
  ```sql
  SELECT department, COUNT(*) FROM employees GROUP BY department;
  ```

### 🔹 `HAVING`
- Dùng để lọc dữ liệu sau khi nhóm (`GROUP BY`).
- Có thể sử dụng với các hàm tổng hợp.
- Phải nhóm dữ liệu đẩy đủ dể tính FUNCTION trên SELECT
- Nhưng không thể sử dụng ALIAS đã gợi ở SELECT
- Ví dụ:
  ```sql
  SELECT department, COUNT(*)
  FROM employees
  GROUP BY department
  HAVING COUNT(*) > 5;
  ```

### 🔹 `ORDER BY`
- Dùng để sắp xếp dữ liệu theo một hoặc nhiều cột.
- Có thể dùng với các hàm tổng hợp nhưng không lọc dữ liệu như `WHERE` hoặc `HAVING`.
- Không được dùng trong  CTE
- Ví dụ:
  ```sql
  SELECT name, salary FROM employees ORDER BY salary DESC;
  ```

💡 **Tóm tắt:**
| Câu lệnh   | Mục đích | Có thể dùng với hàm tổng hợp? | Khi nào chạy? |
|------------|---------|--------------------------------|----------------|
| `WHERE`    | Lọc dữ liệu trước khi nhóm hoặc sắp xếp | ❌ Không | Trước `GROUP BY` |
| `GROUP BY` | Nhóm dữ liệu theo cột | ✅ Có thể | Sau `WHERE`, trước `HAVING` |
| `HAVING`   | Lọc sau khi nhóm dữ liệu | ✅ Có thể | Sau `GROUP BY` |
| `ORDER BY` | Sắp xếp kết quả trả về | ✅ Có thể | Cuối cùng |

---

## 4. Kinh nghiệm sử dụng các câu lệnh SQL cơ bản
✅ **Hạn chế sử dụng `SELECT *`**, chỉ chọn các cột cần thiết để tối ưu hiệu suất.
✅ **Luôn sử dụng `WHERE` khi cần lọc dữ liệu** để tránh truy xuất dữ liệu không cần thiết.
✅ **Dùng `ORDER BY` hợp lý**, tránh sắp xếp trên dữ liệu lớn nếu không cần thiết.
✅ **Sử dụng `LIMIT` để tối ưu truy vấn**, đặc biệt khi làm việc với bảng lớn.
✅ **Sử dụng `HAVING` đúng mục đích**, không dùng thay cho `WHERE` nếu không cần nhóm dữ liệu.

---

💡 **Thực hành là cách tốt nhất để thành thạo SQL!** 🚀



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

