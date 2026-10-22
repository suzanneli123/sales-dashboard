# Sales Performance Dashboard
 
An interactive Power BI dashboard that shows how the business is selling, where the money is coming from, and whether that growth is actually profitable.

# Business Goal
The company sells technology and office products to 12 business customers. Orders come in through three channels (Distributor, Retail and Online) across four regions (North, South, East and West).

Management could see that sales were going up, but they could not answer simple follow-up questions quickly. Is the growth real? Is it making us more money or just more revenue? Which products, channels and regions are carrying the business, and where are we leaving money on the table?
The goal of this project is to give leadership one place to answer those questions and decide where to focus next.

What this project sets out to do
1. Track overall sales, profit and margin, and show whether they are moving up or down compared with last year.
2. Find which regions, channels, products and customers drive the most revenue.
3. Separate "high sales" from "high profit" to spot where we sell a lot but earn little.
4. Compare year-over-year growth by region to see where the business is gaining or losing ground.
5. Turn the numbers into clear recommendations on where to invest, where to fix pricing or cost, and where to grow.

# Data
| Item | Detail |
|---|---|
| Grain | One row per order (240 orders, each with a unique Order ID) |
| Period | January 2024 to December 2025 (2 full years) |
| Customers | 12 |
| Products | 6, grouped into 4 categories |
| Regions | 4 (North, South, East, West) |
| Channels | 3 (Distributor, Retail, Online) |
| Source | Staging sales table loaded into Power BI *(add your source file or system name here)* |
 
**Fields in the sales data:** OrderID, OrderDate, Customer, Region, Product, Category, Channel, Quantity, SalesAmount, CostAmount.

# Methodology
### Step 1: Data preparation (Power Query)
- Loaded a single staging sales table and used it as the base for all other tables.
- **Fact_Sales** was created by removing the Category column, because category belongs to the product, not the order.
- **Dimension tables** (Dim_Product, Dim_Customer, Dim_Region, Dim_Channel) were each created by keeping only the relevant columns and removing duplicates, so every dimension has one row per unique value.
- Data types were set explicitly (dates, whole numbers for quantity, decimals for sales and cost).

### Step 2: Data model (star schema)
The model is a star schema: one central fact table and descriptive dimensions around it. This keeps filters predictable and the DAX simple.
 
```
Dim_Customer ─┐
Dim_Region ───┤
Dim_Product ──┼──►  Fact_Sales  ◄── Dim_Date (calendar table)
Dim_Channel ──┘
```




# Results



# Recommendations 




# Limitations





# Challenges


