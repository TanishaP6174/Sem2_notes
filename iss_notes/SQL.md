[[SQL]] or Structured Query Language, is used to query, manipulate, and transform data from a [[relational database]]. A [[relational database]] represented a collection of related 2D tables, each with a fixed number of named columns. The goal here is to be able to answer questions about the data with simple queries. 

## 1. Writing SQL Queries
Retrieving data from the [[relational database]] is done using `SELECT` statements. 

A query essentially is a statement which declares what data we're looking for, where it's located in the database, and if necessary, how to transform it before it is returned. 
```mysql
SELECT column, another_column FROM mytable;
# comment
SELECT * FROM mytable;
```

## 2. Queries with Constraints (with numbers)
We can select columns but this might be inefficient, because there might be a lot of data to parse through. So, we filter using `WHERE`, `AND` and `OR` statements. 
```mysql
SELECT column, another FROM mytable 
WHERE condition 
	AND/OR anothercond
	AND/OR anothercondi;
```

More complex cases can arise where you adjoin multiple logical keywords. A couple common ones are as follows

| Operator            | Condition                                            | SQL Example                   |
| ------------------- | ---------------------------------------------------- | ----------------------------- |
| =, !=, <, <=, >, >= | Standard numerical operators                         | col_name != 4                 |
| BETWEEN AND         | Number is within range of two values (inclusive)     | col_name BETWEEN 1.5 AND 10.5 |
| NOT BETWEEN AND     | Number is not within range of two values (inclusive) | col_name NOT BETWEEN 1 AND 10 |
| IN                  | Number exists in a list                              | col_name IN (2, 4, 6)         |
| NOT IN              | Number doesn't exist in a list                       | col_name NOT IN (1, 3, 5)     |
This also results in the query running faster due to the reduction in unnecessary data being returned. 

## 3. Queries with Constraints (with text)
This is the same a the previous section, but we handle text data instead, which SQL has multiple handling cases for. Note that the mathematical operators below are case-sensitive but the SQL commands aren't.

| Operator   | Condition                                                                                             | Example                                                                 |
| ---------- | ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| =          | Case sensitive exact string comparison (_notice the single equals_)                                   | col_name = "abc"                                                        |
| != or <>   | Case sensitive exact string inequality comparison                                                     | col_name != "abcd"                                                      |
| LIKE       | Case insensitive exact string comparison                                                              | col_name LIKE "ABC"                                                     |
| NOT LIKE   | Case insensitive exact string inequality comparison                                                   | col_name NOT LIKE "ABCD"                                                |
| %          | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) | col_name LIKE "%AT%"  <br>(matches "AT", "ATTIC", "CAT" or even "BATS") |
| _          | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE)                    | col_name LIKE "AN_"  <br>(matches "AND", but not "AN")                  |
| IN (…)     | String exists in a list                                                                               | col_name IN ("A", "B", "C")                                             |
| NOT IN (…) | String does not exist in a list                                                                       | col_name NOT IN ("D", "E", "F")                                         |

## 4. Filtering and Sorting Query Results
#### Filtering data
Data is unique but the results might not be, so we use `DISTINCT` to discard rows that have a duplicate column value.
```mysql
SELECT DISTINCT column, column1 FROM mytable
WHERE condition(s);
```

#### Ordering results
It can be difficult to read through and understand the results of a query as the size of a table increases to thousands or even millions rows. So we use ORDER BY to give in ascending or descending.
```mysql
SELECT column, column2 FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC;
```

#### Limiting results to a subset
Another clause used with the previous is LIMIT and OFFSET, which are useful optimizations to indicate to the database the subset of results you care about. 
LIMIT reduces the number of rows returned, OFFSET specifies where to begin counting from.
NOTE: OFFSET HAS TO GO AFTER LIMIT
```mysql
SELECT column, another_column FROM mytable
WHERE condition(s) 
ORDER BY column ASC/DESC 
LIMIT num_limit 
OFFSET num_offset;
```
## 5. Simple SELECT queries
Just a couple basic exercises, and actually using SQL to solve different problems in a database, whether its finding a specific datum etc. 

## 6. Multi-table queries with JOINs
Entity data in the real world is often broken down into pieces and stored across multiple orthogonal tables, using a process called <mark style="background: #FF5582A6;">normalization</mark>.

#### Database Normalization
It minimizes duplicate data in any single table, allows data in the database to grow independently of each other. As a trade-off, queries get slightly more complex since they have to find data from different parts of the database, performance issues might arise when working with many large tables. 

