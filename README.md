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

**Customers Count:** Find out how many total ocustomers are in the dataset.

**Orders Count:** Find out how many unique orders are in the dataset.

**ProductCategory Count:** Identify all unique product categories in the dataset.

**ProductsubCategory Count:** Identify all unique product categories in the dataset.

**ProductCount:** Identify all unique products in the dataset.



```sql

SELECT * FROM sales_data;
SELECT COUNT(*) FROM sales_data;
SELECT COUNT(DISTINCT customerid) as total_customers FROM sales_data;
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
**Top 5 Customers by revenue**
```sql
SELECT 
    customerid,
    sum(totaldue) AS revenue
FROM 
    sales_data
WHERE 
    TRIM(LOWER(status)) = 'shipped'
GROUP BY 
    customerid
ORDER BY 
    revenue DESC
LIMIT 5;
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
**Top 5 Salespersons by Revenue**
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
**Total number of Customers and their purchase count**
```sql
SELECT 
    customerid, 
    COUNT(orderid) as purchase_count, 
    SUM(totaldue) as total_spent 
FROM 
    sales_data
WHERE Status = 'Shipped'
GROUP BY 
    customerid;
```
-- Customer Growth Over the Years
SELECT 
   EXTRACT(YEAR FROM OrderDate) AS year,
   COUNT(DISTINCT CustomerID) AS TotalDistinctCustomers
FROM 
    sales_data
GROUP BY year
ORDER BY year;
-- Customer Reach Over the Years
SELECT 
   EXTRACT(YEAR FROM OrderDate) AS Year,
   COUNT(DISTINCT CustomerID) AS TotalDistinctCustomers
FROM 
    Sales_Data
WHERE 
    TRIM(LOWER(Status)) = 'shipped'
GROUP BY 
    EXTRACT(YEAR FROM OrderDate)
ORDER BY 
    Year;
**Identify repeat purchases**
```sql
SELECT CustomerID, Status
FROM (
    SELECT CustomerID, Status, COUNT(OrderID) AS PurchaseCount
    FROM Sales_Data
    WHERE Status = 'Shipped'
    GROUP BY CustomerID, Status
) AS RepeatPurchases
WHERE PurchaseCount > 1;
```
**Identify single purchasers**
```sql
SELECT CustomerID, Status
FROM (
    SELECT CustomerID, Status, COUNT(OrderID) AS PurchaseCount
    FROM Sales_Data
    WHERE Status = 'Shipped'
    GROUP BY CustomerID, Status
) AS RepeatPurchases
WHERE PurchaseCount <= 1;
```
**High-frequency Customers**
```sql
SELECT 
    customerid 
FROM 
    (
        SELECT 
            customerid, 
            COUNT(orderid) as purchase_count, 
            SUM(totaldue) as total_spent 
        FROM 
            sales_data 
		WHERE Status = 'Shipped'
        GROUP BY 
            customerid
    )
WHERE 
    purchase_count > 10;
```
**Low-frequency Customers**
```sql
SELECT 
    customerid 
FROM 
    (
        SELECT 
            customerid, 
            COUNT(orderid) as purchase_count, 
            SUM(totaldue) as total_spent 
        FROM 
            sales_data
		WHERE Status = 'Shipped'
        GROUP BY 
            customerid
    )
WHERE 
    purchase_count <= 10;
```

**5.Data Visualization:** Used tableau for data visualization

**Findings:**

**Customer Demographics:** The dataset includes customers from different demographics, with sales distributed across various product categories such as Bikes, Components, Clothing, and Accessories.

**Sales Trends:** Both monthly and weekly analysis reveal variations in sales, helping to identify peak seasons and days.

**Product Flow Analysis:** This analysis illustrates the status of total orders, showing how many products got approved, backordered, cancelled, are in-process, rejected, and shipped.

**Product Insights:** Sales and revenue are analyzed by product category and sub-category, providing a detailed understanding of product performance.

**Revenue Insights:** The analysis identifies the top-spending customers, the most popular products, and the highest-revenue salespersons.

**Freight Charges Insights:** This section shows the average freight charges for each product category.

**Reports**

