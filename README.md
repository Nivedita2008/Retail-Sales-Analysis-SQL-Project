# Retail-Sales-Analysis-SQL-Project
This project demonstrates SQL skills used in real-world data analysis for the retail industry.
It includes creating and managing a sales database, performing data cleaning, exploratory data analysis (EDA), and solving key business problems through SQL queries.
The dataset contains transactional information such as customer demographics, product categories, sales values, and purchase timestamps.

## Objectives

- Create a retail sales table
- Identify and remove records with missing values
- Explore and summarize customer and sales data
- Analyze business performance using SQL analytical queries

## Tools Used

- PostgreSQL 

- PgAdmin

## Concepts Used:

- CRUD operations

- Aggregate functions

- Grouping & filtering

- Window functions (RANK)

- Date & Time Extract logic

- CTE (Common Table Expressions)

## SQL Code Used in This Project
### Create Table
CREATE TABLE sales(  
    transaction_id INT PRIMARY KEY,  
    sale_date DATE,  
    sale_time TIME,  
    customer_id INT,  
    gender VARCHAR(15),  
    age INT,  
    category VARCHAR(25),  
    quantity INT,  
    price_per_unit FLOAT,  
    cogs FLOAT,  
    total_sale FLOAT  
);  
SELECT * FROM sales  
LIMIT 10;  
SELECT count(*) FROM sales  

### DATA CLEANING  

SELECT * FROM sales  
WHERE transaction_id is Null  
OR  
 sale_date is Null  
OR  
 sale_time is Null  
OR  
 customer_id is Null  
OR  
 gender is Null  
OR  
 age is Null  
OR  
 category is Null  
OR  
 quantity is Null  
OR  
 price_per_unit is Null  
OR  
 cogs is Null  
OR  
 total_sale is Null;  
 
#### DELETE DUPLICATES

DELETE FROM sales   
WHERE  
transaction_id is Null   
OR  
 sale_date is Null  
OR  
 sale_time is Null  
OR  
 customer_id is Null  
OR  
 gender is Null  
OR  
 age is Null  
OR  
 category is Null  
OR  
 quantity is Null  
OR  
 price_per_unit is Null  
OR  
 cogs is Null  
OR  
 total_sale is Null;  

## DATA EXPLORATION  

#### How many sales we have ?  
SELECT   
count(*) AS Total_sales   
FROM sales;  

  
#### How many unique customers we have ?  
SELECT   
count(distinct customer_id) AS Total_customer   
FROM sales;  

#### How many unique category we have ?  
SELECT   
distinct category   
FROM sales;  

-- Data Analysis and Business Key Problems & Answers  

#### Q.1  
SELECT * FROM sales  
WHERE sale_date = '2022-11-05';  

#### Q.2  
SELECT *   
FROM sales  
WHERE category = 'Clothing'   
AND quantity >= 4   
AND TO_CHAR(sale_date,'YYYY-MM') = '2022-11';  

#### Q.3  
SELECT category,   
SUM(total_sale) AS net_sale,  
COUNT(category) AS Total_order   
FROM sales  
GROUP BY 1;  

#### Q.4  
SELECT ROUND(avg(age),2) as avg_sale   
FROM sales  
WHERE category = 'Beauty';  

#### Q.5  
SELECT * FROM sales  
WHERE total_sale >1000;  

#### Q.6  
SELECT category,  
gender,   
COUNT(transaction_id) AS Total_trans   
FROM sales  
GROUP BY 1,2  
ORDER BY 1;  

#### Q.7  
SELECT   
Year,   
Month,  
avg_sale  
FROM  
(  
SELECT   
EXTRACT(YEAR FROM sale_date) AS Year,  
EXTRACT(MONTH FROM sale_date) AS Month,  
AVG(total_sale) AS avg_sale,  
RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC)  
FROM sales  
GROUP BY 1,2    
) AS T1  
WHERE RANK = 1;    

#### Q.8  
SELECT customer_id,   
SUM(total_sale) AS total_sales   
FROM sales  
GROUP BY 1  
ORDER BY 2 DESC  
LIMIT 5;  

#### Q.9  
SELECT   
category,   
count(distinct customer_id) AS Customers   
FROM sales  
GROUP BY 1  
ORDER BY 2 DESC ;  

#### Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)|
WITH hourly_sale
AS
(
SELECT *,
CASE 
    WHEN EXTRACT(HOUR FROM sale_time)<12 THEN 'Morning'
	WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
	ELSE 'Evening'
	END AS Shift
	FROM sales
)
SELECT shift, count(*) AS Total_orders FROM hourly_sale
GROUP BY shift;

## Key Insights Summary
- Identified top categories by sales
- Found top customers contributing highest revenue
- Analyzed buying behavior by gender & categories
- Calculated best selling month per year
- Classified orders by time-based shifts
