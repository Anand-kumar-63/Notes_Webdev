-  Sql is not a case sensitive language
  
# What is MySQL Workbench?
  **MySQL Workbench** is an official **graphical user interface (GUI)** tool provided by MySQL. It helps you **design, manage, and query MySQL databases** visually.

# Clauses
    - use { the particular database }
    - select { select which column to view fron that database }
    - from { from which table }
    - order by {to get table data in some specific order}
    - As(alias) { to give custoom names to a column }
    - distinct { to get unique data }

	- comparision operators { >  <  >=  <=  == }
# And , or , not operator 
- { and operator is always evaluated first as the and operator has higher precedence and not is use to negate a condition }
# In operator , Not In operator 
- { to combine multiple conditions }
    ```SQL
    select * // select every column
	from city // from the table named city
    where country_id in (87,101,97) // either country_id is 87 or 101 or 97
	```
# between operator {}
 - To specify a range 
	```SQL
    use sakila;
	select * 
	from customer 
	where address_id between 10 and 80
	```
# Like Operator
- Like operators { to get a row where a coloum data follows a particular                              seqeunce }
## use of %letter or %letter%  or letter%
- % represent any number of characters 
 #examples
```SQL
use sakila;
    select * from actor where last_name like "b%" // every lastname that starts 
                                                  with b and can have any                                                              sequence after that   

	use sakila;
	select * from actor where last_name like "%b%" // every lastname that includes 
	                                               b can have any seqeunce of 
	                                               letters before and after  
	select * from address where phone like "1%1" // every phone that starts and 
	                                                 ends with 1


    use sakila;
    select * from address where address like "%loop%" or "%drive%"
    // every address that includes loop or drive
```
    
## use of  **__letter  or letter**__
- _  represent a single character_
- number of _ denotes the size and a letter denotes ending or starting 
# REGEXP Operator
- **in MySQL**, regex is used for **pattern matching** using **regular expressions**. It allows you to perform flexible and complex string matching in SQL queries.
###### Commonly Used Metacharacters in SQL REGEX allow complex ****pattern creation****.
- **`.` (dot)** – Matches any single character except newline.  
  #Example:
  SELECT 'hat' REGEXP 'h.t';  -- ✅ Matches 'hat', 'hot', 'hit'
- **`^` (caret)** – Matches the beginning of a string.  
  #Example:
  SELECT 'apple' REGEXP '^a';  -- ✅ Matches because 'apple' starts with 'a'
- **`$` (dollar sign)** – Matches the end of a string.  
	  #Example:
	SELECT 'cake' REGEXP 'e$';  -- ✅ Matches because 'cake' ends with 'e'
 - **`[]` (square brackets)** – Matches any one character inside the brackets.  
	  #Example:
	SELECT 'cat' REGEXP '[ch]at';  -- ✅ Matches 'cat' and 'hat'
- **`[^]` (caret inside brackets)** – Matches any character **not** inside the brackets.  
	  #Example:
	SELECT 'bat' REGEXP '[^ch]at';  -- ✅ Matches 'bat', not 'cat' or 'hat'
- **`[a-z]` (range)** – Matches any character in the specified range.  
	  #Example:
	SELECT 'car' REGEXP '[a-c]ar';  -- ✅ Matches 'bar' and 'car', not 'far'
- **`|` (pipe)** – Acts as OR; matches either left or right pattern.  
	  #Example:
	SELECT 'dog' REGEXP 'cat | dog';  -- ✅ Matches 'dog' or 'cat'
- **`*` (asterisk)** – Matches **zero or more** occurrences of the previous character.  
	  #Example:
	SELECT 'lool' REGEXP 'lo * l';  -- ✅ Matches 'll', 'lol', 'lool'
- **`+` (plus)** – Matches **one or more** occurrences of the previous character.  
  #Example:
	SELECT 'lool' REGEXP 'lo+l';  -- ✅ Matches 'lol', 'lool', but not 'll'
