
# Retail Sales Analysis

## Overview

This project analyzes retail sales data using SQL to extract valuable business insights. The analysis covers data cleaning, table creation, and answering key business questions such as identifying top customers, sales trends, and revenue distribution across product categories. This comprehensive approach helps businesses make informed decisions based on the data.

---

## Data Preparation and Cleaning

### Table Creation

The `retail_sales` table was created to store the transactional data with the following SQL script:

```sql
DROP TABLE IF EXISTS retail_sales;
CREATE TABLE retail_sales
(
    transaction_id INT PRIMARY KEY,    
    sale_date DATE,     
    sale_time TIME,    
    customer_id    INT,
    gender    VARCHAR(15),
    age    INT,
    category VARCHAR(15),    
    quantity    INT,
    price_per_unit FLOAT,    
    cogs    FLOAT,
    total_sale FLOAT
);
```

After creating the table, we used `SELECT * FROM retail_sales LIMIT 10` to preview the first few rows of the table for validation.

### Data Cleaning

To ensure the dataset was clean and ready for analysis, the following steps were performed:

1. **Checking for Null Values:**

```sql
SELECT * FROM retail_sales
WHERE transaction_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

2. **Removing Null Values:** All rows with null values in critical columns were removed:

```sql
DELETE FROM retail_sales
WHERE transaction_id IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR gender IS NULL
   OR category IS NULL
   OR quantity IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

After cleaning, the dataset was ready for exploration and analysis.

---

## Data Exploration and Analysis

### Question 1: Who are the top customers by total spending?

**SQL Query:**

```sql
SELECT customer_id, SUM(total_sale) AS total_spent
FROM retail_sales
GROUP BY customer_id
ORDER BY total_spent DESC;
```

**Insight:**

- Identifies customers who contribute the most revenue, enabling targeted loyalty programs.
- Example: Customer ID `3` is the highest spender with `38,440` in total purchases.

---

### Question 2: Which product categories generate the most revenue?

**SQL Query:**

```sql
SELECT category, SUM(total_sale) AS total_revenue
FROM retail_sales
GROUP BY category
ORDER BY total_revenue DESC;
```

**Insight:**

- Reveals high-revenue categories to prioritize inventory and marketing strategies.

---

### Question 3: What are the daily sales trends?

**SQL Query:**

```sql
SELECT sale_date, SUM(total_sale) AS daily_sales
FROM retail_sales
GROUP BY sale_date
ORDER BY sale_date;
```

**Insight:**

- Shows sales trends over time, helping identify peak periods for promotions.

---

### Question 4: Which gender contributes more to sales?

**SQL Query:**

```sql
SELECT gender, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY gender
ORDER BY total_sales DESC;
```

**Insight:**

- Helps develop gender-specific marketing campaigns.

---

### Additional Analysis

#### Sales by Shifts

**SQL Query:**

```sql
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

**Insight:**

- Categorizes sales into shifts (Morning, Afternoon, Evening) for better staffing and promotional planning.

---

## Project Structure

The project is organized as follows:

```
retail-sales-analysis/
├── README.md               # Project documentation
├── data/
│   └── retail_sales.csv    # Cleaned dataset
├── sql/
│   ├── create_table.sql    # Script for table creation
│   ├── data_cleaning.sql   # Script for data cleaning
│   ├── top_customers.sql   # Query to find top customers
│   ├── revenue_by_category.sql   # Query for revenue by category
│   ├── daily_sales.sql     # Query for daily sales trends
│   └── shift_sales.sql     # Query for sales by shifts
├── results/
│   ├── top_customers.csv   # Results for top customers
│   ├── revenue_by_category.csv   # Results for revenue by category
│   ├── daily_sales.csv     # Results for daily sales trends
│   └── shift_sales.csv     # Results for shift-based sales
└── visualizations/         # Visualizations (if any)
```

---

## How to Run the Queries

1. Open the SQL file corresponding to the analysis (e.g., `top_customers.sql`).
2. Execute the query in a PostgreSQL environment (e.g., pgAdmin).
3. Export the results to the `results/` folder for documentation.

---

## Results and Findings

- **Top Customers:** Customer ID `3` is the highest spender.
- **Top Category:** [Insert result after query execution].
- **Daily Trends:** [Insert result after query execution].
- **Gender Insights:** [Insert result after query execution].
- **Shift Insights:** [Insert result after query execution].

---

## Next Steps

- Create visualizations for better storytelling (e.g., bar charts for sales trends).
- Extend the analysis to include profit margin calculations.
- Implement predictive modeling to forecast sales trends.
