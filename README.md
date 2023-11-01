# 📁 SQL_Window_Functions
Time to discover the world of Window Functions with examples; lead( ), lag( ), first_value( ), last_value( ), row_number( ), rank( ), dense_rank( ) …

## 📋 Table of Contents
- [Window Functions](#window-functions)
- [About The Northwind Database](#relational-database-diagram)
- [1) Aggregate Window Functions](#queries-and-solutions)
  - 1.1 COUNT (), SUM ()
  - 1.2 MIN (), MAX (), AVG ()
- [2) Ranking Window Functions](#queries-and-solutions)
  - 2.1 RANK (), DENSE_RANK () and ROW_NUMBER ()
  - 2.2 PERCENT_RANK (), CUME_DIST ()
  - 2.3 NTILE()
- [2) Value Window Functions](#queries-and-solutions)
  - 3.1 LAG(), LEAD()
  - 3.2 FIRST_VALUE(), LAST VALUE()

You can access [here](https://www.udemy.com/course/alistirmalarla-sql-ogreniyorum/) the article related with this repo.

If you have any questions, reach out to me on:
[LinkedIn](https://www.linkedin.com/in/hasanhuseyincosgun/),
[Medium](https://medium.com/@hhuseyincosgun),
[Kaggle](https://www.kaggle.com/huseyincosgun).
***

## Window Functions

SQL window functions enable efficient and precise data analysis by allowing calculations within specific partitions or rows. They're crucial for tasks like ranking, aggregation, and trend analysis in SQL queries.
These functions are applied to each row of a result set, and they use an OVER() clause to determine how each row is processed within a "window," allowing control over the function's behavior within a group of ordered data.




 