**Yearly Sales Performance:**

Sales peaked in 2013, contributing to 54.04% of the total sales over the years. This indicates that 2013 was a particularly successful year for sales.

**Monthly Revenue Trend:**

March had the highest monthly revenue at 14.49%, while April had the lowest at 1.83%. This suggests a potential seasonal impact on sales, with March being a strong month for revenue.

**Sales by Geographical Location:**

Canada was the leading region, accounting for 38.28% of total sales. This highlights the importance of the Canadian market for the business.

**Product Flow Analysis:**

The highest number of products in the "shippied" category were clothing with 6582 and Bikes with 6139 units . This indicates strong demand for clothing and bike products.

The majority of orders are in the "Approved" (6,371) and "In process" (6,049) categories, indicating a strong pipeline of orders.

A significant number of orders are "Cancelled" (2,647) and "Rejected" (1,845), which may need further investigation to understand the reasons for these high numbers.

"Shipped" orders are 5,691.

**Sales Trend by Day of the Week:**

Sales were significantly higher during weekdays (72.16%) compared to weekends (27.84%). This suggests that customers are more likely to make purchases on weekdays.

**Weekly Revenue Trend:**

Wednesday recorded the highest revenue at 22.31%, while Friday had the lowest at 5.39%. This could indicate mid-week promotional activities or other factors driving sales on Wednesdays.

**Sales and Revenue by Category:**

**Clothing:** While accounting for 32.49% of sales, Clothing only contributes 2.64% to revenue. This indicates a low revenue per sale for Clothing items.

**Bikes:** Although they make up only 30.31% of sales, Bikes contribute a significant 79.72% to revenue. Bikes are the highest revenue-generating category.

**Revenue by Product Sub-Category:**

Mountain Bikes: Contribute the highest revenue at 36.30%.

Road Bikes: Follow closely with 30.03% of revenue.

Touring Bikes: Account for 21.65% of revenue.

Mountain and Road Frames: Together, they make up a smaller portion of revenue at 12.01%.

**Top 5 Products by Revenue:**

Mountain-200 Black, 38: Contributes the highest revenue at 28.05%.

Mountain-200 Black, 42: Follows with 20.92% revenue.

Mountain-200 Silver, 38 and 42: Together, these products contribute 36.66% of revenue.

Touring-1000 Blue, 60: Accounts for 14.37% of revenue.

**Top 5 Customers by Revenue:**

Customer 29580: Contributes the highest revenue at 34.81%.

Customer 29722 and Customer 29639: Together, they contribute 50.99% of revenue.

These top customers are significant revenue contributors and maintaining a good relationship with them could be beneficial.

**Top 5 Salespersons by Revenue Contribution:**

Salesperson 289, 282, 283 and 290: Major Contributors for revenue

**Find the interactive Tableau dahsboards here**

https://public.tableau.com/app/profile/soujanya.tumma/viz/Sales_Analysis_17392234878200/Dashboard1

https://public.tableau.com/app/profile/soujanya.tumma/viz/Sales_Analysis_2_17392235361990/Dashboard2

**Final Conclusion:**

The dashboards provide a detailed overview of business performance, highlighting key opportunities for improvement in product category diversification, geographical reach, and customer growth. 

A significant challenge identified is the reliance on the bike category, indicating the need for expansion into other product categories and regions. 

Additionally, a significant number of orders are "Cancelled" (2,647) and "Rejected" (1,845), which may need further investigation to understand the reasons behind these high numbers, potentially indicating underlying issues in the order process or customer satisfaction.

**Sales Improvement Recommendations:**

**Promotions for Underperforming Categories:** Introduce special promotions and offers for accessories and components to increase their revenue contribution.

**Geographical Targeting:** Invest in marketing campaigns in less saturated regions to expand the customer base and build loyalty.

**Seasonal Promotions:** Maximize sales through discounts and limited-time offers during peak seasons.

**Order Process and Customer Satisfaction Analysis:** Conduct a thorough investigation into the high number of cancelled and rejected orders. Review the order process, gather customer feedback, ensure accurate inventory management, address payment, security issues and strengthen customer support.
