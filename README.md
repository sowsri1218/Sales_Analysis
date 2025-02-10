# Sales_Analysis


**Project Overview**

This project is aims to explore, clean, and analyze sales data by using SQL. IT involves setting up a sales_analysis_project database, performing exploratory data analysis (EDA), and derive insights through SQL queries. and tableau is used for data visualization.

**Objectives**

**1. Set up sales_analysis_project database**: Create and populate a sales analysis project database with the provided sales data.

**2.Data Cleaning:** Identify and remove any records with missing or null values.

**3. Exploratory Data Analysis (EDA):** Perfor basic exploratory data analysis to understand the dataset.

**4. SQL Queries:** Utilizing SQL queries to derive meaningful insights from the sales data.

**5.Data Visualization:** Using Tableau to create interactive and informative visualizations that effectively communicate the insights.


**Project Structure**

**1. Database Setup**

**Database Creation**: The project starts by creating a database named sales_analysis_project.

**Table Creation:** A table named Sales_data is created to store the sales data. The table structure includes columns for SalesPersonID,	TerritoryID,	Territory,	TerritoryGroup,	ShipMethodID,	ShipMethod,	ProductID,	Product,	ProductSubCategory,	ProductCategory,	OrderQty,	UnitPrice,	LineTotal,	TaxAmt,	Freight and	TotalDue.

```sql

create databases sales_analysis_project;

CREATE TABLE Sales_Data (
	id INT PRIMARY KEY,
    	OrderDetailID INT,
    	OrderID INT,
    	OrderDate DATETIME,
    	DueDate DATETIME,
    	ShipDate DATETIME,
    	StatusID INT,
    	Status VARCHAR(255),
    	OnlineOrderFlag INT,
    	CustomerID VARCHAR(255),
    	SalesPersonID INT,
    	TerritoryID INT,
    	Territory VARCHAR(255),
    	TerritoryGroup VARCHAR(255),
    	ShipMethodID VARCHAR(255),
    	ShipMethod VARCHAR(255),
    	ProductID VARCHAR(255),
    	Product VARCHAR(255),
    	ProductSubCategory VARCHAR(255),
    	ProductCategory VARCHAR(255),
    	OrderQty INT,
    	UnitPrice DECIMAL(10, 2),
    	LineTotal DECIMAL(10, 2),
    	TaxAmt DECIMAL(10, 2),
    	Freight DECIMAL(10, 2),
    	TotalDue DECIMAL(10, 2)
);

```
Then Import the data from the CSV file into the Sales_Data Table.

**2. Data Cleaning**

**Null Value Check:** Check for any null values in the dataset and if present delete records with missing data.

```sql

SELECT * FROM sales_data;
SELECT COUNT(*) FROM sales_data;
SELECT COUNT(DISTINCT orderid) FROM sales_data;
SELECT DISTINCT productcategory FROM sales_data;

SELECT * FROM sales_data
WHERE 
    orderdetailid IS NULL OR orderid IS NULL OR orderdate IS NULL OR 
    duedate IS NULL OR shipdate IS NULL OR statusid IS NULL OR status IS NULL OR
    onlineorderflag IS NULL OR customerid IS NULL OR salespersonid IS NULL OR
    territoryid IS NULL OR territory IS NULL OR territorygroup IS NULL OR 
    shipmethodid IS NULL OR shipmethod IS NULL OR productid IS NULL OR 
    product IS NULL OR ProductSubCategory IS NULL OR ProductCategory IS NULL OR	
    OrderQty IS NULL OR UnitPrice IS NULL OR LineTotal IS NULL OR TaxAmt IS NULL OR	
    Freight IS NULL OR	TotalDue IS NULL;


DELETE FROM sales_data
WHERE 
    orderdetailid IS NULL OR orderid IS NULL OR orderdate IS NULL OR 
    duedate IS NULL OR shipdate IS NULL OR statusid IS NULL OR status IS NULL OR
    onlineorderflag IS NULL OR customerid IS NULL OR salespersonid IS NULL OR
    territoryid IS NULL OR territory IS NULL OR territorygroup IS NULL OR 
    shipmethodid IS NULL OR shipmethod IS NULL OR productid IS NULL OR 
    product IS NULL OR ProductSubCategory IS NULL OR ProductCategory IS NULL OR	
    OrderQty IS NULL OR UnitPrice IS NULL OR LineTotal IS NULL OR TaxAmt IS NULL OR	
    Freight IS NULL OR	TotalDue IS NULL;

```

**3. Exploratory Data Analysis (EDA):**


**View All Column Names and Data** : To see all the column names and data in the dataset

**Record Count**: Determine the total number of records in the dataset.

**Orders Count:** Find out how many unique orders are in the dataset.

**ProductCategory Count:** Identify all unique product categories in the dataset.

**ProductsubCategory Count:** Identify all unique product categories in the dataset.

**ProductCount:** Identify all unique products in the dataset.



```sql

SELECT * FROM sales_data;
SELECT COUNT(*) FROM sales_data;
SELECT COUNT(DISTINCT orderid) FROM sales_data;
SELECT DISTINCT productcategory FROM sales_data;
SELECT DISTINCT productsubcategory FROM sales_data;
SELECT DISTINCT product FROM sales_data;

```

**4. SQL Queries:**