#### Multi-table queries with JOINs
Tables that have various information about a single entity need to have a primary key that identified that entity uniquely across the database. No two rows can have the same primary key value, like as follows:
```md
students(student_id, name, age)
marks(student_id, quiz1, quiz2)
attendance(student_id, days_present)
```

Using the `JOIN` clause in a query, we can combine row data from two separate tables using this key. First one is `INNER JOIN`.
```mysql
SELECT column FROM mytable
INNER JOIN another_table
	ON mytable.id = anothertable.id
WHERE condition(s)
ORDER BY column ASC
LIMIT num_limit
OFFSET num_offset;
```

INNER JOIN matches row from the first table and the second table which have the same key, which is defined by the ON constraint, creating a result row with combined columns from both tables. 

## 7. Outer Joins
The INNER JOIN previously used might not be useful because it only returns a table that contains data that belongs in both tables. If the tables have asymmetric data, which is what happens when data is entered in different stages, then we'd have to use either a `LEFT JOIN`, `RIGHT JOIN`, or a `FULL JOIN`, to ensure the required data isn't excluded from the results. 
```mysql
SELECT column FROM table
INNER/LEFT/RIGHT/FULL JOIN table1
	on table.id = table1.otherid
WHERE condition(s)
ORDER BY column ASC
LIMIT limit
OFFSET off;
```

When joining table A to table B, a `LEFT JOIN` simply includes rows from A regardless of whether a matching row is found in B. The `RIGHT JOIN` is the same, but reversed, keeping rows in B regardless of whether a match is found in A. Finally, a `FULL JOIN` simply means that rows from both tables are kept, regardless of whether a matching row exists in the other table.

Using an example:
LEFT JOIN vs RIGHT JOIN (using buildings–employees example)
- LEFT JOIN (buildings LEFT JOIN employees):
    Returns all buildings. If a building has no employees, `employees.role` is NULL.  
    → Preserves the _buildings_ table.
- RIGHT JOIN (buildings RIGHT JOIN employees):  
    Returns all employees. If an employee’s building is not found, `buildings.building_name` is NULL.
    → Preserves the _employees_ table.

One-line rule:  
LEFT JOIN keeps everything from the left table; RIGHT JOIN keeps everything from the right table.

## 8. A Short Note on NULLS
It's always good to reduce the possibility of NULL values in databases because they require special attention when constructing queries, constraints (certain functions behave differently with null values) and when processing the results. 

A better alternative to NULL is data-type appropriate defaults, like 0 or empty strings, etc. Sometimes its also not possible to avoid NULLS, when outer-joining two tables with asymmetric data for example. So you can test using:
```mysql
SELECT column FROM table
WHERE column IS NULL
```

a code is given below for the names of the buildings that hold no employees:
```mysql
SELECT DISTINCT building_name FROM buildings 
  LEFT JOIN employees
    ON building_name = building
WHERE role IS NULL;
```

## 9. Queries with Expressions
You can use expressions to write more complex logic on column values, and they can use mathematical and string functions along with basic arithmetic to transform values.
```mysql
SELECT col_expression AS expr_description FROM mytable;
```

And we can use this for regular columns and tables to have aliases to make them easier to reference, like follows:
```mysql
SELECT column AS better_column_name 
FROM a_long_widgets_table_name AS mywidgets 
INNER JOIN widget_sales 
	ON mywidgets.id = widget_sales.widget_id;
```

Note:
When you write `JOIN` without any modifiers (like `LEFT` or `RIGHT`), the MySQL parser defaults to an **inner join**
```mysql
SELECT p.ProductName, c.CategoryName
FROM Products AS p
INNER JOIN Categories AS c ON p.CategoryID = c.CategoryID;
```

## 10. Queries with aggregates
SQL also supports the usage of aggregate expressions or functions that allow you to summarize information about a group of rows of data.
amples:

| Function                         | Description                                                                                                                                                                                     |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **COUNT**, **COUNT(**column**)** | A common function used to counts the number of rows in the group if no column name is specified. Otherwise, count the number of rows in the group with non-NULL values in the specified column. |
| **MIN(**column**)**              | Finds the smallest numerical value in the specified column for all rows in the group.                                                                                                           |
| **MAX(**column**)**              | Finds the largest numerical value in the specified column for all rows in the group.                                                                                                            |
| **AVG(**column)                  | Finds the average numerical value in the specified column for all rows in the group.                                                                                                            |
| **SUM(**column**)**              | Finds the sum of all numerical values in the specified column for the rows in the group.                                                                                                        |
|                                  |                                                                                                                                                                                                 |
#### Grouped Aggregate Functions
In addition to aggregating across all rows, you can instead apply the functions to individual groups of data within that group, like i.e. (box office sales for Comedies v Action movies). This would then create as many results as there are unique groups defined by the `GROUP BY` clause.
```mysql
SELECT agg_func(column_or_expression) AS aggregatedescription
FROM table
WHERE constraint
GROUP BY column;
```
###### NOTE: only if you're defining a new parameter do you need AS.

