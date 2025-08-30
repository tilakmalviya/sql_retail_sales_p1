# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`

This project demonstrates SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.  

---

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.  
2. **Data Cleaning**: Identify and remove any records with missing or null values.  
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.  
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.  

---

## Project Structure

### 1. Database Setup
- **Database Creation**: Create a database named `p1_retail_db`.  
- **Table Creation**: A table named `retail_sales` is created to store the sales data.  

```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

---

### 2. Data Exploration & Cleaning
- Record count (total transactions).  
- Count unique customers.  
- Identify product categories.  
- Check and remove null values.  

---

### 3. Data Analysis & Findings
The following SQL queries were developed to answer specific business questions:

**Write a SQL query to retrieve all columns for sales made on '2022-11-05:**  
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

**Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:**  
```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4;
```

**Write a SQL query to calculate the total sales (total_sale) for each category.:**  
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1;
```

**Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:**  
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

**Write a SQL query to find all transactions where the total_sale is greater than 1000.:**  
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000;
```

**Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:**  
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY 1;
```

**Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:**  
```sql
SELECT 
       year,
       month,
       avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1;
```

**Write a SQL query to find the top 5 customers based on the highest total sales:**  
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

**Write a SQL query to find the number of unique customers who purchased items from each category.:**  
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category;
```

**Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):**  
```sql
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END as shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;
```

---

## Findings
- Customers span across multiple age groups.  
- High-value transactions (>1000) indicate premium purchases.  
- Monthly analysis highlights sales seasonality.  
- Insights into top customers and popular categories.  

---

## Reports
- **Sales Summary**: Total sales, customer demographics, category performance.  
- **Trend Analysis**: Monthly and shift-based sales insights.  
- **Customer Insights**: Top customers and unique buyers per category.  

---

## Conclusion
This project covers database setup, data cleaning, exploratory analysis, and solving business problems using SQL. The insights help in understanding sales patterns, customer behavior, and product performance.  

---

## How to Use
1. Clone the repository.  
2. Run the SQL script in PostgreSQL to set up the database.  
3. Execute the queries for analysis.  
4. Modify queries to explore further insights.  