- **`?` (question mark)** – Matches **zero or one** occurrence of the previous character.  
	  #Example:
	SELECT 'lol' REGEXP 'lo?l';  -- ✅ Matches 'll' and 'lol'
- **`{n}` (curly braces)** – Matches **exactly n** occurrences.  
  #Example:
	SELECT 'aaa' REGEXP 'a{3}';  -- ✅ Matches 'aaa' only
- **`{n,}`** – Matches **at least n** occurrences.  
  #Example:
	SELECT 'aaaa' REGEXP 'a{2,}';  -- ✅ Matches 'aa', 'aaa', 'aaaa'

#example 
#usingREGEXP
  get the list of the customers whose first name starts with mary or linda
  last name ends with ey or on 
  last name starts with my or contains se 
  last name contains b followed by either r or u
```SQL
use sakila;
select first_name , last_name , email , address_id // columns
from customer // table
where first_name regexp '^mary|^linda' or last_name regexp "ey$|on$|^my|se|b[ru]"  

```
# IS NULL OPERATOR
In SQL (including MySQL), the **`IS NULL` operator** is used to check whether a **column data or expression has a NULL value**.
SELECT * FROM table_name WHERE column_name IS NULL;

// this will return all the rows having columns values equals  null

#example
select * from sakila.address where address2 is null;

# Order By Operator
  It is use to arrange the data table in a specific order using the data of a particular column
 #Example 
 ```sql
 select * 
 from sakila.address
 order by last_update desc // return table arranged on the basis of last_update 
                             column

 use sakila;
 select * , amount*1.1 as "updated_amount"
 from payment order by updated_amount desc

 use sakila;
 select * , quantity*perunit as total_amount  
 from payamount
 order by total_amount desc
 ```

# Limit Clause
The `LIMIT` clause is used to **restrict the number of rows** returned by a query.
It’s especially useful when:
- You're previewing data.
- You only need the top results.
- You’re paginating (e.g., showing 10 results per page). 
```sql
// syntax
SELECT column1, column2, ...
FROM table_name
LIMIT number_of_rows;

#Example1
SELECT * FROM students
LIMIT 5;
#example2
select * 
from sakila.customer 
where customer_id > 10 
limit 9 offset 10
// print all the rows having customer_id > 10 , also skip 10 rows and print only 9 rows 
```
- *we* can also #skip some rows with OFFSET
 ```sql
 // syntax 
 SELECT * FROM students
 LIMIT row_count OFFSET offset;

# Example1 
SELECT * FROM students
LIMIT 5 OFFSET 10;
//This skips the first 10 rows and returns the **next 5 rows** (i.e., rows 11 to 15)
#Example2 
select * 
from sakila.customer 
order by last_update 
limit 3 offset 10
// return 3 customers that ordered something recently and skip first 10 of them 
(i.e row 11-13)
```

# Join clause
### ✅ What is a `JOIN`?
The `JOIN` clause is used to **combine rows from two or more tables** based on a **related column between them** — typically a **foreign key**.
## ✅ Does the number of rows increase when you join two tables?
**Yes, it can — depending on how the data matches between the two tables on the common column.**
### 🔄 Types of Joins in SQL:
1. **`INNER JOIN`**  or **`JOIN`** – Returns only **matching** rows from both tables.
2. **`LEFT JOIN` (or LEFT OUTER JOIN)** – Returns **all rows from the left** table and the **matching rows from the right** table.
3. **`RIGHT JOIN` (or RIGHT OUTER JOIN)** – Returns **all rows from the right** table and the **matching rows from the left**.
4. **`FULL JOIN` (or FULL OUTER JOIN)** – Returns all rows when there is a match in **either** table. _(Note: MySQL does not support FULL JOIN directly — use `UNION` as a workaround.)_
5. **`CROSS JOIN`** – Returns the **Cartesian product** (all possible combinations) of both tables.
6. **`SELF JOIN`** – Joins a table **to itself**.
#Example 
```sql
#Example1
use sakila;
select c.customer_id , first_name , last_name , rental_id , amount 
from customer as c join payment as p on c.customer_id = p.customer_id

// here joining two tables customer and payment on the basic of customer_id column 
 returning first_name last_name from table1 and rental_id , amount from table 2

#Exampl2
use store;
select c.customer_id , first_name , last_name , city , order_date , shipped_date , shipper_id
from customers c join orders o on c.customer_id = o.customer_id 

#example3
use store;
SELECT quantity , order_id , so.unit_price + sp.unit_price as Updated_unit_price
from order_items as so 
join products as sp 
on so.product_id = sp.product_id
// return quantity order_it and some of unit prices of both the tables 
```

