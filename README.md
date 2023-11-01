# 📁 SQL_Window_Functions
Time to discover the world of Window Functions with examples; lead( ), lag( ), first_value( ), last_value( ), row_number( ), rank( ), dense_rank( ) …

![Window_Func](https://github.com/hhuseyincosgun/SQL_Window_Functions/blob/main/Window_Func.png)


## 📋 Table of Contents
- [Window Functions](#window-functions)
- [Northwind Database Diagram](#northwind-database-diagram)
- [1) Aggregate Window Functions](#1)-aggregate-window-functions)
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

![image](https://github.com/hhuseyincosgun/SQL_Window_Functions/assets/21257660/1b882d0a-0080-4931-b06e-8e12c338ff3b)

## Northwind Database Diagram
The Northwind database is a sample database that was originally created by Microsoft for educational and training purposes. It's a relational database model that represents a fictional company's operations, particularly a company involved in the wholesale distribution of various products. The Northwind database is often used as a teaching and learning tool for practicing SQL queries, data modeling, and database management.

![White diagram](https://github.com/hhuseyincosgun/SQL_Window_Functions/assets/21257660/d4329263-a798-4a61-99ac-533291a527f8)

## 1) Aggregate Window Functions
Aggregate window functions refer to the application of aggregate functions like SUM(), COUNT(), AVERAGE(), MAX(), and MIN() over a specific window, which is a defined set of rows within a result set.

### 1.1 COUNT (), SUM ()
In each order: 
How many unique products are there? 
How many products in total? 
What is the total amount paid?

- Query with Group by:
````sql
SELECT OrderID,
   COUNT(OrderID) Unique_Product,
   SUM (Quantity) Total_Quantity,
   SUM (UnitPrice * Quantity) Total_Price
FROM [Order Details]
GROUP BY OrderID;
````

- Query with Window Function:
````sql
SELECT DISTINCT OrderID,
   COUNT(OrderID) OVER (PARTITION BY OrderID) Unique_Product,
   SUM (Quantity) OVER (PARTITION BY OrderID) Total_Quantity,
   SUM (UnitPrice * Quantity) OVER (PARTITION BY OrderID) Total_Price
FROM [Order Details];
````

![count_sum](https://github.com/hhuseyincosgun/SQL_Window_Functions/assets/21257660/8d4a59d1-1e8e-4655-a90f-a004b199ce36)

### 1.2 MIN (), MAX (), AVG ()

What are the minimum, maximum and average amounts of freight pay for each customer?

````sql
SELECT CustomerID,
   MIN(Freight) Min_Freight,
   MAX(Freight) Max_Freight,
   AVG(Freight) Avg_Freight
FROM Orders
GROUP BY CustomerID
ORDER BY CustomerID;
````

````sql
SELECT DISTINCT CustomerID,
   MIN(Freight) OVER (PARTITION BY CustomerID) Min_Freight,
   MAX(Freight) OVER (PARTITION BY CustomerID) Max_Freight,
   AVG(Freight) OVER (PARTITION BY CustomerID) Avg_Freight
FROM Orders
ORDER BY CustomerID;
````
![min_max_avg](https://github.com/hhuseyincosgun/SQL_Window_Functions/assets/21257660/3b6adc26-12fe-4c03-ae8e-a88c8c7d340f)

## 2) Ranking Window Functions
 
