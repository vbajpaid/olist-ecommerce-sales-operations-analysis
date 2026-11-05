# Olist E-Commerce Sales & Operations Analytics

An end-to-end data analytics portfolio project using **Python,
PostgreSQL, SQL and Power BI** to explore Brazilian e-commerce data,
assess sales and fulfilment performance, and communicate business
insights through an interactive dashboard and project report.

> **Project status:** In progress --- dashboard built; final metric
> reconciliation, documentation and portfolio packaging are ongoing.

## 1. Project Overview

This project analyses the **Brazilian E-Commerce Public Dataset by
Olist** to understand sales trends, order behaviour, product-category
performance and delivery operations.

The workflow covers: 1. Loading and exploring the source datasets with
Python. 2. Cleaning and preparing data, including date conversion and
missing-value assessment. 3. Performing exploratory data analysis (EDA).
4. Loading relational datasets into PostgreSQL and querying them with
SQL. 5. Building a Power BI data model, DAX measures and an interactive
dashboard. 6. Documenting findings, recommendations, limitations and
future work.

## 2. Business Problem

E-commerce teams need a reliable view of commercial performance and
order fulfilment. Total sales alone do not explain which product
categories contribute most, how order volumes change over time, or how
consistently orders reach customers by the estimated delivery date.

**Key business question:**\
How can raw e-commerce data be transformed into a clear, interactive
reporting solution that helps stakeholders monitor sales performance and
identify operational issues?

## 3. Dataset