# Joining across Database
- joining rows from two or more tables from different databases on the basis of related column between them
```SQL
use store;
select * 
from sql_inventory.products as sp
join order_items c on c.product_id = sp.product_id;

#Example2
use store;
SELECT *
from order_items as so 
join sql_inventory.products as sp 
on so.product_id = sp.product_id
// store and sql_inventory are different databases
```

# Self join
# Join multiple tables
- To **join multiple tables within the same database**, you use SQL `JOIN` clauses (typically `INNER JOIN`, `LEFT JOIN`, etc.) to combine rows based on related columns between tables.
- joining multiple tables within the same db
- use join multiple time with single from table to join table 1 join table2 and so on......
#syntax
```sql
SELECT columns
FROM table1
JOIN table2 ON table1.common_column = table2.common_column

// to join table3 with table1 with common column between them  
JOIN table3 on table1.common_column2 = table3.common_column2

or// to join table 3 with table 2 with the help of common column between them
JOIN table3 ON table2.other_column = table3.other_column

WHERE conditions;
```
#Example 
```sql
use store;
select o.order_date , c.first_name , c.last_name , c.city , os.order_status_id  ,os.name
from orders o 
join customers c on o.customer_id = c.customer_id
join order_statuses os on o.status = os.order_status_id
// joining three table orders , customers , order_statuses
```
- No of rows increases **depending on how the data matches between the two tables on the common column.**
```sql

#Example1 
use sql_hr_simple;
select e.first_name , e.last_name , e.job_title , o.city , o.state , d.department_name
from employees e
join offices o on e.office_id = o.office_id  
join departments d on d.office_id = e.office_id 

#Example2
use store;
select * 
from orders o
join customers c on o.customer_id = c.customer_id
join shippers s on s.shipper_id = o.shipper_id
// join orders table with customer table using common column customer_id 
and join orders table with shipper table using common column shipper_id 
```
# Compound join conditions
- A **compound join condition** means using **more than one condition** to join two tables — i.e., combining multiple `ON` conditions using `AND` (or sometimes `OR`.
- ### 🎯 Why use a compound join?
  Sometimes, a single column isn’t enough to uniquely match rows between tables — you need to consider **two or more columns together**.
#Example 
 ```sql
 SELECT ...
 FROM table1
 JOIN table2
 ON table1.colA = table2.colA
 AND table1.colB = table2.colB;
 %% You're matching rows where **both conditions** are true .%%
```

```SQL
  SELECT g.student_id, g.subject, g.grade, s.teacher_name
  FROM students_grades g
  JOIN subjects_info s
  ON g.subject = s.subject
  AND g.term = s.term;
  //Joins using both `subject` and `term` (compound condition).
```

```SQL
SELECT s.sale_id, s.product_id, d.discount_percent
FROM sales s
JOIN discounts d
ON s.product_id = d.product_id
AND s.sale_date = d.discount_date;
//Ensures the discount applies **only on the correct date and product**.

```
# Implicit  Join Syntax
# Outer Joins[left join  right join]

  #Example 
 #InnerJoin
 - employees table
            ![[Pasted image 20250705162817.png]] 
 - departments table 
             ![[Pasted image 20250705162827.png]]
 - joining both on the basis of similar column office_id
          ![[Pasted image 20250705162925.png]]
               
 #Example 
 #outerjoin
 -  using outer Join {left join }
            ![[Pasted image 20250705164548.png]]


# hey
