The **`COUNT()`** function in SQL is used to **count the number of rows** that match a specific condition.  
It’s one of the most commonly used **aggregate functions**, along with `SUM()`, `AVG()`, `MIN()`, and `MAX()`.

---
### 🧠 **1. Basic Syntax**
`SELECT COUNT(column_name) FROM table_name WHERE condition;`

---
### 📊 **2. Different Uses of COUNT()**
#### ✅ **a) Count all rows in a table**
`SELECT COUNT(*) AS total_rows FROM employees;`
> Counts **every row** in the `employees` table — even if there are `NULL` values.

---
#### ✅ **b) Count only non-null values**
`SELECT COUNT(department_id) AS dept_count FROM employees;`
> Counts only rows where `department_id` **is not NULL**.

---
#### ✅ **c) Count with a condition**
`SELECT COUNT(*) AS total_IT FROM employees WHERE department = 'IT';`
> Counts employees **only in the IT department**.

---
#### ✅ **d) Count distinct values**
`SELECT COUNT(DISTINCT department_id) AS unique_departments FROM employees;`
> Counts **unique (non-repeated)** department IDs.

---
#### ✅ **e) Count with GROUP BY**
Used to count **per category** (department, city, etc.)
`SELECT department, COUNT(*) AS total_employees FROM employees GROUP BY department;`
> Shows how many employees are in each department.
**Example Output:**

| department | total_employees |
| ---------- | --------------- |
| HR         | 5               |
| IT         | 7               |
| Sales      | 4               |

---
#### ✅ **f) Count with HAVING**
Used to filter **groups** after using `GROUP BY`.
`SELECT department, COUNT(*) AS total_employees FROM employees GROUP BY department HAVING COUNT(*) > 5;`
> Shows only departments where **more than 5 employees** work.
---
### 💡 **3. COUNT() vs COUNT(column_name)**

| Expression               | Counts NULLs? | Description                         |
| ------------------------ | ------------- | ----------------------------------- |
| `COUNT(*)`               | ✅ Yes         | Counts all rows (including NULLs)   |
| `COUNT(column)`          | ❌ No          | Counts only non-null values         |
| `COUNT(DISTINCT column)` | ❌ No          | Counts only unique, non-null values |

---
### ⚙️ **4. Example Table: employees**

| emp_id | name  | department | salary |
| ------ | ----- | ---------- | ------ |
| 1      | Alice | HR         | 50000  |
| 2      | Bob   | IT         | 60000  |
| 3      | Carol | IT         | 65000  |
| 4      | David | NULL       | 55000  |
#### Example Queries:
`-- Count all employees SELECT COUNT(*) FROM employees;  -- Result: 4  -- Count employees with a department SELECT COUNT(department) FROM employees;  -- Result: 3  -- Count distinct departments SELECT COUNT(DISTINCT department) FROM employees; -- Result: 2`