**Dataset:** Brazilian E-Commerce Public Dataset by Olist\
**Source:** [Kaggle --- Olist Brazilian E-Commerce
Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

The project uses related CSV files, including:

  -------------------------------------------------------------------------
  Dataset / table                       Purpose
  ------------------------------------- -----------------------------------
  `orders`                              Order status, purchase date and
                                        delivery milestones

  `order_items`                         Products, item prices and freight
                                        by order

  `products`                            Product details and category
                                        identifiers

  `product_category_name_translation`   English category-name translations

  `customers`                           Customer identifiers and state
                                        information

  `order_payments`                      Payment methods and payment amounts

  `DateTable`                           Calendar dimension created in Power
                                        BI
  -------------------------------------------------------------------------

The source data contains multiple records per order in tables such as
`order_items` and `order_payments`. Distinct order counts and careful
join logic are therefore important.

**Data handling note:** The original dataset is not included in this
repository. Download it from Kaggle and place the required CSV files in
your local data folder. Do not commit credentials, passwords or private
connection details.

## 4. Tools & Technologies

  Tool                         Use
  ---------------------------- ---------------------------------------------------
  Python                       Data loading, cleaning and exploratory analysis
  Pandas, NumPy                Data manipulation and preparation
  Matplotlib                   Exploratory visualisation
  PostgreSQL                   Relational database storage
  SQL                          Data validation, aggregation and business queries
  Power BI                     Data modelling and interactive dashboard
  DAX                          KPI and analytical measure development
  Git & GitHub                 Version control and project documentation
  Gamma (planned / optional)   Presentation development

## 5. Project Workflow

### Step 1 --- Data Loading

-   Imported the Olist CSV datasets into Python using Pandas.
-   Inspected dataset dimensions, column names and data types.
-   Reviewed the structure and relationships between the main tables.

### Step 2 --- Data Cleaning & Preparation

-   Converted order timestamp columns to datetime format.
-   Checked duplicate order identifiers.
-   Profiled missing values in order approval and delivery timestamp
    fields.
-   Reviewed missing values by order status to distinguish expected
    lifecycle gaps from potential data-quality issues.
-   Prepared product and category data for combined analysis.

Missing values were not automatically removed: an undelivered order may
legitimately have no customer-delivery timestamp.

### Step 3 --- Exploratory Data Analysis (EDA)

Analysed: - Order-status distribution. - Monthly order trends. -
Product-category sales contribution. - Delivery duration and on-time
delivery performance. - Missing-value patterns and date coverage.

### Step 4 --- PostgreSQL & SQL Analysis

-   Created the `olist_analytics` database.
-   Imported the required relational tables.
-   Used SQL to examine order status, order counts, item-price sales and
    monthly performance.
-   Compared query outputs with Python and Power BI results.

Example query --- orders by status:

``` sql
SELECT
    order_status,
    COUNT(DISTINCT order_id) AS total_orders
FROM orders
GROUP BY order_status
ORDER BY total_orders DESC;
```

Example query --- total item-price sales:

``` sql
SELECT
    SUM(price) AS total_item_sales
FROM order_items;
```

### Step 5 --- Power BI Data Modelling

The Power BI model combines orders, order items, products, customers,
payments, category translations and a dedicated calendar table.

Key modelling considerations: - Use one-to-many relationships where
appropriate. - Use a date-only order purchase field connected to
`DateTable`. - Use distinct order counts for order-level KPIs. - Avoid
joins or relationships that multiply item or payment records. - Validate
filter propagation, especially for payment-type slicers.

### Step 6 --- DAX & KPI Development

The dashboard includes measures for:

-   Total Sales
-   Total Orders
-   Total Delivered Orders
-   On-Time Delivery Rate
-   Cancellation Rate
-   Average Item Price
-   Average Delivery Time

**Metric definitions:** - **Total Sales:** Sum of `order_items[price]`;
this is item-price sales, not profit and does not automatically include
freight or other adjustments. - **On-Time Delivery Rate:** Delivered
orders received on or before the estimated delivery date, divided by
delivered orders. - **Cancellation Rate:** Canceled orders divided by
all orders in the current filter context. - **Average Delivery Time:**
Average number of days between purchase and customer delivery for
delivered orders.

### Step 7 --- Power BI Dashboard

The interactive **Sales & Operations Performance Dashboard** includes:

-   KPI cards for sales, orders and delivery performance.
-   Monthly Sales Performance trend.
-   Monthly Order Volume trend.
-   Top 10 Product Categories by Sales.
-   Order Status Distribution.
-   Slicers for product category, customer state, date, order status and
    payment type.

### Step 8 --- Reporting & Presentation

The project report documents the business problem, dataset, tools,
analytical workflow, data model, KPI definitions, dashboard findings,
recommendations and limitations.

A concise stakeholder presentation can be prepared using Gamma or
PowerPoint. Add the final presentation to the repository when it is
complete.

## 6. Dashboard Results

The latest dashboard version displays the following figures:

  KPI                       Current dashboard value
  ----------------------- -------------------------
  Total Sales                             R\$13.59M
  Total Orders                               99.44K
  Delivered Orders                           96.48K
  On-Time Delivery Rate                      91.88%
  Cancellation Rate                           0.63%
  Average Item Price                      R\$120.65
  Average Delivery Time                  12.50 days

These are development-stage dashboard values and should be reconciled
against source-level SQL totals before being treated as final certified
results.

### Initial Observations

-   Health & Beauty is the leading product category by item-price sales
    in the current analysis.
-   Watches & Gifts and Bed, Bath & Table are also among the leading
    categories.
-   The dashboard reports an on-time delivery rate of 91.88%.
-   The cancellation rate is 0.63%; unavailable orders are tracked as a
    separate status.
-   A sharp decline appears at the end of the monthly trend. The final
    months require additional validation before a business explanation
    is assigned.

## 7. Business Recommendations

1.  **Improve delivery monitoring:** Review late-delivery performance by
    month and customer state to identify operational hotspots.
2.  **Focus category planning:** Use category sales contribution to
    guide merchandising discussions, while adding margin and inventory
    data before making profitability decisions.
3.  **Monitor order exceptions:** Analyse canceled and unavailable
    orders separately to understand where orders leave the fulfilment
    process.
4.  **Strengthen data quality:** Reconcile row counts, date coverage,
    unique keys and monthly totals across Python, SQL and Power BI.
5.  **Validate payment analysis:** Account for multiple payment records
    per order and confirm that payment slicers filter the intended
    measures.

## 8. Repository Structure

``` text
olist-ecommerce-sales-operations-analysis/
│
├── data/
│   └── README.md
│
├── notebooks/
│   ├── 01_data_loading.ipynb
│   ├── 02_data_cleaning.ipynb
│   ├── 03_exploratory_data_analysis.ipynb
│   └── 04_sales_operations_analysis.ipynb
│
├── sql/
│   ├── 01_database_setup.sql
│   ├── 02_order_analysis.sql
│   ├── 03_delivery_analysis.sql
│   ├── 04_product_sales_analysis.sql
│   └── 05_monthly_sales_analysis.sql
│
├── powerbi/
│   ├── Olist_Sales_Operations_Dashboard.pbix
│   └── dashboard_screenshot.png
│
├── reports/
│   ├── Olist_Ecommerce_Project_Report.pdf
│   └── business_insights.md
│
├── presentation/
│   └── project_presentation.pdf
│
├── requirements.txt
├── .gitignore
└── README.md
```

## 9. How to Run the Project

### Prerequisites

-   Python 3.x
-   Jupyter Notebook
-   PostgreSQL and pgAdmin (or another PostgreSQL client)
-   Power BI Desktop
-   Git

### Setup

1.  Clone the repository:

``` bash
git clone https://github.com/<your-username>/olist-ecommerce-sales-operations-analysis.git
cd olist-ecommerce-sales-operations-analysis
```

2.  Create and activate a virtual environment (recommended):

``` bash
python -m venv .venv
```

Windows:

``` bash
.venv\Scripts\activate
```

3.  Install the Python dependencies:

``` bash
pip install -r requirements.txt
```

4.  Download the Olist dataset from Kaggle and place the CSV files in
    your local `data/` folder. Follow `data/README.md` if provided.

5.  Open Jupyter Notebook and run the notebooks in numerical order:

``` bash
jupyter notebook
```

6.  Create the PostgreSQL database named `olist_analytics`, import the
    required CSV tables, and execute the SQL scripts in the `sql/`
    folder.

7.  Open the `.pbix` file in Power BI Desktop. If prompted, update the
    local data-source connection settings. Refresh the model and
    validate the KPI totals and slicer behaviour.

**Important:** Do not publish database passwords, connection strings or
other secrets in notebooks, SQL scripts or this README.

## 10. Validation & Known Limitations

Before treating the dashboard as final:

-   Reconcile monthly sales and order counts against SQL queries on the
    base tables.
-   Confirm the DateTable relationship and chronological month sorting.
-   Test slicers individually and in combination.
-   Verify payment-type filter propagation.
-   Confirm order counts use distinct order IDs.
-   Document the denominator used for each percentage KPI.
-   Investigate the sharp end-of-period decline before describing it as
    a genuine business trend.

The current analysis does not calculate net profit because product costs
and other required financial inputs are not included. Sales should
therefore be described as item-price sales.

## 11. Future Enhancements

-   Investigate the September--October 2018 trend and reconcile the
    latest source dates.
-   Add customer retention and repeat-purchase analysis.
-   Analyse freight charges and delivery cost patterns.
-   Extend seller-level and review-score analysis.
-   Add profitability analysis if reliable cost data becomes available.

## 12. Author

**Vishal Bajpai**\
Aspiring Data Analyst \| Python \| SQL \| PostgreSQL \| Power BI

-   GitHub: `https://github.com/vbajpaid`
-   LinkedIn: `https://www.linkedin.com/in/vishal-bajpai57/`

------------------------------------------------------------------------

*This project is created for learning and portfolio demonstration using
a publicly available dataset. Findings are descriptive and should be
validated before being used for real business decisions.*
