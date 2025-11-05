In **SQL**, the **`JOIN`** clause is used to combine rows from **two or more tables** based on a **related column** between them — usually a **foreign key**.

Let’s go step-by-step 👇

---
### 🧠 **1. Basic Syntax**
`SELECT columns FROM table1 JOIN table2 ON table1.common_column = table2.common_column;`

---
### 🔗 **2. Types of JOINs**
#### **a) INNER JOIN**
Returns only the **matching rows** from both tables.

`SELECT employees.name, departments.department_name FROM employees INNER JOIN departments ON employees.department_id = departments.department_id;`

👉 Only employees who **belong to a department** appear.

---
#### **b) LEFT JOIN (or LEFT OUTER JOIN)**
Returns **all rows from the left table**, and **matching rows from the right**.  
If no match, you get `NULL` on the right side.

`SELECT employees.name, departments.department_name FROM employees LEFT JOIN departments ON employees.department_id = departments.department_id;`

👉 All employees are shown — even those **without a department**.

---
#### **c) RIGHT JOIN (or RIGHT OUTER JOIN)**
Opposite of LEFT JOIN — returns **all rows from the right table**, and matching rows from the left.

`SELECT employees.name, departments.department_name FROM employees RIGHT JOIN departments ON employees.department_id = departments.department_id;`

👉 Shows **all departments**, even those **without employees**.

---
#### **d) FULL JOIN (or FULL OUTER JOIN)**

Returns **all rows when there is a match** in either left or right table.  
Non-matching rows are filled with `NULL`.

> ⚠️ Not supported in all databases (like MySQL). You can simulate it with `UNION`.

`SELECT employees.name, departments.department_name FROM employees LEFT JOIN departments ON employees.department_id = departments.department_id  UNION  SELECT employees.name, departments.department_name FROM employees RIGHT JOIN departments ON employees.department_id = departments.department_id;`

---
#### **e) CROSS JOIN**
Produces a **Cartesian product** — every row of one table with every row of another.

`SELECT employees.name, departments.department_name FROM employees CROSS JOIN departments;`

👉 If you have 10 employees and 3 departments → **30 rows** total.

---
### ⚙️ **3. Example with Sample Tables**
#### **Employees Table**

|emp_id|name|dept_id|
|---|---|---|
|1|Alice|101|
|2|Bob|102|
|3|Carol|NULL|

#### **Departments Table**

| dept_id | department_name |
| ------- | --------------- |
| 101     | HR              |
| 102     | IT              |
| 103     | Sales           |

---
#### **INNER JOIN**

`SELECT name, department_name FROM employees INNER JOIN departments ON employees.dept_id = departments.dept_id;`
➡️ **Output:**

| name  | department_name |
| ----- | --------------- |
| Alice | HR              |
| Bob   | IT              |

---
#### **LEFT JOIN**

`SELECT name, department_name FROM employees LEFT JOIN departments ON employees.dept_id = departments.dept_id;`
➡️ **Output:**

| name  | department_name |
| ----- | --------------- |
| Alice | HR              |
| Bob   | IT              |
| Carol | NULL            |
