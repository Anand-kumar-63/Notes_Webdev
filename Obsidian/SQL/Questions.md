# Distinct and Order by 
## 🟢 `DISTINCT` Clause — Removes Duplicates
The **`DISTINCT`** keyword in SQL is used to **return only unique (different)** values from a column or combination of columns.
### 🧠 Syntax
`SELECT DISTINCT column_name FROM table_name;`
### 💡 Example
**Table: `students`**

| id  | name  | age |
| --- | ----- | --- |
| 1   | John  | 20  |
| 2   | Alice | 22  |
| 3   | John  | 20  |
| 4   | Mark  | 21  |

**Query:**
`SELECT DISTINCT name FROM students;`
✅ **Output:**

|name|
|---|
|John|
|Alice|
|Mark|

👉 Explanation:
- There were two “John” entries.
- `DISTINCT` removes duplicates and keeps only unique values.
---
### 🔹 Multiple Columns with DISTINCT
If you use `DISTINCT` on multiple columns, it considers the **combination** of those columns as unique.
`SELECT DISTINCT name, age FROM students;`
✅ This keeps unique _name + age_ pairs (e.g., if “John” appears twice with age 20, only one will remain).

---
## 🟣 `ORDER BY` Clause — Sorts the Output
The **`ORDER BY`** clause sorts the result set (rows returned by the query) in either **ascending** (`ASC`) or **descending** (`DESC`) order.
### 🧠 Syntax
`SELECT column1, column2 FROM table_name ORDER BY column_name [ASC | DESC];`
### 💡 Example
`SELECT name, age FROM students ORDER BY age ASC;`
✅ **Output:**

| name  | age |
| ----- | --- |
| John  | 20  |
| Mark  | 21  |
| Alice | 22  |

👉 Explanation:
- Sorts the students from **youngest to oldest**. 
- Default order is `ASC` (ascending)   
---
### 🔹 Descending Order Example
`SELECT name, age FROM students ORDER BY age DESC;`
✅ Now results are **from oldest to youngest**.

---
## 🧩 Using Both Together
You can combine both:
`SELECT DISTINCT name FROM students ORDER BY name ASC;`
👉 This returns **unique names**, sorted alphabetically.

---
## ✅ Summary Table

| Clause                | Purpose                | Example                                             | Result              |
| --------------------- | ---------------------- | --------------------------------------------------- | ------------------- |
| `DISTINCT`            | Removes duplicate rows | `SELECT DISTINCT city FROM customers;`              | Unique cities       |
| `ORDER BY`            | Sorts rows (ASC/DESC)  | `SELECT * FROM customers ORDER BY age DESC;`        | Sorted by age       |
| `DISTINCT + ORDER BY` | Unique + sorted output | `SELECT DISTINCT name FROM students ORDER BY name;` | Unique sorted names |
# 🧠 `CHAR_LENGTH()` in MySQL
**Purpose:**  
Returns the **number of characters** in a string (not bytes).  
Equivalent to `CHARACTER_LENGTH()`.
### 🧩 Syntax
`CHAR_LENGTH(string)`

---
### 💡 Basic Example
`SELECT CHAR_LENGTH('Anand Kumar') AS length;`
✅ Output → `11`  
(includes spaces)

---
### ⚙️ Difference from `LENGTH()`
`SELECT CHAR_LENGTH('😊'), LENGTH('😊');`
✅ Output:

| CHAR_LENGTH                              | LENGTH |
| ---------------------------------------- | ------ |
| 1                                        | 4      |
| 👉 One emoji = 1 character, but 4 bytes. |        |

---
### 🔹 Example 1: Filter by Length
Find customers with long names:
`SELECT id, name FROM customer WHERE CHAR_LENGTH(name) > 15;`

---
### 🔹 Example 2: Sort by String Length
`SELECT name, CHAR_LENGTH(name) AS len FROM customer ORDER BY len DESC;`
👉 Shows longest names first.

---
### 🔹 Example 3: Group by Length
```sql
SELECT CHAR_LENGTH(name) AS length , COUNT(*) AS count FROM customer GROUP BY length ORDER BY length DESC;
```
👉 Counts how many names have each length.

---

### 🔹 Example 4: Complex Use — Validate Input
```sql
`SELECT id, name, CASE   WHEN CHAR_LENGTH(name) < 3 THEN 'Too Short'   WHEN CHAR_LENGTH(name) > 20 THEN 'Too Long'   ELSE 'Valid' END AS name_status FROM customer;`
```
👉 Checks if name length is within limits.