**Total sales Per year**
```sql
SELECT 
    EXTRACT(YEAR FROM OrderDate) AS year,
    sum(orderqty) AS TotalSales
FROM 
    sales_data
WHERE 
    TRIM(LOWER(status)) = 'shipped'
GROUP BY 
    year
ORDER BY year;
```
**Total count of orders by status**
```sql
SELECT status, COUNT(*) 
FROM sales_data 
GROUP BY status
order by status;
```
**Total Sales Per Month**
```sql
SELECT  TO_CHAR(OrderDate, 'Month') AS Month,
sum(orderqty) AS TotalSales
FROM sales_data 
WHERE 
    TRIM(LOWER(status)) = 'shipped'
GROUP BY Month
ORDER BY totalsales desc;
```
**Total Revenue per month**
```sql
SELECT  TO_CHAR(OrderDate, 'Month') AS Month,
SUM(totaldue) AS Revenue,
sum(orderqty) AS TotalSales
FROM sales_data 
WHERE 
    TRIM(LOWER(status)) = 'shipped'
GROUP BY Month
ORDER BY revenue desc;
```
**Total sales by weekday and weekend**
```sql
SELECT 
    CASE WHEN EXTRACT(DOW FROM OrderDate) IN (1, 2, 3, 4, 5) THEN 'Weekday'
    ELSE 'Weekend' END AS DayType,
SUM(orderqty) AS TotalSales
FROM sales_data
WHERE 
    TRIM(LOWER(status)) = 'shipped'
GROUP BY DayType
ORDER BY totalsales desc;
```
**Total sales for each month and best selling month in each year**
```sql
select 
	year,
	month,
	totalsales 
from (
select 
		extract(year from orderdate)as year, 
		TO_CHAR(orderdate, 'month')as month,
		SUM(orderqty) AS TotalSales,
		rank() over (partition by extract(year from orderdate)
		order by SUM(orderqty) desc)as rank 
from sales_data 
WHERE 
    TRIM(LOWER(status)) = 'shipped'
group by year,month 
) 
as t1 where rank = 1;
```
**Total sales by Category**
```sql
select productcategory,
sum(orderqty) as totalsales
from sales_data
WHERE 
    TRIM(LOWER(status)) = 'shipped'
group by productcategory
order by totalsales desc;
```
**Product Flow Analysis by Category and Status with Total Orders**
```sql
SELECT
    productcategory,
    status,
    COUNT(*) AS total,
    SUM(COUNT(*)) OVER (PARTITION BY productcategory) AS total_orders
FROM
    sales_data
GROUP BY
    productcategory,
    status
ORDER BY
    productcategory, status;
```
**Revenue generated by each category**
```sql
SELECT
    ProductCategory,
    SUM(totaldue) AS Revenue
FROM
    Sales_data
WHERE
    Status IN ('Shipped')
GROUP BY
    ProductCategory
order by revenue desc;

```
**Tops 5 Products by revenue**
```sql
select product, productcategory, SUM(totaldue) AS Revenue
from  sales_data
WHERE
    Status IN ('Shipped')
GROUP BY
    Product, productcategory
order by Revenue desc limit 5;
```
**Top 5 orders and their revenue by Product Sub-Category**
```sql
select productsubcategory, sum(totaldue) as revenue,
sum(orderqty) as totalsales
from sales_data
WHERE 
    TRIM(LOWER(status)) = 'shipped'
group by productsubcategory 
order by revenue desc limit 5; 
```
**Top 5 Customers based on the highest total sales**
```sql
select customerid,territory, sum(totaldue) as revenue,
sum(orderqty) as totalsales
from sales_data
WHERE 
    TRIM(LOWER(status)) = 'shipped'
group by customerid, territory
order by revenue desc limit 5;
```
**Find the number of unique customers who purchased items from each category**
```sql
select productcategory,
count(distinct customerid) as unique_customers
from sales_data 
WHERE 
    TRIM(LOWER(status)) = 'shipped'
group by productcategory
order by unique_customers desc;
```
**Sales by region**
```sql
select territory, sum(orderqty) as totalsales 
from sales_data
WHERE 
    TRIM(LOWER(status)) = 'shipped'
group by territory
order by totalsales desc;
```
**The freight charges for each category**
```sql
SELECT
    productcategory,
    round(avg(Freight),2) AS average_freight_charges,
	sum(Freight) as total_freight_charges
FROM
    sales_data
WHERE 
    TRIM(LOWER(status)) = 'shipped'
GROUP BY
    productcategory
ORDER BY
   average_freight_charges desc ;
```
**Top 5 Salespersons-Product Category Pairs by Total Sales and Revenue**
```sql
SELECT
    salespersonid,productcategory,
    SUM(totaldue) AS revenue
FROM
    sales_data
WHERE 
    TRIM(LOWER(status)) = 'shipped'
GROUP BY
    salespersonid,productcategory
ORDER BY
   revenue DESC limit 5;
```
**Findings:**

**Customer Demographics:** The dataset includes customers from different demographics, with sales distributed across various product categories such as Bikes, Components, Clothing, and Accessories.

**Sales Trends:** Both monthly and weekly analysis reveal variations in sales, helping to identify peak seasons and days.

**Product Flow Analysis:** This analysis illustrates the status of total orders, showing how many products got approved, backordered, cancelled, are in-process, rejected, and shipped.

**Product Insights:** Sales and revenue are analyzed by product category and sub-category, providing a detailed understanding of product performance.

**Revenue Insights:** The analysis identifies the top-spending customers, the most popular products, and the highest-revenue salespersons.

**Freight Charges Insights:** This section shows the average freight charges for each product category.
