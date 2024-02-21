# 📁 SQL Window Functions
Time to discover the world of Window Functions with examples. You can improve your skills by using window functions. 

lead( ), lag( ), first_value( ), last_value( ), row_number( ), rank( ), dense_rank( ) …

![Window_Func](https://github.com/hhuseyincosgun/SQL_Window_Functions/blob/main/Window_Func.png)


## 📋 Table of Contents
- [Window Functions](#window-functions)
- [The Northwind Relationship Diagram](#the-northwind-relationship-diagram)
- [1) Aggregate Window Functions](#1-aggregate-window-functions)
  - 1.1 COUNT (), SUM ()
  - 1.2 MIN (), MAX (), AVG ()
- [2) Ranking Window Functions](#2-ranking-window-functions)
  - 2.1 RANK (), DENSE_RANK () and ROW_NUMBER ()
  - 2.2 PERCENT_RANK (), CUME_DIST ()
  - 2.3 NTILE()
- [3) Value Window Functions](#3-value-window-functions)
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

(source: https://www.postgresqltutorial.com/postgresql-window-function/)
## The Northwind Relationship Diagram
The Northwind database is a sample database that was originally created by Microsoft for educational and training purposes. It's a relational database model that represents a fictional company's operations, particularly a company involved in the wholesale distribution of various products. The Northwind database is often used as a teaching and learning tool for practicing SQL queries, data modeling, and database management.

![White diagram](https://github.com/hhuseyincosgun/SQL_Window_Functions/assets/21257660/d4329263-a798-4a61-99ac-533291a527f8)

## 1-Aggregate Window Functions
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

## 2-Ranking Window Functions
**RANK()**: Assigns a unique rank to each row, leaving gaps in case of ties.

**DENSE_RANK ()**: Assigns a unique rank to each row, with continuous ranks for tied rows.

**ROW_NUMBER ()**: Assigns a unique sequential integer to each row, regardless of ties, without gaps.

**PERCENT_RANK()**: Calculates the relative rank of a specific row within the result set as a percentage.

**CUME_DIST ()**: Calculates the cumulative distribution of a value in the result set. It represents the proportion of rows that are less than or equal to the current row.

### 2.1 RANK (), DENSE_RANK () and ROW_NUMBER ()

Ranking of the most paid products in each order:


````sql
SELECT  OrderID, P.ProductName, (O.UnitPrice * O.Quantity) AS Total_Sale,
   ROW_NUMBER() OVER(ORDER BY (O.UnitPrice * Quantity) DESC) AS Order_RN, 
   RANK() OVER(ORDER BY (O.UnitPrice * O.Quantity) DESC) AS Order_Rank, 
   DENSE_RANK() OVER(ORDER BY (O.UnitPrice * O.Quantity) DESC) AS Order_Dense
FROM  [Order Details] O
JOIN Products P
ON P.ProductID = O.ProductID;
````

![row_number](https://github.com/hhuseyincosgun/SQL_Window_Functions/assets/21257660/b353889c-52ec-4da7-8a9c-a864a7bd9482)

### 2.2 PERCENT_RANK (), CUME_DIST ()

````sql
SELECT  OrderID, UnitPrice * Quantity AS Total_Sale,
   ROUND(PERCENT_RANK() OVER(PARTITION BY OrderID 
    ORDER BY (O.UnitPrice * O.Quantity) DESC), 2)
     AS Order_Percent_Rank, 
   ROUND(CUME_DIST() OVER(PARTITION BY OrderID 
    ORDER BY (O.UnitPrice * O.Quantity) DESC),2) 
     AS Order_Cume_Dist
FROM  [Order Details] O;
````
![percent](https://github.com/hhuseyincosgun/SQL_Window_Functions/assets/21257660/f26517e6-2bac-4ac1-87c0-058d81e1a28c)

### 2.3 NTILE()

The NTILE() function in SQL is used to divide the result set into a specified number of roughly equal parts or "tiles" and assign a group or bucket number to each row based on its position within the ordered result set.

List employees by dividing them into 3 groups:

````sql
SELECT FirstName, LastName, Title,
   NTILE(3) OVER(ORDER BY FirstName)
FROM Employees;
````

![tittle](https://github.com/hhuseyincosgun/SQL_Window_Functions/assets/21257660/584222c7-6f33-44b6-8271-4bea6646bec0)

 ## 3-Value Window Functions
Value window functions in SQL are employed to assign values from one row to another, typically within a specific window. Similar to ranking window functions, and unlike aggregate functions, these value functions lack straightforward equivalents that don't involve windowing.

### 3.1 LAG(), LEAD()

Sorting the shipping costs paid by customers according to their order dates:

````sql
SELECT CustomerID, 
  CONVERT(VARCHAR(10), orderdate , 111) AS OrderDate, 
  Shippers.CompanyName Shipper_Name, 
     LAG(Freight) OVER(PARTITION BY customerID 
       ORDER BY orderdate DESC) AS Previous_Order_Freight, 
  Freight AS Order_Freight, 
     LEAD(Freight) OVER(PARTITION BY customerID 
       ORDER BY orderdate DESC) AS Next_Order_Freight
FROM Orders
JOIN Shippers 
ON Shippers.ShipperID = Orders.ShipVia;
````

![lag](https://github.com/hhuseyincosgun/SQL_Window_Functions/assets/21257660/171e510c-e4af-4a8b-9d1d-376a04bf698f)

### 3.2 FIRST_VALUE(), LAST VALUE()

Listing of total prices by customers for each order:

````sql
SELECT O.CustomerID, OD.OrderID,
 CONVERT(VARCHAR(10), O.OrderDate , 111) AS OrderDate,
 SUM(OD.UnitPrice*OD.Quantity) AS Total_Price, 
    FIRST_VALUE(SUM(OD.UnitPrice*OD.Quantity)) OVER 
       (PARTITION BY O.CustomerID 
          ORDER BY O.CustomerID,O.OrderDate) First_Order,
    LAST_VALUE(SUM(OD.UnitPrice*OD.Quantity)) OVER 
       (PARTITION BY O.CustomerID 
          ORDER BY O.CustomerID,O.OrderDate) Last_Order
FROM [Order Details] OD
JOIN Orders O
ON O.OrderID = OD.OrderID
GROUP BY O.CustomerID, OD.OrderID, O.OrderDate
ORDER BY O.CustomerID, O.OrderDate;
````

![first_last](https://github.com/hhuseyincosgun/SQL_Window_Functions/assets/21257660/e34c5402-b241-4304-97e9-12b7af4ca78e)



