The AdventureWorks database supports standard online transaction processing scenarios for a fictitious bicycle
manufacturer (Adventure Works Cycles). Scenarios include Manufacturing, Sales, Purchasing, Product Management,
Contact Management, and Human Resources.

Adventure Works Cycles (AWC), is an international
manufacturer and seller of bicycles and accessories. The company has 3 main regional sales offices in America,
European and Pacific.AWC sales mainly come from two sales channels, such as resellers and internet/online . There are
three major product categories for AWC online sales namely bikes, clothing and accessories & components. The core
income for AWC is obtained by selling three main brands of bikes such as mountain bikes, road bikes and touring bikes
bicycles. An additional income is generated by selling cycling accessories such as bottles, caps, gloves, jerseys, and
components namely bike racks, brakes chains, handlebars, etc.[1]

In this study we just use  Sales.SalesOrderHeader table [2] and filter to United State (US) market, which consists of 12041order records to get RFM values, 
The table has fieilds such as SalesOrderID (primary key), OrderDate (Date the sales order was created), CustomerID (Cutomer identification number who had ordered) 
SubTotal (Computed as Sum(UnitPrice * (1 - UnitPriceDiscount) * OrderQty) for the appropriate SalesOrderID), TerritoryID (Territory in which the sale was made, which TerritoryID between 1 and 5 are United States market)
We use SQL query to calculate Frequency, Recency, Monetary for each customer base on table above as view. After that, we export to excel for analysis.

Some SQL queries we used to get Frequency, Recency and Monetary for each customer such as:

/* Getting Frequency, Requency, Monetary from Sales.SalesOrderHeader table
in case of customers are in US  and save to RFM_US view */
CREATE OR ALTER VIEW RFM_US AS
SELECT CustomerID, DATEDIFF(DAY, MAX(OrderDate), (SELECT MAX(OrderDate) FROM Sales.SalesOrderHeader)) + 1 AS 'Recency', 
COUNT(SalesOrderNumber) AS 'Frequency', SUM(SubTotal) AS 'Monetary'
FROM Sales.SalesOrderHeader
WHERE TerritoryID BETWEEN 1 AND 5
GROUP BY CustomerID

/* Join Sales.SalesOrderHeader, Sales.SalesOrderDetail to get product and quantity  each SalesOrderID */
CREATE OR ALTER VIEW INVOICE_DATASET AS
SELECT H.CustomerID, H.SalesOrderNumber, H.OrderDate, D.OrderQty, D.UnitPrice, D.ProductID
FROM Sales.SalesOrderHeader H JOIN Sales.SalesOrderDetail D ON H.SalesOrderID = D.SalesOrderID
WHERE H.TerritoryID BETWEEN 1 AND 5

[1] https://www.ijrte.org/wp-content/uploads/papers/v8i6/E7007018520.pdf

[2] AdventureWorks - Data Dictionary: https://dataedo.com/download/AdventureWorks.pdf


