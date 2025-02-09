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

**Database Creation**: The project starts by creating a database named sales_project_analysis.
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
    	TerritoryID` INT,
    	Territory` VARCHAR(255),
    	TerritoryGroup` VARCHAR(255),
    	ShipMethodID` VARCHAR(255),
    	ShipMethod` VARCHAR(255),
    	ProductID` VARCHAR(255),
    	Product` VARCHAR(255),
    	ProductSubCategory` VARCHAR(255),
    	ProductCategory` VARCHAR(255),
    	OrderQty` INT,
    	UnitPrice` DECIMAL(10, 2),
    	LineTotal` DECIMAL(10, 2),
    	TaxAmt` DECIMAL(10, 2),
    	Freight` DECIMAL(10, 2),
    	TotalDue` DECIMAL(10, 2)
);

```
**2. Data Exploration & Cleaning**

**View All Column Names and Data** : To see all the column names and data in the dataset
**Record Count**: Determine the total number of records in the dataset.
**Orders Count:** Find out how many unique orders are in the dataset.
**ProductCategory Count:** Identify all unique product categories in the dataset.
**Null Value Check:** Check for any null values in the dataset and if present delete records with missing data.

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
	  OrderQty IS NULL OR UnitPrice IS NULL OR LineTotal IS NULL OR	TaxAmt IS NULL OR	
	  Freight IS NULL OR	TotalDue IS NULL;

DELETE FROM sales_data
WHERE 
    orderdetailid IS NULL OR orderid IS NULL OR orderdate IS NULL OR 
    duedate IS NULL OR shipdate IS NULL OR statusid IS NULL OR status IS NULL OR
    onlineorderflag IS NULL OR customerid IS NULL OR salespersonid IS NULL OR
	  territoryid IS NULL OR territory IS NULL OR territorygroup IS NULL OR 
    shipmethodid IS NULL OR shipmethod IS NULL OR productid IS NULL OR 
    product IS NULL OR ProductSubCategory IS NULL OR ProductCategory IS NULL OR	
	  OrderQty IS NULL OR UnitPrice IS NULL OR LineTotal IS NULL OR	TaxAmt IS NULL OR	
	  Freight IS NULL OR	TotalDue IS NULL;
