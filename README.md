# visualizations

This repository includes 3 visualization practices about different topics, using Excel, Power BI or Tableau.

## EXCEL [Coffee Shop: Sales Performance]
![Alt text](/demos/dashboard-excel.png "original photo")

### Visual Elements

#### Dashboard Layout and Interactivity
Date Selector: The top panel includes a timeline selector allowing users to filter data by year and quarter, providing flexibility in viewing historical data trends.

#### Slicers:
Roast Type: Filter by roast type (Dark, Light, Medium).
Loyalty Card: Filter by customer loyalty status (Yes/No).
Size: Filter by coffee package size (e.g., 0.2 kg, 1.0 kg).
Coffee Type: Filter by specific coffee varieties (e.g., Arbica, Excelsa, Liberica, Robusta).

#### Key Metrics
Whole Sales: Displays the total sales amount.
Whole Profit: Shows the total profit generated.
Profitability Ratio: Provides the profit as a percentage of sales, giving insights into profitability.

#### Visualizations
##### Line Chart (Total Sales Trend Over Time):
Displays the sales trend over time for each coffee type (Arbica, Excelsa, Liberica, Robusta), allowing for seasonal and trend analysis.
##### Bar Chart (Total Sales by Country):
Visualizes sales by country, with each segment representing a different coffee type, showing geographic distribution and customer preferences.
##### Dual-Axis Chart (Relationship Between Sales Amount and Order Number):
Compares the frequency of orders (Order Count) against the sales amount, highlighting high-frequency orders and their contribution to total sales.

### Technical Components

#### Data Tables
The workbook contains multiple data tables for different dimensions, including:
Orders: Details each sale, including Roast Type, Coffee Type, Size, Profit, Sales, Total Profit, and Loyalty Card status.
Customers: Stores customer details such as Customer ID, Name, Email, Phone Number, Address, and Loyalty Card status.
Products: Lists available coffee products with specific details for indexing in the orders table.

#### Key Formulas
INDEX-MATCH for Product Lookup

Used in the orders table to retrieve product details based on a combination of attributes.

=INDEX(products!$A$1:$G$49, MATCH(orders!$D7, products!$A$1:$A$49, 0), MATCH(orders!$Q$1, products!$A$1:$G$1, 0))
Retrieves specific information (e.g., Unit Price, Size) from the products table based on attributes like Roast Type and Coffee Type.

#### Slicers and Filters
Slicers are used to dynamically filter visuals and key metrics by roast type, loyalty status, coffee size, and coffee type. The timeline filter allows for time-based filtering by year and quarter, enabling comparative analysis over different periods.


## POWERBI [Retail: Sales Performance]
![Alt text](/demos/dashboard-powerbi.png "original photo")

### Visual Elements

#### Dashboard Layout and Interactivity:
The dashboard is designed with a central filter panel at the top, allowing users to choose a year and a metric to display (Profit, Quantity, Sales). This enhances interactivity and personalization for different data views. Key metrics, such as "YTD Profit," "Profit Change," "YTD Quantity," "Quant Change," and "Sales Change," are highlighted at the top. These provide an instant snapshot of critical figures, helping users quickly grasp the performance status.

#### Treemap Visualization (YTD Values by Country):
A treemap displays YTD values categorized by country. The visual impact is strong, as larger and more dominant blocks (e.g., China) represent countries with higher values, making geographic performance comparisons intuitive. Color-coded regions aid in visually distinguishing different countries.

#### Waterfall Chart (YTD vs. PYTD Values by Month):
The waterfall chart visually represents the change in YTD (Year-To-Date) values compared to PYTD (Previous Year-To-Date) values on a monthly basis. This is particularly effective for showing trends over time, such as increases (green) and decreases (red). The chart includes a "Total" bar, summarizing the entire year's trend, providing a clear picture of cumulative performance.

#### Stacked Column Chart (YTD Comparison by Product Type and Month):
A stacked column chart categorizes performance by month and product type (e.g., Indoor, Landscape, Outdoor). This format allows users to understand the distribution and contribution of different product categories over time. The line representing PYTD serves as a benchmark, making it easy to compare current performance with the previous year.

#### Scatter Plot (Relationship between Profit Ratio and YTD):
A scatter plot shows the relationship between Profit Ratio and YTD, with each dot representing a customer. This visualization highlights potential correlations between profitability and other metrics.

### Technical Components

#### Data Model Structure:
The data model consists of fact and dimension tables:
FACT_SALES: This table holds transactional data, including Account_id, COGS_USD, Date_Time, Price_USD, Product_id, quantity, and Sales_USD.
DIM_DATE, DIM_PRODUCT, DIM_ACCOUNT: These dimension tables provide context for the fact table, covering date, product, and account details.
_Measures table: This separate table contains calculated measures for profit, quantity, and sales metrics, allowing for efficient and centralized measure management.

#### Measures and Calculations:
Key DAX measures such as YTD Profit, PYTD Profit, Profit Change, and Sales Change calculate year-to-date, previous-year-to-date, and changes over time.

PYTD_Profit = 
CALCULATE(
    [Profit],
    SAMEPERIODLASTYEAR(DIM_DATE[Date]),
    DIM_DATE[Inpast] = TRUE()
)
This DAX formula calculates profit from the same period last year, filtered by dates marked as "Inpast" (excluding future dates).

Switch Functionality for Dynamic Metrics:
PYTD (switch) measure uses the SWITCH function with SELECTEDVALUE to dynamically switch between sales, quantity, and profit metrics based on the slicer selection.

PYTD (switch) =
VAR selected_value = SELECTEDVALUE(Slicer_Values[Values])
VAR result = SWITCH(selected_value,
    "Sales", [PYTD_Sales],
    "Quantity", [PYTD_Quantity],
    "Profit", [PYTD_Profit],
    BLANK()
)
RETURN result

This enables users to toggle between different metrics in a single visual, enhancing flexibility and reducing the need for multiple visuals.

#### Data Slicer and Filtering:
Slicer_Values table provides the options for selecting different metrics (Profit, Quantity, Sales), which interact with the PYTD (switch) measure. This approach allows for a dynamic dashboard, where visuals update based on user selections.

## TABLEAU [Health: Covid-19 Vaccination]
![Alt text](/demos/dashboard-tableau.png "original photo")
