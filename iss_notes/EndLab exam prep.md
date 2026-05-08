## Questions to revisit/NOTE

- Qn 8 from leetcode SQL 50
- Note:
	- w1.recordDate = DATE(w2.recordDate, '+1 day') = format on sqlite3
	- DATEDIFF(w1.recordDate, w2.recordDate) = 1 ->format on mysql
- ROUND(col_name, decimal_place)
- Qn on confirmation rate
```mysql
# Write your MySQL query statement below

SELECT s.user_id, ROUND(IFNULL(SUM(action = 'confirmed')/COUNT(c.user_id),0),2) AS confirmation_rate FROM Signups AS s LEFT JOIN Confirmations AS c ON s.user_id = c.user_id GROUP BY s.user_id;

#ALternate approach

select s.user_id, round(avg(if(c.action="confirmed",1,0)),2) as confirmation_rate
from Signups as s left join Confirmations as c on s.user_id= c.user_id group by user_id;

```

| Function / Keyword | MySQL Syntax                                    | SQLite Syntax               | What It Does                      | Common Use Case          |
| ------------------ | ----------------------------------------------- | --------------------------- | --------------------------------- | ------------------------ |
| COUNT              | `COUNT(column)`                                 | `COUNT(column)`             | Counts non-NULL rows              | Count employees/orders   |
| COUNT ALL          | `COUNT(*)`                                      | `COUNT(*)`                  | Counts all rows                   | Total rows               |
| SUM                | `SUM(column)`                                   | `SUM(column)`               | Adds values                       | Total sales              |
| AVG                | `AVG(column)`                                   | `AVG(column)`               | Average value                     | Average salary           |
| MIN                | `MIN(column)`                                   | `MIN(column)`               | Smallest value                    | Lowest marks             |
| MAX                | `MAX(column)`                                   | `MAX(column)`               | Largest value                     | Highest salary           |
| ROUND              | `ROUND(column, 2)`                              | `ROUND(column, 2)`          | Rounds number                     | Percentages              |
| TRUNCATE           | `TRUNCATE(column, 2)`                           | Not supported               | Cuts decimals without rounding    | Financial values         |
| CEIL / CEILING     | `CEIL(column)`                                  | `CEIL(column)`              | Round upward                      | Pagination               |
| FLOOR              | `FLOOR(column)`                                 | `FLOOR(column)`             | Round downward                    | Bucket grouping          |
| ABS                | `ABS(column)`                                   | `ABS(column)`               | Absolute value                    | Distance difference      |
| MOD                | `MOD(column, 2)`                                | `column % 2`                | Remainder                         | Odd/even checks          |
| POWER              | `POWER(column, 2)`                              | `POWER(column, 2)`          | Exponent                          | Math calculations        |
| SQRT               | `SQRT(column)`                                  | `SQRT(column)`              | Square root                       | Statistics               |
| RAND               | `RAND()`                                        | `RANDOM()`                  | Random number                     | Random sampling          |
| DATE               | `DATE(column)`                                  | `DATE(column)`              | Extract date                      | Remove time part         |
| NOW                | `NOW()`                                         | `DATETIME('now')`           | Current datetime                  | Timestamps               |
| CURDATE            | `CURDATE()`                                     | `DATE('now')`               | Current date                      | Today's data             |
| DATEDIFF           | `DATEDIFF(date1, date2)`                        | Not supported               | Difference in days                | Compare dates            |
| DATE_ADD           | `DATE_ADD(column, INTERVAL 1 DAY)`              | `DATE(column, '+1 day')`    | Add date interval                 | Yesterday/tomorrow       |
| DATE_SUB           | `DATE_SUB(column, INTERVAL 1 DAY)`              | `DATE(column, '-1 day')`    | Subtract date interval            | Previous dates           |
| YEAR               | `YEAR(column)`                                  | `strftime('%Y', column)`    | Extract year                      | Year grouping            |
| MONTH              | `MONTH(column)`                                 | `strftime('%m', column)`    | Extract month                     | Monthly reports          |
| DAY                | `DAY(column)`                                   | `strftime('%d', column)`    | Extract day                       | Daily reports            |
| HOUR               | `HOUR(column)`                                  | `strftime('%H', column)`    | Extract hour                      | Hourly stats             |
| CONCAT             | `CONCAT(col1, col2)`                            | `col1 \| col2`              | Join strings                      | Full names               |
| LENGTH             | `LENGTH(column)`                                | `LENGTH(column)`            | String length                     | Validation               |
| LOWER              | `LOWER(column)`                                 | `LOWER(column)`             | Lowercase                         | Case-insensitive compare |
| UPPER              | `UPPER(column)`                                 | `UPPER(column)`             | Uppercase                         | Formatting               |
| SUBSTRING          | `SUBSTRING(column, 1, 3)`                       | `SUBSTR(column, 1, 3)`      | Extract substring                 | Parsing                  |
| REPLACE            | `REPLACE(column, 'a', 'b')`                     | `REPLACE(column, 'a', 'b')` | Replace text                      | Cleaning data            |
| TRIM               | `TRIM(column)`                                  | `TRIM(column)`              | Remove spaces                     | Cleanup                  |
| LEFT               | `LEFT(column, 3)`                               | `SUBSTR(column, 1, 3)`      | Left part of string               | Prefix extraction        |
| RIGHT              | `RIGHT(column, 3)`                              | `SUBSTR(column, -3)`        | Right part                        | File extensions          |
| IFNULL             | `IFNULL(column, 0)`                             | `IFNULL(column, 0)`         | Replace NULL                      | Missing values           |
| COALESCE           | `COALESCE(column, 0)`                           | `COALESCE(column, 0)`       | First non-null value              | Fallback values          |
| NULLIF             | `NULLIF(column, 0)`                             | `NULLIF(column, 0)`         | NULL if equal                     | Prevent divide-by-zero   |
| CASE               | `CASE WHEN column > 0 THEN 'Yes' ELSE 'No' END` | Same                        | Conditional logic                 | Categorization           |
| DISTINCT           | `SELECT DISTINCT column`                        | Same                        | Remove duplicates                 | Unique users             |
| ORDER BY           | `ORDER BY column DESC`                          | Same                        | Sort rows                         | Rankings                 |
| GROUP BY           | `GROUP BY column`                               | Same                        | Aggregate groups                  | Dept salaries            |
| HAVING             | `HAVING COUNT(*) > 1`                           | Same                        | Filter groups                     | Duplicate detection      |
| LIMIT              | `LIMIT 5`                                       | Same                        | Restrict rows                     | Top results              |
| BETWEEN            | `column BETWEEN 1 AND 10`                       | Same                        | Range filter                      | Salary range             |
| IN                 | `column IN (1,2,3)`                             | Same                        | Multiple match check              | Filter categories        |
| LIKE               | `column LIKE '%abc%'`                           | Same                        | Pattern matching                  | Search                   |
| EXISTS             | `EXISTS(subquery)`                              | Same                        | Check subquery existence          | Correlated queries       |
| UNION              | `query1 UNION query2`                           | Same                        | Merge results removing duplicates | Combine datasets         |
| UNION ALL          | `query1 UNION ALL query2`                       | Same                        | Merge results keeping duplicates  | Append datasets          |
| INNER JOIN         | `table1 JOIN table2 ON condition`               | Same                        | Matching rows only                | Related records          |
| LEFT JOIN          | `table1 LEFT JOIN table2 ON condition`          | Same                        | Keep left table rows              | Missing records          |
| RIGHT JOIN         | `table1 RIGHT JOIN table2 ON condition`         | Not supported               | Keep right table rows             | Reverse left join        |
| FULL OUTER JOIN    | Not directly supported                          | Not supported               | Keep all rows                     | Complete merge           |
| ROW_NUMBER         | `ROW_NUMBER() OVER(...)`                        | Same                        | Sequential numbering              | Ranking                  |
| RANK               | `RANK() OVER(...)`                              | Same                        | Ranking with gaps                 | Leaderboards             |
| DENSE_RANK         | `DENSE_RANK() OVER(...)`                        | Same                        | Ranking without gaps              | Salary ranking           |
| LAG                | `LAG(column)`                                   | Same                        | Previous row value                | Compare yesterday        |
| LEAD               | `LEAD(column)`                                  | Same                        | Next row value                    | Future comparison        |
| PARTITION BY       | `OVER(PARTITION BY column)`                     | Same                        | Group window function             | Per-group ranking        |

what i did
```mysql
SELECT r.contest_id, ROUND(COUNT(u.user_id) * 100 /(SELECT COUNT(*) FROM Users), 2) AS percentage FROM Register AS r
LEFT JOIN Users AS u ON r.user_id = u.user_id
GROUP BY r.contest_id
ORDER BY percentage DESC, contest_id ASC;

```


alt approach
SELECT contest_id,
       ROUND(COUNT(*) * 100 /
       (SELECT COUNT(*) FROM Users), 2)
FROM Register
GROUP BY contest_id;

