# Northwind Foods Sales Dashboard

Northwind Foods SQL &amp; Tableau Dashboard

This data analytics project focuses on the preparation and visualization of sales data for Northwind Traders, a fictional food supplier. The Maven Analytics Northwind Traders [Dataset](https://www.mavenanalytics.io/data-playground?dataStructure=2lXwWbWANQgI727tVx3DRC) was utilized for this purpose. The dataset encompasses detailed information about sales, orders, customers, products, shippers, and employees.

To begin, I established a SQL database to store the dataset and employed SQL queries to extract valuable insights and address critical business inquiries. A comprehensive sales report was generated using SQL, serving as the foundation for the creation of a visually compelling Tableau dashboard.

The primary objective of this project repository is to provide a clear demonstration of my expertise and proficiency in utilizing Tableau and SQL. 

[SQL](https://github.com/MyrthaDestra/Northwind/blob/main/Northwind%20Traders%20SQL.sql)
**Query Samples:**
---

Joining tables and performing aggregations:
```
/* Calculate the total sales revenue for each product category during the year 2014. */
SELECT 
Year(o.orderdate) As Year,
Sum(od.unitprice*od.quantity) as Total,
ct.categoryName as Category
FROM order_details od
LEFT JOIN products pd on od.productid=pd.productid
LEFT JOIN categories ct on pd.categoryID=ct.categoryID
LEFT JOIN orders o on od.orderid=o.orderid
WHERE Year(o.orderdate) = 2014
GROUP BY Category;
```

Combining CTE and case statements:
```
/* Find the day of the week with the highest sales revenue and present a sorted list of each day of the week alongside its corresponding sales revenue. */

WITH Total_Sales_By_Date as (
SELECT 
od.orderid, 
od.unitprice*quantity as total,
o.orderdate
FROM Order_details od
JOIN Orders o ON od.orderid=o.orderid
)
SELECT sum(total) As Grand_Total, DayOFWeek(orderdate) as Day,
CASE DayOFWeek(orderdate)
WHEN 2 THEN 'Monday'
WHEN 3 THEN 'Tuesday'
WHEN 4 THEN 'Wednesday'
WHEN 5 THEN 'Thursday'
WHEN 6 THEN 'Friday'
ELSE DayOFWeek(orderdate)
END AS 'Weekday'
FROM Total_Sales_By_Date 
GROUP BY Day
Order by Grand_Total Desc;
```
Combining CTE with rank window functions:
```
/* Determine the month with the second-highest total sales for the year 2014. */
WITH Months_Total as (
SELECT 
Month(o.orderdate) As Month, 
Sum(od.unitprice*od.quantity) as Total
FROM order_details od
LEFT Join orders o on od.orderID=o.orderID
WHERE Year(o.orderdate)=2014
GROUP BY Month
), 
Ranked as (
Select *, 
RANK() OVER(ORDER BY Total DESC) as Month_Rank
FROM Months_Total
) 
SELECT * from Ranked where Month_Rank = 2;
```

Simple self join: 
```
/* Generate a query that presents a comprehensive list of employees along with their corresponding managers. In cases where employees do not have managers, they should be displayed as reporting to themselves. */
SELECT
e.employeeid as 'Employee',
e.employeeName as 'Employee Name',
IFNULL(m.employeeName, e.employeeName) as 'Manager Name'
From employees e
LEFT JOIN employees m ON m.employeeID=e.reportsTo
ORDER BY Employee;
```

The tableau dashboard is located [here](https://public.tableau.com/app/profile/myrtha.destra/viz/NorthwindFoodsSalesDashboard/SalesOverview).

![Final Tableau Screenshot](https://github.com/MyrthaDestra/Northwind/assets/134890000/f6ad473d-30e7-41f6-9d55-0e57e4c76fd0)



