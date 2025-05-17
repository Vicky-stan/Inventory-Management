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
- Determine the top 5 products with the highest holding costs.
- Identify suppliers responsible for stockouts.
- Are there regional demand patterns affecting stock levels.
- What is the total number of stock quantity every month?
- What are the inventory holding period of top selling products?

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
