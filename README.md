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
- Four relationships are many-to-one, single direction, from Fact_Sales to the Customer, Region, Product and Channel dimensions.
- **Dim_Date** is a calculated calendar table covering every day from 1 January of the first year to 31 December of the last year in the data. It adds Year, Quarter, Month, Month Year and a numeric sort key so months sort chronologically (Jan 2024, Feb 2024, ...) rather than alphabetically.
- A dedicated **Measure** table holds all DAX measures, which keeps the field list tidy.
> Note: the Date table is used by the time-intelligence measures (year-over-year) and by the Year slicer. If it is not already related to `Fact_Sales[OrderDate]` on the model view, add that relationship (Dim_Date[Date] to Fact_Sales[OrderDate], one-to-many) so the Year slicer and YoY calculations filter the facts correctly.

### Step 3: KPI design (DAX)
All business metrics are defined once and reused everywhere.
 
| KPI | Definition | DAX logic |
|---|---|---|
| Total Sales | Revenue from all orders | `SUM(SalesAmount)` |
| Total Cost | Cost of goods sold | `SUM(CostAmount)` |
| Gross Profit | Sales minus cost | `[Total Sales] - [Total Cost]` |
| Gross Profit Margin / Margin % | Share of each sales dollar kept as profit | `DIVIDE([Gross Profit], [Total Sales])` |
| Total Orders | Number of distinct orders | `DISTINCTCOUNT(OrderID)` |
| Total Customers | Number of distinct customers | `DISTINCTCOUNT(Customer)` |
| Average Order Value | Revenue per order | `DIVIDE([Total Sales], [Total Orders])` |
| Units Sold | Total quantity sold | `SUM(Quantity)` |
| Average Selling Price | Revenue per unit | `DIVIDE([Total Sales], [Units Sold])` |
| Gross Profit per Unit | Profit earned on each unit | `DIVIDE([Gross Profit], [Units Sold])` |
| Sales per Customer | Revenue per customer | `DIVIDE([Total Sales], [Total Customers])` |
| Transactions per Customer | Orders per customer | `DIVIDE([Total Orders], [Total Customers])` |
| Sales Previous Year | Same period last year | `CALCULATE([Total Sales], SAMEPERIODLASTYEAR(Dim_Date[Date]))` |
| Sales YoY % | Growth vs last year | `DIVIDE([Total Sales] - [Sales Previous Year], [Sales Previous Year])` |
| Units Sold YoY % | Volume growth vs last year | Same pattern applied to units |
| Top Revenue Product | Best-selling product in the current filter | `TOPN(1, products, [Total Sales])` |
| Highest-Margin Category | Most profitable category in the current filter | `TOPN(1, categories, [Gross Profit Margin])` |

### Step 4: Data Visualization
- Fixed 1920 x 1080 canvas with a dark navy header band and a consistent blue and gray-blue palette.
- Green and red are used only for growth and decline, so colour always carries meaning.
- Total Sales and Gross Profit are always shown side by side, so "big but not profitable" is easy to spot.
- Margin % is placed in tooltips on most charts to keep the visuals clean while still giving the detail on hover.

## 5. Key Findings
 
All figures below are for the full period (2024-2025)
 
**Overall performance**
- Total sales are **$362.7K** and gross profit is **$138.9K**, a margin of **38.3%**, across 240 orders.
- Sales grew **11.0%** from $171.9K in 2024 to $190.8K in 2025. Gross profit grew faster, by **18.6%**, and margin improved from **37.0% to 39.5%**. The business is growing profitably.
- Units sold actually fell slightly (382 to 367), so the growth came from higher value per sale, not from selling more items.

**Where sales come from**
- **Distributor** is the largest channel at **$154.4K (43% of sales)**, but it has the lowest margin (36.3%).
- **Online** earns the best margin (**40.9%**) on $113.9K of sales, so it is the most efficient channel to grow.
- **South** is the biggest region at **$133.8K (37% of sales)**.
**Regional growth is very uneven**
 
| Region | 2024 | 2025 | YoY |
|---|---|---|---|
| South | $45.4K | $88.4K | **+94%** |
| West | $33.0K | $35.5K | +8% |
| North | $48.0K | $39.2K | -18% |
| East | $45.5K | $27.7K | **-39%** |

Almost all of the company's growth comes from South. North and East are shrinking.
 
**Products and categories**
- **Electronics** (Laptop and Smartphone) brings in **68% of revenue** ($245.6K), but at the lowest category margin (35.5%). Laptops alone are $151.2K at a 34.4% margin.
- **Accessories** (Headphones and Monitor) has the best category margin (**49.9%**) but a small share of revenue. Headphones have the highest product margin (53.6%).
- Furniture (Office Chair) and Office Equipment (Printer) are small, steady contributors at around 40% margin.
**Customers**
- Sales are fairly spread out. The top customer, Delta Mart, accounts for about 13% of sales, and the top three (Delta Mart, Lighthouse Co, Indigo Corp) for about 36%.
---

# Recommendations 




# Limitations





# Challenges