## 12. Queries with aggregate part 2
Note that above, the `GROUP BY` clause is done after the `WHERE` clause, so to filter results *after* grouping, we use a clause called `HAVING`, allowing us to filter groups rows. 
```mysql
SELECT column, agg(column_expression) AS aggdesc
FROM mytable
WHERE condition
GROUP BY column
HAVING groupcondition
```

NOTE: If you aren't using the `GROUP BY` clause, a simple `WHERE` clause will suffice.
## 13. Order execution of a query
We have all the main parts of a query now we need to understand how they fit together. Below is an example.
```mysql
SELECT DISTINCT column, AGG_FUNC(column_or_exp) FROM mytable
JOIN another 
	ON mytable.column = another.column1
	WHERE constraint
	GROUP BY column
	HAVING constraint
	ORDER BY column ASC
	LIMIT count
	OFFSET counts;
```

#### Query Order of Execution
1. The `FROM` and the subsequent `JOIN`s are first executed to determine the final working data set, and this includes subqueries. 
2. The first pass `WHERE` constraints are applied to rows and the dataset is shrunk. Each of the constraints can only access tables requested in the `FROM` clause. Some aliases in the `SELECT` might not be accessible since they might include/depend on parts of the query that haven't been executed.
3. `GROUP BY` groups based on common values after constraints, and rows only as many unique values in that column. Implicitly, we only use this when we have aggregate functions.
4. `HAVING` is used to set constraints on the grouped rows and similar to the `WHERE` clause, aliases aren't accessible from this step. 
5. `SELECT` and `DISTINCT` are finally fully executed.
6. `ORDER BY` to get the final queried dataset in a clean and orderly fashion.
7. `LIMIT` and `OFFSET` are finally executed, leaving the parts of the set that don't belong in the constraints set by limits and offsets.

## 13. Inserting rows
Real quick, a **database schema** is what describes the structure of each table, and the datatypes that each table can contain. For example, Year must be an integer, Title must be a string, etc. Now, when we insert data into a database, we use `INSERT` which declares which table we're inserting into, the columns or data and one or more rows of data. You can insert multiple rows at a time by listing them sequentially.
```mysql
INSERT INTO mytable
VALUES (val, val2, val3..), (valx1, valx2, valx3...)...;
```

And, if your column data is incomplete, i.e. there are certain columns for which you don't know the values, you specify which columns you're filling as follows:
```mysql
INSERT INTO mytable
(column1, column2,...)
VALUES (val1, val2...);
```

Number of values need to match the number of columns given. Also, if you add a column with a default value, then you won't have to specify the columns you're inserting into.
## 14. Update rows
After adding, the next natural action is to update existing rows, and its done similar to insertion, using `UPDATE`.
```mysql
UPDATE table
SET column = val_exp,
	othercol = val_exp2
WHERE condition;
```

## 15. Delete rows
Using `DELETE` clause and then specifying which table to act on and the condition for what row to delete. A word of caution is that without a proper backup/test database, you might end up deleting the wrong sets of data, so use a `SELECT` statement first to fully ensure it's correct.

```mysql
DELETE FROM mytable
WHERE condition;
```

## 16. Creating tables
Making another table with new columns using `CREATE TABLE`. Structure is defined by its table schema, which define a series of columns. Each column has a name, datatype allowed, optional constraint on data and an optional default value. If a table with the same name exists, might throw an error so use `IF NOT EXISTS`.
Note: dont forget the table
```mysql
CREATE TABLE IF NOT EXISTS gay (
	column datatype tableconstraint DEFAULT defval, 
	another datatype2 tablecons2 DEFAULT defval2
);
```

Common Datatypes: `INTEGER`, `BOOL`, `FLOAT`, `DOUBLE`, `REAL`, `CHARACTER`, `VARCHAR`, `TEXT`, `DATE`, `DATETIME`, `BLOB`.

