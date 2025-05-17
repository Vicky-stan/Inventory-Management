# Inventory-Management
# Data-Driven Inventory Management with SQL: Using AMAZON as a case study, perform a calculation to optimize inventory management

## Introduction
In this Project, SQL was used to analyze and optimize inventory managememt using Amazon as a case study. The focus was on improving stock efficiency, reducing overstock/stockouts, and supporting data-driven decisions.

## Project Description
This project involves the analysis of inventory data. The tasks to be completed are;
- Explore the data to determine the products that are in-stock and out-of-stock.
- Explore the data to determine the suppliers and warehouse locations and their average lead times.
- Explore the data to determine the reorder points of products.
- Explore the data to determine the holding period of products.
- Create a markdown file.
- Host the code on Github or Gitlab.

## Research Questions
- Determine the products that are in-stock and out-of-stock.
- Count the total number of products in each category.
- Determine the available stock quantity of each product.
- Which products are below their reorder points?
- What are the average lead times for different suppliers and fulfilment centres?
- Determine the products that are at risk of going out-of-stock in the next 2 weeks.
- Determine the products with high inventory value (Unit_Price × Stock_Quantity). 
- Identify suppliers responsible for stockouts.
- What are the inventory holding period of top selling products?
- What percentage of inventory is classified as dead stock or slow-moving?
- Are there regional demand patterns affecting stock levels.

## About the Dataset
The dataset contains 17 columns and 5001 rows. Each data entry represents an information about the product inventory with a product unique identifier. 
Link to dataset
The columns are described as follows;
- ProductID: A unique identifier for each product
- ProductName: The name of each product.
- Category: Specific grouping of each product into a category.
- Unit_Price: Price per unit of the product.
- Stock_quantity: Current quantity of the product in stock.
- Stock level: Stock level classification (e.g low, mid, High).
- Reorder Point: Minimum quantity before reordering is triggered.
- Lead Time: Days required to receieve the product after placing an order
- Last Restock date: Last date the product was restocked.
- Supplier ID: ID of the supplier providing the product.
- Warehouse Location: Location or aisle of the product in the warehouse.
- Minimum Order Quantity: Minimum number of units that can be ordered from the supplier.
- Stock Status: Stock status of the item (Whether the product is in-stock or out-of-stock).
- Entry Date: Date the item was first entered into the inventory system.
- Country: Country where the warehouse is located or supplier operates.
- Latitude: Geolocation latitude of the product warehouse or origin.
- Longititude: Geolocation longititude of the product warehouse or origin.

## Languages, Utilities, and Environment Used
- SQL: Data transformation, data cleaning & Exploration
- Environment: Microsoft SQL Management Server (SSMS).

## Data Importation Into SSMS
- Launched the Microsoft SQL Management StudiO
- Navigated to the database > created a database > selected Tasks > selected Import flat file
- In the new window that appears, I clicked on Next
- Used the drop-down menu and selected Flat File Source > Next
- Clicked on Browse > selected the file from my computer > Open
- Validated column properties such as primary key, data type, allow nulls.
  
The above steps successfully imported the dataset into my database in SQL Server.

## Data Analysis Using SQL Generated Queries
1. Determine the products that are in-stock and out-of-stock
   ```
   SELECT 
	     Product_ID,
	     Product_Name,
	     Status
   FROM Inventory
   WHERE Status = 'In Stock'
  ```
  SELECT 
	   Product_ID,
	   Product_Name,
	   Status
 FROM Inventory
 WHERE Status = 'Out of Stock'
```
2. Count the total number of products in each category.
```
SELECT 
	 Category,
	 COUNT(Product_ID) AS Total_products
FROM Inventory
GROUP BY Category
```
3. Determine the available stock quantity of each product.
```
SELECT 
	  Product_ID,
	  Product_Name,
	  SUM(Stock_Quantity) AS total_stocks_per_product
FROM Inventory
GROUP BY Product_ID, Product_Name
ORDER BY total_stocks_per_product DESC
```
4. Which products are below their reorder points?
```
SELECT 
	    Product_ID,
	    Product_Name,
	    Stock_Quantity,
	    Reorder_Point,
	    (Reorder_Point - Stock_Quantity) AS Stock_deficit
FROM Inventory
WHERE Stock_Quantity < Reorder_Point
ORDER BY Stock_deficit DESC
```
5. What are the average lead times for different suppliers and fulfilment centres?
```
SELECT 
	Supplier_ID,
	Warehouse_Location,
	AVG(Lead_Time_Days) AS Average_lead_time
FROM Inventory
GROUP BY Supplier_ID, Warehouse_Location
ORDER BY Average_lead_time DESC
```
6. Determine the products that are at risk of going out-of-stock in the next 2 weeks.
```
SELECT 
	Product_ID,
	Product_Name,
	Category,
	Stock_Quantity,
	Reorder_Point,
	Lead_Time_Days,
    (Reorder_Point - Stock_Quantity) AS Stock_Deficit
FROM Inventory
WHERE Stock_Quantity < Reorder_Point
AND Lead_Time_Days > 14
```
7. Determine the products with high inventory value (Unit_Price × Stock_Quantity). 
```
SELECT 
	 Product_ID,
	 Product_Name,
	 Stock_Quantity,
	 Unit_Price,
	 (Unit_Price * Stock_Quantity) AS Inventory_value
FROM Inventory
ORDER BY Inventory_value DESC
```
8. Identify suppliers responsible for stockouts.
```
SELECT 
	Supplier_ID,
	COUNT(*) AS Stockout_products,
	AVG(Lead_Time_Days) AS Avg_lead_time,
SUM(CASE 
        WHEN Stock_Quantity = 0 THEN 1 
        ELSE 0 
        END) AS True_Stockouts,
SUM(CASE 
        WHEN Stock_Quantity < Reorder_Point THEN 1 
        ELSE 0 
        END) AS Low_Stock_Items
FROM Inventory
WHERE 
    Stock_Quantity <= Reorder_Point
GROUP BY 
    Supplier_ID
ORDER BY Stockout_products DESC, Avg_lead_time DESC
```
9. What are the inventory holding period of top selling products?
```
SELECT TOP 10
	Product_ID,
	Product_Name,
	DATEDIFF(DAY, Entry_Date, Last_Restock_Date) AS Holding_period
FROM Inventory
ORDER BY Holding_period  DESC 
```
10. What percentage of inventory is classified as dead stock or slow-moving?
```
SELECT 
    COUNT(*) AS Total_Products,
    SUM(CASE 
            WHEN Status = 'In Stock' AND Last_Restock_Date < DATEADD(DAY, -180, GETDATE()) 
            THEN 1 
            ELSE 0 

        END) AS Slow_Moving_Products,
    CAST(
        100.0 * SUM(CASE 
                        WHEN Status = 'In Stock' AND Last_Restock_Date < DATEADD(DAY, -180, GETDATE()) 
                        THEN 1 
                        ELSE 0 
                    END) / COUNT(*) 
        AS DECIMAL(5,2)
    ) AS Slow_Moving_Percentage
FROM 
    Inventory
```
11. Rank products within each category by inventory value
```
SELECT 
    Category,
    Product_ID,
    Product_Name,
    Stock_Quantity,
    Unit_Price,
    (Stock_Quantity * Unit_Price) AS Inventory_Value,
    RANK() OVER (
        PARTITION BY Category 
        ORDER BY (Stock_Quantity * Unit_Price) DESC
    ) AS Inventory_Value_Rank
FROM 
    Inventory;
```
