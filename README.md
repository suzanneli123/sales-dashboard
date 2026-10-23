 # Sales Performance Dashboard
 
An interactive Power BI dashboard that shows how the business is selling, where the money is coming from, and whether that growth is actually profitable.

# Business Goal
The company sells technology and office products to 12 business customers. Orders come in through three channels (Distributor, Retail and Online) across four regions (North, South, East and West).

Management could see that sales were going up, but they could not answer simple follow-up questions quickly. Is the growth real? Is it making us more money or just more revenue? Which products, channels and regions are carrying the business, and where are we leaving money on the table?

The goals of this project:
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
| Source | Staging sales table loaded into Power BI *(sales_data.xlsx)* |
 
**Fields in the sales data:** OrderID, OrderDate, Customer, Region, Product, Category, Channel, Quantity, SalesAmount, CostAmount.

# Methodology
### Step 1: Data preparation (Power Query)
- Loaded a single staging sales table and used it as the base for all other tables.
- **Fact_Sales** was created by removing the Category column, because category belongs to the product, not the order.
- **Dimension tables** (Dim_Product, Dim_Customer, Dim_Region, Dim_Channel) were each created by keeping only the relevant columns and removing duplicates, so every dimension has one row per unique value.
- Data types were set explicitly (dates, whole numbers for quantity, decimals for sales and cost).

### Step 2: Data model (star schema)

The data follows a star schema, with one central fact table linked to five dimension tables through one-to-many relationships.
<img width="1700" height="1050" alt="image" src="https://github.com/user-attachments/assets/bddef79a-1538-42ba-b746-755b11a259b2" />

### Step 3: KPI design (DAX)
 
| KPI | Definition | DAX logic |
|---|---|---|
| Total Sales | Revenue from all orders | `SUM(SalesAmount)` |
| Gross Profit | Sales minus cost | `[Total Sales] - [Total Cost]` |
| Margin % | Share of each sales dollar kept as profit | `DIVIDE([Gross Profit], [Total Sales])` |
| Total Orders | Number of distinct orders | `DISTINCTCOUNT(OrderID)` |
| Average Order Value | Revenue per order | `DIVIDE([Total Sales], [Total Orders])` |
| Units Sold | Total quantity sold | `SUM(Quantity)` |
| Average Selling Price | Revenue per unit | `DIVIDE([Total Sales], [Units Sold])` |
| Gross Profit per Unit | Profit earned on each unit | `DIVIDE([Gross Profit], [Units Sold])` |
| Top Revenue Product | Best-selling product in the current filter | `TOPN(1, products, [Total Sales])` |
| Highest-Margin Category | Most profitable category in the current filter | `TOPN(1, categories, [Gross Profit Margin])` |

### Step 4: Data Visualization


# Key Findings
 
* Sales grew 11% year over year, from $171.9K to $190.8K. Profit grew faster (+18.6%), so margin improved from 37.0% to 39.5%. The business is growing profitably.
* The growth came from higher value per sale, not from selling more. Units sold slipped slightly (382 to 367).
* South is the growth engine. It is the largest region (37% of sales) and grew 94% year over year, while West grew 8%, North fell 18% and East fell 39%. Almost all of the company's growth comes from South.
* Distributor is the largest channel (43% of sales) but has the lowest margin (36.3%). Online has the best margin (40.9%), which makes it the most
* Electronics (Laptop and Smartphone) brings in 68% of revenue but earns the lowest category margin (35.5%). Accessories earns the best (49.9%, with Headphones at 53.6%) but is a small share of sales.
* Sales are fairly spread across customers. The top customer, Delta Mart, is about 13% of sales and the top three together are about 36%.

# Recommendations
 
1. **Make more profit on laptops and distributor sales**. These bring in the most money, but we keep the least profit from each dollar. Charging a little more or spending a little less on them would add a lot of profit.
2. **Copy what worked in South.** Find out what drove the 94% growth (customers, pricing, channel mix, sales coverage) and test it in other regions.
3. **Investigate the East and North declines.** East lost nearly 40% of its sales year over year. Check whether this is lost customers, fewer orders or pricing.
4. **Push high-margin Accessories.** Headphones and Monitors can be bundled or cross-sold with Laptops and Smartphones to lift overall margin.
5. **Watch customer concentration.** Keep the top accounts happy, and also target the growth of smaller ones like Evergreen Co and Fusion Retail

# Limitations

- The dataset is small (240 orders, 12 customers, 2 years), so percentages such as regional growth can swing sharply on a few large orders. Treat them as directions to investigate, not final conclusions.
- Profit here is **gross profit** (sales minus cost of goods). It does not include marketing, logistics, discounts or overheads.
- There is no sales target or budget in the data, so performance is compared with last year only.

 ## What I would do next time
 - Add targets and budget vs actual
 - customer segmentation (for example RFM) to identify the best accounts
 - forecast of the next 6 to 12 months.

# Tools Used
 
- **Power BI Desktop**
- **Power Query**: data cleaning and data modeling (fact and dimension tables)
- **DAX**: KPIs
- **Custom visuals**

## Author
Suzanne Li