| Constraint           | Description                                                                                                                                                                                                                                                                                                                                                                                        |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `PRIMARY KEY`        | This means that the values in this column are unique, and each value can be used to identify a single row in this table.                                                                                                                                                                                                                                                                           |
| `AUTOINCREMENT`      | For integer values, this means that the value is automatically filled in and incremented with each row insertion. Not supported in all databases. For SQLite, its only used for Primary Key.                                                                                                                                                                                                       |
| `UNIQUE`             | This means that the values in this column have to be unique, so you can't insert another row with the same value in this column as another row in the table. Differs from the `PRIMARY KEY` in that it doesn't have to be a key for a row in the table.                                                                                                                                            |
| `NOT NULL`           | This means that the inserted value can not be `NULL`.                                                                                                                                                                                                                                                                                                                                              |
| `CHECK (expression)` | This allows you to run a more complex expression to test whether the values inserted are valid. For example, you can check that values are positive, or greater than a specific size, or start with a certain prefix, etc.                                                                                                                                                                         |
| `FOREIGN KEY`        | This is a consistency check which ensures that each value in this column corresponds to another value in a column in another table.  <br>  <br>For example, if there are two tables, one listing all Employees by ID, and another listing their payroll information, the `FOREIGN KEY` can ensure that every row in the payroll table corresponds to a valid employee in the master Employee list. |
Here's a clear code for a basic create table.
```mysql
CREATE TABLE movies(
	id INTEGER PRIMARY KEY,
	title TEXT,
	director TEXT,
	year INTEGER,
	length INTEGER
);
```

## 17. Altering tables
You can alter database schemas using `ALTER TABLE` to add, remove or change columns and table constraints. Syntax is similar to creation.
```mysql
ALTER TABLE gayahh
ADD column DataType Constraint
	DEFAULT defval;

ALTER TABLE gayahh
DROP columndel; # not supported in SQLite

ALTER TABLE gayahh
RENAME TO newname;
```

## 18. Dropping tables
Sometimes we want to delete entire tables, and we do this using `DROP TABLE` which differs from `DELETE` because it also removed the table schema from the database entirely.
```mysql
DROP TABLE IF EXISTS mytable;
```
ALTER TABLE Customers   
DROP COLUMN ContactName;
## 19. Subqueries
Even in a complete query, there are many questions unanswered without additional post or preprocessing. So either you make multiple queries or make SQL subqueries.
Example: Some random company!
You maintain a database of associates, which data on the revenue that each associate brings in and their salary. To find which associates are costing the company more than the average revenue per associate, you write something like this:
```mysql
SELECT * FROM sales_associates
WHERE salary > 
	(SELECT AVG(revenue_generated) FROM sales_associates);
```

A subquery can be referenced anywhere a normal table can be references. In FROM clauses, JOIN subqueries with other tables, inside a WHERE or HAVING constraint, test expressions against the results of the subquery. 

#### Correlated subqueries
Instead of the list of just Sales Associates above, imagine if you have a general list of Employees, their departments (engineering, sales, etc.), revenue, and salary. This time, you are now looking across the company to find the employees who perform worse than average in their department.

For each employee, you would need to calculate their cost relative to the average revenue generated by all people in their department. To take the average for the department, the subquery will need to know what department each employee is in:
```mysql
SELECT * FROM employees
WHERE salary > 
	(SELECT AVG(revenue_generated)
	FROM employees as dept_employees
	WHERE dept_employees.department = employees.department
	);
```

## 20. Unions, Intersections & Exceptions
When working with multiple tables, the `UNION` and `UNION ALL` clause allows you to append the results of one query to another assuming that they have the same column count, order and data type. If you use the `UNION` without the `ALL`, duplicate rows between the tables will be removed from the result.
```mysql
SELECT column, another_column FROM mytable 
UNION / UNION ALL / INTERSECT / EXCEPT 
SELECT other_column, yet_another_column 
	FROM another_table
ORDER BY column DESC 
LIMIT n;
```

## 21. Conditional Logic
We use `CASE` to apply if-else logic in a query. It is used in `SELECT`, `ORDER BY` and `UPDATE`. Here's a clean example, for something like grading. 
```mysql
SELECT
	studentid,
	total,
	CASE
		WHEN total >= 90 THEN "A"
		WHEN total >= 75 THEN "B"
		ELSE "C"
	END AS grade
FROM marks;
```

It evaluates top to bottom like any if else loop, it stops and returns at the first true condition, and if `ELSE` is omitted, it returns `NULL`. Data isn't changed unless used inside `UPDATE`.

## 22. CTEs
CTEs or Common Table Expression is a temporary named result set that exists only for the duration of a single query. Essentially you're making a mini-table for that given query to obtain the result. So instead of writing a nested subquery, you give it a name and reuse it.
```mysql
WITH temptable AS (
	SELECT...
)
SELECT * FROM temptable
```

A cleaner example is as follows, where you're creating a mini-table of all the people with salaries higher than a certain value.

```mysql
WITH highsalary AS(
	SELECT name, salary FROM employees
	WHERE SALARY > 50000
)
SELECT * FROM high salary;	
```
